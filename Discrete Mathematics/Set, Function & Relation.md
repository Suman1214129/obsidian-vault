## Set
A set is an unordered collection of distinct elements, which can be anything.

**Representation Type:**
1. List form:
	*{ 1,3,5,7.. }*
2. Vein Diagram
3. Set Builder Notation
	*{ x | some property x satisfies }*

***Subsets:***
A set a called a subset of set A if all elements of set a are also element of set A.

***Power set:***
The *power set* of a set is <mark>the set of all its possible subsets, including the empty set and the set itself</mark>.

**Ex:** the power set of a set with three elements: A = {1, 2, 3}: 
P(A) = {emptyset ,{1},{2},{3},{1,2},{1,3},{2,3},{1,2,3}} 

Cardinality of power set: |P(A)| =  $2^n$


**Important derived solution:**
A - B = A B'
AB = A intersection B

**Cartesian Product:**
If a we have two set A, B then the cartesian product will be
A x B = {*(a,b)* | a belongs to A, b belongs to B}

---
## Function
Function from Set A to Set B is a structure which *maps every element of A* (individually) to *exactly one element of B.*

If f is function from A to B then:
- we call A the *domain* of f.
- we call B the *co-domain* of f.

function f: A -> B
***Range (f)*** =  {y |  y belongs to B; y has at least one pre image}

**Rules for function requirement:**
- No element of domain should be left unmapped.
- No element of domain may map to more than one element of the co-domain.

*Image (Domain) = Range;*

**Types of Function:**
- ***Injective Function (one-one function):*** Every element in domain must maps with distinct images in co-domain.
- ***Injective Function (onto function):*** No element of co-domain should left unmapped. 
	*(Range = co-domain)*
- ***Bijective function:*** Function is Injective & Subjection. / every element of co-domain has exactly one pre-image.

NOTE: 
	if function *f: M -> N*  then number of function will be: *N^M*

***Inverse of function:*** It must be Bijective function.


NOTE:
	If there function F: x->y and |x| = |Y| then
	function is one-one or onto the it will *always bijective function*

---
## Relation
Relation is how the element of set A is related to element of set B.

**Properties of relation:**
1. Reflexive relation: every element is related to itself *(aRa).*
2. Symmetric relation: *aRb -> bRa* 
3. Transitive relation: *aRb ^ bRc -> aRc.*
   
`If a set follows all three properties then it became equivalance relation.`

The fundamental theorem of equivalence relation:

	Let R be the equivalence relation over set A, Every element a of A belongs to exactly one equivalance class of relation R.


NOTE:

	If relation R on set S, if R is symmetric & transitive then
	for all x (xRx (+) for all y (xR'y)
	
means then R can be either reflexive or not related element but not both.


***Q. How many binary relation will be there on set A with n elements?***
$$2^{n^2}$$
	because number of binary relations is <mark>total number of subsets of AxA.</mark>


R: is the set of all binary relation, the probability of *choosing reflexive relation* is: **$$n^{n^2 - n}$$**
R: is the set of all binary relation, the probability of *choosing symmetric relation* is: $$n^{(n^2 + n)/2}$$
R: is the set of all *symmetric* relation, the probability of *choosing reflexive relation* is: $$n^{(n^2 - n)/2}$$
NOTE: 
To find number of equivalence relation in set then *find number of partition ways.*

**Cyclic Relation:** aRb ^ bRc -> cRa

NOTE:
To define the following relations on a set S, *elements of S must be Sets*

---

****POSET:*** A POSET (short for *partially ordered set*) is <mark>a mathematical structure consisting of a set and a binary relation that defines a specific order or hierarchy</mark>.

It should satisfy three conditions: 
1. Reflexive 
2. Antisymmetric
3. Transitive

**Standard POSET (Partial ordered relation):**
1. <=, >=
2. subset of
3. divide on natural number set

**Total order relation (chain):**
If POR && Every pair of element must be comparable

<mark>Hasse diagram only possible for POSET</mark>: Hasse diagram is a relation which we show in upward representation.
*we don't show transitive edge in hasse diagram and horizontal edge as well.*


NOTE: 
	**KONG** is *maximal element* cause he "bows to no one" but not the greatest, greatest means everyone bows to kong.


**UB (s):** Every element of set is related to any element is called *Upper bound.*
**LB (s):** Any element which is related to every element of set is called *Lower bound.*


In any *POSET* these all elements be either unique or they will not exist:
1. Greatest element 
2. Least element
3. GLB(s) also V & Intersection
4. LUB(s) also ^ & Union

---
## **Lattice:** 
In lattice POSET every two incomparable elements have GLB and LUB.

**NOTE:** Idempotent, Commutative, Associative and Absorption Law property always satisfy in lattice

<img src="../Images/Pasted%20image%2020260718134854.png" width="548" alt="">


### Types of lattice:
**1. Bounded lattice:** The lattice which have greatest and least element.

**2. Complementary lattice:** A lattice in which every element has at least one compliment.
we check compliment between un related element
*ex:* Complement of any element (a^b = L), (avb = G)
if lattice is distributive the it has at-most one compliment (one way theorem)

**3. Distributed lattice:** <img src="../Images/Pasted%20image%2020260718153023.png" width="637" alt="">
- Every lattice with <=4 element (at most) is lattice.
- Every total order is set is distributive.
- Every lattice with <=4 elements is distributive.
- Every total order set is distributive.
- If lattice contains M3 or N5 then it is not distributive.
- Finite lattice in which every element has exactly one compliment, is distributive lattice.

**4. Boolean lattice:**
Boolean lattice is which includes all previous three properties i.e: bounded, complementary and distributed lattice is called boolean lattice.

**boolean algebra:** It follows structure:  $$2^n$$
**Total order relation:** 
Partial order set & every two elements are comparable. 
Every total order set is distributive.

**Important and standard lattice:** 
{ P(S), subset }
- has unique structure and same structure for every boolean algebra for 2^n elements as powerset

{ P(S), superset }
- every structure will get upside down, the glb = Union , LUB = Intersection
- greatest = null,  Least = S

**Divisor set:** 
Dn = set of all divisors on n.
*Ex:* D14 = {1,2,7,14}

Greatest: n, Least: 1
GLB = *GCD*,  LUB = *LCM*
- ALWAYS DISTRIBUTIVE AND LATTICE

	 N is square free when n is not divisible by any p^2 for any prime number.
	 if not square free then not will complemented as well
<img src="../Images/Pasted%20image%2020260730182654.png" width="432" alt="">


NOTE:
	*every finite lattice is complete lattice but not the case for infinite lattice.*