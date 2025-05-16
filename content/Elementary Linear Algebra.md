Tags:
- #proj/math/linalg 

# 1. Introduction

## Metamath and Th(Γ) Setup

#### Review and Setup

Logic
- language
	- variables
	- logical signature
	- extralogical  (object theory) signature
	- alphabet
	- strings
	- formulas
	- well-formed formulas
	- statements
		- atomic
- deductive apparatus
	- ...
- structures
- interpretations
- models
- properties

| Property         | First-Order Logic          | Second-Order Logic          |
|------------------|----------------------------|------------------------------|
| **Definability** | Limited to what can be expressed with quantifiers over elements. | Much stronger—can quantify over sets/functions. |
| **Decidability** | Some theories (e.g. Presburger arithmetic) are decidable. | Generally undecidable. |
| **Completeness** | Gödel-complete for FOL theories. | Incomplete in general. |
| **Soundness**    |  |  |
| **Compactness**  | Holds.                     | Fails.                      |
| **Model Theory** | Rich and well-developed.   | Less tractable.             |

FOL = language + deductive system
Object theory: eg. PA or COF or RCF
Meta theory = ZFC
Interpretation & Models
- **RCF** is a first-order theory.
- It doesn't define sets or functions internally.
- It assumes a **signature** of function and relation symbols.
- It provides **axioms** that constrain how these symbols behave.
- **Models** of RCF assign set-theoretic meaning to these symbols in the meta-theory (ZFC, say), but **RCF itself need not refer to sets or tuples**.


What language (signature), logic, axioms and model(s) are we working with?
- Working with **SLE's** we need 
	- field addition
	- multiplication and 
	- order (for pivot stuff in Gaussian Elimination).
- So we need *at least* an **ordered field**
$$
(\mathbb{F},+,\times,0,1,<).
$$
- For a default infinite field model like $\mathbb{R}$ we may desire  the theory of **complete ordered fields**
$$\text{Th}(\Gamma_{COF})$$
- Unfortunately, we also want a first-order theory for the desirable properties of completeness and soundness. 
- theory of complete ordered fields needs second order logic for *completeness*
- We're in 
	- $\text{Th}(\mathbb{R})=RCF$
	- which is real closed fields

- first (and second?) order logic.

Our terms, symbols, operations come from 
- any field $\mathbb{F}$
	- finite or infinite
- a sub*ring* of $\mathbb{R}$ like $\mathbb{Z}$
- not necessarily complete or ordered 
	- consider $\mathbb{F}_{p}=\text{GF}(p)$ or $\mathbb{Q}$, but typically we work in $\mathbb{R}$.

See [[SLE solutions, RCF, and logic|this note]] for the detailed reference.

See the many other notes
#proj/math/analysis

# 2. Systems of Linear Equations

## Linear equations

The familiar "line" equation becomes a *homogenous linear equation in two variables* when $b=0$

```ad-definition

A linear equation, in $\mathbb{F}$ field
- $y=m\cdot x$ $\Leftrightarrow$ 
- $x_{1}=a_{0}\cdot x_{0}$ $\Leftrightarrow$ 
- $a_{0}\cdot x_{0}+-1\cdot x_{1}=0$ 

when $y=x_{1}$, $x=x_{0}$, and $m=a_{0}$,  for fixed $a_{0}\in \mathbb{F}$ and $x_{0},x_{1}\in \mathbb{F}$ free.



When $b\neq 0$ we have affine / *inhomogenous system*
- $y=m\cdot x + b \Leftrightarrow$
- $-a_{0}\cdot x_{0}+1\cdot x_{1}=b$ 

Depending on $b= 0$, we call the system
- homogenous, linear iff $b=0$
- inhomogenous, affine iff $b\neq 0$
```


## Linear combinations

```ad-definition
A linear combination of $n$ variables from field $\mathbb{F}$ is a *weighted sum*

$$
a_{0}\cdot x_{0}+a_{1}\cdot x_{1}+ \cdots+a_{n-1}\cdot x_{n-1}
$$

where the $a_i\in \mathbb{F}$.
```

## Systems of linear equations

If we consider taking $m$ linear combinations each of $n$ variables, and set each combination equal to some $b_{i}\in \mathbb{F}$, we have

```ad-definition
A System of $m$ Linear Equations (SLE) in $n$ unknowns

$$
\begin{align}
a_{11}\cdot x_{0}+a_{12}\cdot x_{1}+ \cdots+a_{1n}\cdot x_{n}&=b_{1} \\
a_{21}\cdot x_{0}+a_{22}\cdot x_{1}+ \cdots+a_{2n}\cdot x_{n}&=b_{2} \\
&\vdots \\
a_{m1}\cdot x_{0}+a_{m2}\cdot x_{1}+ \cdots+a_{mn}\cdot x_{n}&=b_{m} \\
\end{align}
$$
```


The *system* can be viewed as a set of $m$ formulas with (the same) $n$ free variables in each formula:

$$
\phi=
\left\{
\begin{aligned}
a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n = b_1, \\
a_{21}x_1 + a_{22}x_2 + \dots + a_{2n}x_n = b_2, \\
\vdots \\
a_{m1}x_1 + a_{m2}x_2 + \dots + a_{mn}x_n = b_m \\
\end{aligned}
\right\} = \bigcup_{i=1}^m \left( \sum_{j=1}^n a_{ij} x_j = b_i \right)
$$

Where we can simply write 

$$
\phi=\{ e_{1},\ldots,e_{m} \}
$$

when it's understood that each $e_{i}$ is one of the linear equations in the system.

Finally, we can represent the system as a single formula $\varphi$ that is the conjunction of all the 
$e_{i}$, as in:
$$
\varphi[x_{1}\ldots x_{n}]=\bigwedge_{i=1}^m e_{i}[x_{1}\ldots x_{n}] = \bigwedge_{i=1}^m \left( \left( \sum_{j=1}^n a_{ij} x_j \right)= b_i \right)
$$

Since the variable symbols are the same, these equations are *bound*. Any assignment of a value $x\in \mathbb{F}$ to a variable $x\to x_{i}$ affects all equations in which the $x_{i}$ term is not zero'd out.

