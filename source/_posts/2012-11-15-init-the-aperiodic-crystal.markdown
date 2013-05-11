---
layout: post
title: "Init: The Aperiodic Crystal"
date: 2012-11-15 22:23
comments: true
published: true
categories: physics biology Schrödinger qm quantum mechanics whatislife? dna
---

{% blockquote Erwin Schrödinger, What is life? %}
"We believe a gene - or perhaps the whole chromosome fiber - to be an aperiodic
solid"
{% endblockquote %}

About five months ago I read Schrödinger's essay, _What is Life?_ and was
inspired to start this blog.  I want to have a place to work things out for
myself, share them, post little projects, and my philosophical musings on
things. Maybe this is where I can start a habit of writing my own
"[EWDs](http://www.cs.utexas.edu/~EWD/)".  So yes, this is yet another rambling
blog!  I will be keeping Schrödinger's opening remarks close to heart:

> "...some of us have to be willing to express opinions based on incomplete
> knowledge -- at the risk of making fools of ourselves."

So what is the Aperiodic Crystal?  It is the carrier of the hereditary
material, which is, at the same time, the foundation of biological mechanism.
We've heard it all before (!) it is DNA.  Let's unravel this a bit.
Aperiodic does not have any special meaning, it just means not repeating.  By
crystal, you could say that Schrödinger means solid, the salient property of
solids being that they are somehow "stable."  To appreciate the notion of
"stability," a person probably has to know something about the wild world of
molecules and I am not that person right now, but let's try anyway.

There are many ways to go down this rabbit hole.  You could talk about entropy,
about Quantum Mechanics, and perhaps about other things too, but let's just
talk about what my pdf of the essays has as the "\/n rule."  Let's talk about
statistics.  One of the upshots of reading a bootleg copy of the essay is that
there are tons of typos. The \/n rule was difficult to decipher, but I think
I've done it (with a few comments from a friend working on getting his PhD in
physics!), and I'm sure that it relates to "stability."

The \/n rule refers to the standard deviation of the sample mean. Suppose you
have some iid random variables, \\( X\_1, X\_2, \ldots, X\_k \\) and you are
interested in their sample mean, \\(\overline{X}\\)

\\[
\overline{X} = \frac{1}{n} \sum\_{i = 1} ^n X\_i.
\\]

Let's consider this as an estimator for the "true mean," of the distribution
(more on the existence of the "true mean" in a future post. Suffice it to say,
this seems to be a controversial concept).  How much is this estimator going to
vary?  Since the estimator is itself a function of random variables, we can
calculate its variance.

\\[
Var(\hat{\mu}) = Var\frac{1}{n} \sum\_{i = 1} ^n X\_i =
\frac{1}{n ^2} \sum\_{i = 1} ^n Var(X\_i)
\\]

(this follows from the definition of the standard variance).  If we say
that \\(Var(X\_i) = \sigma ^2 \\), then we have this expression

\\[
Var(\hat{\mu}) = \frac{1}{n ^2} \sum\_{i = 1} ^n \sigma ^2 = \frac{1}{n} \sigma ^2
\\]

This is the square root law that Schrödinger is referring to (what we got here
was the variance, the square of the deviation.  Take the square root and you
get your \\( \sqrt{n} \\), or as you have it in my pdf, \/n =) ).

Now how does this relate back to the stability of molecules, that is, their
tendency (or lack thereof) to maintain a certain configuration?  For some
reason (so claims Schrödinger as well as many other physicists, Richard Feynman
for example, discusses this at length in his book *QED*), the configuration of
these molecules scales proportionally to the square root of the number of
molecules.  That is to say, the more molecules you have the more certain you
are of their configuration.  By configuration you could think about their
position, or velocity, tiny balls bouncing around in space, but I think that it
is more correct to think about actual measurements that you could make of these
particules.  For example, if you were to compress them in a box, with how much
force would they push back.  A friend of mine told me once that in some sense,
physics is just a collection of canonical experiments.

But why?  Schrödinger discusses the variance of the number of particles in a
volume, but what are the random variables \\( X\_i \\) that would allow you to
go through the reasoning we did above?  I have a feeling that to really
understand this you need to know Quantum Mechanics.

In any case, the fewer particles you have, the more erratic their group
behavior.  In the microscopic world, where rulers come in micrometers not
meters, a given area is going to have fewer particles than in our, one meter
world.  Therefore, the inclination towards chaos is much higher.

This is problematic for biological systems.  You can't have chunks of machinery
getting blasted apart, shuttled away, or who knows what. And clearly, you also
can't have the script being altered. A change in a particular spot may have
dire consequences.  The DNA molecule, as itself a chunk of stuff, must be
resistant to change.  Therefore, and this is going to sound a bit strange, the
DNA molecule has to be a molecule.  =)

Yes, it sounds strange to us now, but remember that Schrödinger's essay was
written shortly before the structure of DNA was known.  One could imagine that
the carrier of hereditary information and the "script" were different things.
You could imagine that the "script" were somehow a disorganized pool of stuff,
a liquid with less rigid structure.  The brilliance of the essay was to show,
using physics and reason, why this simply could not be the case. The carrier of
heredity must be of some sort of ordered unity, joined together by strong
forces (electrostatic) that prevent it, not just form being ripped apart, but
from being altered in the slightest fashion.  Such a thing is a single
molecule, a crystal, a solid.
