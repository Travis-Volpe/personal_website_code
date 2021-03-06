---
title: the tangent bundle
date: 2016-02-31
---

#The tangent bundle

In [the last post](2016-02-07-tangentspaces.html), we defined the tangent space to a 
manifold at a point. That was the first step towards doing calculus on a manifold. In 
this post, we shall take the second step which is *bundling* the tangent spaces up 
into a single geometric object called the tangent bundle. We shall describe the 
tangent bundle in several different ways: As a vector bundle, as a locally free sheaf 
and as a sheaf of differential operators. Along the way we will see some more 
definitions from abstract sheaf theory.

###Vector bundles

Let $X$ be a manifold. Informally speaking, a vector bundle over $X$ is a family 
of vector spaces parameterized smoothly by the points in $X$. This colloquial language 
does not suggest a definition. Here is a potential definition (which is incorrect): A 
vector bundle over $X$ consists of a manifold $E$ and a smooth map $\pi : E \to X$ 
such that each fiber $\pi^{-1}(x) = \{ v \in E : \pi(v) = x\}$ is a vector space. The 
main problem with this definition is that we are not forcing the vector space 
structure on the fibers to vary smoothly as we move around in $X$. A second problem is 
that from this definition, there is no obvious way to describe a specific vector 
bundle structure. It should be easy to describe a vector bundle you are working with to your 
friends (assuming that you can describe the base manifold to them). Before we give the 
correct definition, we need some new terminology. Suppose 
that $\pi : E \to X$ is a smooth map. A vector bundle chart on $E$ consists of an open 
subset $U \subseteq X$ and a 
[diffeomorphism](https://en.wikipedia.org/wiki/Diffeomorphism)
$\pi^{-1}(U) = U \times \mathbb{R}^r$. Now we can give the correct definition: A 
vector bundle over $X$ consists of a manifold $E$ and a smooth map $\pi : E \to X$ 
such that 

- we can cover $E$ using vector bundle charts 
- given two vector bundle charts $\pi^{-1}(U) = U \times \mathbb{R}^r$ and 
$\pi^{-1}(V) = V \times \mathbb{R}^r$, if $U \cap V$ is nonempty, then the change of 
coordinates from $\pi^{-1}(U)$ to $\pi^{-1}(V)$ is given by
$$(x,v) \mapsto (x,\phi(x)v)$$
where $\phi : U \cap V \to {\rm GL}_r(\mathbb{R})$ is a smooth map (this just means 
that each matrix entry is a smooth function of $x \in U \cap V$)

If the fibers are dimension $r$, we call $E$ a vector bundle of rank $r$. Let $X$ be a 
manifold. As a set, the tangent bundle is defined by
$$TX = \coprod_{x \in X}T_x X$$
Write $\pi : TX \to X$ for the projection map which sends $T_xX$ to $x$. Assume that 
$x_1,\dots,x_d$ are coordinates on $U \subseteq X$. Then we have a bundle chart
$$ \pi^{-1}(U) \ni \left( x, \sum a_i \frac{\partial}{\partial x_i} \lvert_x \right) 
\mapsto 
(x,(a_i)) \in U \times \mathbb{R}^d$$ 
Suppose that $y_1,\dots,y_d$ are coordinates on $V \subseteq X$. Then 
$$\frac{\partial}{\partial x_j}\lvert_x = \sum_i \frac{\partial y_i}{\partial x_j}(x) 
\frac{\partial}{\partial y_i} \lvert_x$$
Therefore the change of coordinates from $U$ to $V$, $\phi : U \cap V \to {\rm 
GL}_d(\mathbb{R})$ is given by
$$\phi(x) = \left( \frac{\partial y_i}{\partial x_j}(x) \right)$$
Maybe you are worried that we did not prove that $TX$ was a manifold before we prove 
that it was a vector bundle. Everything is OK, because the vector bundle charts we 
have described on $TX$ are also plain old coordinate charts. We have defined a new 
type of mathematical object, so we need to define the morphisms. If $E,F$ are vector 
bundles over $X$ then a map of vector bundles $f : E \to F$ is a smooth map such that 

- $\pi_F \circ f = \pi_E$ (i.e $f$ maps fibers into fibers)
- $f$ is linear on the fibers.

###Gluing Data

Suppose that $\pi : E \to X$ is a vector bundle. Let $X = \cup_{\alpha} U_{\alpha}$ be 
an open cover such that each $\pi^{-1}(U_{\alpha})$ is a bundle chart on $E$. Write 
$\phi_{\beta\alpha} : U_{\alpha} \cap U_{\beta} \to {\rm GL}_r(\mathbb{R})$ for the 
change of coordinates from $\pi^{-1}(U_{\alpha})$ to $\pi^{-1}(U_{\beta})$. Then we 
have

- $\phi_{\alpha\alpha} = {\rm id}$
- $\phi_{\gamma\beta} \phi_{\beta\alpha} = \phi_{\gamma\alpha}$ on $U_{\alpha} \cap 
U_{\beta} \cap U_{\gamma}$. 

We call $(\phi_{\beta\alpha}, U_{\alpha})$ gluing data for the vector bundle $E$. From 
the gluing data we can reconstruct $E$: The gluing data tells us where the vector 
bundle charts are and how to change coordinates between them. We can also describe 
vector bundle maps in terms of gluing data. Suppose that we have two vector bundles $E$ 
and $F$ described by gluing data $(\phi_{\beta\alpha},U_{\alpha})$ and 
$(\psi_{\beta\alpha},U_{\alpha})$. Then a vector bundle morphism $f : E \to F$ 
consists of maps $f_{\alpha} : U_{\alpha} \to {\rm Mat}_{s \times r}(\mathbb{R})$ 
such that
$$\psi_{\beta\alpha} f_{\alpha} = f_{\beta} \phi_{\beta\alpha}$$
on $U_{\alpha} \cap U_{\beta}$. 

This definition is not without flaws. Suppose we write 
$U_{\alpha} = \cup_p V_{\alpha, p}$. Then we get new gluing data  
$(\phi_{\beta,q,\alpha,p}, V_{\alpha,p})$ defined by 

- $\phi_{\alpha,q,\alpha,p} = {\rm id}$
- $\phi_{\beta,q,\alpha,p} = \phi_{\beta,\alpha} \lvert_{V_{\beta,q} \cap 
V_{\alpha,p}}$. 

This gluing data describes the same vector bundle as 
$(\phi_{\beta\alpha}, U_{\alpha})$, despite being defined over a different open cover 
of $X$. Eventually, when we talk about sheaf cohomology, we will fix this problem, but 
for now we shall just work with the imperfect definition. In practice, this does not 
cause problems. If $U \subseteq X$ is a
[contractible](https://en.wikipedia.org/wiki/Contractible_space) coordinate chart, 
then $\pi^{-1}(U)$ is a 
vector bundle chart on $E$. This is because every vector bundle over $\mathbb{R}^d$ is 
trivial. Therefore, with suitable choices of coordinates on the base, we get gluing 
data for every vector bundle with the same open cover.

###An Example

We have seen that every manifold $X$ has a tangent bundle $TX$ and written down its 
gluing data. Let us work through another example to demonstrate how one works with 
vector bundles. Consider $\mathbb{RP}^1$, the set of 1-dimensional subspaces of 
$\mathbb{R}^2$. We write $[a:b]$ for the line spanned by $(a,b)$. If $X,Y$ are linear 
coordinates on $\mathbb{R}^2$, then $X/Y$ and $Y/X$ are smooth partial functions on 
$\mathbb{RP}^1$. In high school mathematics, the function $Y/X$ is often called the 
gradient. We shall write $m = Y/X$. We also write $n = X/Y$. Then $m = 1/n$ were both 
functions are defined. Infact, 
if we set
$$ {\rm D}(X) = \{ L \in \mathbb{RP}^1 : X(L) \not= 0 \}$$
$$ {\rm D}(Y) = \{ L \in \mathbb{RP}^1 : Y(L) \not= 0 \}$$
then $\mathbb{RP}^1 = {\rm D}(X) \cup {\rm D}(Y)$, $m = Y/X$ is a coordinate on ${\rm 
D}(X)$ and $n = X/Y$ is a coordinate on ${\rm D}(Y)$. Define
$$ L = \{ (p,L) \in \mathbb{R}^2 \times \mathbb{RP}^1 : p \in L \} $$
Then $L$ is a rank $1$ vector bundle (or a line bundle) over $\mathbb{RP}^1$. Write 
$\pi : L \to \mathbb{RP}^1$ for the obvious projection. We have bundle coordinates
$$ (\lambda,m) \mapsto ( (\lambda,\lambda m),[1:m]) \quad \text{over ${\rm D}(X)$}$$
$$ (\lambda,n) \mapsto ( (\lambda n,\lambda),[n:1]) \quad \text{over ${\rm D}(Y)$} $$
The change of coordinates map is
$$(\lambda,m) \mapsto ((\lambda,\lambda m),[1:m]) = ((\lambda m \frac{1}{m}, \lambda 
m), [1/m:1]) \mapsto (m \lambda, 1/m)$$
Therefore the change of coordinate map from $D(X)$ to $D(Y)$ is $Y/X : D(X) \cap D(Y) 
\to \mathbb{R}^{\times}$. We don\'t need to worry about the cocycle condition because 
there are only two coordinate charts. We call $L$ the *tautological line bundle* over 
$\mathbb{RP}^1$. Suppose that $f : \mathbb{R}^2 \to \mathbb{R}$ is a linear function. 
Then we have a bundle map $f : L \to \mathbb{R} \times \mathbb{RP}^1$ defined by 
$(p,L) \mapsto (f(p),L)$. If $f = aX + bY$, then we have
$$ f = \left(a + b \frac{Y}{X}\right) \text{ over $D(X)$}$$
$$ f = \left(a \frac{X}{Y} + b\right) \text{ over $D(Y)$}$$


### Locally free sheaves

So far we have been describing vector bundles as manifolds over some base $X$ where 
all the fibers are vector spaces. There is also a sheaf theoretic definition of vector 
bundles which we shall explore in this section.

Let $X = \mathbb{R}$ and consider the vector bundle $E = X \times \mathbb{R}$. We have 
a vector bundle map $f : E \to E$ defined by $(x,\lambda) \mapsto (x, x \lambda)$. We 
can think of $f$ as the matrix $(x)$. If we try and take the kernel of $f$, something 
bad happens: The map $f$ is injective when $x \not= 0$ and zero when $x=0$. Therefore 
the kernel should be like a vector bundle which is zero dimensional over $\mathbb{R} 
\backslash \{ 0 \}$ and one dimensional over $\{ 0 \}$. Obviously, no such vector 
bundle exists. The reason we need the sheaf theoretic definition of vector bundle is 
because it gives us a meaningful way to assign kernels and cokernels to all vector 
bundle maps, not just those with constant rank.

Let $X$ be a manifold. Recall that $\mathscr{C}$ is the sheaf of smooth functions on 
$X$. A $\mathscr{C}$-module is a sheaf of abelian groups $\mathscr{F}$ such that each 
$\mathscr{F}(U)$ is a $\mathscr{C}(U)$-module and the restriction maps are compatible 
with the $\mathscr{C}$-action: $(s v) \lvert_V = s \lvert_V v \lvert_V$. Let $\pi : E 
\to X$ be a vector bundle. We can build a $\mathscr{C}$-module $\mathscr{E}$ called 
the *sheaf of sections* as 
follows. If $U \subseteq X$ is open, then $\mathscr{E}(U)$ is the sections of $E$ over 
$U$:
$$\mathscr{E}(U) = \{ s : U \to E : \pi s = {\rm id}_U \}$$
If $s,t \in \mathscr{E}(U)$ then we define $(s+t)(x) = s(x) + t(x)$. This makes sense 
because both $s(x)$ and $t(x)$ are vectors in the fiber $\pi^{-1}(x)$. Therefore 
$\mathscr{E}$ is a presheaf of abelian groups. To check that $\mathscr{E}$ is a sheaf, 
you just need to unravel the definitions. If $f \in \mathscr{C}(U)$ and $s \in 
\mathscr{E}(U)$, then we define $(fs)(x) = f(x) s(x)$. Therefore $\mathscr{E}$ is a 
$\mathscr{C}$-module. The sheaf of sections $\mathscr{E}$ is much nicer than an 
arbitrary $\mathscr{C}$-module. Indeed, $\mathscr{E}$ is a locally free 
$\mathscr{C}$-module! This means that for each point $p \in X$, there is a 
neighborhood $p \in U$ such that $\mathscr{E}\lvert_U \cong \mathscr{C}_{U}^{\oplus 
r}$. To see this, choose a bundle chart $\pi^{-1}(U) = U \times \mathbb{R}^r$. Write 
$e_1,\dots,e_r$ for the standard basis of $\mathbb{R}^r$. Then
$$\mathscr{E}(U) = \{ f_1 e_1 + \dots + f_r e_r : f_i \in \mathscr{C}(U) \} \cong 
\mathscr{C}(U)^{\oplus r}.$$
To denote this, we shall write $\mathscr{E}\lvert_U = \mathscr{C} \lvert_U \{ 
e_1,\dots,e_r \}$. The map $E \mapsto \mathscr{E}$ sends vector bundles to locally 
free sheaves. It is an amazing fact that every locally free sheaf arises this way! 
Suppose that $\mathscr{F}$ is a locally free sheaf. Suppose that $\mathscr{F}\lvert_U 
= \mathscr{C} \lvert_U \{ s_1,\dots,s_r \}$ and $\mathscr{F} \lvert_V = \mathscr{C} 
\lvert_U \{ t_1,\dots,t_r \}$. Then the change of basis matrix from $s_1,\dots,s_r$ to 
$t_1,\dots,t_r$ is a smooth map $\phi : U \cap V \to {\rm GL}_r(\mathbb{R})$. 
Moreover, if you do this over three open sets, then the cocycle condition is 
satisfied, so we can construct a vector bundle $F$ such that $F \mapsto \mathscr{F}$. 
We can capture all of this in the following precise statement: The functor $E \mapsto 
\mathscr{E}$ from the category of vector bundles to the category of locally free 
sheaves is an [equivalence of 
categories](https://en.wikipedia.org/wiki/Equivalence_of_categories). Lets explore the 
map $f : E \to E$ from the start of the section using our new sheaf theoretic 
language. The sheaf of sections $\mathscr{E}$ is just $\mathscr{C}$, the sheaf of 
smooth functions on $X$. The induced map $\mathscr{C} \to \mathscr{C}$ is given by 
multiplication by $x$. If $f$ is a smooth map and $fx = 0$ then $f = 0$ by continuity. 
Therefore the kernel of $f$ is the zero sheaf! What the heck? Shouldn\'t the kernel 
somehow represent a \"vector bundle\" which is zero over $\mathbb{R} \backslash \{ 0 
\}$ and one dimensional over $0$? The answer is no. We are simply taking the 
[kernel](https://en.wikipedia.org/wiki/Kernel_%28category_theory) in the category of 
$\mathscr{C}$-modules. If it doesn\'t behave how we expect, it is not the categories 
fault, it is our fault for working inside the wrong category. On the bright side, the 
cokernel of $f$ will represent a \"vector bundle\" which is 1-dimensional over $0$ and 
zero over $\mathbb{R} \backslash \{ 0 \}$, but we need to talk about sheafification 
before we can talk about cokernels (we will do this in a later post).


###Sections of the tangent bundle

Let $X$ be a manifold. Write $T$ for the tangent bundle and $\mathscr{T}$ for the 
associated sheaf of sections. Elements in $\mathscr{T}(X)$ assign tangent vectors to 
each point in $X$, so they are vector fields on $X$. In [the last 
post](2016-02-07-tangentspaces.html), we proved that if $x_i$ are coordinates on $U 
\subseteq X$, then
$$\mathscr{T} \lvert_U = \mathscr{C} \lvert_U \left\{ \frac{\partial}{\partial x_1}, 
\dots, \frac{\partial}{\partial x_d} \right\} $$
where $\partial / \partial x_i$ represents the section which sends $x$ to 
$\partial / \partial x_i \lvert_x$. There is another way to describe the locally free 
sheaf $\mathscr{T}$ which is important. We can define a sheaf ${\rm 
Hom}_{\mathbb{R}}(\mathscr{C},\mathscr{C})$ as follows: The sections over $U$ are the
$\mathbb{R}$-linear sheaf homomorphisms $\mathscr{C} \lvert_U \to \mathscr{C} 
\lvert_U$. It is routine to check that the sheaf axioms are satisfied. Define
$$\mathscr{D} = \{ D \in {\rm Hom}_{\mathbb{R}}(\mathscr{C},\mathscr{C}) : D(fg) = f 
D(g) + g D(f) \}$$
This is the sheaf of first order differential operators. Pointwise multiplication 
turns $\mathscr{D}$ into a $\mathscr{C}$-module. There is a map $\mathscr{T} 
\to \mathscr{D}$ defined by $V \mapsto f \mapsto (x \mapsto V_x(f))$. Under this map, 
the vector field $\partial / \partial x_i$ is sent to partial differentiation with 
respect to $x_i$. We can define the inverse map $\mathscr{D} \to \mathscr{T}$ by $D 
\mapsto x \mapsto f \mapsto D(f)(x)$. It is routine to check that these maps establish 
an isomorphism of $\mathscr{C}$-modules.



###Exercises
1. write down gluing data for the tautological line bundle on $\mathbb{RP}^n$.
2. Prove that $E \mapsto \mathscr{E}$ is an equivalence of categories between vector 
bundles and locally free sheaves.
4. Check that ${\rm Hom}(\mathscr{F},\mathscr{G})$ is actually a sheaf.
3. **(Harder)** Prove that every vector bundle over $\mathbb{R}^d$ is trivial.