So the *solutions* to the *system*, are those assignments $\{ x\to x_{i} \}$ which satisfy every relation (equality) in the "system".

This implies that no free variable is left unassigned, otherwise one or more relation would not yet have a truth value.

One way to write this in formal logic, is if we let $\Gamma$ be a complete assignment to variables, like

$$
\Gamma =\{ x_{0}=3, x_{1}=2, \ldots, x_{n-1}=8\}
$$

hen we ask if this assignment satisfies $\varphi[\ldots]$, eg.
$$
\begin{align}
&\Gamma\models \varphi[x_{1}\ldots x_{n}] \Leftrightarrow \\
&\Gamma\models\bigwedge e_{i}.
\end{align}
$$

Note, there may be no such $\Gamma$ that satisfies $\phi^*$, there may be one, there may be infinitely many. And furthermore, how do we find them via some effective procedure?

We address these questions in the following.

## Solutions

Here we describe the general shape of solutions as they depend on characteristics of the SLE.

Some terminology has not yet been introduced, nor has the methodology to deterministically arrive at solutions or conclude that none exist.

For now, know that **Gaussian Elimination** is the method and it transforms the **SLE** through various algebraic operations without changing the solution. This method exposes **pivot** positions, which are key in the solving process. The "rank" of a SLE is a characteristic in solution determination that we'll also get into after this overview.

### **1. Structure of the System**
- A system is typically written as:  
  $$
  A\vec{x} = \vec{b}
  $$  
  where:
  - $A \in \mathbb{F}^{m \times n}$: coefficient matrix
  - $\vec{x} \in \mathbb{F}^n$: vector of unknowns
  - $\vec{b} \in \mathbb{F}^m$: constants (right-hand side)

### **2. Key Factors That Influence the Solution Set**

#### **(a) Homogeneous vs. Inhomogeneous**
- If $\vec{b} = \vec{0}$: the system is **homogeneous**
  - Always has at least the **trivial solution** $\vec{x} = \vec{0}$
  - May have **infinitely many solutions** (if free variables exist)
- If $\vec{b} \ne \vec{0}$: the system is **inhomogeneous**
  - May have **no solution**, **one**, or **infinitely many**


#### **(b) Shape of the Matrix $A$**
- $m < n$: **underdetermined** system — more unknowns than equations
  - May have infinitely many solutions (if consistent)
- $m = n$: **square system**
  - Full-rank $\Rightarrow$ unique solution
  - Rank-deficient $\Rightarrow$ no or infinite solutions
- $m > n$: **overdetermined** — more equations than unknowns
  - Often inconsistent unless there's redundancy


#### **(c) Rank and Row Reduction**
- Use **Gaussian elimination** (or Gauss-Jordan) to reduce $A$ to:
  - **Row Echelon Form (REF)**: reveals pivot structure
  - **Reduced Row Echelon Form (RREF)**: shows dependencies explicitly

- Let:
  - $\operatorname{rank}(A) = r$
  - $\operatorname{rank}([A \,|\, \vec{b}]) = r'$

Then:

| Rank Conditions                      | Solution Type                             |
|-------------------------------------|-------------------------------------------|
| $r = r' = n$                    | Unique solution                           |
| $r = r' < n$                    | Infinite solutions (free variables exist) |
| $r < r'$                        | **Inconsistent** (no solution)            |

#### **(d) Location and Number of Pivots**
- **Pivot columns** correspond to **leading variables**
- **Non-pivot columns** correspond to **free variables**
- The number of **free variables = n – rank(A)**

#### **(e) Relationships Between Coefficients**
- **Linear dependence** or **linear combinations** among rows/columns of $A$ affect:
  - Whether certain equations are redundant
  - Whether the system can be consistent
- The **precise values** in $\vec{b}$ must lie in the **column space of $A$** for consistency.

#### **(f) Domain of the System**
- If over $\mathbb{R}$ or $\mathbb{Q}$: solutions can be continuous
- If over $\mathbb{Z}$: integral constraints (and limited row ops) can restrict solvability
- If over finite fields: solutions are combinatorially constrained

### **Conclusion**
To determine the **solution structure** of an SLE:

1. Identify if it’s homogeneous or not.
2. Reduce $A$ (and $[A\,|\,\vec{b}]$) to REF/RREF.
3. Count pivots and free variables.
4. Compare ranks of $A$ and augmented matrix.
5. Consider the domain and permissible operations.

These steps reveal whether the system is **inconsistent**, has a **unique solution**, or an **infinite family of solutions**.

## Domain of discourse (Ring vs. Field)

In a SLE, it's useful to note that the coefficients and unknowns are symbols in the formal language. 

```ad-caution
And, often without directly remarking such, the domain of discourse is assumed to be all of $\mathbb{R}$, yet linear algebra books and other resources often only show members of $\mathbb{Z}$!
```

Before anything. Fix the domain.

Solving a SLE over $\mathbb{R}$ versus over $\mathbb{Z}$ are very different matters.

In $\mathbb{R}$, we have linear combinations, in $\mathbb{Z}$ we have Diophantine equations.

Before moving on and fixing the domain to $\mathbb{R}$, we'll give a brief treatment of solving a SLE over the integers.

When solving systems of linear equations over the **integers** (rather than a field), and restricting yourself to **integer row operations only** (no division except by $\pm1$), you're entering the domain of **integer linear algebra** and **lattice theory**.

Let’s walk through this carefully and answer your question about the **algebraic conditions** that govern what reductions are possible, and what that says about **existence and nature of solutions**.

### **1. Setup and Restrictions**

You're given a system:

$$
A \vec{x} = \vec{b}, \quad A \in \mathbb{Z}^{m \times n},\; \vec{b} \in \mathbb{Z}^m,\; \vec{x} \in \mathbb{Z}^n
$$

You're allowed to perform:

- Row swaps.
- Integer multiples of one row added to another.
- Multiplying a row by $\pm1$ only.

**No division** unless by a unit in $\mathbb{Z}$, i.e., only $\pm1$.

This excludes Gaussian elimination in the usual sense — since we can’t divide by pivots unless they’re $\pm1$. Instead, we use special forms like **Hermite Normal Form (HNF)** and **Smith Normal Form (SNF)** that preserve integer structure.

