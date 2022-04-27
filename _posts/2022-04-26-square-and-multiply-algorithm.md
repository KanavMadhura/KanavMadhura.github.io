---
layout: page
title:  "The Square-And-Multiply Algorithm"
subtitle: "Computing Large Powers Modulo n"
date:   2022-04-26 09:30:21 +0530
categories: ["math"]
usemathjax: true
---

So, I took an introductory class on number theory this semester. We learned a LOT of interesting content, but something that I thought would be perfect for a blog post was the "square-and-multiply" algorithm.  

First, let's get some prerequisites out of the way. I will assume that you are reasonable comfortable with modular arithmetic, but will briefly go over how it is defined and some related notation.  
#### Definition: Modular Congurence

Let \\(a,b\\) be integers. We say that \\(a \equiv b \pmod{n}\\), or \(a\) is *congruent* to \(b\) mod \(n\), if both \(a\) and \(b\) have the same remainder when divided by \(n\). There is a more [formal definition](https://en.wikipedia.org/wiki/Modular_arithmetic), but this will suffice for our purposes.  

#### Notation: Congruence Classes (With Respect to Modularity)  

This is just for notational convenience, so there is no need to go too much into detail here. We say that the *congruence class* of some integer \(a\) modulo \(n\) is the set of all integers \(b\) such that \(a \equiv b \pmod{n}\). We denote this set as \([a]_n\).  

To end things off, let me say that there are a lot of different ways to think about this! Specifically, in the integer case, we can convert everything to binary and proceed from there. See this [Numberphile video](https://www.youtube.com/watch?v=cbGB__V8MNk&t=715s) for more.
