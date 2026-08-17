
# Proposition Logic:
- **Proposition:** Proposition is a declarative statement that can be either *true* or *false* but not both.
- **Proposition Variable**: It is a variable which represent the proposition.

		Note: In proposition Logic, we do not care about proposition but proposition variable

#### **Logical Connectives:**
-  **NOT:** *Make any any statement FALSE.*
- **AND (^):** *When both statement true.* (conjuction)
- **OR (v, +):** *At least one of them are true.* (disconjuction)
- **XOR:** *Exactly one of them are true but not both.* (Exclusive)
- **NAND:** *(A ^ B)'*
- **NOR:** *(A + B)'*


#### **Implication/ Conditional Statement:**
(P -> Q) :  *If P then Q*

| P   | Q   | P->Q |
| --- | --- | ---- |
| F   | F   | T    |
| F   | T   | T    |
| T   | F   | F    |
| T   | T   | T    |
***Note:***
- **If P is sufficient for Q =**  *(P -> Q)*  (if P happens then Q may happen or may not)
- **If P is Necessary for Q =**  *(Q -> P)* *;  (~P -> ~Q)* (If Q happens then P must happen)

		P -> Q =  P' + Q


#### **Bi-implication/ Bi-Conditional Statement:**
***(P <-> Q) :***  *If P = Q*  (If P happens then Q will must happen & if P not happens then Q also must not).

(P<->Q)' = P XOR R  (exactly one of them are true)

| P   | Q   | P->Q | Q->P | P<->Q |
| --- | --- | ---- | ---- | ----- |
| F   | F   | T    | T    | T     |
| F   | T   | T    | F    | F     |
| T   | F   | F    | T    | F     |
| T   | T   | T    | T    | T     |
<img src="../Images/Pasted%20image%2020260619155850.png" width="241" alt="">


	P -> Q: P` + Q

#### **Priority Order of Logical Connectors:**
1. ( )
2. Negation ~
3. AND ^
4. OR +
5. Implication
6. Bi-Implication


**Tautology:** A proposition that is *always True*.
**Contradiction:** A proposition that is *always False*.
**Contingency:** A proposition that is *neither True* or *nor False*.



***Note:*** 
	We will not use Truth table method to evaluate Proposition formula but *By-Case Method*. In this method we let any of the single variable as **True** or **False.**


#### **Logical Equivalence**
Compound propositions that have the same truth values in all cases are called logically equivalent.
Their Bi-conditional will be *tautology*.

	For n atomic propositions variable - 2^n rows will be there.


#### **De-Morgan's Law**
	(P+Q)'  =  P' ^ Q'
	(P^Q)'  =  P' + Q'


#### **Absorption Law**
	 P + (P^Q) = P
	 P ^ (P+Q) = P

**Another :**  P + P'Q =  P + Q


<img src="../Images/Pasted%20image%2020260623161211.png" width="386" alt="">

***Practice:*** <img src="../Images/Pasted%20image%2020260622232519.png" width="587" alt="">


***Note:***  *Implies (->) is not Associative.*



# First Order Logic

In first-order logic, each variable refers to some object in a set called the domain of discourse.

	First-Order Logic speaks about objects, which are the domain of discourse or the universe.


#### **Predicate**
A logical expression containing some variable that becomes a proposition when we substitute any particular value from the universe for this variable is called a predicate.
- *It is also called propositional function.*


#### **Quantifier**
Quantification words in English:  few, all, many, some, any 
Quantification word in logic:  All = Every, Some= at least one

**Universal Quantifier:** A statement of the form Ax. some-formula is true if, for every choice of x, the statement some-formula is true when x is plugged into it

**Existential Quantifier:**  tement of the form Ex. some-formula is true if, for some choice of x, the statement some-formula is true when that x is plugged into it. Examples: *Ex. (Even(x) ^ Prime(x).*
A sta
<img src="../Images/Pasted%20image%2020260625143959.png" width="319" alt="">


**Nested Quantifier:** Two quantifiers are nested if one is within the scope of the other. 


**English <-> First Order Logic:**

1. No rabbit is cute.
- ***Ax ( Rx -> Cx' )*** :  *For all x, If x is rabbit then not cute.*
  or
- ***Ex' ( Rx ^ Cx ) :***  *There does not exist x, such that x is rabbit and cute.*

2. Some rabbit is not cute.
- ***Ex ( Rx ^ Cx' ) :***  *There exist an x, x is rabbit and not cute.*
  or
- ***Ax'  ( Rx -> Cx ) :***  *It is not the case that every rabbit is cute.*

2. Only rabbits are cute.
- ***Ax ( Cx -> Rx ) :***  *If you are cute, then you are rabbit.*



#### **Distribution of Quantifiers over logic connectives:**
<img src="../Images/Pasted%20image%2020260629230541.png" width="475" alt="">


#### **Null Quantifiers:**
Take Two cases - True or False (to prove equivalence)