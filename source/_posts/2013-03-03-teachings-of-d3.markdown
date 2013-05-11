---
layout: post
title: "The Teachings of d3js"
date: 2013-03-03 17:50
comments: true
categories: d3 d3js data_visualization commutative_diagrams TCGA OncoPrint cBioPortal
---

So it's been a while since my first and only post.  This one has been in the
works for far too long.  So long in fact, that I think that I've actually
forgotton many of the details that originally inspired me. On the upside I've
had a chance to think more deeply on some of the more interesting aspects of
this and may have something more general and interesting to say.  We'll just
have to see how it goes!

First of all there's the [d3js library](http://d3js.org/) that everyone seems
to love.  D3 is a visualization library for the browser with a very simple (and
therefore brilliant) design principle: data gets bound to DOM elements which
then get instructions on how to be rendered based on whether the data has
already been rendered, is changing the way it is being rendered, or are being
removed.  D3 does a great job of creating the proper abstractions for making
data visualizations and as a result, teaches you to think about data and
visualization simply and clearly.

Side Note: While riding the subway the other day, I thought of this
commutative diagram for explaining d3's model of data visualization.
Commutative diagrams are one of the ways that mathematicians express their
ideas.  An equation, like \\(2x = 5\\), is a succinct way of expressing
equality. A commutative diagram is a succinct way of expressing equality
between function compositions (composing functions just means taking the output
of one function and putting it into another function in a particular order).
So anyway, here's the picture:

\\[
\begin{array}{ccccccccc}
data & \xrightarrow{\sim} & DOM \newline
\downarrow{\Delta} & & \downarrow{\Delta} \newline
data & \xrightarrow{\sim} & DOM
\end{array}
\\]

The data and the DOM should always share the same structure (represented by
\\(\sim\\), ["twiddle"](http://en.wikipedia.org/wiki/Tilde#Mathematics)).  When
the data changes (Delta, \\( \Delta \\), for difference or change), perhaps
because the data is coming from some source in real time, the DOM should change
accordingly.

Anyway, I recently had the pleasure of using d3 at work to (re)create one of
our data visualizations called *OncoPrint*.  An OncoPrint is just the overlay
of a couple matrices where the data is represented as certain basic glyphs like
rectangles or triangles.  What it gives you is a concise summary of multiple
datatypes across a cohort.  You can read more about them
[here](http://www.cbioportal.org/public-portal/faq.jsp#what-are-oncoprints).

OncoPrints are an important part of the cBioPortal, so I went through the
trouble of implementing them in a number of different ways.  I started with a
JSON structure that was already in place (actually I don't really remember what
the data structure was anymore, but it was something like this):

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

I'll include an actual example because I think that the data is the most
interesting thing about my work, and it's cool to see it as it is in real life
instead of an abstraction.  So `lung` is a cancer type, `BRAF` is a gene, and
`TCGA-01` is just a barcode for a patient.  We generally have been getting data
from the [TCGA Project](http://cancergenome.nih.gov/), which is why I put TCGA
there.  And then there's the actual data - mutations (V is for Valine, 600 is
for the protein location, though how does this work exactly since proteins are
3D?, E is for another amino acid.  So this means a change from V to E at
protein location 600.  Thanks [Hannah](https://files.nyu.edu/hfp209/public/)!)

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
datum and then add some more logic to calculate the x position. Of course, I
could have gotten this to work, but the beauty and simplicity of d3 was an
inspiration to do better.  Who wants to use a beautiful tool to make something
messy?

I think the lesson here is that data structure is extremely important.  That
decision to break up the data into essentially different layers, one for each
data type, was not the right one, though it may be a good way of
conceptualizing OncoPrints (as overlayed matrices).  The "unit" of the
visualization is the sample (that is, a cancer patient's tumor sample) and so
the unit of the data structure should also be a sample.  That's one of the
things about d3 -- there should be a structural correspondence between the data
and the visualization (and the DOM structure behind the scenes).

So that's how a tiny problem -- a couple pixels not lining up correctly -- has
some general implications for data visualization.
