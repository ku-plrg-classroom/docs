# Examples of Pushdown Automata

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/pda-examples.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>pda-examples
└─ src
   ├─ main/scala/kuplrg
   │  ├── PDA.scala ───────────── The class of Pushdown Automata (PDA)
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of PDAs that you must implement
   │  ├── basics.scala ────────── The definitions of basic functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

**The goal of this assignment is to implement the pushdown automata (PDA)
objects in the `Implementation.scala` file.**

- [**Pushdown Automata (PDA)**](#pushdown-automata-pda)
  - [(Problem #1) `pda_eq_a_c_empty` (10 points)](#problem-1-pda_eq_a_c_empty-10-points)
  - [(Problem #2) `pda_excess_a_final` (10 points)](#problem-2-pda_excess_a_final-10-points)
  - [(Problem #3) `pda_ab_2c_empty` (10 points)](#problem-3-pda_ab_2c_empty-10-points)
  - [(Problem #4) `pda_hamming_one_final` (15 points)](#problem-4-pda_hamming_one_final-15-points)
  - [(Problem #5) `pda_pal_concat_empty` (15 points)](#problem-5-pda_pal_concat_empty-15-points)
  - [(Problem #6) `pda_block_pal_match_final` (15 points)](#problem-6-pda_block_pal_match_final-15-points)
  - [(Problem #7) `pda_pal_factor_empty` (15 points)](#problem-7-pda_pal_factor_empty-15-points)
  - [(Problem #8) `pda_triple_final` (15 points)](#problem-8-pda_triple_final-15-points)
- [Appendix](#appendix)
  - [Playground](#playground)



## Pushdown Automata (PDA)

A **pushdown automaton (PDA)** is defined as the following `case class`:
```scala
case class PDA(
  states: Set[State],
  symbols: Set[Symbol],
  alphabets: Set[Alphabet],
  trans: Map[(State, Option[Symbol], Alphabet), Set[(State, List[Alphabet])]],
  initState: State,
  initAlphabet: Alphabet,
  finalStates: Set[State],
) extends Acceptable
```

For instance, an example PDA `pda_an_bn_final` is defined as follows:
```scala
val pda_an_bn_final: PDA = PDA(
  states = Set(0, 1, 2),
  symbols = Set('a', 'b'),
  alphabets = Set("X", "Z"),
  trans = Map(
    (0, Some('a'), "Z") -> Set((0, List("X", "Z"))),
    (0, Some('a'), "X") -> Set((0, List("X", "X"))),
    (0, None, "Z") -> Set((1, List("Z"))),
    (0, None, "X") -> Set((1, List("X"))),
    (1, Some('b'), "X") -> Set((1, List())),
    (1, None, "Z") -> Set((2, List("Z"))),
  ).withDefaultValue(Set()),
  initState = 0,
  initAlphabet = "Z",
  finalStates = Set(2),
)
```
whose language accepted by **final states** is equal to the following language:
$${\large
L = \lbrace \texttt{a}^n \texttt{b}^n \mid n \geq 0 \rbrace
}$$

You need to implement the `PDA` instances whose languages are equal to the given
languages.



### (Problem #1) `pda_eq_a_c_empty` (10 points)

Please implement the PDA `pda_eq_a_c_empty` whose language accepted by **empty
stacks** is equal to the following language:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b}, \texttt{c} \rbrace^* \mid
N_a(w) = N_c(w) \rbrace
}$$

where $N_a(w)$ and $N_c(w)$ are the number of $\texttt{a}$ and $\texttt{c}$ in
$w$, respectively. The number of $\texttt{b}$'s is unconstrained.

For example, the following words are **in the language**:
```plaintext
                // because N_a(w) = 0   =   0 = N_c(w)
ac              // because N_a(w) = 1   =   1 = N_c(w)
ca              // because N_a(w) = 1   =   1 = N_c(w)
abc             // because N_a(w) = 1   =   1 = N_c(w)
bbb             // because N_a(w) = 0   =   0 = N_c(w)
acca            // because N_a(w) = 2   =   2 = N_c(w)
abcbabc         // because N_a(w) = 2   =   2 = N_c(w)
```
However, the following words are **not in the language**:
```plaintext
a               // because N_a(w) = 1   !=  0 = N_c(w)
c               // because N_a(w) = 0   !=  1 = N_c(w)
ab              // because N_a(w) = 1   !=  0 = N_c(w)
aac             // because N_a(w) = 2   !=  1 = N_c(w)
abca            // because N_a(w) = 2   !=  1 = N_c(w)
ccab            // because N_a(w) = 1   !=  2 = N_c(w)
```



### (Problem #2) `pda_excess_a_final` (10 points)

Please implement the PDA `pda_excess_a_final` whose language accepted by
**final states** is equal to the following language:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid N_a(w) \geq
N_b(w) + 2 \rbrace
}$$

That is, the count of $\texttt{a}$'s exceeds the count of $\texttt{b}$'s by at
least two.

For example, the following words are **in the language**:
```plaintext
aa              // because N_a(w) = 2   =   0 + 2 = N_b(w) + 2
aaa             // because N_a(w) = 3   >   0 + 2 = N_b(w) + 2
aaab            // because N_a(w) = 3   =   1 + 2 = N_b(w) + 2
aabaa           // because N_a(w) = 4   >   1 + 2 = N_b(w) + 2
baaaa           // because N_a(w) = 4   >   1 + 2 = N_b(w) + 2
abaaba          // because N_a(w) = 4   =   2 + 2 = N_b(w) + 2
```
However, the following words are **not in the language**:
```plaintext
                // because N_a(w) = 0   <   0 + 2 = N_b(w) + 2
a               // because N_a(w) = 1   <   0 + 2 = N_b(w) + 2
b               // because N_a(w) = 0   <   1 + 2 = N_b(w) + 2
ab              // because N_a(w) = 1   <   1 + 2 = N_b(w) + 2
aab             // because N_a(w) = 2   <   1 + 2 = N_b(w) + 2
abab            // because N_a(w) = 2   <   2 + 2 = N_b(w) + 2
aabb            // because N_a(w) = 2   <   2 + 2 = N_b(w) + 2
```



### (Problem #3) `pda_ab_2c_empty` (10 points)

Please implement the PDA `pda_ab_2c_empty` whose language accepted by **empty
stacks** is equal to the following language:

$${\large
L = \lbrace \texttt{a}^i \texttt{b}^j \texttt{c}^k \mid i, j, k \geq 0 \land i +
j = 2k \rbrace
}$$

That is, $w$ is in the form $\texttt{a}^i \texttt{b}^j \texttt{c}^k$ where the
total count of $\texttt{a}$'s and $\texttt{b}$'s equals twice the count of
$\texttt{c}$'s.

For example, the following words are **in the language**:
```plaintext
                // because i = j = k = 0   and   0 + 0 = 0 = 2*0
aac             // because i = 2, j = 0, k = 1   and   2 + 0 = 2 = 2*1
abc             // because i = 1, j = 1, k = 1   and   1 + 1 = 2 = 2*1
bbc             // because i = 0, j = 2, k = 1   and   0 + 2 = 2 = 2*1
aabbcc          // because i = 2, j = 2, k = 2   and   2 + 2 = 4 = 2*2
aaaacc          // because i = 4, j = 0, k = 2   and   4 + 0 = 4 = 2*2
abbbcc          // because i = 1, j = 3, k = 2   and   1 + 3 = 4 = 2*2
```
However, the following words are **not in the language**:
```plaintext
ac              // because i + j = 1   !=  2 = 2k
acc             // because i + j = 1   !=  4 = 2k
abcc            // because i + j = 2   !=  4 = 2k
aabc            // because i + j = 3   !=  2 = 2k
ca              // because not in the form of a^i b^j c^k
cab             // because not in the form of a^i b^j c^k
```



### (Problem #4) `pda_hamming_one_final` (15 points)

Please implement the PDA `pda_hamming_one_final` whose language accepted by
**final states** is equal to the following language:

$${\large
L = \lbrace x \texttt{\\\#} y \mid x, y \in \lbrace \texttt{a}, \texttt{b}
\rbrace^* \land |x| = |y| \land \mathrm{Hamming}(y, x^R) = 1 \rbrace
}$$

where $w^R$ is the reverse of $w$ and $\mathrm{Hamming}(u, v)$ is the number of
positions where $u$ and $v$ differ. That is, $y$ has the same length as $x$ and
differs from $x^R$ at **exactly one** position.

For example, the following words are **in the language**:
```plaintext
a#b             // because x^R = a,    y = b,    1 mismatch
b#a             // because x^R = b,    y = a,    1 mismatch
aa#ab           // because x^R = aa,   y = ab,   1 mismatch at position 2
aa#ba           // because x^R = aa,   y = ba,   1 mismatch at position 1
ab#aa           // because x^R = ba,   y = aa,   1 mismatch at position 1
ab#bb           // because x^R = ba,   y = bb,   1 mismatch at position 2
aab#aaa         // because x^R = baa,  y = aaa,  1 mismatch at position 1
aab#bba         // because x^R = baa,  y = bba,  1 mismatch at position 3
```
However, the following words are **not in the language**:
```plaintext
                // because no separator
#               // because |x| = |y| = 0   and   0 mismatches
a#a             // because 0 mismatches (palindrome with separator)
ab#ba           // because 0 mismatches
aab#aab         // because x^R = baa,  y = aab,  2 mismatches
aab#bab         // because x^R = baa,  y = bab,  2 mismatches
a#ab            // because |x| = 1   !=  2 = |y|
ab#a            // because |x| = 2   !=  1 = |y|
a##a            // because more than one '#'
```



### (Problem #5) `pda_pal_concat_empty` (15 points)

Please implement the PDA `pda_pal_concat_empty` whose language accepted by
**empty stacks** is equal to the following language:

$${\large
L = \lbrace u u^R v v^R \mid u, v \in \lbrace \texttt{a}, \texttt{b} \rbrace^+
\rbrace
}$$

That is, $w$ is the concatenation of two non-empty even-length palindromes.

For example, the following words are **in the language**:
```plaintext
aaaa            // because u = a,    uu^R = aa     and  v = a,    vv^R = aa
aabb            // because u = a,    uu^R = aa     and  v = b,    vv^R = bb
bbaa            // because u = b,    uu^R = bb     and  v = a,    vv^R = aa
abbaaa          // because u = ab,   uu^R = abba   and  v = a,    vv^R = aa
aaabba          // because u = a,    uu^R = aa     and  v = ab,   vv^R = abba
abbaabba        // because u = ab,   uu^R = abba   and  v = ab,   vv^R = abba
```
However, the following words are **not in the language**:
```plaintext
                // because length must be at least 4
aa              // because |v| = 0
abba            // because single segment, no second non-empty segment
ab              // because halves are not even palindromes
abab            // because halves "ab" and "ab" are not palindromes
aaa             // because length is odd
aabba           // because length is odd
```



### (Problem #6) `pda_block_pal_match_final` (15 points)

Please implement the PDA `pda_block_pal_match_final` whose language accepted by
**final states** is equal to the following language:

$${\large
L = \lbrace x_1 \texttt{\\\#} x_2 \texttt{\\\#} \cdots \texttt{\\\#} x_n
\texttt{\\\#} \mid n \geq 2 \land \exists i \neq j.\ x_i = x_j^R \rbrace
}$$

where each $x_k \in \lbrace \texttt{a}, \texttt{b} \rbrace^*$. That is, the
input is a sequence of segments separated by `#` and ending with `#`, with at
least two segments, and there exists a pair of distinct segments such that one
is the reverse of the other.

For example, the following words are **in the language**:
```plaintext
##              // because x_1 = x_2 = ε   and   ε = ε^R
a#a#            // because x_1 = x_2 = a   and   a = a^R
ab#ba#          // because x_1 = ab,   x_2 = ba   and   ab = ba^R
ab#aa#ba#       // because x_1 = ab,   x_3 = ba   and   ab = ba^R
abb#aab#bba#    // because x_1 = abb,  x_3 = bba  and   abb = bba^R
ab#ab#ba#       // because x_2 = ab,   x_3 = ba   and   ab = ba^R
```
However, the following words are **not in the language**:
```plaintext
                // because empty input
#               // because n = 1
a               // because no '#'
a#              // because n = 1
ab              // because no '#'
ab#ab#          // because x_1 = x_2 = ab   and   ab != ba = ab^R
##a             // because does not end with '#'
ab#ba           // because does not end with '#'
```



### (Problem #7) `pda_pal_factor_empty` (15 points)

Please implement the PDA `pda_pal_factor_empty` whose language accepted by
**empty stacks** is equal to the following language:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid w = p_1 p_2
\cdots p_n \text{ for some } n \geq 0 \text{ and each } p_i \text{ is a
palindrome with } |p_i| \geq 2 \rbrace
}$$

That is, $w$ can be written as a concatenation of zero or more palindromes,
each of length at least two. The empty word is in the language with $n = 0$.

For example, the following words are **in the language**:
```plaintext
                // because n = 0 (empty factorization)
aa              // because single palindrome aa of length 2
bb              // because single palindrome bb of length 2
aba             // because single palindrome aba of length 3
abba            // because single palindrome abba of length 4
aabb            // because aa + bb (two palindromes)
aabaa           // because single palindrome aabaa of length 5
aaaa            // because single palindrome OR aa + aa
ababbaba        // because single palindrome ababbaba of length 8
```
However, the following words are **not in the language**:
```plaintext
a               // because length < 2
b               // because length < 2
ab              // because length 2 but not a palindrome
ba              // because not a palindrome
aab             // because no valid factorization
abb             // because no valid factorization
abab            // because no valid factorization
aabba           // because no valid factorization
```



### (Problem #8) `pda_triple_final` (15 points)

Please implement the PDA `pda_triple_final` whose language accepted by **final
states** is equal to the following language:

$${\large
L = \lbrace x \texttt{\\\$} y \mid x, y \in \lbrace \texttt{0}, \texttt{1}
\rbrace^* \land \mathbb{N}(y^R) = 3 \cdot \mathbb{N}(x) \rbrace
}$$

where $w^R$ is the reverse of $w$, and $\mathbb{N}(w)$ is the natural number
represented by $w$ in binary (MSB-first). For example,
$\mathbb{N}(\texttt{101}) = 4 + 1 = 5$ and $\mathbb{N}(\texttt{0}) = 0$. It
allows the leading zeroes, so $\mathbb{N}(\epsilon) = 0$ and
$\mathbb{N}(\texttt{0011}) = 2 + 1 = 3$.

For example, the following words are **in the language**:
```plaintext
$               // because 3 * 0 = 0
0$              // because 3 * 0 = 0
$0              // because 3 * 0 = 0
0$0             // because 3 * 0 = 0
1$11            // because 3 * 1 = 3   =   N(11^R) = N(11)
10$011          // because 3 * 2 = 6   =   N(011^R) = N(110)
11$1001         // because 3 * 3 = 9   =   N(1001^R) = N(1001)
100$0011        // because 3 * 4 = 12  =   N(0011^R) = N(1100)
```
However, the following words are **not in the language**:
```plaintext
                // because no '$'
1               // because no '$'
1$              // because 3 * 1 = 3   !=  0 = N(ε)
1$1             // because 3 * 1 = 3   !=  1 = N(1^R)
10$10           // because 3 * 2 = 6   !=  1 = N(10^R) = N(01)
11$11           // because 3 * 3 = 9   !=  3 = N(11^R)
$1              // because 3 * 0 = 0   !=  1 = N(1^R)
1$$1            // because more than one '$'
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
