---
layout: post
title: List-decodability of Random Linear Codes
date: 2024-07-12
description:
tags:
categories: coding-theory exposition
---

## Introduction
List-decodability (at least the combinatorial aspect of it) is
concerned with producing codes where only a few codewords fall within any
given Hamming ball of small radius. A code $$\mathcal{C} \subseteq \mathbb{F}_2^n$$ is
said to be list-decodable with list size $$L$$ and radius  $$\rho$$ if at
most $$L$$ codewords lie within every Hamming Ball in $$\mathbb{F}_2^n$$ of radius
$$\rho \cdot n$$. A natural question to ask about list-decodable codes is: for a
fixed (normalized) radius $$\rho$$ and list size $$L$$, what is the maximum rate that a
$$(\rho, L)$$ list-decodable code can have? A simple computation proves that
it is at most $$1-h(\rho) + o(1)$$, where $$h: [0, 1] \rightarrow [0, 1]$$
is the binary entropy function. The quantity $$1-h(\rho)$$ is also known as
the *list-decoding capacity*.

On the positive side, we know of the existence of codes reaching the
list-decoding capacity, by the probabilistic method. Specifically, for any
small $$\epsilon > 0$$, a random code having rate
\\[
    1-h(\rho)-\epsilon
\\]
is list-decodable with list size $$O(1/\epsilon)$$, with high probability. A
proof sketch for this result is as follows: pick a random code of rate $$r$$
by independently choosing $$2^{rn}$$ vectors uniformly from $$\mathbb{F}_2^n$$. For
some fixed set of $$L+1$$ vectors, the probability that all vectors from 
this fixed set are simultaneously contained in the random code is at most 
\\[
    \left(\frac{2^{rn}}{2^n}\right)^{L+1}.
\\]
Moreover, for a fixed Hamming ball centered at some vector $$y \in \mathbb{F}_2^n$$, 
there are at most $$(2^{h(\rho)n})^{L+1}$$ "bad sets" that the 
random code must avoid containing. Lastly, there are $$2^n$$ Hamming balls in
total, centered around each of the $$2^n$$ vectors in $$\mathbb{F}_2^n$$. Therefore, 
we require:
\\[
    2^n \cdot (2^{h(\rho)n})^{L+1} \cdot \left(\frac{2^{rn}}{2^n}\right)^{L+1} < 1.
\\]
Solving for $$r$$, we see that $$r$$ can only be at most $$1-h(\rho)-1/(L+1)$$. 
Upon denoting $$\epsilon = 1/(L+1)$$, we get that a random code of rate at most
\\[
    1-h(\rho)-\epsilon
\\]
is list-decodable with list size $$L = O(1/\epsilon)$$, which is what we wanted to
prove.

For linear codes, a probabilistic proof similar to the one used for the
result above gives a much weaker bound. Specifically, it guarantees that a
random linear code having rate $$1-h(\rho)-\epsilon$$ is list-decodable with
list size $$exp(O(1/\epsilon))$$. Note that the list size in this case is
exponentially larger than the one guaranteed for random codes above. The point
where the proof differs from the one above is where the probability of a 
random linear code excluding a fixed set of $$L+1$$ many vectors needs to be
calculated. Because every codeword is independent of one another in a random
code, we can simply multiply the probabilities of a random codeword equaling
some fixed vector. However, this is not possible with a random linear code, because
the codewords are not linearly independent. (In fact, they are not even 3-wise
independent, because if $$X_1$$ and $$X_2$$ are two codewords from a linear
code $$\mathcal{C}$$, then we know that $$X_1 + X_2$$ also belongs to $$\mathcal{C}$$.) But
we can still use a weaker property: every set of $$L+1$$ vectors has a linearly
independent subset of size at least $$\log(L+1)$$, and a necessary condition
for all $$L+1$$ points to lie within a Hamming ball is for the linearly
independent subset to lie within the aforementioned Hamming ball. However, as
we noted above, this workaround significantly worsens the bound on the list
size, bumping it up from $$O(1/\epsilon)$$ to $$exp(O(1/\epsilon))$$.