### **2. Goal: Understand Structure & Solutions with Integer Row Ops**

#### Let’s take this in parts:

#### **A. Reduction to echelon form**

We can still reduce integer matrices to a triangular-like form using integer row operations, although not necessarily to REF or RREF as in the field case.

This leads to **Hermite Normal Form** (HNF): a form like echelon form where:

- Entries below pivots are zero.
- Pivots are **positive integers** (not necessarily 1).
- Entries above pivots can remain nonzero, but constrained.
  
Example HNF for a $3 \times 3$ matrix might look like:

$$
\begin{bmatrix}
2 & 3 & 1 \\
0 & 4 & 5 \\
0 & 0 & 6
\end{bmatrix}
$$

We can't make those pivots into 1s without dividing — which isn’t allowed over $\mathbb{Z}$.

#### **B. Solution existence: when does $A \vec{x} = \vec{b}$ have an integer solution?**

This is the crux.

**Over $\mathbb{Z}$,** for a single equation like:

$$
a_1 x_1 + a_2 x_2 + \cdots + a_n x_n = b
$$

We say that **a solution exists iff**:

$$
\gcd(a_1, \ldots, a_n) \mid b
$$

This generalizes:

> The system $A \vec{x} = \vec{b}$ has a solution over $\mathbb{Z}$ **iff** every integral linear relation among the rows of $A$ holds with $\vec{b}$ as well.

Another way to say it:

- $\vec{b}$ must lie in the **integer span (i.e., lattice)** generated by the columns of $A$.

#### **C. Type and Number of Solutions**

Let’s classify:

##### 1. **No solution (inconsistent)**:
- When $\vec{b} \notin \text{lattice span of columns of } A$
- Often visible as: a row like $[0\ 0\ 0\ |\ c]$ with $c \neq 0$ appears in the augmented matrix during reduction.

##### 2. **Unique solution**:
- When the reduced matrix is square, full rank, and invertible over $\mathbb{Z}$, i.e., determinant $\pm1$.
- Then the inverse is also over $\mathbb{Z}$, and solution is unique and integer.

##### 3. **Infinitely many solutions**:
- When the nullspace has dimension > 0, i.e., some free variables remain.
- The general solution is an **affine lattice**: a particular solution plus integer combinations of a set of basis vectors in the nullspace.

#### **D. Algebraic Characteristics That Allow Reduction**

So what must be true **algebraically** about $A$ to reduce to some echelon-like form over $\mathbb{Z}$?

- The greatest common divisors (GCDs) of entries must allow row combinations to eliminate entries — **without needing division**.
- For example, if you want to eliminate 6 using 4, you can do:
  $$
  R_2 \leftarrow R_2 - 1 \cdot R_1 \quad \text{if } R_1 = [4\ \cdots], R_2 = [6\ \cdots]
  $$
  But you can’t do $\frac{1}{4} R_1$, because $\frac{1}{4} \notin \mathbb{Z}$.

Thus:

- The Euclidean algorithm is your main tool for reducing pairs of integers to their GCDs.
- **Bézout's identity** plays a role: if $d = \gcd(a, b)$, then there exist integers $x, y$ such that $ax + by = d$. That’s the essence of elimination in $\mathbb{Z}$.

### **3. Summary of Key Characteristics**

To reduce systems over the integers to echelon-like form and understand solutions, these are the key conditions:

| Algebraic Property | Implication |
|--------------------|-------------|
| $\gcd(a_1, \dots, a_n) \mid b$ | Single equation has integer solution |
| Integer row operations preserve lattice span | Reductions don’t change solvability |
| Integer matrix $A$ invertible iff $\det A = \pm1$ | Then solution is unique, integer |
| Pivoting without division | Only possible when pivot divides entry below |
| Free variables imply affine lattice of solutions | Structure: particular solution + nullspace |



## Matrices

When representing and manipulating SLE's, there's a convenient notational, algebraic and diagrammatic form  called a matrix. Arthur Cayley 1850's. It lends naturally to the build up of elementary linear algebra and we find that vectors are special cases of matrices, and matrices represent linear transformations and other foundational connections.

For our purposes, a matrix is a table of numbers, with rows and columns. The matrix has two primary aspects. The one just mentioned is its container aspect. The other, which we'll study in great detail later, is its algebraic aspect.

For the purpose of SLE solving via Gaussian elimination, we'll introduce the container aspect of the matrix as well as some of its basic algebra. Later we'll circle back and explore the deeper connection to vector spaces and linear transformations.

First, we must describe the matrix in a rigorous, unambiguous way. There are three ways to do this, each of which can provide a complete foundation for the development of elementary linear algebra. These are
1. **Tensorial**: tensor product of vector spaces
$$
\mathbb{R}^{m \times n} \cong \mathbb{R}^m \otimes \mathbb{R}^n
$$
2. **Functional**: from indices to Reals
$$
(i,j) \mapsto a_{ij} \in \mathbb{R}
$$
3. **Set-theoretic**: tuple-of-tuples from cartesian product
$$
\begin{align}
v\in\mathbb{R}^n=\mathbb{R}\times\dots\times \mathbb{R}&=\{ (x_{1}\dots x_{n}) \;\vert\; x_{i}\in \mathbb{R} \}, \text{and}\\
A\in\mathbb{R}^{m\times n} &= (\mathbb{R}^m)^n
\end{align}
$$
for $v$ vector and $A$ matrix.

Whichever foundation to build the matrix concept upon, the diagrammatic notation is universal:

$$
A=\left[ \begin{array}{ccc}
a_{11} & \cdots & a_{1n} \\
\vdots & \ddots & \vdots \\
a_{m1} & \cdots & a_{mn}
\end{array} \right]
$$
where $A$ denotes the matrix of $m$ rows and $n$ columns. And  $A[i,j]=A_{i,j}=A(i,j)$ is the item at the $i^{th}$ row and $j^{th}$ column.

## Matrices and SLEs

So given a SLE of $m$ equations in $n$ unkowns

$$
S=\begin{align}
a_{11}\cdot x_{0}+a_{12}\cdot x_{1}+ \cdots+a_{1n}\cdot x_{n}&=b_{1} \\
a_{21}\cdot x_{0}+a_{22}\cdot x_{1}+ \cdots+a_{2n}\cdot x_{n}&=b_{2} \\
&\vdots \\
a_{m1}\cdot x_{0}+a_{m2}\cdot x_{1}+ \cdots+a_{mn}\cdot x_{n}&=b_{m} \\
\end{align}
$$

