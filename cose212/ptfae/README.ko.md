# `PTFAE` - `TFAE` with Parametric Polymorphism

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/ptfae.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>ptfae
└─ src
   ├─ main/scala/kuplrg
   │  ├── PTFAE.scala ─────────── PTFAE의 정의와 파서들
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

`PTFAE` 언어는 [`TFAE`](../tfae/README.ko.md) 언어에 매개변수 다형성(parametric
polymorphism)을 추가한 언어입니다. 이 과제에서는 `typeCheck`와 `interp` 함수를
구현합니다.

## `PTFAE` 언어의 명세

`PTFAE` 언어의 문법(syntax)과 의미가(semantics)가 궁금하면,
[`ptfae-spec.pdf`](./ptfae-spec.pdf) 참조하세요.

### 타입 오류

만약 주어진 표현식이 타입 검사를 통과하지 못하는 경우, `typeCheck` 함수는
`error` 함수를 통해 오류를 발생시켜야 합니다:
```scala
testExc(eval("(x: Number) => x(1)"))
```

### 실행 오류

비슷한 방식으로, 만약 주어진 표현식이 실행 중에 오류를 발생시키는 경우, `interp`
함수는 `error` 함수를 통해 오류를 발생시켜야 합니다:
```scala
testExc(eval("x"))
```

하지만, 타입 오류 및 실행 오류의 오류 메시지는 검사 대상이 아니며, 오직 `error`
함수를 호출하여 오류를 발생시키는지 여부를 기준으로만 채점합니다.

## `eval` 함수

`eval` 함수는 `typeCheck` 함수와 `interp` 함수의 편의 함수로, 다음의 과정으로
표현식을 계산합니다:

1. 주어진 문자열을 파싱하여 표현식으로 변환합니다.
1. `typeCheck` 함수를 호출하여 타입 검사를 수행합니다.
1. `interp` 함수를 호출하여 표현식을 계산합니다.

마지막으로, `eval` 함수는 계산 결과와 타입을 문자열 형태로 반환합니다:
```scala
def eval(str: String): String =
  val expr = Expr(str)
  val ty = typeCheck(expr, Map.empty)
  val v = interp(expr, Map.empty)
  s"${v.str}: ${ty.str}"
```

## (문제 #1) `typeCheck`

`typeCheck` 함수는 주어진 표현식(`expr`)이 주어진 타입 환경(`tenv`)에서 타입
검사를 진행하고, 표현식의 타입을 반환합니다:
```scala
def typeCheck(expr: Expr, tenv: TypeEnv): Type = ???
```
**`Implementation.scala` 파일에 `typeCheck` 함수를 구현하세요.**

## (문제 #2) `interp`

`interp` 함수는 주어진 표현식(`expr`)을 주어진 환경(`env`)에서 계산하여 결과를
반환합니다:
```scala
def interp(expr: Expr, env: Env): Value = ???
```
**`Implementation.scala` 파일에 `interp` 함수를 구현하세요.**