Following this, the next development was by Guruswami, Håstad, Sudan and
Zuckerman[^1], where they proved the *existence* of linear binary $$
(\rho, 1/\epsilon)$$-list-decodable codes having rate $$1-h_2
(\rho)-\epsilon$$. Even though they got very good upper bounds on the list
size, the result fell short of claiming the existence of such codes *with high
probability*.  The proof followed a potential function argument; they showed
that if we build the linear code one basis vector at a time, then a certain,
carefully crafted potential function does not increase by much each time a
basis vector is added, and so in the end, the quantity is still small. The
smallness of this quantity in the end implies an upper bound of $$1/\epsilon +
o(1)$$ on the list size. (In 2018, Li and Wootters[^2] were able to tweak
this argument to show that the result does hold with high
probability.)

In 2010, a paper by Guruswami, Håstad and Kopparty[^3] used a different
technique to show that such random linear codes exist with high probability.
This is the proof technique I want to talk about in this blog post, because it
makes use of a clever little structural theorem concerning the intersection of
linear subspaces and Hamming balls, and is particularly interesting. The 
statement of the structural theorem is

> If $$X_1, \ldots, X_\ell \in \mathbb{F}_2^n$$ are vectors sampled independently and
  uniformly from a Hamming ball of radius $$\rho$$, then the probability that
  more than $$C\ell$$ vectors from the span of $$X_1, \ldots, X_\ell$$ also lie
  in the Hamming ball is exponentially small (it is at most $$2^{-5n}$$).

I will give an overview of how this proof technique works to make use of this
structural theorem, and also give a short description of the proof of the
structural theorem itself.

## Identifying problematic cases
Recall that the overall goal is to show that
for a random linear code $$\mathcal{C}$$ of rate $$1-h(\rho)-\epsilon$$ and for
list size $$L=10/\epsilon$$, the probability of any bad event happening is
very low. That is, the quantity:
\\[
    \Pr_{\mathcal{C}}[ \exists x \in \mathbb{F}_2^n, |B_n(x, \rho) \cap \mathcal{C}| \geq L+1]
\\]

should be very small, where $$B_n(x, \rho)$$ is the set of all vectors
which have Hamming distance at most $$\rho n$$ from $$x$$). Glossing over
some minor technical details, it suffices to show that
\\[
    \Pr_{\mathcal{C}}[|B_n(0^n, \rho) \cap \mathcal{C}| \geq L+1] < 2^{-\lambda n}
\\]
for some positive constant $$\lambda$$. That is, it is enough to show that the
probability of a random linear code having too many codewords around the
*origin-centered* Hamming ball is small. The same upper bound will then
translate for any center $$x \in \mathbb{F}_2^n$$, and so we can apply the union
bound to get an upper bound for the first probability.

Going back to the naive probabilistic proof sketched above, one immediately
notices that we are playing it too safe by assuming the worst case scenario:
that every set of $$L+1$$ vectors will have a maximal linearly independent subset
of size only $$\log(L+1)$$. But most of the time, this is not true: most
$$L+1$$-sized subsets will have many more linearly independent vectors within
them. Separating these two cases and dealing with them differently allows us to
achieve our goal.

We first need to list out all the cases before separating them. Consider the
family of all $$L+1$$-sized sets of vectors that appear within a
$$\rho$$-radius Hamming ball centered at the origin. We need our random linear
code to avoid containing every one of these ''bad" sets within it. Divide
this ''bad" set family into two parts: one containing those $$L+1$$-sized sets of
vectors such that the maxmimal linearly independent subset within it is of size at most
$$L/4$$, and the other containing ones having a maximal linearly independent subset of
size greater than or equal to $$L/4$$. These will be our two cases, and the
former case is the more problematic one.

## Slight digression: The Probabilistic Method

Recall the goal of the probabilistic method: given a collection of mathematical
objects, use probabilistic techniques to prove that the collection contains at
least one good mathematical object. (For concreteness, think of the collection
as the set of all linear subspaces of $$\mathbb{F}_2^n$$, and the good mathematical
object as a linear subspace having good list-decoding properties, when viewing
the subspace as an error-correcting code.) In the field of random
(linear) codes, most probabilistic approaches follow one basic template: show
that the probability of a bad event occurring is $$\epsilon$$ for some tiny $$0
<\epsilon < 1$$, then show that the number of bad events is strictly less than
$$1/\epsilon$$, and finally apply the union bound to get that the probability
that none of the bad events occur is non-zero.

