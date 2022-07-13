---
layout: page
title:  "The Square-And-Multiply Algorithm"
subtitle: "Computing Large Powers Modulo n"
date:   2022-07-12 19:57:21 +0530
categories: ["math"]
usemathjax: true
---

## Some Background 

So, I took an introductory class on number theory this semester. We learned a LOT of interesting content, but something that I thought would be perfect for a blog post was the "square-and-multiply" algorithm.  

First, let's get some prerequisites out of the way. I will assume that you are reasonable comfortable with modular arithmetic, but will briefly go over how it is defined and some related notation.  

**Definition: Modular Congurence:** Let \\(a,b\\) be integers. We say that \\(a \equiv b \pmod{n}\\), or \\(a\\) is *congruent* to \\(b\\) mod \\(n\\), if both \\(a\\) and \\(b\\) have the same remainder when divided by \\(n\\). For example, every even number is congruent to every even number modulo \\(2\\), and \\(16 \equiv 10 \pmod{6}\\). Furthermore, there exists a unique natural number \\(0 \leq r < n\\) such that \\(a \equiv r \pmod{n}\\). For example, \\(9 \equiv 4 \pmod{5}\\). There is a more [formal definition](https://en.wikipedia.org/wiki/Modular_arithmetic), but this will suffice for our purposes.   

**Notation: Congruence Classes (With Respect to Modularity):** This is just for notational convenience, so there is no need to go too much into detail here. We say that the *congruence class* of some integer \\(a\\) modulo \\(n\\) is the set of all integers \\(b\\) such that \\(a \equiv b \pmod{n}\\). We denote this set as \\([a]_n\\). From this can derive a nice property:, \\([a^{k/2}]^2_n = [a^k]_n\\).

With these out of the way, we can now get to the actual algorithm.  
## Square and Multiply Algorithm  

Say we want to compute the remainder of some intger to the power of a huge number, when divided by some other integer. This may seem like a really niche situation, however this sort of problem comes up all the time in computer science, and is of special significance to crypotgraphy.  

More formally, we can say the following: Let \\(a,k,n \in \mathbb{N}\\). Say we want to to compute \\([a^k]_n\\), where by compute we mean that we want to find the unique integer \\(0 \leq r < n\\) such that \\([a^k]_n = [r]_n\\). 

How do we do this? Well, we want to use the fact that every integer has a unique binary representation. While I find that actually using binary makes this algorithm unnecessarily hard to read, this fact is important. It allows us to extrapolate and conclude that every integer is the sum of unique powers of two; specifically at most \\(\log_2(k)\\) powers of two. This is definitely non-trivial, but not very difficult to prove. (Hint: Use cases for even and odd integers. The odd case follows immediately from the even one) 

Anyways, here are the steps of the algorithm (noting that \\(m=\lfloor\log_2(k)\rfloor\\)):

**1. Represent \\(k\\) as a sum of powers of two; \\(\sum^m\_0 c_i2^i\\) where \\(c_i \in \{0,1\}\\)**  
    - The notation here may seem like a lot but we will need it for the next steps. We need the \\(c\_i\\) coefficient so we can get rid of powers that aren't in the sum; for example \\(5 = (1)2^0 + (0)2^1 + (1)2^2\\)  
**2. Compute \\([a^{2^i}]\_n = [r\_i]\_n\\) for \\(1 \leq i \leq m\\) iteratively, where the next iteration is computed by \\([a^{2^{i+1}}]\_n = [a^{2^i}]^2\_n=[r\_i^2]\_n\\)**  
    - We will need to compute with every \\(i\\) even if \\(2^i\\) has a coefficient of \\(0\\) from step 1. This is so we can compute the next modulo easily.  
**3. Find \\([a^{k}]\_n = \Pi^m\_{i=0}[a^{c_i2^i}]\_n = \Pi^m\_{i=0}[r_i]\_n\\)**  
    - This turns the problem into simply finding a product of numbers, where each number is less than \\(n\\). It may get out of hand if computing it manually, but a computer will do this with ease. The reason this works is because the modulo of a product is the product of the modulo.  
    
When I first saw the algorithm introduced like this, I was pretty intimidated, but an example should help.  
#### Example

Compute \\([5^{4100}]_{36}\\).  

First we have that \\(4100 = 2^2 + 2^{12}\\). Then we iteratively compute:  
\\[
\begin{align\*}
    &[5^{2^0}]\_{36} = [5]\_{36} \\ 
    &[5^{2^1}]\_{36} = [5^{2^0}]^2\_{36} = [5]^2\_{36} = [25]\_{36} \\
    &[5^{2^2}]\_{36} = [5^{2^1}]^2\_{36} = [25]^2\_{36} = [625]\_{36} = [13]\_{36} \\
    &[5^{2^3}]\_{36} = [5^{2^2}]^2\_{36} = [13]^2\_{36} = [169]\_{36} = [25]\_{36}
\end{align\*}
\\]
We can see that \\([13]\_{36}\\) and \\([25]\_{36}\\) begin to alternate. Continuing
this pattern, we see that \\([5^{2^{12}}]\_{36} = [13]\_{36}\\)  

Thus we finally obtain \\([5^{4100}]\_{36} = [5^{2^2}]\_{36}[5^{2^{12}}]\_{36} = [13]^2\_{36} = [25]\_{36}.\\)  
## Alternate Perspectives

To end things off, let me say that there are a lot of different ways to think about this! Specifically, we can convert everything to binary and proceed from there. See this [Numberphile video](https://www.youtube.com/watch?v=cbGB__V8MNk&t=715s) for more.

We can also use this algorithm to compute polynomials modulo polynomials, although slightly modified. For example, instead of considering \\(r < n\\), we have \\(\deg(r(x)) < \deg(n(x))\\). I may make an addendum post going over it later.
