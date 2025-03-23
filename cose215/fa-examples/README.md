# Examples of Finite Automata

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/fa-examples.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>fa-examples
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
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of FAs that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement the finite automata (FA) objects in
the `Implementation.scala` file.**

- [**Deterministic Finite Automata (DFA) (40 points)**](#deterministic-finite-automata-dfa-40-points)
  - [(Problem #1) `dfa_a_b_plus` (10 points)](#problem-1-dfa_a_b_plus-10-points)
  - [(Problem #2) `dfa_div_3_2` (10 points)](#problem-2-dfa_div_3_2-10-points)
  - [(Problem #3) `dfa_subseq_011` (10 points)](#problem-3-dfa_subseq_011-10-points)
  - [(Problem #4) `dfa_even_0_1` (10 points)](#problem-4-dfa_even_0_1-10-points)
- [**Nondeterministic Finite Automata (NFA) (30 points)**](#nondeterministic-finite-automata-nfa-30-points)
  - [(Problem #5) `nfa_most_three_0` (10 points)](#problem-5-nfa_most_three_0-10-points)
  - [(Problem #6) `nfa_not_substr_bab` (10 points)](#problem-6-nfa_not_substr_bab-10-points)
  - [(Problem #7) `nfa_comb_aaa_bb` (10 points)](#problem-7-nfa_comb_aaa_bb-10-points)
- [**ε-Nondeterministic Finite Automata (ε-NFA) (30 points)**](#ε-nondeterministic-finite-automata-ε-nfa-30-points)
  - [(Problem #8) `enfa_aba_plus` (10 points)](#problem-8-enfa_aba_plus-10-points)
  - [(Problem #9) `enfa_same_digits` (10 points)](#problem-9-enfa_same_digits-10-points)
  - [(Problem #10) `enfa_complex` (10 points)](#problem-10-enfa_complex-10-points)
- [Appendix](#appendix)
  - [Playground](#playground)
  - [Automata Viewer](#automata-viewer)



## Deterministic Finite Automata (DFA) (40 points)

A **deterministic finite automaton (DFA)** is defined as the following `case
class`:
```scala
case class DFA(
  states: Set[State],
  symbols: Set[Symbol],
  trans: Map[(State, Symbol), State],
  initState: State,
  finalStates: Set[State],
) extends FA
```

For instance, an example DFA `dfa_waa` is defined as follows:
```scala
def dfa_waa: DFA = DFA(
  states      = Set(0, 1, 2),
  symbols     = Set('a', 'b'),
  trans       = Map(
    (0, 'a') -> 1,
    (0, 'b') -> 0,
    (1, 'a') -> 2,
    (1, 'b') -> 0,
    (2, 'a') -> 2,
    (2, 'b') -> 0,
  ),
  initState   = 0,
  finalStates = Set(2),
)
```
whose language is:
$${\large
L = \lbrace w \texttt{aa} \mid w \in \lbrace \texttt{a}, \texttt{b} \rbrace^*
\rbrace
}$$

In this part, you need to implement the `DFA` instances whose languages are
equal to the given languages.



### (Problem #1) `dfa_a_b_plus` (10 points)

Please implement the DFA `dfa_a_b_plus` whose language is equal to the following
language:

$${\large
L = \lbrace \texttt{a} \texttt{b}^n \mid n \geq 1 \rbrace
}$$

For example, $\texttt{ab}$, $\texttt{abb}$, and $\texttt{abbbbb}$ are in the
language, but $\texttt{a}$, $\texttt{b}$, $\texttt{ba}$, $\texttt{bb}$,
$\texttt{aaa}$, and $\texttt{abab}$ are not in the language.



### (Problem #2) `dfa_div_3_2` (10 points)

Please implement the DFA `dfa_div_3_2` whose language is equal to the following
language:

$${\large
L = \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid \mathbb{N}(w)
\equiv 2 (\text{mod } 3) \rbrace
}$$

where $\mathbb{N}(w)$ is the natural number represented by $w$ in binary. For
example, $\mathbb{N}(\texttt{101}) = 4 + 1 = 5$ and $\mathbb{N}(\texttt{111}) =
4 + 2 + 1 = 7$. In addition, it allows the leading zeroes, so
$\mathbb{N}(\epsilon) = 0$ and $\mathbb{N}(\texttt{00101}) = 4 + 1 = 5$. Thus,
$\texttt{101}$, $\texttt{10001}$, and $\texttt{01000}$ are in the language, but
$\texttt{11}$, $\texttt{100}$, $\texttt{110}$, and $\texttt{1101}$ are not in
the language.



### (Problem #3) `dfa_subseq_011` (10 points)

Please implement the DFA `dfa_subseq_011` whose language is equal to the
following language:

$${\large
L = \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid \texttt{011}
\text{ is a subsequence of } w \rbrace
}$$

> [!WARNING]
>
> Note that the **SUBSEQUENCE** is not necessarily contiguous unlike the
> **SUBSTRING**. For example, $\texttt{011}$ is a subsequence of
> $\texttt{00101}$ because the first ($\texttt{0}$), third ($\texttt{1}$), and
> fifth ($\texttt{1}$) characters of $\texttt{00101}$ are $\texttt{101}$.

For example, $\texttt{011}$, $\texttt{01001}$, $\texttt{100011}$, and
$\texttt{10100010}$ are in the language, but $\texttt{0}$, $\texttt{1}$,
$\texttt{10}$, $\texttt{100}$, $\texttt{10000}$, $\texttt{00001000}$, and
$\texttt{1111100}$ are not in the language.



### (Problem #4) `dfa_even_0_1` (10 points)

Please implement the DFA `dfa_even_0_1` whose language is equal to the following
language:

$${\large
L = \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid
\textsf{zeros}(w) \equiv 0 (\text{mod } 2) \wedge \textsf{ones}(w) \equiv 0
(\text{mod } 2) \rbrace
}$$

where $\textsf{zeros}(w)$ and $\textsf{ones}(w)$ are the number of $\texttt{0}$
and $\texttt{1}$, respectively. For example, $\textsf{zeros}(\texttt{10101}) =
2$ and $\textsf{ones}(\texttt{10101}) = 3$. Thus, $\texttt{00}$,
$\texttt{11}$, $\texttt{1010}$, $\texttt{1111}$, and $\texttt{110101}$ are in
the language, but $\epsilon$, $\texttt{1}$, $\texttt{0}$, $\texttt{10}$,
$\texttt{01}$, $\texttt{110}$, $\texttt{010}$, $\texttt{0100}$, $\texttt{1110}$,
and $\texttt{101001}$ are not in the language.




## Nondeterministic Finite Automata (NFA) (30 points)

A **nondeterministic finite automaton (NFA)** is defined as the following `case
class`:
```scala
case class NFA(
  states: Set[State],
  symbols: Set[Symbol],
  trans: Map[(State, Symbol), Set[State]],
  initState: State,
  finalStates: Set[State],
) extends FA
```

For instance, an example NFA `nfa_least_two_0` is defined as follows:
```scala
def nfa_least_two_0: NFA = NFA(
  states      = Set(0, 1, 2),
  symbols     = Set('0', '1'),
  trans       = Map(
    (0, '0') -> Set(0, 1),
    (0, '1') -> Set(0),
    (1, '0') -> Set(1, 2),
    (1, '1') -> Set(1),
    (2, '0') -> Set(2),
    (2, '1') -> Set(2),
  ).withDefaultValue(Set()),
  initState   = 0,
  finalStates = Set(2),
)
```
whose language is:
$${\large
L = \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid w \text{
contains at least two } \texttt{0} \text{'s} \rbrace
}$$

In this part, you need to implement the `NFA` instances whose languages are
equal to the given languages.



### (Problem #5) `nfa_most_three_0` (10 points)

Please implement the NFA `nfa_most_three_0` whose language is equal to the
following language:

$${\large
L = \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid w \text{
contains at most three } \texttt{0} \text{'s} \rbrace
}$$

For example, $\epsilon$, $\texttt{11}$, $\texttt{101}$, $\texttt{010}$,
$\texttt{100}$, $\texttt{01010}$, and $\texttt{110110101}$ are in the language,
but $\texttt{0000}$, $\texttt{00010}$, $\texttt{010100}$, $\texttt{00000}$,
$\texttt{01010110}$, and $\texttt{11011011010}$ are not in the language.



### (Problem #6) `nfa_not_substr_bab` (10 points)

Please implement the NFA `nfa_not_substr_bab` whose language is equal to the
following language:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid \texttt{abb}
\text{ is not a substring of } w \rbrace
}$$

For example, $\texttt{aba}$, $\texttt{abab}$, and $\texttt{baaab}$ are in the
language, but $\texttt{abba}$, $\texttt{abb}$, and $\texttt{babba}$ are not
in the language.



### (Problem #7) `nfa_comb_aaa_bb` (10 points)

Please implement the NFA `nfa_comb_aaa_bb` whose language is equal to the
following language:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid w
\text{ is any combination of } \texttt{aaa} \text{ and } \texttt{bb} \rbrace
}$$

For example, $\epsilon$, $\texttt{aaa}$, $\texttt{bb}$, $\texttt{aaabb}$,
and $\texttt{aaabbaaa}$ are in the language, but $\texttt{aba}$,
$\texttt{aaaba}$, $\texttt{bbaab}$, $\texttt{aabb}$, $\texttt{ababa}$,
$\texttt{aaabbaab}$, and $\texttt{bbbbb}$ are not in the language.




## ε-Nondeterministic Finite Automata (ε-NFA) (30 points)

An **ε-nondeterministic finite automaton (ε-NFA)** is defined as the following
`case class`:
```scala
case class ENFA(
  states: Set[State],
  symbols: Set[Symbol],
  trans: Map[(State, Option[Symbol]), Set[State]],
  initState: State,
  finalStates: Set[State],
) extends FA
```

For instance, an example ε-NFA `enfa_ai_bj_ck` is defined as follows:
```scala
def enfa_ai_bj_ck: ENFA = ENFA(
  states      = Set(0, 1, 2),
  symbols     = Set('a', 'b', 'c'),
  trans       = Map(
    (0, None)      -> Set(1),
    (0, Some('a')) -> Set(0),
    (1, None)      -> Set(2),
    (1, Some('b')) -> Set(1),
    (2, Some('c')) -> Set(2),
  ).withDefaultValue(Set()),
  initState   = 0,
  finalStates = Set(2),
)
```
whose language is:
$${\large
L = \lbrace \texttt{a}^i \texttt{b}^j \texttt{c}^k \mid i, j, k \geq 0 \rbrace
}$$

In this part, you need to implement the `ENFA` instances whose languages are
equal to the given languages.




### (Problem #8) `enfa_aba_plus` (10 points)

Please implement the ε-NFA `enfa_aba_plus` whose language is equal to the
following language:

$${\large
L = \lbrace (\texttt{ab})^n \mid n \geq 1 \rbrace
}$$

For example, $\texttt{ab}$, $\texttt{abab}$, and $\texttt{ababab}$ are in the
language, but $\epsilon$, $\texttt{a}$, $\texttt{b}$, and $\texttt{abba}$ are
not.



### (Problem #9) `enfa_same_digits` (10 points)

Please implement the ε-NFA `enfa_same_digits` whose language is equal to the
following language:

$${\large
L = \lbrace \texttt{0}^n \mid n \geq 1 \rbrace \cup \lbrace \texttt{1}^n \mid n
\geq 1 \rbrace
}$$

For example, \texttt{0}$, $\texttt{1}$, $\texttt{000}$, and $\texttt{11111}$ are
in the language, but $\epsilon, $\texttt{01}$, $\texttt{10}$, and $\texttt{011}$
are not.



### (Problem #10) `enfa_complex` (10 points)

Please implement the ε-NFA `enfa_complex` whose language is equal to the
following language:

$${\large
L = (L_1 \cup L_2)^+
}$$
where
$${\large
\begin{array}{l@{}l@{}l}
L_1 &=& \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid
\mathbb{N}(w) \equiv 1 (\text{mod } 3) \rbrace \\
L_2 &=& \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid
\mathbb{N}(w) \equiv 0 (\text{mod } 4) \rbrace \\
\end{array}
}$$

where $\mathbb{N}(w)$ is the natural number represented by $w$ in binary.
For example, $\texttt{100011101100}$ is in the language because
$\texttt{100011101100}$ is the concatenation of $\texttt{1000}$,
$\texttt{111}$, and $\texttt{01100}$, and
$\mathbb{N}(\texttt{1000}) = 8 \equiv 0 (\text{mod } 4)$,
$\mathbb{N}(\texttt{111}) = 4 + 2 + 1 = 7 \equiv 1 (\text{mod } 3)$, and
$\mathbb{N}(\texttt{01100}) = 8 + 4 = 12 \equiv 0 (\text{mod } 4)$.



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


### Automata Viewer

> [!NOTE]
>
> You can skip this section if you are not interested in the automata
> viewer. However, it is **HIGHLY RECOMMENDED** to use the automata viewer to
> check your automata when your implementation cannot pass the test cases.

You can **dump your automata** in HTML format to interactively visualize them in
the web browser.

For example, you can dump the automaton `dfa_waa` to the automata viewer by
invoking the `dump` method of `dfa_waa` in the playground:
```scala
object Implementation extends Template {
  ...
  @main def playground: Unit = {
    ...
    dfa_waa.dump
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

Similarly, you can dump and visualize any other automata (including `DFA`,
`NFA`, and `ENFA`) that you implemented to check how they work.

This automata viewer will help you to understand the automata you defined. You
can check whether your automata accept a given word or not by entering the word
in the text box and clicking the `ACCEPT` button (or pressing the `Enter` key).

You can also check each step-by-step transition in the automata by clicking the
`STEP` button after clicking the `START` button. It will highlight the current
possible states. Finally, you can stop the step-by-step execution by clicking
the `STOP` button.
