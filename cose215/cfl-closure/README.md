# Closure Properties of Context-Free Languages

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/cfl-closure.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>cfl-closure
└─ src
   ├─ main/scala/kuplrg
   │  ├── CFG.scala ───────────── The class of context-free grammars (CFGs)
   │  ├── PDA.scala ───────────── The class of pushdown automata (PDAs)
   │  ├── DFA.scala ───────────── The class of deterministic finite automata (DFAs)
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of functions that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement seven different functions applying
corresponding closure operations on context-free grammars (CFGs) or pushdown
automata (PDAs).**

- [**(Problem #1) `unionCFG`: Union of CFGs (10 points)**](#problem-1-unioncfg-union-of-cfgs-10-points)
- [**(Problem #2) `concatCFG`: Concatenation of CFGs (10 points)**](#problem-2-concatcfg-concatenation-of-cfgs-10-points)
- [**(Problem #3) `starCFG`: Kleene Star of CFGs (10 points)**](#problem-3-starcfg-kleene-star-of-cfgs-10-points)
- [**(Problem #4) `reverseCFG`: Reversal of CFGs (15 points)**](#problem-4-reversecfg-reversal-of-cfgs-15-points)
- [**(Problem #5) `homCFG`: Homomorphism of CFGs (15 points)**](#problem-5-homcfg-homomorphism-of-cfgs-15-points)
- [**(Problem #6) `ihomPDA`: Inverse Homomorphism of PDAs (20 points)**](#problem-6-ihompda-inverse-homomorphism-of-pdas-20-points)
- [**(Problem #7) `intersectPDA`: Intersection of a PDA with a DFA (20 points)**](#problem-7-intersectpda-intersection-of-a-pda-with-a-dfa-20-points)
- [Appendix](#appendix)
  - [Playground](#playground)
  - [String Form of CFGs](#string-form-of-cfgs)
  - [String Form of PDAs](#string-form-of-pdas)




## (Problem 1) `unionCFG`: Union of CFGs (10 points)

The first task is to implement the `unionCFG` function that returns the
**union** of the given **context-free grammars (CFGs)**.

```scala
def unionCFG(lcfg: CFG, rcfg: CFG): CFG = ???
```


## (Problem 2) `concatCFG`: Concatenation of CFGs (10 points)

The second task is to implement the `concatCFG` function that returns the
**concatenation** of the given **context-free grammars (CFGs)**.

```scala
def concatCFG(lcfg: CFG, rcfg: CFG): CFG = ???
```


## (Problem 3) `starCFG`: Kleene Star of CFGs (10 points)

The third task is to implement the `starCFG` function that returns the
**Kleene star** of the given **context-free grammar (CFG)**.

```scala
def starCFG(cfg: CFG): CFG = ???
```


## (Problem 4) `reverseCFG`: Reversal of CFGs (15 points)

The fourth task is to implement the `reverseCFG` function that returns the
**reversal** of the given **context-free grammar (CFG)**.

```scala
def reverseCFG(cfg: CFG): CFG = ???
```


## (Problem 5) `homCFG`: Homomorphism of CFGs (15 points)

The fifth task is to implement the `homCFG` function that returns the
**homomorphism** of the given **context-free grammar (CFG)**.

```scala
def homCFG(cfg: CFG, h: Hom): CFG = ???
```

where the `Hom` type is defined as a mapping from symbols to words in the `basics.scala` file as follows:
```scala
type Hom = Map[Symbol, Word]
```


## (Problem 6) `ihomPDA`: Inverse Homomorphism of PDAs (20 points)

The sixth task is to implement the `ihomPDA` function that returns the
**inverse homomorphism** of the given **pushdown automaton (PDA)** by final
states.

```scala
def ihomPDA(pda: PDA, h: Hom): PDA = ???
```

where the `Hom` type is defined as a mapping from symbols to words in the `basics.scala` file as follows:
```scala
type Hom = Map[Symbol, Word]
```


## (Problem 7) `intersectPDA`: Intersection of a PDA with a DFA (20 points)

The seventh task is to implement the `intersectPDA` function that returns the
**intersection** of the given **pushdown automaton (PDA)** by final states
and the given **deterministic finite automaton (DFA)**.

```scala
def intersectPDA(pda: PDA, dfa: DFA): PDA = ???
```



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


### String Form of CFGs

You can define a CFG in a string form as follows:

```scala
val cfg: CFG = CFG("""
  S -> 'a' S 'b' | <e> ;
""")
```

The string form of the CFG is defined as follows:
- `A` represents a variable $A$ (one-or-more uppercase letters).
- `'a'` represents a terminal symbol $\texttt{a}$ (digits, lowercase letters,
  or one of `'+'`, `'-'`, `'*'`, `'{'`, `'}'`, `'('`, `')'`).
- `<e>` represents an empty sequence.
- `S -> x | y ;` is a short form of `S -> x ; S -> y ;`.
- Every production rule must end with a semicolon `;`.

You can see the string form of the CFG using the `dump` method:
```scala
val cfg: CFG = ...

// Dump the CFG instance
cfg.dump
```


### String Form of PDAs

You can see the string form of the PDA using the `dump` method:
```scala
val pda: PDA = ...

// Dump the PDA instance
pda.dump
```

Then, you can see the string form of the PDA as follows:
```plaintext
* A pushdown automaton is dumped:
  * String form:
    - states: 0, 1, 2,
    - symbols: a, b,
    - alphabets: X, Z,
    - initState: 0,
    - initAlphabet: Z,
    - finalStates: 2,
    - trans:
        0 -> 0 - a [Z -> X Z]
        0 -> 0 - a [X -> X X]
        0 -> 1 - ε [Z -> Z]
        0 -> 1 - ε [X -> X]
        1 -> 1 - b [X -> ]
        1 -> 2 - ε [Z -> Z]
```
