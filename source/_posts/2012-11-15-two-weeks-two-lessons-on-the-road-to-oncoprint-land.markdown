---
layout: post
title: "Two Weeks Two Lessons: on the road to oncoprint land"
date: 2012-11-15 23:17
comments: true
categories: d3, data visualization
---

I'm not the only one who loves the [d3js library](http://d3js.org/).  Do any
google search on the library and you will find countless accolades.

Recently, I had the pleasure of delving into d3 as part of my work at
[cBio](www.cbio.mskcc.org) to (re)create our trademark "oncoprint"
visualization for the [cBio Cancer Genomics Portal](http://www.cbioportal.org).
Previously, the visualization had been rendered "manually" using
[Raphael](http://raphaeljs.com/).  Thinking back to this older implementation,
it's hard to express how beautiful and well-thought-out the abstractions are in
d3.  It does a great job of abstracting precisely the things that should be
abstracted.

Since this is an important part of the cBioPortal, I went through the trouble
of implementing the oncoprint in a number of different ways.  I started with
the data structure that was already in place (actually I don't really remember
what the data structure was anymore, it was *something* like this):

I'll include an actual example because I think that the data is the most
interesting thing about my work, and it's cool to see it as it is in real life
instead of an abstraction.

    lung: {
        { BRAF:
            TCGA-01: {
                mutation:   ['V600E'],
                cna:        'HOMOZYGOUSDELETION',
                mrna:       'DOWNREGULATED',
                rppa:       'UPREGULATED'
            }
        }
    }

I took this and broke it up into a list for each gene and datatype.  So for
BRAF I had a list of things like this,
    { sample-id: 'TCGA-01', mutation: {'V600E'} }
and this
    { sample-id: 'TCGA-01', cna: 'HOMOZYGOUSDELETION' }
and so on for each data type.

I bound each one of these to a rectangle per d3js, hoping to get something like
this:

<svg width=80 height=30>
<rect class="cna" fill="#D3D3D3" x="5" width="5.5" height="23"></rect>
<rect class="mut" fill="#008000" x="4" y="7.666666666666667" width="7.5" height="7.666666666666667"></rect>
<text x="50" y="17.6">(A)</text>
</svg>

The tall rectangle representing a copy number alteration and the small
representing the presence of a mutation.  Each rectangle has it's own piece of
data bound to it, independent of the other data and rectangles.  This is independence is what caused the problem,

<svg width=80 height=30>
<rect class="cna" fill="#D3D3D3" x="5" width="5.5" height="23"></rect>
<rect class="mut" fill="#008000" x="5" y="7.666666666666667" width="7.5" height="7.666666666666667"></rect>
<text x="50" y="17.6">(B)</text>
</svg>

That's right, squint a little.  There is a pixel or two difference between the
two.  In (A) the little green rectangle is centered, in (B) it's flush left.
I'm not going to pretend that this is anything but an utter triviality, but the
reason it was happening was because I wasn't distinguishing between the
different types of data each of which required slightly different positioning.

{% codeblock lang:javascript %}
d3.selectAll('rect')
    .data(data)
    .enter()
    .append('rect')
    .x(function(d, i) {
        return i * space;       // space is the space between each rectangle
        })
{% endcodeblock %}

This just renders the rectangles in whatever order they appear in the list. The
actual code had some if statements that tested for data values: "AMPLIFIED",
"DELETED", "UPREGULATED", or nonempty list (I had to find one of those hacks to
test for `Array` type in javascript since `typeof []` returns `"object"`,
already you can see how this code is becoming "complected").  But in order to
create that difference in space I would have to test for datatype for every
piece of data and then add some more logic to calculating the x position.

I think it is that data structures are extremely important.  People certainly
say this all the time, but this was one of those moments when I realized it for
myself and was happy. The visualization is not organized around datatype (copy
number, mutation, etc), but rather around samples (or take the transpose of the
matrix and consider it gene-wise, shouldn't matter).  Why fight the natural
thing and try to draw it in layers?

So the data structure should probably be as flat as possible and the rendered
elements should use svg `<g>` elements that correspond to samples and which
contain whatever glyphs.

Philosophically, I may as well make a few small points about this.

Data is a list.

One of the things that d3 teaches me is that data is a list.  What does this
mean?

1. You iterate over it
2. There is some uniformity to it, i.e. and of course by list I mean a typed
   list.  So I don't mean some list `[elephant, mars, chair]`.  I don't even
   mean a list like this `[1, "higgly", "biggly"]`.  I mean a list of "structs"
   with the same structure.  Like a bunch of integers, `[1,2,3, ...]`.  Or a
   bunch of names `["Arabic poet", "Chinese poet", ...]`.  Or a bunch of
   combinations of names and numbers `[{name: "Joseph", index: 10}, {name:
   "Reuben", index:2}, ....]`

What follows is a number of realizations that I hope will bring a smile to the
face of the more experienced reader, as opposed to a sad shaking of the head,
"Obviously!" She might say to herself.

The structure of the data is of vital importance to the visualization.  The
structure of the data is a reflection of how you choose to think about the
data, to organize it, categorize it, break it off into little chunks.  Making
these decisions are the core of the discipline and should not be taken lightly,
nor should they be taken merely from an engineering or from a scientific point
of view.  In fact, in this case, d3's engineering teaches you about your data,
in other words, it has an effect on your scientific view of the data -- how you
think about it.

After watching a talk from Rich Hickey, I think that it may be fair to say that
most data structures amount to lists and hashmaps.  Well, really what he is
saying is that these are probably the most widely used and useful data
structures out there.  There are many others.  What are the most effective ones
for "data."  Well, aren't they all for data?  Then it depends on what you want
to do with the data.

What does it mean to think about data?  It means to think about how to break it
up into different pieces, organize it, reorganize it, re-divide it and reform
it.  To actually carry these actions out you need to have computer programming
skills, especially if you are going to be dealing with "big data," defined by
the lower bound of your computer's RAM, but I would say equally bounded below
by common data applications like Excel which only has something on the order of
10,000 rows (??).

I've realized that a good portion of my work on the cBio Portal consists in
creating such lists of uniformity.  This will amuse the more experienced
reader, but I'll say it anyway: it seems like much of the work of data science
is maintaining such a list of data.  I don't mean to demean it at all when I
say that it is very much secretarial work -- organizing, codifying, etc.  It is
from Richard Feyman's **Lectures on Computation** that reminds me of this
analogy, and he was the first to point out that such considerations have lead
to some interesting mathematics.

This is leading to a post about **data science**.  Maybe I should combine them
into one post -- d3 and data science.

What I want to say about data science is that, though I think the name may be a
bit far fetched (and I'd like to be able to just express my current opinions
without insulting anyone) since I don't know if the word *science* should
really be used here since I think that *science* should almost always refer to
physical science, there really is something here, by which I mean that there
seems to be a shared set of skills, maybe even a point of view on things, that
spans across various disciplines and industries that involves knowing what to
do with data.

It was this meet up that got me thinking about this, when I realized that many
of the same problems that we were running into at the lab are actually being
experienced by people at NGOs and in industry around the world.  Science has
been dealing with these issues for longer however, and so they think that they
know what they're doing, but perhaps they are wrong.  Perhaps those old notions
and techniques about data, how it should be collected, managed, and shared, are
actually no longer applicable.

In industry and probably at many NGOs (though the UN and others have been using
statistics from the beginning...right?) people may be more open minded to the
new techniques, but in science it really is quite difficult to get people to
radically shift the way they are thinking about their work.

Which leads to the other post about my thoughts regarding graduate school for
myself, but also the state of biological science.

## Post Title : Data : Long, Uniform Lists and how we create/maintain them (quote from Feynman)