we can use matrix notation to organize the representation, and as we'll see, the matrix *algebra* to manipulate the system in accordance with the rules of field arithmetic of which we're already familiar.

To begin, to represent the coefficients from the system $S$ above in a matrix we simply have:

$$
A=\left[
\begin{array}{cccc}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{array}
\right]
$$

Including the affine terms we can *augment* the matrix of coefficients like

$$
S\cong A_{b}=\left[
\begin{array}{cccc|c}
a_{11} & a_{12} & \cdots & a_{1n} & b_1 \\
a_{21} & a_{22} & \cdots & a_{2n} & b_2 \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn} & b_m \\
\end{array}
\right]
$$

We now have a notation that makes our work of solving SLEs compact and neat. Which will later develop into a deep connection between systems of linear equations and vector spaces at the heart of linear algebra.

## Solving SLEs

Recall from above, that a SLE is really a set of statements (equations) in our formal language. And we can represent this set as a single statement using conjunction.

That is, for system $S$

$$
\begin{align}
a_{11}\cdot x_{0}+a_{12}\cdot x_{1}+ \cdots+a_{1n}\cdot x_{n}&=b_{1} \\
a_{21}\cdot x_{0}+a_{22}\cdot x_{1}+ \cdots+a_{2n}\cdot x_{n}&=b_{2} \\
&\vdots \\
a_{m1}\cdot x_{0}+a_{m2}\cdot x_{1}+ \cdots+a_{mn}\cdot x_{n}&=b_{m} \\
\end{align}
$$

we have the set of formulas $\phi$

$$
\phi=
\left\{
\begin{aligned}
a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n = b_1, \\
a_{21}x_1 + a_{22}x_2 + \dots + a_{2n}x_n = b_2, \\
\vdots \\
a_{m1}x_1 + a_{m2}x_2 + \dots + a_{mn}x_n = b_m \\
\end{aligned}
\right\} = \bigcup_{i=1}^m \left( \sum_{j=1}^n a_{ij} x_j = b_i \right)
$$

or simply

$$
\phi=\{ e_{1},\ldots,e_{m} \}
$$

which can be treated a single conjunction, $\varphi$,  without changing the nature of the solution to $S$

$$
\varphi[x_{1}\ldots x_{n}]=\bigwedge e_{i}[x_{1}\ldots x_{n}] = \bigwedge_{i=1}^m \left( \sum_{j=1}^n a_{ij} x_j = b_i \right)
$$

which has a solution when there exists some assignment $x_{i} = c_{i}$  for all the $x_{i}$ to a number $c_{i}$, call this set of assignments $\Gamma$ that satisfies $\varphi$

$$
\Gamma\models \varphi[x_{1}\ldots x_{n}].
$$

Each equation in the system $S$ is a **linear combination** set equal to some real number $b_{i}$. This involves a set of real variables $x_{1}\ldots x_{n}\in \mathbb{R}$ each multiplied (scaled) by real coefficients $a_{1}\dots a_{n}\in \mathbb{R}$ and summed. Finally, each **linear combination** is set equal to a real number $b_{i}$.

We're already familiar with methods to solve a single linear equation in one free variable. Familiarity with ring operations and field operations applies in this case. But now instead of a single equation, we're a dealing with a set of them, where they are related by containing the same unknowns.

That is, solving

$$
a_1 x_1 + a_2 x_2 + \dots + a_n x_n = b
$$

means finding an $n$-tuple $(x_1, \dots, x_n) \in \mathbb{R}^n$ that satisfies it. Or when there are many solutions, This defines a geometric object like a line, plane, or hyperplane in $\mathbb{R}^n$, depending on $n$.

When we **combine multiple such equations** and require them to be satisfied **simultaneously** by the same variables $x_1, \dots, x_n$, we form a **system of linear equations (SLE)**:

Now, we look for a **single tuple** $\vec{x} = (x_1, \dots, x_n) \in \mathbb{R}^n$ that satisfies **all** the equations at once.

The key insight is that all equations are functions from $\mathbb{R}^n \to \mathbb{R}$, and solving the system means finding $\vec{x} \in \mathbb{R}^n$ in the intersection of their kernels (after subtracting the right-hand sides).


### **Step 1: Single Equation**

A single linear equation has the form:

$$
\exists x_1, \dots, x_n \in \mathbb{R} \quad \text{s.t.} \quad \sum_{j=1}^n a_j x_j = b
$$

### **Step 2: System of Equations**

Now consider $m$ equations in the **same variables** $x_1, \dots, x_n$. The system is:

$$
\exists x_1 \dots x_n \quad \bigwedge_{i=1}^m \left( \sum_{j=1}^n a_{ij} x_j = b_i \right)
$$

This says: *there exists a tuple $(x_1, \dots, x_n) \in \mathbb{R}^n$ that satisfies all $m$ linear constraints simultaneously.*

Each linear equation defines an affine hyperplane, and the solution set is their intersection — possibly empty, a point, or a subspace.

We can't simply just try every candidate solution, that would be impossible in $\mathbb{R}$!

So what we do in practice is transform the system $S$ in such a way, that each transformation does not change the equality (solutions) of a single equation $e_{i}$, and that any transformation does not change the solution to the entire system $S$.

We desire a procedural method using such transformations that is both correct and guaranteed to finish. Thankfully, the mathematician Gauss figured this out in what is called **Gaussian Elimination**.

## Gaussian Elimination

### **1. What Is a System of Linear Equations (SLE)?**

A system of $m$ linear equations in $n$ variables looks like this:

$$
\begin{cases}
a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n = b_1 \\
a_{21}x_1 + a_{22}x_2 + \dots + a_{2n}x_n = b_2 \\
\vdots \\
a_{m1}x_1 + a_{m2}x_2 + \dots + a_{mn}x_n = b_m \\
\end{cases}
$$

We can think of each equation as a constraint on the vector $x = (x_1, x_2, \dots, x_n) \in \mathbb{R}^n$, and the solution set is the set of all vectors satisfying all the equations simultaneously.


