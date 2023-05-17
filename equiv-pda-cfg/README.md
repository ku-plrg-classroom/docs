# Equivalence of Pushdown Automata and Context-Free Grammars

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/equiv-pda-cfg.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
```
equiv-pda-cfg
└─ src
   ├─ main/scala/kuplrg
   │  ├── PDA.scala ───────────── The class of pushdown automata (PDA)
   │  ├── CFG.scala ───────────── The class of context-free grammars (CFGs)
   │  ├── Implementation.scala ── [[ IMPLEMENT AND SUBMIT THIS FILE ]]
   │  ├── Template.scala ──────── The templates of FAs that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── [[ ADD YOUR OWN TESTS ]]
      └─ SpecBase.scala ───────── The base class of test cases
```

**The goal of this assignment is to implement `pdafs2es`, `pdaes2fs`, and
`cfg2pdaes` functions in the `Implementation.scala` file.**

- [(Problem #1) PDA with Final States to PDA with Empty Stacks (30 points)](#problem-1-pda-with-final-states-to-pda-with-empty-stacks-30-points)
- [(Problem #2) PDA with Empty Stacks to PDA with Final States (30 points)](#problem-2-pda-with-empty-stacks-to-pda-with-final-states-30-points)
- [(Problem #3) CFGs to PDA with Empty Stacks (40 points)](#problem-3-cfgs-to-pda-with-empty-stacks-40-points)
- [Appendix](#appendix)
  - [String Form of PDA](#string-form-of-pda)
  - [String Form of CFGs](#string-form-of-cfgs)



## (Problem #1) PDA with Final States to PDA with Empty Stacks (30 points)

The first task is to implement the `pdafs2es` function that converts a **PDA
with final states** to a **PDA with empty stacks**:

```scala
  // Convert a PDA with final states to a PDA with empty stacks
  def pdafs2es(pda: PDA): PDA = ???
```

### Test Cases

The test cases are defined in the `Spec.scala` file. You can add your own test
cases in the `Spec.scala` file. The test cases are executed when you run `sbt
test`.

You can dump your implementation by using the `dump` method of the `PDA` class.
Please refer to the [String Form of PDA](#string-form-of-pda) section for more
details.


## (Problem #2) PDA with Empty Stacks to PDA with Final States (30 points)

The second task is the exact opposite task, which is to implement the
`pdaes2fs` function that converts a **PDA with empty stacks** to a **PDA with
final states**:

```scala
  // Convert a PDA with empty stacks to a PDA with final states
  def pdaes2fs(pda: PDA): PDA = ???
```

### Test Cases

The test cases are defined in the `Spec.scala` file. You can add your own test
cases in the `Spec.scala` file. The test cases are executed when you run `sbt
test`.

You can dump your implementation by using the `dump` method of the `PDA` class.
Please refer to the [String Form of PDA](#string-form-of-pda) section for more
details.


## (Problem #3) CFGs to PDA with Empty Stacks (40 points)

The last task is to implement the `cfg2pdaes` function that converts a **CFG**
to a **PDA with empty stacks**:

```scala
  // Convert a CFG to a PDA with empty stacks
  def cfg2pdaes(cfg: CFG): PDA = ???
```

### Test Cases

The test cases are defined in the `Spec.scala` file. You can add your own test
cases in the `Spec.scala` file. The test cases are executed when you run `sbt
test`.

The test cases are defined using the string form of the CFGs. If you want to
understand the string form, please refer to [String Form of
CFGs](#string-form-of-cfgs). And, you can dump your implementation by using the
`dump` method of the `CFG` class.


## Appendix

### String Form of PDA

You can see the string form of the PDA using the `dump` method:
```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can dump any PDA
    pda1.dump

    ...
  }
  ...
}
```

When you run `sbt test`, the string form and Scala object will be printed after
the test cases are executed:

```bash
$ sbt
...
sbt:...> test
...
----------------------------------------
[SCORE] ...
----------------------------------------
...
* A PDA is dumped:
  * String form:
    - states: 0, 1, 2
    - symbols: a, b
    - alphabets: A, C
    - initState: 0
    - initAlphabet: A
    - finalStates: 2
    - trans: {
        0 -<e>-> 1 [A -> A]
        0 -<e>-> 1 [C -> C]
        0 - a -> 0 [A -> CA]
        0 - a -> 0 [C -> CC]
        1 -<e>-> 2 [A -> A]
        1 - b -> 1 [C -> ]
      }
  * Scala object: PDA(HashSet(0, 1, 2),HashSet(a, b),HashSet(0, 2),HashMap((1,Some(b),0) -> Set(), (0,None,2) -> Set((1,List(2))), (2,Some(b),2) -> Set(), (1,Some(a),2) -> Set(), (0,None,0) -> Set((1,List(0))), (2,Some(b),0) -> Set(), (0,Some(b),0) -> Set(), (2,None,0) -> Set(), (2,None,2) -> Set(), (0,Some(a),0) -> Set((0,List(2, 0))), (0,Some(a),2) -> Set((0,List(2, 2))), (1,Some(a),0) -> Set(), (1,None,0) -> Set((2,List(0))), (1,None,2) -> Set(), (2,Some(a),0) -> Set(), (2,Some(a),2) -> Set(), (1,Some(b),2) -> Set((1,List())), (0,Some(b),2) -> Set()),0,0,Set(2))
...
```


### String Form of CFGs

You can define a CFG in a string form as follows:

```scala
val cfg: CFG = CFG("""
  'S -> b 'S bb | 'A ;;
  'A -> a 'A | <e> ;;
""")
```

It is equivalent to the following definition:

```scala
val A = 0
val S = 18
val cfg: CFG = CFG(
  variables = Set(A, S),
  symbols = Set('a', 'b'),
  start = S,
  productions = Set(
    S -> List('b', S, 'b', 'b'),
    S -> List(A),
    A -> List('a', A),
    A -> List(),
  )
)
```

The string form of the CFG is defined as follows:
- `'A` represents a variable $A$.
- `<e>` represents an empty sequence.
- `a` represents a symbol $\texttt{a}$.
- `S -> x | y` is a short form of `S -> x ;; S -> y`

You can see the string form of the CFG using the `dump` method:
```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can dump any CFG
    cfg.dump

    ...
  }
  ...
}
```

When you run `sbt test`, the string form and Scala object will be printed after
the test cases are executed:

```bash
$ sbt
...
sbt:...> test
...
----------------------------------------
[SCORE] ...
----------------------------------------
...
* A CFG is dumped:
  * String form:
    'S -> 'A | b 'S b b ;;
    'A -> <e> | a 'A ;;
  * Scala object: CFG(Set(18, 0),Set(b, a),18,Set((18,List(b, 18, b, b)), (18,List(0)), (0,List(a, 0)), (0,List())))
...
```
