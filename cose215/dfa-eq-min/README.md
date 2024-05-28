# Equivalence and Minimization of Deterministic Finite Automata

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/dfa-eq-min.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>dfa-eq-min
├─ viewer
│  ├── index.html ─────────────── The HTML file for the automata viewer
│  ├── js/data.js ─────────────── The data of automata
│  └── ...
└─ src
   ├─ main/scala/kuplrg
   │  ├── FA.scala ────────────── The base class of finite automata (FA)
   │  ├── DFA.scala ───────────── The class of deterministic finite automata (DFA)
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of FAs that you must implement
   │  ├── Fuzzer.scala ────────── The fuzzer for random generation of FAs and REs
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement three functions to check the
equivalence and minimize the deterministic finite automata (DFA).**

- [**(Problem #1) `nonEqPairs`: Table-Filling Algorithm (40 points)**](#problem-1-noneqpairs-table-filling-algorithm-40-points)
- [**(Problem #2) `isEqual`: Equivalence of DFAs (30 points)**](#problem-2-isequal-equivalence-of-dfas-30-points)
- [**(Problem #3) `minimize`: Minimization of DFAs (30 points)**](#problem-3-minimize-minimization-of-dfas-30-points)
- [Appendix](#appendix)
  - [Playground](#playground)
  - [Short Definition of FA](#short-definition-of-fa)
  - [Automata Viewer](#automata-viewer)



## (Problem 1) `nonEqPairs`: Table-Filling Algorithm (40 points)

The first task is to implement the `nonEqPairs` function that returns the set of
**non-equivalent pairs** of states in the given **deterministic finite automaton
(DFA)** using the **table-filling algorithm**.

```scala
def nonEqPairs(dfa: DFA): Set[(State, State)] = ???
```


## (Problem 2) `isEqual`: Equivalence of DFAs (30 points)

The second task is to implement the `isEqual` function that returns the
**equivalence** of the given two **deterministic finite automata (DFAs)**.

> [!TIP]
>
> Please use the `nonEqPairs` function you implemented in the previous problem.

```scala
def isEqual(ldfa: DFA, rdfa: DFA): Boolean = ???
```


## (Problem 3) `minimize`: Minimization of DFAs (30 points)

The third task is to implement the `minimize` function that returns the
**minimized** version of **deterministic finite automaton (DFA)** of the given
**DFA**. The number of states in the minimized DFA should be minimum compared to
other equivalent DFAs.

The `minimize` function already partially implemented as follows:

```scala
def minimize(dfa: DFA): DFA =
  val DFA(states, symbols, trans, initState, finalStates) = dfa
  val nonEqMap: Map[State, Set[State]] = nonEqPairs(dfa).foldLeft(
    Map[State, Set[State]]().withDefaultValue(Set()),
  ) { case (m, (p, q)) => m + (p -> (m(p) + q)) }
  def aux(qs: List[State], visited: Set[State]): Set[State] = qs match
    case Nil => visited
    case q :: rest =>
      aux(
        (symbols.map(trans(q, _)) -- visited - q).toList.sorted ++ rest,
        visited + q,
      )
  val reachable = aux(List(initState), Set())
  ???
```

You can fill in the `???` part to complete the implementation of the `minimize`
function.

> [!NOTE]
>
> However, it is just a hint, and you can implement the `minimize` function in
> your way from scratch if you want.



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


### Short Definition of FA

You can define a DFA in a short way as follows:
```scala
DFA(3, "ab", "51", "4")
```

It is equivalent to the following definition:

```scala
DFA(
  states = (1 to 3).toSet,
  symbols = "ab".toSet,
  trans = Map(
    (1, 'a') -> 2,
    (1, 'b') -> 1,
    (2, 'a') -> 3,
    (2, 'b') -> 1,
    (3, 'a') -> 3,
    (3, 'b') -> 1
  ),
  initState = 1,
  finalStates = Set(3),
)
```

Each argument of the `DFA` constructor is explained as follows:
- The first argument is **the number of states**
    > For example, `3` means the states are `1`, `2`, and `3`.
- The second argument is **the set of symbols**
    > For example, `"ab"` means the symbols are `'a'` and `'b'`.
- The third argument is **the transition function**
    > For example, the above example has three states and two symbols, so six
    > transitions should be defined. And, the targets of the transitions are:
    > (2, 1, 3, 1, 3, 1). Then, after decreasing each number by 1 and
    > considering the opposite order, we can represent it as `020201` in base 3.
    > Finally, it is equal to the decimal number `181` and equal to `51` in base
    > 36. Therefore, the third argument is `"51"`).
- The fourth argument is **the set of final states**
    > For example, `4` in base 36 is equal to `100` in base 2. In opposite
    > order, it means the final state is the state `3`.

If you want to see the result of the short form of the automata, you can use the
`dump` method of each automaton (Please refer to the [Automata
Viewer](#automata-viewer) section for more details).

> [!NOTE]
>
> The initial state is always `1`.


### Automata Viewer

> [!NOTE]
>
> You can skip this section if you are not interested in the automata
> viewer. However, it is **HIGHLY RECOMMENDED** to use the automata viewer to
> check your automata when your implementation cannot pass the test cases.

You can **dump your automata** in HTML format to interactively visualize them in
the web browser.

For example, you can dump an automaton `DFA(3, "ab", "51", "4")` to the automata
viewer by invoking its `dump` in the playground:
```scala
object Implementation extends Template {
  ...
  @main def playground: Unit = {
    ...
    DFA(3, "ab", "51", "4").dump
    ...
  }
  ...
}
```
and run the program using `sbt run`:
```bash
$ sbt run
# Dumped the DFA to `viewer/js/data.js`.
# Please open `viewer/index.html` in your browser.
```
Then, the automaton will be dumped to `viewer/js/data.js`, and you can see the
dumped automaton in the automata viewer by opening `viewer/index.html` in your
browser (e.g., Chrome, Edge, Safari, Firefox, etc.):
<p align="center">
  <img src="../viewer.png" width="800px"/>
</p>

Similarly, you can dump and visualize any other `DFA` that you implemented to
check how they work.

This automata viewer will help you to understand the automata you defined. You
can check whether your automata accept a given word or not by entering the word
in the text box and clicking the `ACCEPT` button (or pressing the `Enter` key).

You can also check each step-by-step transition in the automata by clicking the
`STEP` button after clicking the `START` button. It will highlight the current
possible states. Finally, you can stop the step-by-step execution by clicking
the `STOP` button.
