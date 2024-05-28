# Examples of Turing Machines

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/tm-examples.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>tm-examples
└─ src
   ├─ main/scala/kuplrg
   │  ├── TM.scala ────────────── The class of Turing Machines (TM)
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of TMs that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement Turing machine (TM) objects in the
`Implementation.scala` file.**

- [**Turing Machines (TMs)**](#turing-machines-tms)
  - [(Problem #1) `tm_square` (20 points)](#problem-1-tm_square-20-points)
  - [(Problem #2) `tm_fib` (20 points)](#problem-2-tm_fib-20-points)
  - [(Problem #3) `tm_eq_abc` (20 points)](#problem-3-tm_eq_abc-20-points)
  - [(Problem #4) `tm_dec` (20 points)](#problem-4-tm_dec-20-points)
  - [(Problem #5) `tm_add` (20 points)](#problem-5-tm_add-20-points)
- [Appendix](#appendix)
  - [Playground](#playground)
  - [Paths for Words in TMs](#paths-for-words-in-tms)


## Turing Machines (TMs)

A **Turing machine (TM)** is defined as the following `case class`:
```scala
case class TM(
  states: Set[State],
  symbols: Set[Symbol],
  tapeSymbols: Set[TapeSymbol],
  trans: Map[(State, TapeSymbol), (State, TapeSymbol, HeadMove)],
  initState: State,
  blank: TapeSymbol,
  finalStates: Set[State],
) extends Acceptable with Computable
```

For instance, an example TM `tm_an_bn_cn` is defined as follows:
```scala
  val tm_an_bn_cn: TM = TM(
    states = Set(0, 1, 2, 3, 4, 5),
    symbols = Set('a', 'b', 'c'),
    tapeSymbols = Set('a', 'b', 'c', 'X', 'Y', 'Z', 'B'),
    trans = Map(
      (0, 'a') -> (1, 'X', R), (0, 'Y') -> (4, 'Y', R), (0, 'B') -> (5, 'B', L),
      (1, 'a') -> (1, 'a', R), (1, 'Y') -> (1, 'Y', R), (1, 'b') -> (2, 'Y', R),
      (2, 'b') -> (2, 'b', R), (2, 'Z') -> (2, 'Z', R), (2, 'c') -> (3, 'Z', L),
      (3, 'a') -> (3, 'a', L), (3, 'b') -> (3, 'b', L), (3, 'Y') -> (3, 'Y', L),
      (3, 'Z') -> (3, 'Z', L), (3, 'X') -> (0, 'X', R), (4, 'Y') -> (4, 'Y', R),
      (4, 'Z') -> (4, 'Z', R), (4, 'B') -> (5, 'B', L),),
    initState = 0,
    blank = 'B',
    finalStates = Set(5),
  )
```
whose language is equal to the following language:
$${\large
L = \lbrace \texttt{a}^n \texttt{b}^n \texttt{c}^n \mid n \geq 0 \rbrace
}$$

You need to implement the `TM` instances whose languages are equal to the given
languages.

> [!TIP]
>
> You can use the `dumpPath` method to dump the paths for words in the TMs.
> Please refer to the [Paths for Words in TMs](#paths-for-words-in-tms) section
> for more details.



### (Problem #1) `tm_square` (20 points)

Please implement the **TM** `tm_square` whose language is equal to the following
**language**:

$${\large
L = \lbrace \texttt{a}^{n^2} \mid n \geq 0 \rbrace
}$$

For example, the following words are **in the language**:
```plaintext
                         // because 0      =    0^2
a                        // because 1      =    1^2
aaaa                     // because 4      =    2^2
aaaaaaaaa                // because 9      =    3^2
aaaaaaaaaaaaaaaa         // because 16     =    4^2
```
However, the following words are **not in the language**:
```plaintext
aa                       // because 2 is not a square number
aaaaa                    // because 5 is not a square number
aaaaaaaa                 // because 8 is not a square number
aaaaaaaaaaaa             // because 12 is not a square number
aaaaaaaaaaaaaaaaa        // because 17 is not a square number
```



### (Problem #2) `tm_fib` (20 points)

Please implement the **TM** `tm_fib` whose language is equal to the following
**language**:

$${\large
L = \lbrace \texttt{a}^n \mid n \text{ is a Fibonacci number} \rbrace
}$$

A Fibonacci number is defined as follows:
- $F_0 = 0$
- $F_1 = 1$
- $F_n = F_{n-1} + F_{n-2}$ for $n \geq 2$

For example, the following words are **in the language**:
```plaintext
                         // because 0 = F_0
a                        // because 1 = F_2 = F_1
aa                       // because 2 = F_3 = F_2 + F_1 = 1 + 1
aaa                      // because 3 = F_4 = F_3 + F_2 = 2 + 1
aaaaa                    // because 5 = F_5 = F_4 + F_3 = 3 + 2
aaaaaaaa                 // because 8 = F_6 = F_5 + F_4 = 5 + 3
aaaaaaaaaaaaa            // because 13 = F_7 = F_6 + F_5 = 8 + 5
aaaaaaaaaaaaaaaaaaaaaaa  // because 21 = F_8 = F_7 + F_6 = 13 + 8
```
However, the following words are **not in the language**:
```plaintext
aaaa                     // because 4 is not a Fibonacci number
aaaaaa                   // because 6 is not a Fibonacci number
aaaaaaaaa                // because 9 is not a Fibonacci number
aaaaaaaaaaaa             // because 12 is not a Fibonacci number
aaaaaaaaaaaaaaaaa        // because 17 is not a Fibonacci number
```


### (Problem #3) `tm_eq_abc` (20 points)

Please implement the **TM** `tm_eq_abc` whose language is equal to the following
**language**:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b}, \texttt{c} \rbrace^* \mid
N_a(w) = N_b(w) = N_c(w) \rbrace
}$$
where $N_a(w)$, $N_b(w)$, and $N_c(w)$ are the number of $\texttt{a}$,
$\texttt{b}$, and $\texttt{c}$ in $w$, respectively.

For example, the following words are **in the language**:
```plaintext
                         // because N_a(w) = N_b(w) = N_c(w) = 0
bac                      // because N_a(w) = N_b(w) = N_c(w) = 1
bccaba                   // because N_a(w) = N_b(w) = N_c(w) = 2
abcabcabc                // because N_a(w) = N_b(w) = N_c(w) = 3
bcbacbaca                // because N_a(w) = N_b(w) = N_c(w) = 3
cbbacbacbaca             // because N_a(w) = N_b(w) = N_c(w) = 4
```
However, the following words are **not in the language**:
```plaintext
a                        // because N_a(w) = 1   !=   0 = N_b(w)
ab                       // because N_a(w) = 1   !=   0 = N_c(w)
abcbb                    // because N_a(w) = 1   !=   3 = N_b(w)
aabbcab                  // because N_a(w) = 3   !=   1 = N_c(w)
bacbabcabac              // because N_a(w) = 4   !=   3 = N_c(w)
aaabbcccbabcbaacc        // because N_a(w) = 6   !=   5 = N_b(w)
```



### (Problem #4) `tm_dec` (20 points)

Please implement the **TM** `tm_dec` that **computes** the following **partial
function**:

$${\large
f(w \in \lbrace \texttt{0}, \texttt{1} \rbrace^*) = w - 1
\qquad \text{ where } \qquad
w \text{ starts with } \texttt{1}
}$$

For example, the followings are the **expected outputs** for given words:
```plaintext
f(1)                     = 0
f(10)                    = 1
f(1111)                  = 1110
f(1000)                  = 111
f(101000)                = 100111
f(100000000)             = 11111111
```
However, the following words are **not in the domain** of the function:
```plaintext
                         // because w does not start with 1
0                        // because w does not start with 1
010                      // because w does not start with 1
0011                     // because w does not start with 1
0010111                  // because w does not start with 1
000110100111             // because w does not start with 1
```



### (Problem #5) `tm_add` (20 points)

Please implement the **TM** `tm_add` that **computes** the following **partial
function**:

$${\large
f(x \texttt{+} y) = z
\qquad \text{ where } \qquad
x, y \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \text{ start with } \texttt{1}
\text{ and } z = x + y
}$$

For example, the followings are the **expected outputs** for given words:
```plaintext
f(1+1)                   = 10
f(111+1)                 = 1000
f(1+100)                 = 101
f(110+1011)              = 10001
f(10010111+1101011)      = 100000010
f(11010011+100010101)    = 111101000
```
However, the following words are **not in the domain** of the function:
```plaintext
11                       // because no `+` in the word
1+1+1                    // because there are two `+` in the word
+11                      // because ε does not start with 1
0+1                      // because 0 does not start with 1
1+01                     // because 01 does not start with 1
000+111                  // because 000 does not start with 1
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

### Paths for Words in TMs

You can dump the paths for words in the TMs using the `dumpPath` method.

For example, the following code snippet shows the paths for the word `abc` in
the `tm_an_bn_cn` TM:
```scala
tm_an_bn_cn.dumpPath("abc")
```
The output is as follows:
```plaintext
abc
^
0
Xbc
 ^
 1
XYc
  ^
  2
XYZ
 ^
 3
XYZ
^
3
XYZ
 ^
 0
XYZ
  ^
  4
XYZB
   ^
   4
XYZ
  ^
  5
```

A **configuration** for TMs is represented as follows:
- The first line shows the **current tape**.
- The second line shows the **current head position**.
- The third line shows the **current state**.

For example, the following string describes the configuration of the TM:
- The tape is `XYZ`.
- The head position is pointing to the first symbol `X`.
- The current state is `3`.
```plaintext
XYZ
^
3
```
