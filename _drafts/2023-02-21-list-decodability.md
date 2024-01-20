---
layout: post
title: List-decodability of Random Linear Codes
date: 2023-02-21
description: 
tags: 
categories: coding-theory exposition
---

## Introduction
List-decodability (at least the combinatorial aspect of it) is concerned with producing codes where only a few codewords fall within any given Hamming ball of small radius. Formally, a code $$\calC \subseteq \F_q^n$$ is said to be $$(\rho, L)$$ list-decodable if for every $$x \in \F_q^n$$, the number of codewords with the Hamming ball $$B(x, \rho n)$$ is at most $$L$$. A natural question to ask about list-decodable codes is: for a given radius $$\rho$$ and list size $$L$$, what is the maximum rate that a $$(\rho, L)$$ list-decodable code can have? We already know the answer to this, it is at most:
\\[
1-h_q(\rho) + o(1)
\\]
where $$h_q: [0, 1] \rightarrow [0, 1]$$ is the $$q$$-ary entropy function. The quantity $$1-h_q(\rho)$$ is also known as the *list-decoding capacity*.

On the postive side, we know of the existence of codes reaching the list-decoding capacity. Specifically, for any small $$\epsilon > 0$$, we know of existence of codes with rate $$1-h_q(\rho)-\epsilon$$ that are list-decodable with list size $$O_q(1/\epsilon)$$. Unfortunately, we do not know how to produce explicit codes that are able to meet this bound.


The same result holds with high probability for random codes too (in fact, the above result is implied by the result for random codes). This result follows from a simple probabilistic argument, and it is tight as well (that is, there is a constant $$c_{\rho, q}$$ such that with high probability, a random code of rate $$1-h_q(\rho)-\epsilon$$ is not list-decodable with list size less than $$c_{\rho, q}/\epsilon$$. This result was proved in [this paper](https://cse.buffalo.edu/faculty/atri/papers/coding/rand-list-lb.pdf)).

Figuring out the bounds for random linear codes was much trickier, and even today, precise bounds are still not known. As with the case of random codes, we know of upper and lower bounds for the list size. Zyablov and Pisnker proved an upper bound of $$exp(O_q(1/\epsilon))$$ for the list size of random codes with rate $$1-h_q(\rho)-\epsilon$$. This is exponentially worse than the bound of $$O(1/\epsilon)$$ for random codes. The proofs for both results follow the same template, and they differ at the point where the probability of a set of $$L$$ codewords from $$\calC$$ lying within a Hamming ball needs to be calculated. Because every codeword is independent of one another in a random code, we can simply multiply the probability that each codeword lies in that Hamming ball. However, this is not possible with a random linear code, because the codewords are not linearly independnent. In fact, they are not even 3-wise independent, because if $$X_1$$ and $$X_2$$ are two codewords from a linear code $$\calC$$, then we know that $$X_1 + X_2$$ also belongs to $$\calC$$. But we can still use a weaker property: every set of $$L$$ vectors has a linearly independent subset of size at least $$log(L)$$, and a necessary condition for all $$L$$ points to lie within a Hamming ball is for the linearly independent subset to lie within the aforementioned Hamming ball. This however, significantly worsens the bound on the list size, bumping it up from $$O(1/\epsilon)$$ to $$exp(O_q(1/\epsilon))$$.

Following this, the next development was by Guruswami, Håstad, Sudan and Zuckerman[^1], where they proved the *existence* of linear binary $$(\rho, 1/\epsilon)$$-list-decodable codes having rate $$1-h_2(\rho)-\epsilon$$. Even though it got very good upper bounds on the list size, there were two shortcomings with this result: first, it is only applicable to binary linear codes (and it uses this fact in a crucial way), and second, it was not able to say that this result holds with high probability. Li and Wootters[^2] were able to tackle the second obstacle and show that the result holds with high probability, however they were not able to extend it to higher alphabet codes. Both results followed a potential function argument; they showed that if we build the linear code one basis vector at a time, then a certain, carefully crafted potential function does not increase by much each time a basis vector is added, and so in the end, the quantity is still small. The smallness of this quantity in the end implies an upper bound of $$1/\epsilon + o(1)$$ on the list size.

The next improvement was by Guruswami, Håstad and Kopparty[^3], where they showed that with high probability, random linear codes having rate $$1-h_q(\rho)-\epsilon$$ are $$(\rho, c_{\rho, q}/\epsilon)$$-list-decodable. The starting point for this result was the obstacle faced by Zyablov and Pinsker mentioned above, where they were forced to use:
\\[
Pr[\text{L codewords lie in a Hamming ball}] \leq
\\]
\\[
 Pr[\text{A log(L) sized linearly independent subset of the L codewords lie in the Hamming ball}]
\\]
and then union bound over all linearly independent subsets of size $$L$$. So the only way we can 'cover' the events that we want ($$L$$ codewords lying in a Hamming ball), is to cover some 'larger' events ($$log(L)$$ sized linearly independent subsets lying in a Hamming ball). It turns out that this is quite wasteful, and it covers much more than we want, which in turn degrades the bound by quite a bit. GHK formally showed this by proving the following structural result:

> If $$X_1, \ldots, X_\ell \in \F_q^n$$ are independent random variables sampled uniformly from a Hamming ball of radius $$\rho$$, then the probability that more than $$C\ell$$ vectors from the span of $$X_1, \ldots, X_\ell$$ also fall in the Hamming ball is exponentially small.











[^1]: Combinatorial Bounds for List Decoding: <https://people.csail.mit.edu/madhu/papers/2000/ghsz-conf.pdf>
[^2]: Improved list-decodability of random linear binary codes: <https://arxiv.org/abs/1801.07839>
[^3]: On the List-Decodability of Random Linear Codes <https://arxiv.org/abs/1001.1386>














