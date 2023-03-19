---
title: "Reinventing the wheel"
date: 2023-03-18
draft: false
toc: false
images:
tags:
  - untagged
---

![](https://i.giphy.com/media/UqPhCdioYHmdq/giphy.gif)


After receiving a Rubik’s cube for free at KubeCon NA, I decided to teach my six
year old daughter how to solve them, but first, I had to teach myself.

It took approximately 2 hours of watching Youtube videos to learn. Around half of that
time was spent finding a tutorial that didn't suck.

I taught my daughter over the course of a few evenings. She picked it up like a
baws and impresses the heck out of people with it.

A few people have asked me, “Could you have figured it out on your own?”

My answer is an unequivocal, “F\*ck no."

Solving the final layer requires executing the right algorithm (four turns) once
on the right, then once on the left, then five times on the right, then five
times on the left. About a fourth of the time, you have to run through that
whole sequence twice.

That comes out to:

```
2 * (1 + 1 + 5 * 4 + 5 * 4)
```

i.e. **84** discrete turns for the top layer alone!

How someone is meant to figure that out on their own is beyond me. And why the
hell would I want to bang my head against the wall for months when I can just
watch a video and learn it in two hours? Apparently, Ernő Rubik, the inventor of
the dang thing, took a month to solve it.

This experience got me thinking about the dialectic between knowledge transfer
and figuring sh\*t out for yourself -- in particular, the often vague boundary
between when to deploy one strategy vs the other, particularly in the context of
data structures and algorithms.

I love learning about brilliant and elegant solutions for complex problems. I
don't particularly enjoy trying to reinvent them from scratch. It feels like a
waste of time.

If watching the Rubik's cube tutorial _first_ was so effective, efficient, and
rewarding, why was I staring at a blank leetcode editor for so long?

I decided to try a new strategy.

I would watch a video on [neetcode](https://neetcode.io/) before bed. The
following night, I would implement the solution from memory on leetcode. 

It has been a complete game changer.

Previously, I only turned to explanations after I'd given up reinventing the
wheel. They came to represent my inadequacy. They were a crutch, a white flag of
surrender.

Now, they're a delight. The efficient solutions are elegant and inspired. The
explanations are clear and succinct.

Next day implementation has been surprisingly quick and easy. I look forward to
testing recall again further down the line.

Everyone learns differently, but on the off chance that someone else could
benefit from it, I wanted to share my positive experience. There's nothing wrong
with starting with the explanations first!
