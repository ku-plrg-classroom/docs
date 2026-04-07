# Examples of Context-Free Grammars (CFGs)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/cfg-examples.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>cfg-examples
└─ src
   ├─ main/scala/kuplrg
   │  ├── CFG.scala ───────────── The base class of context-free grammars (CFGs)
   │  ├── Parser.scala ────────── The definition of the `Parser` object
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of CFGs that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement the context-free grammar (CFG)
instances whose languages are equal to the given languages. You need to
implement the CFG instances in the `Implementation.scala` file.**

- [**Context-Free Grammars (CFGs)**](#context-free-grammars-cfgs)
  - [(Problem #1) `cfg_balanced`](#problem-1-cfg_balanced-10-points)
  - [(Problem #2) `cfg_an_b2n`](#problem-2-cfg_an_b2n-10-points)
  - [(Problem #3) `cfg_an_bm_between`](#problem-3-cfg_an_bm_between-10-points)
  - [(Problem #4) `cfg_an_bm_neq`](#problem-4-cfg_an_bm_neq-10-points)
  - [(Problem #5) `cfg_ai_bj_ak`](#problem-5-cfg_ai_bj_ak-10-points)
  - [(Problem #6) `cfg_more_bs`](#problem-6-cfg_more_bs-10-points)
  - [(Problem #7) `cfg_prefix_neq`](#problem-7-cfg_prefix_neq-10-points)
  - [(Problem #8) `cfg_between`](#problem-8-cfg_between-10-points)
  - [(Problem #9) `cfg_neq_reverse`](#problem-9-cfg_neq_reverse-10-points)
  - [(Problem #10) `cfg_not_ww`](#problem-10-cfg_not_ww-10-points)
- [Appendix](#appendix)
  - [Playground](#playground)



## Context-Free Grammars (CFGs)

A **context-free grammar (CFG)** is defined as the following `case class`:
```scala
case class CFG(
  nts: Set[Nt],
  symbols: Set[Symbol],
  start: Nt,
  rules: Map[Nt, List[Rhs]],
) extends Acceptable:
```
with the following components:
```scala
/** The type definition of nonterminal symbol */
type Nt = String

/** The definition of right-hand side of a production rule */
case class Rhs(seq: List[Nt | Symbol])
```

You can create a CFG instance by using the following notation:
- **Nonterminal symbols** are represented by one-or-more uppercase letters
  (e.g., `S`, `NAME`).
- **Terminal symbols** are represented by one of the following characters
  enclosed by single quotes:
  - Any numeric digit (e.g., `'0'`, `'1'`, `'2'`)
  - Any lowercase letter (e.g., `'a'`, `'b'`, `'c'`)
  - Other characters: `'+'`, `'-'`, `'{'`, `'}'`, `'('`, `')'`
- `A -> ... | ...` represents a **production rule** with the left-hand side `A`
  and multiple right-hand sides separated by `|`.
- `<e>` represents the **empty right-hand side**.
- You must add a semicolon `;` to represent the **end of a production rule.

For instance, an example CFG `cfg_an_bn` is defined as follows:
```scala
def cfg_an_bn: CFG = CFG("""
  S -> A | <e> ;
  A -> 'a' S 'b' ;
""")
```
whose language is equal to the following language:

$${\large
L = \lbrace \texttt{a}^n \texttt{b}^n \mid n \geq 0 \rbrace
}$$

You need to implement the `CFG` instances whose languages are equal to the given
languages.



### (Problem #1) `cfg_balanced` (10 points)

Please implement the CFG `cfg_balanced` that accepts the language of balanced
parentheses strings:

$${\large
L = \lbrace w \in \lbrace \texttt{(}, \texttt{)} \rbrace^* \mid w \text{ is a
balanced parentheses string} \rbrace
}$$


### (Problem #2) `cfg_an_b2n` (10 points)

Please implement the CFG `cfg_an_b2n` that accepts the language of words
consisting of $n$ `a`s followed by $2n$ `b`s:

$${\large
L = \lbrace \texttt{a}^n \texttt{b}^{2n} \mid n \geq 0 \rbrace
}$$


### (Problem #3) `cfg_an_bm_between` (10 points)

Please implement the CFG `cfg_an_bm_between` that accepts the language of
words consisting of $n$ `a`s followed by $m$ `b`s, where $m$ is between $n$
and $2n$:

$${\large
L = \lbrace \texttt{a}^n \texttt{b}^m \mid n \leq m \leq 2n \rbrace
}$$


### (Problem #4) `cfg_an_bm_neq` (10 points)

Please implement the CFG `cfg_an_bm_neq` that accepts the language of words
consisting of $n$ `a`s followed by $m$ `b`s, where $n$ and $m$ are different:

$${\large
L = \lbrace \texttt{a}^n \texttt{b}^m \mid n \neq m \rbrace
}$$


### (Problem #5) `cfg_ai_bj_ak` (10 points)

Please implement the CFG `cfg_ai_bj_ak` that accepts the language of words
consisting of $i$ `a`s followed by $j$ `b`s followed by $k$ `a`s, where $i + k =
j$:

$${\large
L = \lbrace \texttt{a}^i \texttt{b}^j \texttt{a}^k \mid i + k = j \rbrace
}$$


### (Problem #6) `cfg_more_bs` (10 points)

Please implement the CFG `cfg_more_bs` that accepts the language of words over
the alphabet $\lbrace \texttt{a}, \texttt{b} \rbrace$ such that the number of
`a`s is less than or equal to the number of `b`s:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid N_a(w) \leq
N_b(w) \rbrace
}$$

where $N_a(w)$ and $N_b(w)$ are the number of `a`s and `b`s in $w$,
respectively.


### (Problem #7) `cfg_prefix_neq` (10 points)

Please implement the CFG `cfg_prefix_neq` that accepts the language of words
over the alphabet $\lbrace \texttt{a}, \texttt{b} \rbrace$ such that all
non-empty prefixes of the word have different numbers of `a`s and `b`s:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid \forall
\text{non-empty prefix } u \text{ of } w, N_a(u) \neq N_b(u) \rbrace
}$$

where $N_a(u)$ and $N_b(u)$ are the number of `a`s and `b`s in $u$,
respectively.

> [!NOTE]
>
> The empty word $\epsilon$ is included in the language since it has no
> non-empty prefix.


### (Problem #8) `cfg_between` (10 points)

Please implement the CFG `cfg_between` that accepts the language of words over
the alphabet $\lbrace \texttt{a}, \texttt{b} \rbrace$ such that the number of
`b`s is between the number of `a`s and twice the number of `a`s:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid N_a(w) \leq
N_b(w) \leq 2N_a(w) \rbrace
}$$

where $N_a(w)$ and $N_b(w)$ are the number of `a`s and `b`s in $w$,
respectively.


### (Problem #9) `cfg_neq_reverse` (10 points)

Please implement the CFG `cfg_neq_reverse` that accepts the language of words
over the alphabet $\lbrace \texttt{a}, \texttt{b} \rbrace$ such that the word
is not equal to its reverse:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid w \neq w^R
\rbrace
}$$

where $w^R$ is the reverse of $w$.


### (Problem #10) `cfg_not_ww` (10 points)

Please implement the CFG `cfg_not_ww` that accepts the language of words over
the alphabet $\lbrace \texttt{a}, \texttt{b} \rbrace$ such that the word is
not of the form $ww$ for some word $w$:

$${\large
L = \lbrace w w \mid w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \rbrace^c
}$$

where $L^c$ is the complement of $L$.


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
