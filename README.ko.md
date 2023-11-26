# 프로그래밍 과제 공용 지침서

[English](./README.md) | [한국어](./README.ko.md)

- [과제](#과제)
- [사전 준비](#사전-준비)
- [다운로드](#다운로드)
- [디렉토리 구조](#디렉토리-구조)
- [제약사항](#제약사항)
- [예외처리](#예외처리)
- [구현에 대한 테스트](#구현에-대한-테스트)
  - [테스트 케이스 작성](#테스트-케이스-작성)
  - [테스트 케이스 실행](#테스트-케이스-실행)
  - [상호작용 기반 테스트](#상호작용-기반-테스트)
- [제출](#제출)
- [채점](#채점)

이 저장소는 [고려대학교](https://korea.ac.kr) 소속 [프로그래밍 언어 연구실](https://plrg.korea.ac.kr/)에서 진행하는 수업의 프로그래밍 과제에 대한 문서들을 관리합니다.

## 과제

### 기본

| 이름 | 설명 |
| :--: | ---- |
| [scala-tutorial](./scala-tutorial/README.ko.md) | Scala 튜토리얼 |

### [COSE212: 프로그래밍 언어](https://plrg.korea.ac.kr/courses/cose212/)

| 이름 | 설명 |
| :--: | ---- |
| [ae](./cose212/ae/README.ko.md) | `AE` - Arithmetic Expressions |
| [vae](./cose212/vae/README.ko.md) | `VAE` - `AE` with Variables |
| [f1vae](./cose212/f1vae/README.ko.md) | `F1VAE` - `VAE` with First-Order Functions |
| [fvae](./cose212/fvae/README.ko.md) | `FVAE` - `VAE` with First-Class Functions |
| [fae](./cose212/fae/README.ko.md) | `FAE` - `AE` with First-Class Functions |
| [rfae](./cose212/rfae/README.ko.md) | `RFAE` - `FAE` with Recursion and Conditionals |
| [bfae](./cose212/bfae/README.ko.md) | `BFAE` - `FAE` with Mutable Boxes |
| [mfae](./cose212/mfae/README.ko.md) | `MFAE` - `FAE` with Mutable Variables |
| [lfae](./cose212/lfae/README.ko.md) | `LFAE` - `FAE` with Lazy Evaluation |
| [fae-cps](./cose212/fae-cps/README.ko.md) | `FAE-cps` - `FAE` with Continuation-Passing Style |
| [kfae](./cose212/kfae/README.ko.md) | `KFAE` - `FAE` with First-Class Continuations |
| [tfae](./cose212/tfae/README.ko.md) | `TFAE` - `FAE` with Type System |
| [trfae](./cose212/trfae/README.ko.md) | `TRFAE` - `RFAE` with Type System |
| [atfae](./cose212/atfae/README.md) | `ATFAE` - `TRFAE` with Algebraic Data Types |

#### 프로젝트

| 이름 | 설명 |
| :--: | ---- |
| [cobalt](./cose212/cobalt/README.ko.md) | `COBALT` - Comprehension-supported Boolean and Arithmetic Expression with Lists and Tuples |
| [magnet](./cose212/magnet/README.ko.md) | `MAGNET` - Mutable Arithmetic Expressions with Generators and Exceptions |

### [COSE215: 계산이론](https://plrg.korea.ac.kr/courses/cose215/)

| 이름 | 설명 |
| :--: | ---- |
| [fa-examples](./cose215/fa-examples/README.ko.md) | Finite Automata 예제 |
| [equiv-re-fa](./cose215/equiv-re-fa/README.ko.md) | Regular Expression과 Finite Automata의 동치성 |
| [equiv-pda-cfg](./cose215/equiv-pda-cfg/README.ko.md) | Pushdown Automata와 Context-Free Grammar의 동치성 |

## 사전 준비

* JDK >= 8 (<https://www.oracle.com/java/technologies/downloads/>)
* sbt (<https://www.scala-sbt.org/download.html>)

## 다운로드

터미널에서 `sbt new ku-plrg-classroom/[assignment name].g8`를 실행합니다.
현재 디렉토리 아래에 과제 이름과 동일한 디렉토리가 생성됩니다.

> :warning: `[assignment name]`을 과제 이름으로 바꿔주세요. 혹은, 각 과제의
> 문서에서 명령어를 복사해서 붙여넣어도 됩니다.

아래는 예시입니다.

```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
...
name [Scala Tutorial]:
```

> :warning: 기본 디렉토리 이름을 사용하려면 `Enter` 키를 눌러주세요.

## 디렉토리 구조

다음과 같은 디렉토리와 파일이 숙제 디렉토리에 있습니다.

<pre><code>src
├─ main/scala/kuplrg
│  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
│  └─ Template.scala
└─ test/scala/kuplrg
   ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
   └─ SpecBase.scala</code></pre>

**절대로** `src/main/scala/kuplrg/Implementation.scala`와
`src/test/scala/kuplrg/Spec.scala` 이외의 파일을 수정하지 마세요.

* `src/main/scala/kuplrg/Implementation.scala`:
이 파일에 구현해야 하는 함수의 정의가 있습니다. 이 파일만 수정하면 과제를 완료할
수 있습니다.

* `src/main/scala/kuplrg/Template.scala`:
이 파일에는 구현해야 하는 함수의 정의가 있습니다. 이 파일은 **절대로** 수정하지
마세요.

* `src/main/scala/kuplrg/error.scala`:
 이 파일은 `error` 함수를 정의하고 있습니다. 자세한 내용은
[예외처리](#예외처리) 부분을 참고하세요.

* `src/test/scala/kuplrg/Spec.scala`
이 파일에는 테스트 케이스가 있습니다. 테스트 케이스 작성과 실행 방법은 [테스트
케이스 작성](#테스트-케이스-작성)과 [테스트 케이스 실행](#테스트-케이스-실행)
부분을 참고하세요.

* `src/test/scala/kuplrg/SpecBase.scala`
이 파일은 테스트 케이스를 위한 기본 프레임워크를 포함하고 있습니다. 이 파일을
읽거나 수정할 필요는 없습니다.


## 제약사항

**절대로** 다음의 기능을 사용하지 마십시오:
* `var`
* `while`
* `asInstanceOf`
* `isInstanceOf`
* `null`
* `return`
* `throw`
* `try-catch`
* 가변 데이터 구조 (`scala.collection.mutable`)

위에서 언급한 기능 외에는 자유롭게 사용하실 수 있습니다. 예를 들어,

* 새로운 함수나 타입을 정의할 수 있습니다.
* 불변 데이터 구조를 사용할 수 있습니다.
* `for` comprehension을 사용할 수 있습니다.
* 이미 정의된 가변 변수/필드를 변경할 수 있습니다.

금지된 기능을 사용하면 코드가 컴파일되지 않습니다. 만약 코드가 성공적으로
컴파일되면 금지된 기능을 사용하지 않았다는 것을 의미합니다.

## 예외처리

`throw`와 `try-catch`를 사용할 수 없지만, `error` 함수에 문자열 메시지 `s`를
전달하여(`error(s)`), 예외를 던질 수 있습니다. 이 예외는 `Template.scala`에
정의된 `PLError` 타입입니다. 메시지를 생략하려면 `error()`를 사용할 수 있습니다.
(`error("")`와 동일합니다.)

```scala
error()       // ""를 메시지로 가지는 `PLError`를 던집니다.
error("foo")  // "foo"를 메시지로 가지는 `PLError`를 던집니다.

def f(x: Int): Int =
  if (x < 0) error("x must be non-negative")
  else x

f(42)         // 42
f(-1)         // "x must be non-negative"를 메시지로 가지는 `PLError`를 던집니다.
```

`testExc` 함수를 사용하여 `PLError` 예외를 테스트할 수 있습니다. 자세한 내용은
[테스트 케이스 작성](#테스트-케이스-작성) 부분을 참고하세요.

## 구현에 대한 테스트

### 테스트 케이스 작성

`Spec.scala`에 테스트 케이스를 작성할 수 있습니다. `check`, `test`, 그리고
`testExc` 함수를 사용하여 구현을 테스트할 수 있습니다:

* `check` 함수는 임의의 표현식을 받아서, 그 표현식이 예외를 던지지 않고 정상적으로
    종료되는지 확인합니다.
* `test` 함수는 두 개의 같은 타입을 가지는 표현식을 받아서, 그 두 표현식이 같은
    값을 가지는지 확인합니다.
* `testExc` 함수는 표현식과 문자열을 받아서, 그 표현식이 주어진 문자열을 포함하는
    메시지를 가지는 `PLError` 예외를 던지는지 확인합니다.

예를 들어, 다음과 같이 `f` 함수를 테스트할 수 있습니다:

```scala
def f(x: Int): Int =
  if (x < 0) error("x must be non-negative")
  else x

// test f(3) normally terminates without any exceptions
check(f(3))

// test f(3) == 3
test(f(3), 3)

// test f(-1) throws a `PLError` with the message containing "non-negative"
testExc(f(-1), "non-negative")
```

> :warning: 주어진 테스트 케이스를 모두 통과한 것이 구현이 올바르다는 것을
> 보장하지는 않습니다. 따라서, 자신만의 테스트 케이스를 작성하여 구현을
> 더 엄밀하게 테스트하는 것을 **매우 권장**합니다.


### 테스트 케이스 실행

테스트 케이스를 실행하기 위해서는 터미널에서 `sbt` 콘솔을 실행해야 합니다.
```
$ sbt
[info] welcome to sbt
...
sbt:...>
```

`Spec.scala`에 작성한 테스트 케이스를 실행하려면 `sbt` 콘솔에서 `test` 명령을
실행하면 됩니다.

```
sbt:...> test
...
[error] Failed tests:
[error]         kuplrg.Spec
```

모든 함수를 올바르게 구현했다면, 모든 테스트 케이스가 성공할 것입니다.

```
sbt:...> test
...
[info] All tests passed.
```

만약 실시간으로 테스트 결과를 확인하고 싶다면, `~test`를 사용할 수 있습니다.
그러면, `sbt`는 파일을 저장할 때마다 테스트 케이스를 실행합니다.

### 상호작용 기반 테스트

만약 상호작용을 하면서 구현을 테스트하고 싶다면, sbt 콘솔에서 `console` 명령을
사용할 수 있습니다. 이 명령어는 숙제의 구현체와 함께 Scala REPL을 실행합니다. 이
곳에서 `Implementation.scala`에서 정의한 변수나 함수를 사용할 수 있습니다.

```
sbt:...> console
...
scala> import kuplrg.*, Implementation.*

scala> ...
```

## 제출

[블랙보드](https://kulms.korea.ac.kr/)를 사용하여 각 숙제의
`Implementation.scala`를 제출하세요.

> :warning: **기존에 존재하던 코드를 단순히 복사해서 제출하면 F를 받게 됩니다.**
> 각 숙제는 반드시 혼자서 구현해야 합니다. 강의 자료, 강의 슬라이드, 인터넷
> 등을 참고할 수 있습니다. 하지만, **자신이 작성한 코드의 세부사항을 설명할 수
> 있어야 합니다. 만약 설명을 제대로 하지 못하면, F를 받게 됩니다.**

제출 시간을 넘기면 제출이 불가능합니다. 대신, 제출은 여러번 할 수 있고, 마지막에
제출한 코드가 채점됩니다.


## 채점

채점은 100점 만점입니다. 금지된 기능을 사용하거나 코드가 컴파일되지 않으면 0점을 받게 됩니다.

채점을 위한 테스트 케이스는 `Spec.scala`에 정의되어 있는 테스트 케이스와
비슷하지만, 완전히 동일하지는 않습니다. 더 많은 테스트 케이스를 통과할수록 더
많은 점수를 얻게 됩니다. 각 테스트 케이스는 서로 영향을 주지 않습니다. 예를
들어, 특정 테스트 케이스에서 구현이 무한 루프에 빠져도 다른 테스트 케이스는
정상적으로 채점됩니다.