### **2. Elementary Row Operations (on the Equations)**

Each elementary row operation transforms the system into another one with the **same solution set**.

#### **(i) Swapping two equations**

- Operation: Exchange two rows/equations: $E_i \leftrightarrow E_j$
- **Justification**: The order of equations doesn’t affect the solution. It's just a reordering of constraints.

#### **(ii) Multiplying an equation by a nonzero scalar**

- Operation: $E_i \mapsto \lambda \cdot E_i$, for $\lambda \neq 0$
- **Justification**: If $a_1x_1 + \dots + a_nx_n = b$, then multiplying both sides by $\lambda$ gives an equivalent equation. Any solution to one is a solution to the other.

#### **(iii) Adding a multiple of one equation to another**

- Operation: $E_j \mapsto E_j + \lambda E_i$
- **Justification**: If a vector satisfies both equations before the operation, it satisfies them after. Adding one equation to another is like combining constraints — solutions to the original system will satisfy the new one, and vice versa.

#### **(iii) MCG: Adding equations proof**

If we have these two equations in some system $S$
$$
\begin{align}
a_{11}x_{1} + a_{12}x_{2}=b_{1} \\
a_{21}x_{1} + a_{22}x_{2}=b_{2}
\end{align}
$$

and want their sum. We have

$$
\begin{align}
f_{1}(x_{1},x_{2})&=b_{1} + \\
f_{2}(x_{1},x_{2})&=b_{2}
\end{align}
$$

But how do we "add" equations? They're statements not terms in an algebraic ring or field!

Well, $f_{i}$ is just a **linear combination** which is simply a term in $\mathbb{R}$. So these terms can be added under field addition. But we have free variables. But in a SLE these free variables are *shared* in each equation and therefore we can add them. 

Certainly we can add two formulas that evaluate to a term in $\mathbb{R}$ when the free variables are bound.

Thus we can add the two left-hand sides

$$
f_{1}(x_{1},x_{2}) + f_{2}(x_{1},x_{2})
$$

But we're interested in solutions to the equations in $S$, so what effect does adding one linear combination to the other's equation have?

Well, we have to add it to both sides to keep the equality.
$$
\begin{align}
f_{1}(x_{1},x_{2}) &= b_{1} \;\wedge \\
f_{2}(x_{1},x_{2}) &= b_{2} \;\wedge \\
f_{1}(x_{1},x_{2}) + c&= b_{1}+c \Rightarrow \\
f_{1}(x_{1},x_{2}) + b_{2}&= b_{1}+b_{2} \Rightarrow \\
f_{1}(x_{1},x_{2}) + f_{2}(x_{1},x_{2})&= b_{1}+b_{2} \;\square. \\
\end{align}
$$

We can now apply this general result
to our particular situation of adding two equations in a SLE.

By substituting in the linear combinations for the general function symbols and simplifying we get: 

$$
\begin{align}
f_{1}(x_{1},x_{2}) + f_{2}(x_{1},x_{2})&= b_{1}+b_{2} \Rightarrow \\
a_{11}x_{1} + a_{12}x_{2} + a_{21}x_{1} + a_{22}x_{2}&=b_{1} + b_{2} \Rightarrow \\ 
(a_{11}x_{1} + a_{21}x_{1}) + (a_{12}x_{2} + a_{22}x_{2})&=b_{1} + b_{2} \Rightarrow \\ 
(a_{11} + a_{21})x_{1} + (a_{12} + a_{22})x_{2}&=b_{1} + b_{2} \;\square. \\ 
\end{align}
$$

A similar proof applies when we consider add a scalar multiple one equation in a SLE to another.

$$
\begin{align}
f_{1}(x_{1},x_{2}) &= b_{1} \;\wedge\;\lambda\in \mathbb{R} \Rightarrow \\
\lambda \cdot f_{1}(x_{1},x_{2}) &= \lambda \cdot b_{1}
\end{align}
$$

Since in the field $(\mathbb{R},+,\cdot,0,1)$ the multiplication operation $\cdot$ is well-defined.

Since the scalar multiple doesn't change the solution to the equation, it doesn't change the solution to the SLE.

So we can gather the coefficient on the left- and right-hand side into a new function and constant.

$$
\begin{align}
\text{Given }\lambda \cdot f_{1}(x_{1},x_{2}) &= \lambda \cdot b_{1}  \\
\text{Let }\lambda \cdot f_{1}(x_{1},x_{2})&=  f_{1}'(x_{1},x_{2})\;\wedge \\
\lambda \cdot b_{1} &= b_{1}' \text{ then}\\
f_{1}'(x_{1},x_{2})&= b_{1}'.
\end{align}
$$

And apply the same proof as above, but with $f_{1}'$ and $b_{1}'$ in the place of the original symbols.

### **3. Why Use These Operations?**

By using these operations systematically (called **Gaussian elimination**), we can reduce the system to:

#### **Row Echelon Form (REF)**

- Upper-triangular shape: Each successive equation has one more leading zero.
- Helps identify **pivots**: the first non-zero coefficient in each row.
- From this, we can:
  - Determine if the system is consistent (has at least one solution).
  - Identify **free variables** and parametrize the solution space.

#### **Reduced Row Echelon Form (RREF)**

- Goes further: each pivot is 1, and all entries above and below the pivot are 0.
- Each variable is either:
  - A **leading variable** (solved directly),
  - Or a **free variable** (can take any value).

### **4. Why This Helps Solve the System**

Once in REF or RREF:

- You can use **back substitution** to solve for the leading variables.
- If there are free variables, you describe the **entire solution set parametrically**.
- If you encounter a contradictory equation (like $0 = 1$), the system is **inconsistent** (no solution).

### **Conclusion**

**Elementary row operations** preserve the **solution set** because they're just restatements or linear combinations of the same original constraints. Reducing to echelon form **reveals the structure** of the system and makes it easy to find all solutions — or see that none exist.

### **Setting**
We’re solving a system of linear equations:

$$
A \vec{x} = \vec{b}, \quad A \in \mathbb{R}^{m \times n},\ \vec{b} \in \mathbb{R}^m,\ \vec{x} \in \mathbb{R}^n
$$