I like to think of this particular probabilistic technique in the following
manner: think of someone dropping objects from the top of a building, some
objects are labeled as 'bad', and the rest of them are labeled 'good'. The goal 
is to design a series of nets such that every bad object is caught in one of 
these nets. It is okay for some good objects to get caught as well, as long 
as at least one of them falls through each and every net, and hits the ground below.
The bad events correspond to the nets, of course, and there is a balance that 
needs to be achieved in how they are designed. They might catch too many good 
objects if they are too big, or too numerous, but on the other hand they 
could allow some bad object to slip through if they are too weak.

## Dealing with the cases
As discussed a couple of sections ago, we will can rework the probability that we 
want to upper bound as:
\\[
    \Pr_{\mathcal{C}}[|B_n(0^n, \rho) \cap \mathcal{C}| \geq L+1] \leq \Pr_{\mathcal{C}}
     [\mathcal{C} \text{ contains a bad set from the easy case}] +
\\]
\\[
    \Pr_{\mathcal{C}}[\mathcal{C} \text{ contains a bad set from the hard case}]
\\]

We will bound the two probabilities on the right hand side individually. 
If both of them are exponentially small, then we will be done.

At this point, we note a fact which will come in handy while
dealing with the two cases: for any fixed bad set, no matter what case it is
from, the probability of a random linear code containing that bad set is upper
bounded by the probability of a random linear code containing the maximal
linearly indepedent subset within the bad set. This is because the latter event
is a necessary event for the former one. Therefore, from here on we will only
focus on upper bounding the latter event.


Let us get the easy case out of the way first: the one where the bad set
contains a lot of linear independent vectors (specifically, at least $$L/4$$
elements in this bad set are linearly independent). In this case, upon using
the handy fact above, we get that the probability that a random linear code of
rate $$r$$ simultaneously contains every vector from this bad set is at most
\\[
    \left(\frac{2^{rn}}{2^n}\right)^{L/4}.
\\] Because the total number of bad sets is at most $$(2^{h(\rho)n})^
  {L+1}$$, this number is also an upper bound on the number of bad sets in the
  easy case, and so upon union bounding over these easy-case-bad-sets, we get:
\\[
    \Pr_{\mathcal{C}}[\mathcal{C} \text{ contains a bad set from the easy case}] \leq (2^{h
     (\rho)n})^{L+1} \cdot \left(\frac{2^{rn}}{2^n}\right)^{L/4} \leq 2^
     {-\lambda n}
\\] for some positive constant $$\lambda$$. Now onto the hard (and more
  interesting!) case.

Because the probability of containing some bad set from the hard case is not
necessarily low (something like $$2^{-cLn}$$), as it was in the easy case, we
can't use the naive upper bound on the number of hard-case-bad sets. We instead
have to turn to investigate whether this quantity---the number of hard-case-bad
sets---is low or not. It turns out that it is indeed low, and this is implied
by the structural theorem we mentioned above! For any $$\ell$$ such that
$$\log (L+1) \leq \ell \leq L/4$$, denote $$\mathcal{F}_\ell$$ to be the number of
hard-case-bad sets where the maximal number of linearly independent vectors is
exactly $$\ell$$. Then, because the $$C$$ from the structural theorem above
satisfies $$C<4$$ (take my word for this!), $$L$$ is always greater than
$$C\ell$$ and so,

$$
    \frac{|\mathcal{F}_\ell|}{(2^{h(\rho)n})^{\ell}} 
    \leq 
    \Pr_{X_1, \ldots, X_\ell \sim B_n(0, \rho)} [|span(X_1,\ldots,X_\ell) \cap B_n
     (0^n, \rho)| \geq C \ell].
$$

According to the theorem, the quantity on the right is at most $$2^
  {-5n}$$, and so we get:
