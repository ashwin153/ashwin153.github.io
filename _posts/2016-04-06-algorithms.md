---
layout:			post
title:			"Algorithms (CS 331H)"
category:		"Classes"
date:			2016-04-06 04:20:00
author:			"Ashwin Madavan"
header-img:		"img/algorithms-bg.jpg"
---

# Workshop Selection
Let $$D$$ be the set of all possible days to hold a workshop and let $$P$$ be the set of people interested in attending the workshop such that each person $$p_{i}$$ wants to attend during $$[s_{i}, f_{i}] \subset D$$. Each day $$d_{i}$$ costs $$c_{i}$$, but each person $$p_{i}$$ will willing pay $$v_{i}$$ to attend. **Suppose there is no limit to the number of people that can attend on a particular day, what days should you select to maximize profit?**

Constrct a flow network $$N = (V, E)$$ as follows:  

* Create a source vertex $$s \in V$$ and a sink vertex $$t \in V$$
* Create a vertex for each person $$p_{i} \in P$$
* Create a vertex for each day $$d_{j} \in D$$
* Create a directed edge $$(s, p_{i})$$ of capacity $$v_{i}$$ for all $$p_{i} \in P$$
* Create a directed edge $$(d_{i}, t)$$ of capacity $$c_{i}$$ for all $$d_{i} \in D$$
* Create a directed edge $$(p_{i}, d_{j})$$ of capacity $$\infty$$ for all $$p_{i}$$ such that $$d_{j} \in [s_{i}, f_{i}]$$

For some subset $$U \subset V$$, let $$rev(U) = \sum\nolimits_{p_{i} \in U} v_{p}$$ and $$cost(U) = \sum\nolimits_{d_{i} \in U} c_{i}$$. Clearly, $$profit(U) = rev(U) - cost(U)$$.

Find the minimum cut $$S, T$$ of $$N$$. Because all $$s-t$$ paths contain edges of finite capacity by construction, the minimum cut must always be finite. Therefore, the cut-set of the minimum cut $$S, T$$ may only contain edges of the form $$(s, p_{i})$$ or $$(d_{i}, t)$$. Because no edges may cross the cut, all the desired days for each person in $$S$$ must be in $$S$$. Consequently, the cut capacity is defined as:

$$c(S, T) = \sum\nolimits_{p_{i} \in T} v_{i} + \sum\nolimits_{d_{j} \in S} c_{j} = rev(T) + cost(S)$$ 

Therefore, 

$$\begin{align*}profit(S) &= rev(S) - cost(S) \\
&= rev(V) - (rev(T) + cost(S)) \\ 
&= rev(V) - c(S, T)\end{align*}$$ 

Because $$c(S, T)$$ is minimal, $$profit(S)$$ must be maximal. Therefore, the days $$d_{i} \in S$$ generate the maximal profit of $$rev(V) - c(S, T)$$.

# Detecting Arbitrage
Let $$C$$ be a set of currencies and let the $$R$$ be the set of exchange rates between currencies such that $$r(c_{i}, c_{j}) \in R$$ is the exchange rate between currencies $$c_{i}$$ and $$c_{j}$$. Clearly, $$r(c_{i}, c_{j}) = \frac{1}{r(c_{j}, c_{i})}$$. Arbitrage occurs when there are opportunities for riskless profit. **How do you detect arbitrage opportunities between currencies?**

In the graph $$G = (C, R)$$ arbitrage occurs where there exists a cycle of currencies $$c_{1},c_{2},\ldots,c_{k},c_{1}$$ such that:

$$\begin{align*}r(c_{1}, c_{2}) r(c_{2}, c_{3}) \ldots r(c_{k-1}, c_{k}) r(c_{k}, c_{1}) &> 1\\
\log r(c_{1}, c_{2}) + \log r(c_{2}, c_{3}) + \ldots + \log r(c_{k-1}, c_{k}) + \log r(c_{k}, c_{1}) &> 0\\
\log \frac{1}{r(c_{1}, c_{2})} + \log \frac{1}{r(c_{2}, c_{3})} + \ldots + \log \frac{1}{r(c_{k-1}, c_{k})} + \log \frac{1}{r(c_{k}, c_{1})} &< 0\end{align*}$$