Using **Gaussian elimination** (or Gauss-Jordan), we reduce the **augmented matrix**:

$$
[A \mid \vec{b}]
$$

to **row echelon form** (REF) or **reduced row echelon form** (RREF), to study the **solution space**.

#### **1. Types of Solutions**

There are three possible cases:

1. **No solution (inconsistent):**
   - The system is overdetermined or contradictory.
   - Detected by a row like:
     $$
     [0 \quad 0 \quad \cdots \quad 0 \mid c] \quad \text{with } c \neq 0
     $$

2. **Unique solution:**
   - The matrix $A$ has full rank: $\text{rank}(A) = n$ (number of variables)
   - Then the system is consistent and has a single solution.
   - Geometrically: the intersection of $n$ hyperplanes in $\mathbb{R}^n$ is a single point.

3. **Infinitely many solutions:**
   - The system is underdetermined: $\text{rank}(A) < n$
   - Some variables are **free**: they can be assigned arbitrary real values.
   - Geometrically: solution is a flat (line, plane, etc.) in $\mathbb{R}^n$.

#### **2. Relationships Between Coefficients That Determine These Outcomes**

##### **A. Consistency**

A system is **consistent** (has at least one solution) **iff**:

$$
\text{rank}(A) = \text{rank}([A \mid \vec{b}])
$$

That is, augmenting $A$ with $\vec{b}$ doesn’t introduce a contradictory row. The **coefficients of $\vec{b}$** must be a linear combination of the rows of $A$. So:

> **Consistency condition**: $\vec{b} \in \operatorname{Col}(A)$, the column space (or range) of $A$.

##### **B. Uniqueness**

A consistent system has a **unique solution** **iff**:

$$
\text{rank}(A) = n \quad \text{(number of variables)}
$$

This means:
- The columns of $A$ are **linearly independent**.
- There are **no free variables**.

In terms of coefficient structure, this means:
- There is **no linear dependence** between the columns.
- The pivot positions in RREF occur in **every column**, so each variable is leading.

##### **C. Infinite Solutions**

This happens when:

$$
\text{rank}(A) < n \quad \text{(fewer pivots than variables)}
$$

So:
- Some columns of $A$ are linear combinations of others.
- These become **free variables** in the reduction process.
- The solution space is a flat of dimension $n - \text{rank}(A)$

So:
> **Dependency among columns** leads to free variables, which means an **infinite solution set**, if consistent.

#### **3. Row Operations and What They Reveal**

When you perform row operations:

- **Row swaps / scaling / elimination** simplify the system, but **do not change the solution set** (when allowed over $\mathbb{R}$).
- The **pivot positions** and **number of non-zero rows** tell you:
  - The **rank**
  - The number of **leading variables** vs **free variables**
  - The **structure of the solution set**

#### **4. Summary: What Relationships Must Exist**

| Relationship Among Coefficients         | Implication                                  |
|----------------------------------------|----------------------------------------------|
| $\vec{b} \in \operatorname{Col}(A)$ | System is consistent                         |
| Columns of $A$ are independent     | Unique solution                              |
| Columns of $A$ are dependent       | Infinite solutions (if consistent)           |
| Row $[0\ 0\ \cdots\ 0\ \vert c]$ appears | No solution (inconsistent), $c \neq 0$    |
| Rank of $A$ equals number of variables $n$ | All variables determined (no free vars)     |
| Rank of $A$ < $n$               | Some variables free → solution is a space    |

Would you like to walk through a real example of a system and watch how these relationships unfold?

```ad-detail
```
Why $A\vec{x}=\vec{b}$ consistent iff $\vec{b}\in Col(A)$?
- See below, $(1)\Rightarrow(2)\Rightarrow(3)$
- $\vec{b}$ is a linear combination of the cols of $A$

$$
\begin{align}
&\exists x_{1}\ldots x_{n} \text{ s.t. } \\
&\vec{b}=A \vec{x} & \Leftrightarrow \tag{1}\\
&\vec{b}=x_{1}A[:,1] + x_{2}A[:,2] +\ldots + x_{n}A[:,n] & \Leftrightarrow \tag{2}\\
&\vec{b} = x_1 
\begin{pmatrix} a_{11} \\ a_{21} \\ \vdots \\ a_{m1} \end{pmatrix}
+ x_2 
\begin{pmatrix} a_{12} \\ a_{22} \\ \vdots \\ a_{m2} \end{pmatrix}
+ \cdots
+ x_n
\begin{pmatrix} a_{1n} \\ a_{2n} \\ \vdots \\ a_{mn} \end{pmatrix} & \Leftrightarrow \tag{3}\\
&b_{1} = x_{1}\cdot a_{11}+x_{2}\cdot a_{12}+ \cdots+x_{n}\cdot a_{1n}\\
&b_{2} =x_{1}\cdot a_{21}+x_{2}\cdot a_{22}+ \cdots+x_{n}\cdot a_{2n} \\
&\vdots &\Leftrightarrow \tag{4}\\
&b_{m}= x_{1}\cdot a_{m1}+x_{2}\cdot a_{m2}+ \cdots+x_{n}\cdot a_{mn} \\
&(\left[
\begin{array}{c|cccc}
b_{1} & a_{11} & a_{12} & \cdots & a_{1n} \\
b_{2} & a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots & \vdots \\
b_{m} & a_{m1} & a_{m2} & \cdots & a_{mn} \\
\end{array}
\right] )rank = rank(A) \tag{5}
\end{align}
$$

$(5)$ is known as the **Kronecker–Capelli** theorem.

How does this relate to GE and the rowspace and solution?
- See $\S2,\S3,\S4$ above
- GE gets us to RREF
- In RREF
	- if $\exists i  \;\;\text{s.t.} \quad\;[A\;\vert\;\vec{b}][i,:] = 0\dots0\vert c$ for some $c\neq 0$
	- then $[A\;\vert\;\vec{b}]$ **inconsistent**

### rank(A)= dim(Col(A))=dim(Row(A))

- The rank of a matrix $A \in \mathbb{R}^{m \times n}$ is the dimension of the span of its rows (or columns — same value).
- Also the "number of variables"
	- which type of variable?
	- leading? free?

### Nomenclature

