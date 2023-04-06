# Equivalence of Regular Expressions and Finite Automata

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/equiv-re-fa.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
```
fa-examples
├─ viewer
│  ├── index.html ─────────────── The HTML file for the automata viewer
│  ├── js/data.js ─────────────── The data of automata
│  └── ...
└─ src
   ├─ main/scala/kuplrg
   │  ├── FA.scala ────────────── The base class of finite automata (FA)
   │  ├── DFA.scala ───────────── The class of deterministic finite automata (DFA)
   │  ├── NFA.scala ───────────── The class of nondeterministic finite automata (NFA)
   │  ├── ENFA.scala ──────────── The class of ε-nondeterministic finite automata (ε-NFA)
   │  ├── RE.scala ────────────── The class of regular expressions (REs)
   │  ├── Implementation.scala ── [[ IMPLEMENT AND SUBMIT THIS FILE ]]
   │  ├── Template.scala ──────── The templates of FAs that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── [[ ADD YOUR OWN TESTS ]]
      └─ SpecBase.scala ───────── The base class of test cases
```

**The goal of this assignment is to implement the `re2enfa` function and the
`dfa2re` function in the `Implementation.scala` file.**

- [(Problem #1) Regular Expressions to ε-Nondeterministic Finite Automata (50 points)](#problem-1-regular-expressions-to-ε-nondeterministic-finite-automata-50-points)
- [(Problem #2) Deterministic Finite Automata to Regular Expressions (50 points)](#problem-2-deterministic-finite-automata-to-regular-expressions-50-points)
- [Appendix](#appendix)
  - [Automata Viewer](#automata-viewer)
  - [Debugger for `dfa2re`](#debugger-for-dfa2re)
  - [Short Definition of DFA](#short-definition-of-dfa)
  - [String Form of Regular Expressions](#string-form-of-regular-expressions)



## (Problem 1) Regular Expressions to ε-Nondeterministic Finite Automata (50 points)

The first task is to implement the `re2enfa` function that converts a **regular
expression** to an **ε-Non-deterministic Finite Automaton (ε-NFA)**.

We recommend you to utilize the **simplified ε-NFA** (`SimpleENFA` class defined
in `Template.scala`). If so, the `re2enfa` function is already implemented as
follows:

```scala
  // Convert a regular expression to a epsilon-NFA
  def re2enfa(re: RE): ENFA =
    def re2enfa(re: RE): ENFA =
      val SimpleENFA(from, trans, to) = re2senfa(re, 1)
      val states = (from to to).toSet
      val symbols = trans.flatMap((_, aOpt, _) => aOpt)
      val map = trans.groupMap((i, aOpt, _) => (i, aOpt))((_, _, j) => j)
      val enfaTrans = states
        .flatMap(q => symbols.map(a => q -> Option(a)) + (q -> None))
        .map(pair => pair -> map.getOrElse(pair, Set()))
        .toMap
      ENFA(
        states = states,
        symbols = symbols,
        trans = enfaTrans,
        initState = from,
        finalStates = Set(to),
      )
```

and it is enough to fill out the body of the `re2senfa` function that converts a
**regular expression** to a **simplified ε-NFA** with an **initial state** as
follows:

```scala
  // Convert a regular expression `re` to a simplified epsilon-NFA with an
  // initial state `i`.
  def re2senfa(re: RE, i: State): SimpleENFA = re match
    case REEmpty() => ???
    case REEpsilon() => ???
    case RESymbol(symbol) => ???
    case REUnion(re1, re2) => ???
    case REConcat(re1, re2) => ???
    case REStar(re) => ???
    case REParen(re) => ???
```

> :warning: However, since it is just a recommendation, implement the `re2enfa`
> function without using the `SimpleENFA` class if you want to do so.

### Test Cases

The test cases are defined in the `Spec.scala` file. You can add your own test
cases in the `Spec.scala` file. The test cases are executed when you run `sbt
test`.

The test cases are defined using the string form of the regular expressions. If
you want to understand the string form, please refer to [String Form of
Regular Expressions](#string-form-of-regular-expressions).

You can debug your implementation by using the `dump` method of the `ENFA`
class. Please refer to the [Automata Viewer](#automata-viewer) section for more
details.


## (Problem 2) Deterministic Finite Automata to Regular Expressions (50 points)

The second task is to implement the `dfa2re` function that converts a
**deterministic finite automaton (DFA)** to a **regular expression**:

```scala
  // Convert a DFA to a regular expression
  def dfa2re(givenDFA: DFA, debug: Boolean = false): RE =
    val dfa = givenDFA.normalized

    // Show details in the conversion from a DFA to a regular expression
    if (debug)
      show("* Details in the conversion from a DFA to a regular expression:")
      val n = dfa.states.size
      for (k <- 0 to n; i <- 1 to n; j <- 1 to n)
        val re = reForPaths(dfa)(i, j, k)
        println(s"  - ($i, $j, $k) -> ${re.stringForm}")

    ???
```

We recommend you to utilize the `reForPaths` function that constructs a
**regular expression** representing the **set of paths** in the given DFA. If
so, it is enough to fill out the remaining part in the `dfa2re` function and the
body of the `reForPaths` function:

```scala
  // A regular expression accepting paths from `i` to `j` with intermediate
  // states bounded by `k` in a given DFA `dfa`. Assume that the given DFA
  // `dfa` is already normalized (i.e., the states of DFA are 1, 2, ..., n).
  def reForPaths(dfa: DFA)(i: State, j: State, k: State): RE = k match
    case 0 => ???
    case _ => ???
```

> :warning: However, it is just a recommendation. If you want to implement the
> `dfa2re` function without using the `reForPaths` function, you can do so.

For debugging, you can use the `debug` option of the `dfa2re` function. Please
refer to the [Debugger for `dfa2re`](#debugger-for-dfa2re) section for more
details.

### Test Cases

The test cases are defined in the `Spec.scala` file. You can add your own test
cases in the `Spec.scala` file. The test cases are executed when you run `sbt
test`.

The test cases are defined using the short definition of the DFA. If you want to
understand the short definition, please refer to [Short Definition of
DFA](#short-definition-of-dfa) section.



## Appendix


### Automata Viewer

> :warning: You can skip this section if you are not interested in the automata
> viewer. However, it is HIGHLY RECOMMENDED to use it for debugging.

You can dump the automata in HTML format by invoking the `dump` method of the
`FA` class. The `dump` method will dump the automata into the
`viewer/js/data.js` file.

For example, assume that a DFA `dfa` is defined as follows:
```scala
// A short definition: DFA(3, "01", 181, 4)
val dfa: DFA = DFA(
  states = Set(0, 1, 2),
  symbols = Set('0', '1'),
  trans = Map(
    (0, '0') -> 1,
    (0, '1') -> 0,
    (1, '0') -> 2,
    (1, '1') -> 0,
    (2, '0') -> 2,
    (2, '1') -> 0,
  ),
  initState = 0,
  finalStates = Set(2),
)
```

Then, you can dump a DFA `dfa` as follows in the `Spec.scala` file:
```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can dump any finite automaton via `dump` method
    dfa.dump

    ...
  }
  ...
}
```

When you run `sbt test`, the automaton will be dumped to `viewer/js/data.js`
after the test cases are executed:

```bash
$ sbt
...
sbt:...> test
...
----------------------------------------
[SCORE] ...
----------------------------------------
...
* A DFA is dumped. Please see viewer/index.html
...
```

Then, please open the `viewer/index.html` file in your browser (e.g., Chrome,
Safari, Firefox, etc.) to see the dumped automaton:
<p align="center">
  <img src="./viewer.png" width="500px"/>
</p>

This automata viewer will help you to understand the automata you defined. You
can check whether your automata accept a given word or not by entering the word
in the text box and clicking the `ACCEPT` button (or pressing the `Enter` key).

You can also check each step-by-step transition in the automata by clicking the
`STEP` button after clicking the `START` button. It will highlight the current
possible states. Finally, you can stop the step-by-step execution by clicking
the `STOP` button.

> :warning: You can use the `dump` method for any finite automaton (i.e., DFA,
> NFA, and ENFA).


### Debugger for `dfa2re`

If you want to see the detailed process of the `dfa2re` function, you can use
the `debug` option of the `dfa2re` function. For example, if you want to debug
the `dfa2re` function for the `dfa` automaton, you can use the following code:
```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can see the detailed process of `dfa2re`,
    // please invoke it with `debug = true` option as follows:
    dfa2re(dfa, debug = true)

    ...
  }
  ...
}
```

When you run `sbt test`, the `dfa2re` function will print the intermediate
results after the test cases are executed:

```bash
$ sbt
...
sbt:...> test
...
----------------------------------------
[SCORE] ...
----------------------------------------
...
* Details in the conversion from a DFA to a regular expression:
  - (1, 1, 0) -> <e>|1
  - (1, 2, 0) -> 0
  - (1, 3, 0) -> </>
  - (2, 1, 0) -> 1
  - (2, 2, 0) -> <e>
  - (2, 3, 0) -> 0
  - (3, 1, 0) -> 1
  - (3, 2, 0) -> </>
  - (3, 3, 0) -> <e>|0
  - (1, 1, 1) -> 1*
  ...
...
```


### Short Definition of DFA

You can define a DFA in a short way as follows:
```scala
val dfa: DFA = DFA(3, "01", 181, 6)
```

It is equivalent to the following definition:

```scala
val dfa: DFA = DFA(
  states = Set(0, 1, 2),
  symbols = Set('0', '1'),
  trans = Map(
    (0, '0') -> 1,
    (0, '1') -> 0,
    (1, '0') -> 2,
    (1, '1') -> 0,
    (2, '0') -> 2,
    (2, '1') -> 0,
  ),
  initState = 0,
  finalStates = Set(1, 2),
)
```

Each argument of the `DFA` constructor is explained as follows:
- The first argument is **the number of states** (e.g., `3`).
- The second argument is **the set of symbols** (e.g., `"01"` will be converted
  to `Set('0', '1')` using `toSet` method).
- The third argument is **the transition function** (e.g., The above example has
    three states and two symbols, so six transitions should be defined. And, the
    targets of the transitions are: (1, 0, 2, 0, 2, 0). Then, we can represent
    it as a ternary number `020201 (base 3)` (in opposite order). Finally, it is
    the decimal number `181`).
- The fourth argument is **the set of final states** (e.g., `6` is the binary
    number `110`. Thus, it will be converted to the set `Set(1, 2)`).

> :warning: The initial state is always `1`.


### String Form of Regular Expressions

You can define a regular expression in a string form as follows:

```scala
val re: RE = RE("(a|b)*|</>|<e>c")
```

It is equivalent to the following definition:

```scala
val re: RE = REUnion(
  REUnion(
    REStar(
      REParen(
        REUnion(
          RESymbol('a'),
          RESymbol('b'),
        ),
      ),
    ),
    REEmpty(),
  ),
  REConcat(
    REEpsilon(),
    RESymbol('c'),
  ),
)
```

The string form of the regular expression is defined as follows:
- `</>` represents `REEmpty()`
- `<e>` represents `REEpsilon()`
- `a` represents `RESymbol('a')` (You can use digits or lower case letters as
    symbols: `0` to `9` and `a` to `z`)
- `x|y` represents `REUnion(x, y)`
- `xy` represents `REConcat(x, y)`
- `x*` represents `REStar(x)`
- `(x)` represents `REParen(x)`

You can see the string form of the regular expression using the `dump` method:
```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can see the string form and Scala object of the regular expression
    re.dump

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
* A regular expression is dumped:
  * String form: (a|b)*|</>|<e>c
  * Scala object: REUnion(REUnion(REStar(REParen(REUnion(RESymbol(a),RESymbol(b)))),REEmpty()),REConcat(REEpsilon(),RESymbol(c)))
...
```