Therefore, detecting arbitrage opportunities in $$G$$ is equivalent to finding negative cost cycles in the graph $$G' = (C, \{ \log \frac{1}{r} : r \in R\})$$, which can be found in $$O(\lvert CR \rvert)$$ using the Bellman-Ford Algorithm.

# Tiling
**How many ways can one tile a 3 x n rectangle using 2 x 1 tiles?** It is immediately clear that $$n$$ must be even, because it is impossible to tile an odd area with even area tiles. Therefore, $$n = 2 \cdot k$$ and $$k \in \mathbb{N}$$. By enumerating the various ways to tile 2 x 1 tiles on a 3 x $$2 \cdot k$$ rectangle for $$k \in \{1, 2, 3\}$$, I determined that the number of tilings: 

$$\begin{equation*}T(k) = 3 \cdot T(k - 1) + 2 \cdot T(k - 2) + 2 \cdot T(k - 3) + ... + 2 \cdot T(0)\end{equation*}$$ 

$$\begin{equation*}T(k - 1) = 3 \cdot T(k - 2) + 2 \cdot T(k - 3) + ... + 2 \cdot T(0)\end{equation*}$$ 

By subtraction of these two equations, 

$$\begin{align*}
T(k) - T(k - 1) &= [3 \cdot T(k - 1) + 2 \cdot T(k - 2) + ... + 2 \cdot T(0)] - \\
&\ \qquad [3 \cdot T(k - 2) + 2 \cdot T(k - 3) + ... + 2 \cdot T(0)] \\
&= 3 \cdot T(k - 1) - T(k - 2)\end{align*}$$

Therefore, $$T(k) = 4 \cdot T(k - 1) - T(k - 2)$$. The characteristic equation of this linear homogeneous recurrence relation is $$r^{2} = 4 \cdot r - 1$$. The roots of this characteristic equation are $$a_{1} = \frac{4 + \sqrt{12}}{2} = 2 + \sqrt{3}$$ and $$a_{2} = \frac{4 - \sqrt{12}}{2} = 2 - \sqrt{3}$$. Suppose the closed-form solution of the recurrence relation is of the form 

$$T(k) = c_{1} a_{1}^{k} + c_{2} a_{2}^{k} = c_{1} (2 + \sqrt{3})^{k} + c_{2} (2 - \sqrt{3})^{k}$$

$$\begin{align*}T(0) = 1 \implies 1 &= c_{1} + c_{2} \implies c_{2} = 1 - c_{1} \\
T(1) = 3 \implies 3 &= c_{1} (2 + \sqrt{3}) + c_{2} (2 - \sqrt{3}) \\
&= c_{1} (2 + \sqrt{3}) + (1 - c_{1}) (2 - \sqrt{3}) \\
&= 2 \sqrt{3} c_{1} + 2 - \sqrt{3}\end{align*}$$

Consequently, $$c_{1} = \frac{1 + \sqrt{3}}{2 \sqrt{3}}$$ and $$c_{2} = 1 - \frac{1 + \sqrt{3}}{2 \sqrt{3}} = \frac{\sqrt{3} - 1}{2 \sqrt{3}}$$. Therefore, the closed-form solution to the recurrence relation is $$T(k) = \frac{1 + \sqrt{3}}{2 \sqrt{3}} (2 + \sqrt{3})^{k} + \frac{\sqrt{3} - 1}{2 \sqrt{3}} (2 - \sqrt{3})^{k}$$ and solves the problem in $$O(1)$$ time and $$O(1)$$ space.

**How many ways can one tile a k x n rectangle using 2 x 1 tiles?** Assuming we can tile the first n-1 columns of the rectangle, how many ways are there to tile the last column? We can either tile elements in the last column one-at-a-time using a horizontal tile or two-at-a-time using a vertical tile. Therefore, the number of ways to tile the last column $$f(k) = f(k-2) + f(k-1)$$. Because the $$n^{th}$$ Fibonnaci number $$F(n) = \frac{\phi^{n}}{\sqrt{5}}$$, the number of ways to tile the last column grows exponentially with $$k$$. Because there are $$n$$ columns in the matrix, the complexity of the algorithm is $$O(\phi^{nk})$$.

Cover photograph by [aspireblog](http://aspireblog.org/wp-content/uploads/2013/04/chalkboard.jpg).