Excellent — you're picking apart the precise terminology now. Let's break these concepts down clearly.

#### **1. Pivot vs. Leading Variable**

These are closely related but used in slightly different contexts:

- **Pivot**:
  - Refers to the **position** (the entry) in the matrix.
  - Specifically, it’s the **first nonzero entry** in a row of an echelon-form matrix.
  - That position corresponds to a **pivot column** and **pivot variable**.

- **Leading variable**:
  - Refers to the **variable** (say, $x_2$) that appears first (i.e., has a pivot in its column) in some row of the system.
  - It’s the variable that is **solved in terms of** the others when reducing the system.
  
> So: **A pivot is a location in the matrix. A leading variable is the corresponding unknown.**

#### **2. Echelon Form vs. Reduced Echelon Form**

Here’s the difference between **row echelon form (REF)** and **reduced row echelon form (RREF)**:

##### **Row Echelon Form (REF):**
A matrix is in REF if:
- All nonzero rows are above any all-zero rows.
- The **leading entry** (pivot) of each nonzero row is to the **right** of the leading entry in the row above it.
- **Entries below a pivot are all zero**.

REF looks "stair-step"-shaped.

Example:
$$
\begin{bmatrix}
1 & 2 & 3 \\
0 & 1 & 4 \\
0 & 0 & 0 \\
\end{bmatrix}
$$

##### **Reduced Row Echelon Form (RREF):**
A matrix is in RREF if:
- It is in REF.
- **Each pivot is 1**.
- **Each pivot is the only nonzero entry in its column.**

This gives a fully "cleaned-up" form where each pivot variable is explicitly solved for.

Example:
$$
\begin{bmatrix}
1 & 0 & a \\
0 & 1 & b \\
0 & 0 & 0 \\
\end{bmatrix}
$$

> Think of **REF** as halfway cleaned, and **RREF** as fully cleaned and ready for reading off solutions.

#### **Quick Summary Table:**

| Term               | Meaning                                                                 |
|--------------------|-------------------------------------------------------------------------|
| **Pivot**          | The first nonzero entry in a row (position in matrix)                  |
| **Pivot column**   | Column that contains a pivot                                            |
| **Leading variable** | Variable corresponding to a pivot column                             |
| **Free variable**  | Variable that doesn’t have a pivot in its column                       |
| **Row Echelon Form (REF)** | Staircase form, zeros below pivots                           |
| **Reduced Row Echelon Form (RREF)** | Fully simplified, pivots are 1s, only entry in their column |

## Gaussian Elimination Algorithm

Simply proceed 
- left-to-right
- top-to-bottom

eliminating entries below the diagonal
- using the three elementary row operations. 

This yields a REF matrix.

Optionally, we can
- reduce the REF matrix to RREF

by eliminating entries *above* the pivots using elementary row operations.

Finally, we
- check the (R)REF matrix to determine if the SLE is consistent

and if so
- back-substitute to get a single solution or 
- distinguish free variables and leading variables to parametrize an infinite solution.

