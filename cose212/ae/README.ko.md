# `AE` - Arithmetic Expressions

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/ae.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
```
fa-examples
└─ src
   ├─ main/scala/kuplrg
   │  ├── AE.scala ────────────── AE의 정의와 파서들
   │  ├── Implementation.scala ── [[ 이 파일을 수정하고 제출하세요. ]]
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── [[ 이 파일에 테스트 케이스를 추가하세요. ]]
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능
```

`AE` 언어는 정수의 덧셈과 곱셈을 지원하는 간단한 산술 표현식 언어입니다. 이
과제에서는 `eval`과 `countNums` 두 함수를 구현합니다.

## (문제 #1) `eval` (50 점)

`eval` 함수는 주어진 표현식을 계산하여 결과를 반환합니다:
```scala
def interp(expr: Expr): Value = ???
```
`Implementation.scala` 파일에 `eval` 함수를 구현하세요.

## (문제 #2) `countNums` (50 점)

`countNums` 함수는 주어진 표현식에 포함된 `Num` 노드의 개수를 세어 반환합니다:
```scala
def countNums(expr: Expr): Int = ???
```
`Implementation.scala` 파일에 `countNums` 함수를 구현하세요.

## `AE`의 정의

<details>
<summary markdown="span"><b>더 보기</b></summary>

### 구체적 문법 (Concrete Syntax)

```bnf
<expr>   ::= <number>
           | <expr> "+" <expr>
           | <expr> "*" <expr>
           | "(" <expr> ")"

<digit>  ::= "0" | "1" | ... | "9"
<nat>    ::= <digit> | <digit> <nat>
<number> ::= <nat> | "-" <nat>
```

| 연산자 | 결합 방향 (Associativity) | 우선 순위 (Precedence) |
|:------:|:-------------------------:|:----------------------:|
| `*`    | 왼쪽 (Left)               | 1                      |
| `+`    | 왼쪽 (Left)               | 2                      |

### 요약 문법 (Abstract Syntax)
$$
\newcommand{\expr}{e}
\newcommand{\num}{n}
\newcommand{\code}[1]{\texttt{#1}}
\newcommand{\eval}[2]{\vdash {#1} \Rightarrow {#2}}

\begin{array}{lcll}
\expr
&\code{::=}& \num & (\code{Num}) \\
&\mid& \expr \; \code{+} \; \expr & (\code{Add}) \\
&\mid& \expr \; \code{*} \; \expr & (\code{Mul}) \\
\end{array}
$$

### 큰 걸음 동작 의미 (Big-Step Operational Semantics)

> :bookmark: 혹은 자연적 의미 (Natural Semantics) 로 불립니다.

$$
\code{Num}\frac{
}{
  \eval{\num}{\num}
}
\qquad

\code{Add}\frac{
  \eval{\expr_1}{\num_1}
  \qquad
  \eval{\expr_2}{\num_2}
}{
  \eval{\expr_1 \; \code{+} \; \expr_2}{\num_1 + \num_2}
}
\qquad

\code{Mul}\frac{
  \eval{\expr_1}{\num_1}
  \qquad
  \eval{\expr_2}{\num_2}
}{
  \eval{\expr_1 \; \code{*} \; \expr_2}{\num_1 \times \num_2}
}
$$

</details>
