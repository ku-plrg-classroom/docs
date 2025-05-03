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
  - [(Problem #1) `pda_even_pal_final` (10 points)](#problem-1-pda_even_pal_final-10-points)
  - [(Problem #2) `pda_more_bs_empty` (10 points)](#problem-2-pda_more_bs_empty-10-points)
  - [(Problem #3) `pda_abc_ij_jk_final` (10 points)](#problem-3-pda_abc_ij_jk_final-10-points)
  - [(Problem #4) `pda_a2n1_b3n2_empty` (10 points)](#problem-4-pda_a2n1_b3n2_empty-10-points)
  - [(Problem #5) `pda_abc_j_i2k_final` (15 points)](#problem-5-pda_abc_j_i2k_final-15-points)
  - [(Problem #6) `pda_ord_brace_empty` (15 points)](#problem-6-pda_ord_brace_empty-15-points)
  - [(Problem #7) `pda_eq_pair_final` (15 points)](#problem-7-pda_eq_pair_final-15-points)
  - [(Problem #8) `pda_inc_empty` (15 points)](#problem-8-pda_inc_empty-15-points)
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

> [!CAUTION]
>
> While there is no restriction on the epsilon transitions in the original
> definition of the PDA, we impose a restriction on the epsilon transitions in
> this assignment for grading purposes.
>
> You **CANNOT** define any **epsilon transitions** that **increase** the stack
> size in the `trans` field.
>
> For example, you **CANNOT** define the following epsilon transition:
> ```scala
> (0, None, "Z") -> Set((0, List("X", "Z")))
> ```
> because it **increases** the stack size by pushing the alphabet `X` into the
> stack.
>
> However, you **CAN** define the following epsilon transitions:
> ```scala
> (0, None, "X") -> Set((0, List("Z")))   // preserve the stack size
> (1, None, "Z") -> Set((1, List("X")))   // preserve the stack size
> (2, None, "Z") -> Set((2, List()))      // decrease the stack size
> ```
> because they **preserve** or **decrease** the stack size.



### (Problem #1) `pda_even_pal_final` (10 points)

Please implement the PDA `pda_even_pal_final` whose language accepted by **final
states** is equal to the following language:

$${\large
L = \lbrace w \in \lbrace \texttt{0}, \texttt{1} \rbrace^* \mid w = w^R \land
|w| \text{ is even} \rbrace
}$$

where $w^R$ is the reverse of $w$.

For example, the following words are **in the language**:
```plaintext
                // because ε           =    ε^R
00              // because 00          =    (00)^R
0110            // because 0110        =    (0110)^R
010010          // because 010010      =    (010010)^R
00111100        // because 00111100    =    (00111100)^R
0100110010      // because 0100110010  =    (0100110010)^R
```
However, the following words are **not in the language**:
```plaintext
1               // because |w| is odd
01              // because 01          !=   (01)^R
0101            // because 0101        !=   (0101)^R
001101          // because 001101      !=   (001101)^R
11100110        // because 11100110    !=   (11100110)^R
1001111101      // because 1001111101  !=   (1001111101)^R
```



### (Problem #2) `pda_more_bs_empty` (10 points)

Please implement the PDA `pdan_more_bs_empty` whose language accepted by **empty
stacks** is equal to the following language:

$${\large
L = \lbrace w \in \lbrace \texttt{a}, \texttt{b} \rbrace^* \mid N_a(w) \leq
N_b(w) \rbrace
}$$

where $N_a(w)$ and $N_b(w)$ are the number of $\texttt{a}$ and $\texttt{b}$ in
$w$, respectively.

For example, the following words are **in the language**:
```plaintext
b               // because N_a(w) = 0   <   1 = N_b(w)
ab              // because N_a(w) = 1   =   1 = N_b(w)
bab             // because N_a(w) = 1   <   2 = N_b(w)
aabb            // because N_a(w) = 2   =   2 = N_b(w)
abaabbb         // because N_a(w) = 3   <   4 = N_b(w)
```
However, the following words are **not in the language**:
```plaintext
a               // because N_a(w) = 1   >   0 = N_b(w)
aab             // because N_a(w) = 2   >   1 = N_b(w)
aba             // because N_a(w) = 2   >   1 = N_b(w)
abaaba          // because N_a(w) = 4   >   2 = N_b(w)
abaaababb       // because N_a(w) = 5   >   4 = N_b(w)
```


### (Problem #3) `pda_abc_ij_jk_final` (10 points)

Please implement the PDA `pda_abc_ij_jk_final` whose language accepted by
**final states** is equal to the following language:

$${\large
L = \lbrace \texttt{a}^i \texttt{b}^j \texttt{c}^k \mid i, j, k \geq 0 \land (i
= j \lor j = k) \rbrace
}$$

For example, the following words are **in the language**:
```plaintext
                // because i = j = 0
a               // because j = k = 0
bc              // because j = k = 1
aabb            // because i = j = 2
abcccc          // because i = j = 1
abbbccc         // because j = k = 3
aaaabbcc        // because j = k = 2
```
However, the following words are **not in the language**:
```plaintext
b               // because i = 0 != 1 = j and j = 1 != 0 = k
ba              // because not in the form of a^i b^j c^k
ac              // because i = 1 != 0 = j and j = 0 != 1 = k
bb              // because i = 0 != 2 = j and j = 2 != 0 = k
cbab            // because not in the form of a^i b^j c^k
bcc             // because i = 0 != 1 = j and j = 1 != 2 = k
abbbcc          // because i = 1 != 3 = j and j = 3 != 2 = k
```



### (Problem #4) `pda_a2n1_b3n2_empty` (10 points)

Please implement the PDA `pda_a2n1_b3n2_empty` whose language accepted by
**empty stacks** is equal to the following language:

$${\large
L = \lbrace \texttt{a}^i \texttt{b}^j \mid (i = 2n+1 \land j = 3n+2)
\text{ for some } n \geq 0 \rbrace
}$$

For example, the following words are **in the language**:
```plaintext
abb                // because i = 1 = 2*0+1 and j = 1 = 3*0+2 and n = 0
aaabbbbb           // because i = 3 = 2*1+1 and j = 4 = 3*1+2 and n = 1
aaaaabbbbbbbb      // because i = 5 = 2*2+1 and j = 7 = 3*2+2 and n = 2
aaaaaaabbbbbbbbbbb // because i = 7 = 2*3+1 and j = 10 = 3*3+2 and n = 3
```
However, the following words are **not in the language**:
```plaintext
                // because i = 0 = 2*0+0
a               // because j = 0 = 3*0+0
ba              // because not in the form of a^i b^j
aa              // because i = 2 = 2*1+0
ab              // because j = 1 = 3*0+1
aaab            // because i = 3 = 2*1+1 but j = 1 = 3*0+1
baba            // because not in the form of a^i b^j
aaabbb          // because i = 3 = 2*1+1 but j = 3 = 3*1+0
aaaaabbbb       // because i = 5 = 2*2+1 but j = 4 = 3*1+1
```



### (Problem #5) `pda_abc_j_i2k_final` (15 points)

Please implement the PDA `pda_abc_j_i2k_final` whose language accepted by
**final states** is equal to the following language:

$${\large
L = \lbrace \texttt{a}^i \texttt{b}^j \texttt{c}^k \mid i, j, k \geq 0 \land j =
i + 2k \rbrace
}$$

For example, the following words are **in the language**:
```plaintext
                // because j = 0   =   0 = 0 + 2*0 = i + 2k
ab              // because j = 1   =   1 = 1 + 2*0 = i + 2k
bbc             // because j = 2   =   2 = 0 + 2*1 = i + 2k
abbbc           // because j = 3   =   3 = 1 + 2*1 = i + 2k
aabbbbc         // because j = 4   =   4 = 2 + 2*1 = i + 2k
aabbbbbbcc      // because j = 6   =   6 = 2 + 2*2 = i + 2k
```
However, the following words are **not in the language**:
```plaintext
a               // because j = 0   !=   1 = 0 + 2*0 = i + 2k
b               // because j = 1   !=   0 = 0 + 2*0 = i + 2k
cba             // because not in the form of a^i b^j c^k
abbb            // because j = 3   !=   1 = 1 + 2*0 = i + 2k
abbbcc          // because j = 3   !=   5 = 1 + 2*2 = i + 2k
aabbbcc         // because j = 3   !=   6 = 2 + 2*2 = i + 2k
```



### (Problem #6) `pda_ord_brace_empty` (15 points)

Please implement the PDA `pda_ord_brace_empty` whose language accepted by
**empty stacks** is equal to the following language:

$${\large
L = \lbrace w \in \lbrace \texttt{(}, \texttt{)}, \texttt{\\\{}, \texttt{\\\}},
\texttt{[}, \texttt{]} \rbrace^* \mid w \text{ is well-formed and satisfies
 the order: } \texttt{()} < \texttt{\\\{\\\}} < \texttt{[]} \rbrace
}$$
It means that $\texttt{[]}$ cannot be inside $\texttt{()}$ or $\texttt{\\\{\\\}}$,
and $\texttt{\\\{\\\}}$ cannot be inside $\texttt{()}$.

For example, the following words are **in the language**:
```plaintext
                // because well-formed and ordered
()              // because well-formed and ordered
(){}[]          // because well-formed and ordered
(()())          // because well-formed and ordered
[{}{{()}}]      // because well-formed and ordered
[(){}]{}()      // because well-formed and ordered
```
However, the following words are **not in the language**:
```plaintext
(               // because ill-formed
}{              // because ill-formed
([])            // because well-formed but not ordered
({}{})          // because well-formed but not ordered
{[()()]}        // because well-formed but not ordered
{[()]}{}()      // because well-formed but not ordered
```


### (Problem #7) `pda_eq_pair_final` (15 points)

Please implement the PDA `pda_eq_pair_final` whose language accepted by **final
states** is equal to the following language:

$${\large
L = \lbrace a_1 a_2 \cdots a_{2n} \in \lbrace \texttt{a}, \texttt{b} \rbrace^*
\mid n \geq 1 \land a_i = a_{n+i} \text{ for some } 1 \leq i \leq n \rbrace
}$$

For example, the following words are **in the language**:
```plaintext
aa              // because n = 1 and a_1 = a_2 = a
abaa            // because n = 2 and a_1 = a_3 = a
bbbaba          // because n = 3 and a_2 = a_5 = b
abbabbbb        // because n = 4 and a_2 = a_6 = a
aaaaaaabaa      // because n = 5 and a_3 = a_8 = a
```
However, the following words are **not in the language**:
```plaintext
a               // because |w| is odd
ab              // because n = 1 and a_i != a_{n+i} for all i
abba            // because n = 2 and a_i != a_{n+i} for all i
aabbba          // because n = 3 and a_i != a_{n+i} for all i
abbabaab        // because n = 4 and a_i != a_{n+i} for all i
ababababab      // because n = 5 and a_i != a_{n+i} for all i
```


### (Problem #8) `pda_inc_empty` (15 points)

Please implement the PDA `pda_inc_empty` whose language accepted by **empty
stacks** is equal to the following language:

$${\large
L = \lbrace x\texttt{\\\$}y \mid x, y \in \lbrace 0, 1 \rbrace^* \text{ and }
\mathbb{N}(x) + 1 = \mathbb{N}(y^R) \rbrace
}$$
where $w^R$ is the reverse of $w$, and $\mathbb{N}(w)$ is the natural number
represented by $w$ in binary. For example, $\mathbb{N}(\texttt{101}) = 4 + 1 =
5$ and $\mathbb{N}(\texttt{111}) = 4 + 2 + 1 = 7$. In addition, it allows the
leading zeroes, so $\mathbb{N}(\epsilon) = 0$ and $\mathbb{N}(\texttt{00101}) =
4 + 1 = 5$.

For example, the following words are **in the language**:
```plaintext
$1              // because 0 + 1 = 1
000$1000        // because 0 + 1 = 1
11$001          // because 3 + 1 = 4
1100$1011       // because 12 + 1 = 13
1000$1001       // because 8 + 1 = 9
1111$00001      // because 15 + 1 = 16
010011$00101000 // because 19 + 1 = 20
```
However, the following words are **not in the language**:
```plaintext
0$0$1           // because invalid format
11$01           // because 3 + 1 != 2
000$0           // because 0 + 1 != 0
000$0000        // because 0 + 1 != 0
010101$1111000  // because 21 + 1 != 15
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
