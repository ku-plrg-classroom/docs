# `AE` - Arithmetic Expressions

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/ae.g8
```

> [!WARNING]
>
> 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>ae
└─ src
   ├─ main/scala/kuplrg
   │  ├── AE.scala ────────────── AE의 정의와 파서들
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

`AE` 언어는 정수의 덧셈과 곱셈을 지원하는 간단한 산술 표현식 언어입니다. 이
과제에서는 `interp`과 `countNums` 두 함수를 구현합니다.

## `AE` 언어의 명세

`AE` 언어의 문법(syntax)과 의미가(semantics)가 궁금하면,
[`ae-spec.pdf`](./ae-spec.pdf) 참조하세요.

## (문제 #1) `interp` (50 점)

`interp` 함수는 주어진 표현식을 계산하여 결과를 반환합니다:
```scala
def interp(expr: Expr): Value = ???
```
`Implementation.scala` 파일에 `interp` 함수를 구현하세요.

## (문제 #2) `countNums` (50 점)

`countNums` 함수는 주어진 표현식에 포함된 `Num` 노드의 개수를 세어 반환합니다:
```scala
def countNums(expr: Expr): Int = ???
```
`Implementation.scala` 파일에 `countNums` 함수를 구현하세요.
