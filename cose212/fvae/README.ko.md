# `FVAE` - `VAE` with First-Class Functions

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/fvae.g8
```

> [!WARNING]
>
> 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>fvae
└─ src
   ├─ main/scala/kuplrg
   │  ├── FVAE.scala ──────────── FVAE의 정의와 파서들
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

`FVAE` 언어는 [`VAE`](../vae/README.ko.md) 언어에 일급 함수를 추가한 것입니다.
이 과제에서는 `interp`과 `interpDS` 함수를 구현합니다.

## `FVAE` 언어의 명세

`FVAE` 언어의 문법(syntax)과 의미가(semantics)가 궁금하면,
[`fvae-spec.pdf`](./fvae-spec.pdf) 참조하세요.


### 실행 오류

만약 실행 도중 다음과 같은 조건을 만족하는 경우, `interp` (혹은 `interpDS`)
함수는 해당하는 오류의 종류를 포함하는 문자열을 메세지로 가지는 오류를 `error`
함수를 통해 발생시켜야 합니다:

| 오류 종류 | 설명 |
|:---------|:-----|
| `free identifier` | 주어진 환경에 해당하는 변수가 없는 경우 |
| `invalid operation` | 주어진 연산자와 피연산자들의 타입이 맞지 않는 경우 |
| `not a function` | 함수 호출의 대상이 함수가 아닌 경우 |


## (문제 #1) `interp` (50 점)

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


## (문제 #2) `interpDS` (50 점)

`evalDS` 함수는 `interpDS` 함수의 편의 함수로써, `eval`과 동일한 작업을 `interp`
대신 `interpDS`를 호출하여 수행합니다:
```scala
def evalDS(str: String): String = interpDS(Expr(str), Map.empty).toString
```

`interpDS` 함수는 주어진 표현식(`expr`)을 주어진 환경(`env`)에서 계산하여 결과를
반환합니다:
```scala
def interpDS(expr: Expr, env: Env): Value = ???
```
> [!WARNING]
>
> 기존의 `interp` 함수는 **정적 스코핑(static scoping)** 를 사용하지만,
> `interpDS` 함수는 **동적 스코프(dynamic scoping)** 를 사용합니다. 이 두 가지
> 스코핑 방식의 차이점은 [명세](./fvae-spec.pdf)의 4.1 절을 참조하세요.

**`Implementation.scala` 파일에 `interpDS` 함수를 구현하세요.**