\\[
    |\mathcal{F}_\ell| \leq (2^{h(\rho)n})^{\ell} \cdot 2^{-5n}
\\]

which further implies that:

$$\begin{align}
    \Pr_{\mathcal{C}}[\mathcal{C} \text{ contains a bad set from the hard case}] &\leq \sum_
     {\ell = \log (L+1)}^{L/4} |\mathcal{F}_\ell| \cdot \left(\frac{2^{rn}}
     {2^n}\right)^{\ell} \\
     &\leq 2^{-5n} \sum_{\ell = \log (L+1)}^{L/4} 2^{(h(\rho)+r-1)n\ell}
\end{align}$$

If we take $$r$$ to be equal to $$1-h(\rho)-\epsilon$$, where $$\epsilon>0$$ is
very small (recall that $$1-h(\rho)$$ was the list-decoding capacity acheived
by random codes, and our goal was to show that random linear codes with rate
approaching $$1-h(\rho)$$ are also list-decodable with the same parameters as
random codes), we get that the last quantity is equal to:

$$ 2^{-5n} \sum_{\ell = \log (L+1)}^{L/4} 2^{-\epsilon n\ell}. $$

The term inside the summation is maximum when $$\ell=\log (L+1)$$, and there are
at most $$L/4$$ terms in the summation, and so we get:

$$\begin{align}
    \Pr_{\mathcal{C}}[\mathcal{C} \text{ contains a bad set from the hard case}] &\leq 2^
     {-5n} \cdot \frac{L}{4} \cdot 2^{-\epsilon n\log (L+1)} \\
            &\leq 2^{-5n}.
\end{align}$$

The only thing left is to prove the structural theorem.

## Proof of the structural theorem
Here's the statement of the structural
theorem once more, so that you don't have to scroll up to see it:

> If $$X_1, \ldots, X_\ell \in \mathbb{F}_2^n$$ are vectors sampled independently and
  uniformly from a Hamming ball of radius $$\rho$$, then the probability that
  more than $$C\ell$$ vectors from the span of $$X_1, \ldots, X_\ell$$ also lie
  in the Hamming ball is exponentially small (it is at most $$2^{-5n}$$).

Intuitively, this is saying that if I pick a few vectors randomly from a Hamming
ball and look at the subspace spanned by them, then with very high probability
this subspace does not intersect too much with the Hamming ball.

Denoting $$L := C\ell$$, the proof is basically a union bound over some bad
events. To describe these bad events, we need to first describe the set of all
possible linear combinations of any $$\ell$$ vectors from $$\mathbb{F}_2^n$$. This is
easy: every $$u \in \mathbb{F}_2^\ell$$ will correspond to the following linear
combination:
\\[
    \sum_{i \in \ell} u(i) \cdot X_i
\\] where $$u(i)$$ denotes the $$i$$th entry of $$u$$. Now for every subset
  $$S \subset \mathbb{F}_2^{\ell}$$ of size exactly $$L+1$$, a bad event is one where
  every vector described by the linear combinations corresponding to all
  vectors from the tuple falls into the Hamming ball. That is, the following
  event is bad:
\\[
\forall u \in S \quad \sum_{i \in \ell} u(i) \cdot X_i \in B_n(0^n, \rho).
\\] There are $${2^{\ell} \choose L+1}$$ bad events, one for each $$L+1$$ sized
  subset of vectors from $$\mathbb{F}_2^\ell$$, and so the only thing that's left
  before we can apply the union bound is to estimate a good upper bound on the
  probability of each bad event. In pursuit of upper bounding the probability,
  we will choose to, for every $$L+1$$-sized subset $$S$$ of $$\mathbb{F}_2^
  {\ell}$$, only focus on a special subset $$T \subset S$$ of linear
  combinations. This special subset of vectors will have an ordering, and will
  have the property that for each vector $$u_i \in T$$, there are at least two
  coordinates appearing in the support of $$u_i$$ that do not appear in the
  support of all $$u_j, \forall j<i$$. Roughly, the reason why we choose to
  focus on this subset is because it will allow us to use the following neat
  result:

