---
title: "Unconscious Algorithms"
date: 2020-12-23T16:54:37-05:00
draft: false
toc: false
images:
tags:
---

The goal of this article is to convey an approach to solving data structure and
algorithm problems. My hope is that it helps you in a situation where you might
otherwise draw a blank. 

I want to start by stating an obvious but important truth: your brain is
**amazing**. You are constantly and rapidly solving unimaginably complex problems
that make even the most challenging FAANG whiteboard problem look like a stroll
in the park.

> “But the gap between robots in imagination and in reality is my starting
> point, for it shows the first step we must take in knowing ourselves:
> appreciating the fantastically complex design behind feats of mental life we
> take for granted. The reason there are no humanlike robots is not that the
> very idea of a mechanical mind is misguided. It is that the engineering
> problems that we humans solve as we see and walk and plan and make it through
> the day are far more challenging than landing on the moon or sequencing the
> human genome. Nature, once again, has found ingenious solutions that human
> engineers cannot yet duplicate. When Hamlet says, “What a piece of work is a
> man! how noble in reason! how infinite in faculty! in form and moving how
> express and admirable!” we should direct our awe not at Shakespeare or Mozart
> or Einstein or Kareem Abdul-Jabbar but at a four-year old carrying out a
> request to put a toy on a shelf.”

Source: *How the Mind Works* by Steven Pinker.

Pinker goes on to say, “The faculty with which we ponder the world has no
ability to peer inside itself or our other faculties to see what makes them
tick.” With all due respect, this claim is hyperbolic. 

We are all students of the mind -- simultaneously scientist and lab rat. When we
make consciousness the subject of consciousness, we can pull back the veil and
bear witness to the built-in algorithms that are otherwise taken for granted. 

This process can be leveraged to help demystify data structure and algorithm
questions. Let's walk through a specific problem to illustrate the concept.


>Implement strStr().
>
>Return the index of the first occurrence of needle in haystack, or -1 if needle
>is not part of haystack.
>
>For the purpose of this problem, we will return 0 when needle is an empty
>string. 

Source: https://leetcode.com/problems/implement-strstr/

First and foremost, you need example input. If it's not provided by default,
either ask for it or define your own inputs. 

In this case, Leetcode provides us with example input:

```
Example 1:
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Resist** the temptation to write code.

This stage is not about coding, it's about observing and deconstructing how your
mind arrives at a solution.

Can you determine, at a glance, whether or not "hello" contains "ll"? Of course
you can. It does. This conclusion seems to materialize instantaneously, doesn't
it? It also seems to require little to no effort. Take a moment here to
appreciate just how effective your mind is at this task.

The operative question is: how? More specifically, how **does** your mind solve it?
The crux of this article is that you should focus on that question as opposed
to the categorically different question of: how **should** you solve this? 

When I slow down and observe how my mind tackles this problem, I arrive at the
following deconstruction. It seems as if my mind identifies the length of the
needle, then iterates through the haystack, making needle-sized chunks as it
goes, comparing each chunk to the needle for equality.

Practically speaking, it looks something like this:

Does "he" equal "ll"? No.

Does "el" equal "ll"? No.

Does "ll" equal "ll"? Yes! 

Now it's just a matter of identifying the matching index, which is 2. 

Simple enough, right? 

We can probably dissect it further using three pointers but for the sake of
brevity I'll sidestep the added complexity. 

Now that we have a proposed explanation for how the mind solves this problem,
translating it into code is comparatively trivial. 

Avoid premature optimization at this stage. Sometimes, the mind's approach is
efficient; other times it is inefficient and you can improve on it. Either way,
defer optimization. First make it work, then make it better. An inefficient
solution is better than no solution. 

Here is a pretty simple implementation of the process outlined above in Python.

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        haystack_length = len(haystack) - 1
        needle_length = len(needle) - 1
        i = 0

        while i <= haystack_length - needle_length: 
            if haystack[i:i + needle_length + 1] == needle:
                return i
            i += 1
            
        return -1
```
In a nutshell, this implementation starts at the first element of haystack,
creates a needle-sized slice, and compares the slice to the needle for equality.
If equality is confirmed, it returns the index. If not, the index is iterated
and the slice and comparison is repeated starting from the next element. This
process continues until an equality check succeeds or the index reaches
`haystack_length - needle_length`, since any slice past that point will not be
long enough to match the needle in size.  

The time and space complexity are both O(n). 

I hope you found this helpful. Remember, your brain is awesome. If you can solve
a problem without code, you already know the solution, it's just a matter of
unpacking it. 