The following diagram shows a matrix being transformed to RREF by Gaussian Elimination.
- Leading terms are colored green.
- Pivots are colored red.
- Matrix 3 is in REF
	- $\text{\# pivots}=\text{rank}(A)=\text{dim}(\text{row space})=\text{dim}(\text{col space})$
		- ^ prove
	- 2 pivots $\Rightarrow \text{rank}(A)=2$
	- $x_{3}$ and $x_{4}$ are **free variables**
- Matrix 4 is in RREF

$$
\begin{gather*}
A_{1}=\begin{bmatrix} \\
\color{green}{1} & 3 & 1 & 9 \\
\color{green}{1} & 1 & -1 & 1 \\
\color{green}{3} & 11 & 5 & 35
\end{bmatrix} \\
\Big\downarrow \\
(1,1)\text{ pivot} \\
\text{clear }(1:,1) \\
\Big\downarrow \\
A_{2}=\begin{bmatrix}
\boxed{\color{red}{1}} & 3 & 1 & 9 \\
0 & \color{green}{-2} & -2 & -8 \\
0 & \color{green}{2} & 2 & 8
\end{bmatrix} \\
\Big\downarrow \\
(2,2)\text{ pivot} \\
\text{clear }(2:,2) \Rightarrow \\
A_{3}\text{ REF} \\
\Big\downarrow \\
A_{3}=\begin{bmatrix}
\color{red}{1} & 3 & 1 & 9 \\
0 & \boxed{\color{red}{-2}} & -2 & -8 \\
0 & 0 & 0 & 0
\end{bmatrix}  \\
\Big\downarrow \\
\text{clear }(:2,2) \Rightarrow \\
A_{4}\text{ RREF} \\
\Big\downarrow \\
A_{4}=\begin{bmatrix}
\color{red}{1} & 0 & -2 & -3 \\
0 & \boxed{\color{red}{1}} & 1 & 4 \\
0 & 0 & 0 & 0
\end{bmatrix}
\end{gather*}
$$

Wiki: https://en.wikipedia.org/wiki/Gaussian_elimination
Video: https://youtu.be/thkEwfhjAMY?si=E43Px4zYIZvQRWEk


```python
import numpy as np

def gaussian_elimination(A):
	# Ensure floating-point division
    A = A.astype(float)  
    m, n = A.shape
    pivot_row = 0

    for col in range(n):
        # Find pivot in the current column
        for row in range(pivot_row, m):
            if A[row, col] != 0:
                break
        else:
			# No pivot found, proc next col
            continue

		# (row, col) are set to pivot

        # Swap rows
        if row != pivot_row:
            A[[pivot_row, row]] = A[[row, pivot_row]]

        # Eliminate entries below pivot
        for r in range(pivot_row + 1, m):
            factor = A[r, col] / A[pivot_row, col]
            A[r, col:] -= factor * A[pivot_row, col:]

        pivot_row += 1

    return A
```

## Wrap-up

```ad-remark
```

How does the system of linear equations (row picture) and the elementary row manipulations into (R)REF relate to the geometric column picture: vector spaces?

1. We can think of a matrix $A$ as storing the coefficients of a SLE. Also, we can think of it as a sequence of row vectors and a sequence of column vectors.
2. Any sequence of vectors of the same dimension, true in the case of matrices, generates a vector space, *spans* a vector space.
3. Since the span of a sequence of vectors is the set of all linear combinations, even a single vector spans a line (vector space), and even the zero vector spans the trivial vector space {0}.
4. So every matrix $A$, when considering the rows as a sequence of vectors: spans a vector space. And same goes for the columns.
5. Now. What are we doing in Gaussian elimination?
6. Well, we're solving the system of linear equations represented by the matrix $A$.
7. The elementary row operations manipulate the representation of the SLE making it easier for a human (or machine) to read off the solution, while still respecting the constraints of the system viz. without altering the solution set.
8. Throughout this manipulation of equations, there is something very exciting happening in the vector space picture as well, and you don't want to miss it!
9. To begin, consider the span of the row vectors of $A$. That is, the set of all linear combinations of those vectors. 
10. The elementary row operations that are manipulating the SLE towards (R)REF, are as well performing a computation in vector land. 👈 Redundant?
11. In fact, just as how our reduction to (R)REF only changes the *representation* of the SLE, leaving the underlying solutions in tact (just more amenable to description), so too we are changing the representation of the row vectors and computing basic properties about the vector space the rows encode (span), such as its dimension and basis. 
12. In thinking about the elementary row operations and the sequence of them that gets $A$ into (R)REF, we know the SLE perspective, but in order to follow what's happening in the row vector space, we should consider the definition of linear combinations of vectors. Once we are familiar with this definition and algebra, we are in a good place to understand span, basis, dimension, and linear dependence.
13. Just like a linear combination of scalars, a linear combination is just a weighted sum of vectors. Where the multiplication is scalar multiplication (which boils down to field multiplication) and the addition is vector addition (which boils down to field addition).
14. Once we understand linear combinations, we can understand span, basis, dimension, and linear dependence. The next place to go is span.
15. The span of a sequence of vectors is simply the set of all linear combinations of those vectors. That's it. Now, there are some results that directly follow this definition. We present these in detail later. 
16. One such result, useful here, is that the span of a sequence of vectors is always a vector space. In fact, a subspace of the vector space from which the spanning set is drawn.
17. We're intuitively familiar with the geometric concept of dimension, for instance of the plane $\mathbb{R}^2$ or the real space $\mathbb{R}^3$. But with the notion of span we can make this precise.
18. So we have a matrix $A$ which has a sequence of row vectors that span a vector space. But are all the row vectors *needed* in a sense to made precise shortly to span this space?
19. The question is: what is the minimal amount of vectors needed to span a vector space? And this amounts to determining if a given spanning set of vectors has "redundant" vectors in a specific sense, in the sense that these vectors are linear combinations of others in the spanning set.
20. When we carry out a process to take a spanning set and remover all the linearly dependent vectors, we are left with a minimal spanning set of vectors that are linearly independent. 
21. These vectors are called a basis of the vector space they span.
22. The number of vectors in this basis is the dimension of the vector space.
23. Claim. When we perform the elementary row operations we are either changing the order of vectors is the sequence of row vectors -- which does not change the vector space they span -- or we are creating a new vector, which is a linear combination of the current row vectors, and as such this new vector is part of the span of the current row space. When we swap out one of the current vectors for this new vector, we've not changed the spanning vector space--the row space--we've simply changed the basis vectors. This is a general result in linear algebra. Given any sequence of vectors they span a vector space, they are *a* basis for that space. We can take any arbitrary vector from that space, which must be linear combination of our particular basis by definition, and add it to the basis without changing the space spanned.
24. Note that we can't just swap arbitrary vectors though. Consider ((1,0),(0,1)), we can't just swap (1,0) for (0,1). We can add it again, like ((1,0),(0,1),(0,1)).
	- this is confused and **wrong**
	- you cannot add constraints to the SLE unless they are equivalent to existing constraints
		- if your goal is to leave the solution set invariant
	- this is worked out in detail in the last few exchanges of "Row picture vs. column ..." in ChatGPT
- **Inconsistent solutions** just because a SLE is *written* with equalities it doesn't mean the SLE is a *consistent set of statements*: that each equation or their conjunction is true. When we perform elementary row ops (valid rules of inference) and arrive at a clear contradiction like $0=1$ this implies that our starting assumptions were false, or rather, they were inconsistent.

Suppose we have the system:

$$
\begin{aligned}
x + y &= 1 \\
x + y &= 2
\end{aligned}
$$

This is:

$$
\mathcal{S} = \{\varphi_1, \varphi_2\}
\quad \text{with} \quad
\varphi_1: x+y=1,\quad
\varphi_2: x+y=2.
$$

Trying to find $X^* = (x^*, y^*)$ such that:

$$
(\mathbb{R}, +, \times, \leq) \models_{X^*} \varphi_1 \land \varphi_2.
$$

By **row subtraction** (valid inference rule):

$$
(x+y)-(x+y) = (1)-(2)
\quad\Rightarrow\quad
0 = -1
\quad\Rightarrow\quad
\bot
$$

Thus:

$$
\mathcal{S} \vdash_{\mathcal{T}} \bot
\quad\text{(syntactic inconsistency)}
$$

and

$$
\forall X^* \in \mathbb{R}^2, \quad (\mathbb{R}, +, \times, \leq) \not\models_{X^*} \varphi_1 \land \varphi_2
\quad\text{(semantic inconsistency)}.
$$

1. So there must be a rule for swapping out basis vectors for vectors in the space, and this corresponds to actions we're taking on the equations in the corresponding SLE.
	- Vectors *only from the solution space* can be added to the basis / matrix $A$/SLE, but we can't just add vectors / equations willy nilly.
2. For instance, if I'm going to scale a vector that's fine. We don't "lose" any components. All non-zeros stay that way, all zeros stay that way.
3. If we are going to take one row vector $\vec{r}_{i}$

In a vector space of dim n, two vectors are either
- identical
- colinear
- coplanar
- co-hyperplanar
- ...
- orthonormal

# 3. Vector spaces

Rank
Basis
Dimension
Span
Linear combination
Linear (in) dependence
Colspace
Rowspace
Transpose

Inner product (standard)
Norm

Linear transformations
- Associated vector

Matrix-vector multiplication
Matrix-matrix multiplication