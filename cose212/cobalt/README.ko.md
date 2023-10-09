# `COBALT` - Comprehension-supported Boolean and Arithmetic Expression with Lists and Tuples

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/cobalt.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>cobalt
└─ src
   ├─ main/scala/kuplrg
   │  ├── COBALT.scala ────────── COBALT의 정의와 파서들
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

`COBALT` 언어는 리스트와 튜플이 추가된 불리언 및 산술 표현식 언어로 comprehension
문법을 지원합니다. 이 과제에서는 `interp` 함수를 구현합니다.

## `COBALT` 언어의 명세

`COBALT` 언어의 문법(syntax)과 의미가(semantics)가 궁금하면,
[`cobalt-spec.pdf`](./cobalt-spec.pdf) 참조하세요.


### 실행 오류

만약 실행 도중 다음과 같은 조건을 만족하는 경우, `interp` (혹은 `interpDS`)
함수는 해당하는 오류의 종류를 포함하는 문자열을 메세지로 가지는 오류를 `error`
함수를 통해 발생시켜야 합니다:

| 오류 종류 | 설명 |
|:---------|:-----|
| `free identifier` | 주어진 환경에 해당하는 변수가 없는 경우 |
| `invalid operation` | 주어진 연산자와 피연산자들의 타입이 맞지 않는 경우 |
| `not a function` | 함수 호출의 대상이 함수가 아닌 경우 |
| `not a boolean` | 조건문의 조건이 불리언이 아닌 경우 |
| `not a boolean` | `filter`에 주어진 함수의 결과가 불리언이 아닌 경우 |
| `not a list` | 리스트 연산에서 리스트가 아닌 경우 |
| `empty list` | 리스트 head나 tail 연산에서 빈 리스트인 경우 |
| `out of bounds` | 튜플 프로젝션에서 인덱스가 튜플의 범위를 벗어난 경우 |
| `not a tuple` | 튜플 프로젝션에서 튜플이 아닌 경우 |

이 언어에서는 특정 경우에 대해 실행 순서가 정의되지 않습니다. 예를 들어, 덧셈
연산의 피연산자들의 순서는 정의되지 않습니다. 따라서, 다음과 같은 표현식은
두 가지 다른 오류를 발생시킬 수 있습니다:
```scala
interp(Expr("(1 + x) + (1 + true)")) // `free identifier` or `invalid operation`
```
두 오류 모두 유효한 오류이며, 둘 중 하나를 발생시켜도 됩니다. 즉, 구현을 할 때,
피연산자들의 순서를 고려할 필요가 없으며, 편한 순서로 가정하고 구현하면 됩니다.
또한, 이 과제에서는 이러한 경우를 테스트하지 않을 것입니다.


## (문제 #1) `interp`

`eval` 함수는 `interp` 함수의 편의 함수로써, 주어진 문자열을 파싱한 후, 비어
있는 환경에서 `interp` 함수를 호출합니다:
```scala
def eval(str: String): String = interp(Expr(str), Map.empty).toString
```

`interp` 함수는 주어진 표현식(`expr`)을 주어진 환경(`env`)에서 계산하여 결과를
반환합니다:
```scala
def interp(expr: Expr, env: Env): Value = ???
```
**`Implementation.scala` 파일에 `interp` 함수를 구현하세요.**

여러분은 `interp` 함수를 구현할 때, 다음과 같은 함수들을 구현 및 활용할 수
있습니다. 다만, 이는 필수적인 것은 아니며, 여러분의 함수들을 자유롭게 구현해도
됩니다.
```scala
def eq(left: Value, right: Value): Boolean = ???
def length(list: Value): BigInt = ???
def map(list: Value, fun: Value): Value = ???
def join(list: Value): Value = ???
def filter(list: Value, fun: Value): Value = ???
def app(fun: Value, args: List[Value]): Value = ???
```
