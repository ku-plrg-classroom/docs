# Equivalence of Context-Free Grammars and Pushdown Automata

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/equiv-cfg-pda.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>fa-examples
└─ src
   ├─ main/scala/kuplrg
   │  ├── CFG.scala ───────────── The class of Context-Free Grammars (CFG)
   │  ├── PDA.scala ───────────── The class of Pushdown Automata (PDA)
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement `pdafs2es`, `pdaes2fs`,
`cfg2pdaes`, and `pdaes2cfg` functions in the `Implementation.scala` file.**

- [(Problem #1) PDA with Final States to PDA with Empty Stacks (20 points)](#problem-1-pda-with-final-states-to-pda-with-empty-stacks-20-points)
- [(Problem #2) PDA with Empty Stacks to PDA with Final States (20 points)](#problem-2-pda-with-empty-stacks-to-pda-with-final-states-20-points)
- [(Problem #3) CFGs to PDA with Empty Stacks (30 points)](#problem-3-cfgs-to-pda-with-empty-stacks-30-points)
- [(Problem #4) PDA with Empty Stacks to CFGs (30 points)](#problem-4-pda-with-empty-stacks-to-cfgs-30-points)
- [Appendix](#appendix)
  - [Playground](#playground)
  - [String Form of PDA](#string-form-of-pda)
  - [String Form of CFGs](#string-form-of-cfgs)


## (Problem #1) PDA with Final States to PDA with Empty Stacks (20 points)

The first task is to implement the `pdafs2es` function that converts a **PDA
with final states** to a **PDA with empty stacks**:

```scala
// Convert a PDA with final states to a PDA with empty stacks
def pdafs2es(pda: PDA): PDA = ???
```

> [!NOTE]
>
> You can dump your implementation by using the `dump` method of the `PDA`
> class. Please refer to the [String Form of PDA](#string-form-of-pda) section
> for more details.


## (Problem #2) PDA with Empty Stacks to PDA with Final States (20 points)

The second task is to implement the `pdaes2fs` function that converts a **PDA
with empty stacks** to a **PDA with final states**:

```scala
// Convert a PDA with empty stacks to a PDA with final states
def pdaes2fs(pda: PDA): PDA = ???
```

> [!NOTE]
>
> You can dump your implementation by using the `dump` method of the `PDA`
> class. Please refer to the [String Form of PDA](#string-form-of-pda) section
> for more details.


## (Problem #3) CFGs to PDA with Empty Stacks (30 points)

The third task is to implement the `cfg2pdaes` function that converts a **CFG**
to a **PDA with empty stacks**:

```scala
// Convert a CFG to a PDA with empty stacks
def cfg2pdaes(cfg: CFG): PDA = ???
```

> [!NOTE]
>
> You can dump your implementation by using the `dump` method of the `PDA`
> class. Please refer to the [String Form of PDA](#string-form-of-pda) section
> for more details.

## (Problem #4) PDA with Empty Stacks to CFGs (30 points)

The last task is to implement the `pdaes2cfg` function that converts a **PDA
with empty stacks** to a **CFG**:

```scala
// Convert a PDA with empty stacks to a CFG
def pdaes2cfg(pda: PDA): CFG = ???
```

> [!NOTE]
>
> You can dump your implementation by using the `dump` method of the `CFG`
> class. Please refer to the [String Form of CFGs](#string-form-of-cfgs) section
> for more details.


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


### String Form of PDA

You can see the string form of the PDA using the `dump` method:
```scala
val pda: PDA = ...

// Dump the PDA instance
pda.dump
```

Then, you can see the string form of the PDA as follows:
```plaintext
* A context-free grammar is dumped:
  * String form:
    - states: 0, 1, 2,
    - symbols: a, b,
    - alphabets: X, Z,
    - initState: 0,
    - initAlphabet: Z,
    - finalStates: 2,
    - trans:
        0 -> 1 - ε [X -> X]
        0 -> 1 - ε [Z -> Z]
        0 -> 0 - a [X -> X X]
        0 -> 0 - a [Z -> X Z]
        1 -> 2 - ε [Z -> Z]
        1 -> 1 - b [X -> ]
  * Scala object: PDA(HashSet(0, 1, 2),HashSet(a, b),HashSet(X, Z),HashMap((2,Some(a),Z) -> Set(), (1,None,Z) -> Set((2,List(Z))), (1,Some(b),Z) -> Set(), (0,None,X) -> Set((1,List(X))), (1,Some(a),Z) -> Set(), (1,Some(b),X) -> Set((1,List())), (0,Some(b),Z) -> Set(), (2,Some(b),X) -> Set(), (0,None,Z) -> Set((1,List(Z))), (2,Some(b),Z) -> Set(), (1,None,X) -> Set(), (0,Some(b),X) -> Set(), (0,Some(a),Z) -> Set((0,List(X, Z))), (2,None,Z) -> Set(), (0,Some(a),X) -> Set((0,List(X, X))), (1,Some(a),X) -> Set(), (2,Some(a),X) -> Set(), (2,None,X) -> Set()),0,Z,Set(2))
```


### String Form of CFGs

You can define a CFG in a string form as follows:

```scala
val cfg: CFG = CFG("""
  S -> 'b' S 'b' 'b' | A ;
  A -> 'a' A | <e> ;
""")
```

It is equivalent to the following definition:

```scala
val cfg: CFG = CFG(
  nts = Set("S", "A"),
  symbols = Set('b', 'a'),
  start = "S",
  rules = Map(
    "S" -> List(
      Rhs(List('b', "S", 'b', 'b')),
      Rhs(List("A"))
    ),
    "A" -> List(
      Rhs(List('a', "A")),
      Rhs(List())
    )
  ),
)
```

The string form of the CFG is defined as follows:
- `A` represents a variable $A$.
- `<e>` represents an empty sequence.
- `'a'` represents a symbol $\texttt{a}$.
- `S -> x | y ;` is a short form of `S -> x ; S -> y ;`

You can see the string form of the CFG using the `dump` method:
```scala
val cfg: CFG = ...

// Dump the CFG instance
cfg.dump
```

Then, you can see the string form of the CFG as follows:
```plaintext
* A context-free grammar is dumped:
  * String form:
    S -> 'b' S 'b' 'b' | A ;
    A -> 'a' A | <e> ;
  * Scala object: CFG(Set(S, A),Set(b, a),S,Map(A -> List(Rhs(List(a, A)), Rhs(List())), S -> List(Rhs(List(b, S, b, b)), Rhs(List(A)))))
```
