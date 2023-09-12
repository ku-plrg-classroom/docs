# `VAE` - Arithmetic Expressions with Variables

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/ae.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>ae
└─ src
   ├─ main/scala/kuplrg
   │  ├── VAE.scala ───────────── VAE의 정의와 파서들
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

`VAE` 언어는 정수의 덧셈과 곱셈 및 변수를 지원하는 간단한 산술 표현식
언어입니다. 이 과제에서는 `interp`, `freeIds`, `bindingIds`, `boundIds`,
`shadowedIds` 함수를 구현합니다.

## `VAE` 언어의 명세

`VAE` 언어의 문법(syntax)과 의미가(semantics)가 궁금하면,
[`vae-spec.pdf`](./vae-spec.pdf) 참조하세요.

## (문제 #1) `interp` (20 점)

`interp` 함수는 주어진 표현식을 계산하여 결과를 반환합니다:
```scala
def interp(expr: Expr): Value = ???
```
`Implementation.scala` 파일에 `interp` 함수를 구현하세요.

## (문제 #2) `freeIds` (20 점)

`freeIds` 함수는 주어진 표현식에서 자유 변수의 집합을 반환합니다:
```scala
def freeIds(expr: Expr): Set[String] = ???
```
`Implementation.scala` 파일에 `freeIds` 함수를 구현하세요.

## (문제 #3) `bindingIds` (20 점)

`bindingIds` 함수는 주어진 표현식에서 바인딩된 변수의 집합을 반환합니다:
```scala
def bindingIds(expr: Expr): Set[String] = ???
```
`Implementation.scala` 파일에 `bindingIds` 함수를 구현하세요.

## (문제 #4) `boundIds` (20 점)

`boundIds` 함수는 주어진 표현식에서 바운드된 변수의 집합을 반환합니다:
```scala
def boundIds(expr: Expr): Set[String] = ???
```
`Implementation.scala` 파일에 `boundIds` 함수를 구현하세요.

## (문제 #5) `shadowedIds` (20 점)

`shadowedIds` 함수는 주어진 표현식에서 같은 이름의 다른 변수를 가려버리는
변수의 집합을 반환합니다:
```scala
def shadowedIds(expr: Expr): Set[String] = ???
```
`Implementation.scala` 파일에 `shadowedIds` 함수를 구현하세요.