> (*Sum of two random vectors from zero-centered Hamming ball doesn't fall into
  any fixed Hamming ball w.h.p.*): Let $$X_1$$ and $$X_2$$ be two vectors
  sampled independently from one another and uniformly at random from $$B_n
  (0,\rho)$$. Then for **any** $$y \in \mathbb{F}_2^n$$, the probability that
  $$X_1+X_2$$ falls within $$B_n(y, \rho)$$ is very low. In particular, it is
  at most $$2^{-\delta n}$$, where $$\delta>0$$ is some small constant.

The intuitive proof for this result is as follows: because $$X_1+X_2$$ is
essentially a uniformly random vector chosen from the set of all vectors having
weight $$2p(1-p)=2p-2p^2$$, it falls into $$B_n(0^n, \rho)$$ with exponentially
low probability. Also, we have:
\\[
    \Pr[X_1+X_2 \in B_n(y,\rho)] \leq \Pr[X_1+X_2 \in B_n(0^n,\rho)].
\\] 

We can thus proceed with the proof of the structural theorem now. The above
discussion now implies:

\\[
    \Pr[\forall u \in S \quad \sum_{i \in \ell} u(i) \cdot X_i \in B_n
     (0^n, \rho)] \leq \Pr[\forall u \in T \quad \sum_{i \in \ell} u
     (i) \cdot X_i \in B_n(0^n, \rho)]
\\] By a chaining arugment, the RHS is equal to:
\\[
\prod_{j=1}^{|T|} \Pr\left[\sum_{i \in \ell} u_j(i) \cdot X_i \in B_n
 (0^n, \rho) \mid \forall 1 \leq k < j, \sum_{i \in \ell} u_k(i) \cdot X_i \in
 B_n(0^n, \rho)\right]
\\]

But now we have set up everything perfectly in order for the neat result mentioned above to be used. Because of the property that the vectors describing linear combinations in $$T$$ have an ordering, and futhermore are such that every vector has at least two coordinates in its support that does not appear in the previous vectors, we know that for every $$u_j$$ there are two coordinates $$i_1$$ and $$i_2$$ in its support that are not in the support of any $$u_i$$, for $$i<j$$. This allows us to write

\\[
    \sum_{i \in \ell} u_j(i) \cdot X_i = u_j(i_1)\cdot X_{i_1} + u_j(i_2)\cdot X_{i_2} + \sum_{i \in \ell, i \neq i_1,i_2} u_j(i) \cdot X_i.
\\]
This further allows us to rewrite that probability corresponding to $$u_j$$ above as:
\\[
\Pr\left[u_j(i_1)\cdot X_{i_1} + u_j(i_2)\cdot X_{i_2} \in B_n
 \left(\sum_{i \in \ell, i \neq i_1,i_2} u_j(i) \cdot X_i, \rho\right) \mid \forall 1 \leq k < j, \sum_{i \in \ell} u_k(i) \cdot X_i \in
 B_n(0^n, \rho)\right]
\\]
Now because $$X_{i_1}$$ and $$X_{i_2}$$ are independent of the conditioning in the probability, the neat result is applicable and hence this probability is at most $$2^{-\delta n}$$, and so the product of probabilities is at most $$2^{-\delta n |T|}$$.

This was the probability of a single bad event occurring. Recall that there were $${2^{\ell} \choose L+1}$$ bad events, one for each $$L+1$$ sized subset of vectors from $$\mathbb{F}_2^\ell$$. Now $$|T|$$ is large enough so that $$2^{-\delta n |T|}$$ is small enough, and this in turn ensures that:
\\[
    2^{-\delta n |T|} \cdot {2^{\ell} \choose L+1} \leq 2^{-5n}.
\\]
This completes the proof for the structural theorem.







[^1]: Combinatorial Bounds for List Decoding: <https://people.csail.mit.edu/madhu/papers/2000/ghsz-conf.pdf>
[^2]: Improved list-decodability of random linear binary codes: <https://arxiv.org/abs/1801.07839>
[^3]: On the List-Decodability of Random Linear Codes <https://arxiv.org/abs/1001.1386>














