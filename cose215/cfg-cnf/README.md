# Conversion to Chomsky Normal Form (CNF)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/cfg-cnf.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>cfg-cnf
└─ src
   ├─ main/scala/kuplrg
   │  ├── CFG.scala ───────────── The base class of context-free grammars (CFGs)
   │  ├── Parser.scala ────────── The definition of the `Parser` object
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of functions that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement ten different functions to
convert a context-free grammar (CFG) into Chomsky normal form (CNF).**

- [**Context-Free Grammars (CFGs)**](#context-free-grammars-cfgs)
- [**(Problem #1) `isStartInRHSs`: Start Variable in RHSs (5 points)**](#problem-1-isstartinrhss-start-variable-in-rhss-5-points)
- [**(Problem #2) `getNullable`: Nullable Variables (10 points)**](#problem-2-getnullable-nullable-variables-10-points)
- [**(Problem #3) `removeEpsilon`: Eliminate Epsilon Productions (10 points)**](#problem-3-removeepsilon-eliminate-epsilon-productions-10-points)
- [**(Problem #4) `getUnitPairs`: Unit Pairs (10 points)**](#problem-4-getunitpairs-unit-pairs-10-points)
- [**(Problem #5) `removeUnit`: Eliminate Unit Productions (10 points)**](#problem-5-removeunit-eliminate-unit-productions-10-points)
- [**(Problem #6) `getGenVars`: Generating Variables (10 points)**](#problem-6-getgenvars-generating-variables-10-points)
- [**(Problem #7) `removeNonGen`: Remove Non-Generating Variables (10 points)**](#problem-7-removenongen-remove-non-generating-variables-10-points)
- [**(Problem #8) `getReachVars`: Reachable Variables (10 points)**](#problem-8-getreachvars-reachable-variables-10-points)
- [**(Problem #9) `removeNonReach`: Remove Non-Reachable Variables (10 points)**](#problem-9-removenonreach-remove-non-reachable-variables-10-points)
- [**(Problem #10) `toCNF`: Conversion to Chomsky Normal Form (15 points)**](#problem-10-tocnf-conversion-to-chomsky-normal-form-15-points)
- [Appendix](#appendix)
  - [Playground](#playground)



## Context-Free Grammars (CFGs)

A **context-free grammar (CFG)** is defined as the following `case class`:
```scala
final case class CFG(
  nts: Set[Nt],
  symbols: Set[Symbol],
  start: Nt,
  rules: Map[Nt, List[Rhs]],
) extends Acceptable
```
with the following components:
```scala
/** The type definition of nonterminal symbol */
type Nt = String

/** The definition of right-hand side of a production rule */
case class Rhs(seq: List[Nt | Symbol])
```

You can create a CFG instance from a string by using the following notation:
- **Nonterminal symbols** are represented by one-or-more uppercase letters
  (e.g., `S`, `A`, `NAME`).
- **Terminal symbols** are represented by one of the following characters
  enclosed by single quotes:
  - Any numeric digit (e.g., `'0'`, `'1'`, `'2'`)
  - Any lowercase letter (e.g., `'a'`, `'b'`, `'c'`)
  - Other characters: `'+'`, `'-'`, `'*'`, `'{'`, `'}'`, `'('`, `')'`
- `A -> ... | ...` represents a **production rule** with the left-hand side `A`
  and multiple right-hand sides separated by `|`.
- `<e>` represents the **empty right-hand side** (i.e., ε).
- You must add a semicolon `;` to represent the **end of a production rule**.

For instance, the following CFG accepts the language
$\lbrace \texttt{a}^n \texttt{b}^n \mid n \geq 0 \rbrace$:
```scala
val cfg: CFG = CFG("""
  S -> 'a' S 'b' | <e> ;
""")
```

A CFG is said to be in **Chomsky normal form (CNF)** if every production rule
has one of the following forms:
- $A \to BC$ where $B$ and $C$ are nonterminals,
- $A \to a$ where $a$ is a terminal, or
- $S \to \varepsilon$ where $S$ is the start variable (allowed only if $S$ does
  **not** appear in any right-hand side).

You need to implement ten functions that are used to convert a given CFG into
an equivalent CFG in Chomsky normal form (CNF).



## (Problem #1) `isStartInRHSs`: Start Variable in RHSs (5 points)

The first task is to implement the `isStartInRHSs` function that returns
`true` if and only if the **start variable** of the given CFG appears on the
**right-hand side of at least one production rule**.

```scala
def isStartInRHSs(cfg: CFG): Boolean = ???
```

For example,
- `S -> 'a' S 'b' | <e>` returns `true` because `S` appears in the RHS
  `'a' S 'b'`.
- `S -> A ; A -> 'a' A | <e>` returns `false` because the start variable `S`
  does not appear in any RHS.

This check is used in [Problem #10](#problem-10-tocnf-conversion-to-chomsky-normal-form-15-points)
to decide whether to introduce a fresh start variable before the conversion.



## (Problem #2) `getNullable`: Nullable Variables (10 points)

The second task is to implement the `getNullable` function that returns the
set of all **nullable** variables in the given CFG.

```scala
def getNullable(cfg: CFG): Set[Nt] = ???
```

A nonterminal $A$ is **nullable** if $A \Rightarrow^* \varepsilon$, i.e., it
can derive the empty string.

For example, given the CFG:
```scala
CFG("""
  S -> '0' A B C | '1' B | B B ;
  A -> A B B '0' | C ;
  B -> '0' B | '1' ;
  C -> C C | <e> ;
  D -> '1' D | A A ;
""")
```
the set of nullable variables is `Set("A", "C", "D")`.



## (Problem #3) `removeEpsilon`: Eliminate Epsilon Productions (10 points)

The third task is to implement the `removeEpsilon` function that returns a
new CFG with all $\varepsilon$-productions eliminated.

```scala
def removeEpsilon(cfg: CFG): CFG = ???
```

> [!TIP]
>
> Please use the `getNullable` function you implemented in the previous problem.

For each production rule $A \to \alpha$, replace it with all possible rules
obtained by deleting any subset of the nullable occurrences in $\alpha$,
except for the rule that derives the empty right-hand side. The resulting CFG
must have **no $\varepsilon$-productions** (except possibly $S \to \varepsilon$
if it is reintroduced in [Problem #10](#problem-10-tocnf-conversion-to-chomsky-normal-form-15-points)).



## (Problem #4) `getUnitPairs`: Unit Pairs (10 points)

The fourth task is to implement the `getUnitPairs` function that returns the
set of all **unit pairs** in the given CFG.

```scala
def getUnitPairs(cfg: CFG): Set[(Nt, Nt)] = ???
```

A pair $(A, B)$ is a **unit pair** if $A \Rightarrow^* B$ using only unit
productions (i.e., productions of the form $X \to Y$ where $Y$ is a single
nonterminal). Note that $(A, A)$ is always a unit pair for every nonterminal
$A$.

For example, given the CFG:
```scala
CFG("""
  S -> B ;
  A -> C | A '+' C | B '+' D ;
  B -> D | A '+' D | B '+' C ;
  C -> '0' | C '*' '0' | C '*' '1' | D '*' '0' ;
  D -> '1' | D '*' '1' ;
""")
```
the set of unit pairs is:
```scala
Set(
  "S" -> "S", "A" -> "A", "B" -> "B", "C" -> "C", "D" -> "D",
  "S" -> "B", "S" -> "D",
  "A" -> "C",
  "B" -> "D",
)
```



## (Problem #5) `removeUnit`: Eliminate Unit Productions (10 points)

The fifth task is to implement the `removeUnit` function that returns a new
CFG with all unit productions eliminated.

```scala
def removeUnit(cfg: CFG): CFG = ???
```

> [!TIP]
>
> Please use the `getUnitPairs` function you implemented in the previous problem.

For each unit pair $(A, B)$ and each non-unit production $B \to \alpha$, add
the rule $A \to \alpha$ to the new CFG. All unit productions must be removed
from the resulting CFG.



## (Problem #6) `getGenVars`: Generating Variables (10 points)

The sixth task is to implement the `getGenVars` function that returns the set
of all **generating variables** in the given CFG.

```scala
def getGenVars(cfg: CFG): Set[Nt] = ???
```

A nonterminal $A$ is **generating** if $A \Rightarrow^* w$ for some terminal
string $w \in \Sigma^*$.



## (Problem #7) `removeNonGen`: Remove Non-Generating Variables (10 points)

The seventh task is to implement the `removeNonGen` function that returns a
new CFG with all **non-generating** variables and all production rules
referencing them removed.

```scala
def removeNonGen(cfg: CFG): CFG = ???
```

> [!TIP]
>
> Please use the `getGenVars` function you implemented in the previous problem.



## (Problem #8) `getReachVars`: Reachable Variables (10 points)

The eighth task is to implement the `getReachVars` function that returns the
set of all **reachable variables** in the given CFG.

```scala
def getReachVars(cfg: CFG): Set[Nt] = ???
```

A nonterminal $A$ is **reachable** if $S \Rightarrow^* \alpha A \beta$ for some
$\alpha, \beta \in (V \cup \Sigma)^*$, where $S$ is the start variable.



## (Problem #9) `removeNonReach`: Remove Non-Reachable Variables (10 points)

The ninth task is to implement the `removeNonReach` function that returns a
new CFG with all **non-reachable** variables and all production rules
referencing them removed.

```scala
def removeNonReach(cfg: CFG): CFG = ???
```

> [!TIP]
>
> Please use the `getReachVars` function you implemented in the previous problem.



## (Problem #10) `toCNF`: Conversion to Chomsky Normal Form (15 points)

The tenth task is to implement the `toCNF` function that returns an equivalent
CFG in **Chomsky normal form (CNF)** for the given CFG.

```scala
def toCNF(cfg: CFG): CFG = ???
```

The conversion to CNF can be done by combining the previously implemented
functions in the following order:

1. If the start variable appears in any RHS, introduce a fresh start variable
   $S_0$ with a single rule $S_0 \to S$ (using
   [`isStartInRHSs`](#problem-1-isstartinrhss-start-variable-in-rhss-5-points)).
2. Eliminate $\varepsilon$-productions
   (using [`removeEpsilon`](#problem-3-removeepsilon-eliminate-epsilon-productions-10-points)).
3. Eliminate unit productions
   (using [`removeUnit`](#problem-5-removeunit-eliminate-unit-productions-10-points)).
4. Remove useless variables
   (using [`removeNonGen`](#problem-7-removenongen-remove-non-generating-variables-10-points)
   followed by [`removeNonReach`](#problem-9-removenonreach-remove-non-reachable-variables-10-points)).
5. Replace each terminal $a$ appearing in a RHS of length at least 2 with a
   fresh nonterminal $T_a$ and add a rule $T_a \to a$.
6. Break every RHS of length at least 3 into a sequence of binary rules by
   introducing fresh nonterminals.

The resulting CFG must satisfy `isCNF == true`, where the `isCNF` property is
defined in `CFG.scala` as:
```scala
lazy val isCNF: Boolean = rules.forall { (from, rhsList) =>
  rhsList.forall(_.seq match
    case Nil                => from == start && !isStartInRHSs
    case List(_: Symbol)    => true
    case List(_: Nt, _: Nt) => true
    case _                  => false,
  )
}
```

Moreover, the language of the resulting CFG must be **equal to the language of
the original CFG**.



## Appendix


### Playground

You can run your implementation in the `playground` method in the
`Implementation.scala` file.

```scala
object Implementation extends Template {
  ...
  @main def playground: Unit = {
    ...
    // Do whatever you want here
    // For example, you can print "Hello, World!" as follows:
    println("Hello, World!")
    ...
  }
  ...
}
```
and run the program using `sbt run`:
```bash
$ sbt run
# Hello, World!
```
