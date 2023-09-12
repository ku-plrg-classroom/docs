# Pushdown Automata와 Context-Free Grammar의 동치성

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/equiv-pda-cfg.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>equiv-pda-cfg
└─ src
   ├─ main/scala/kuplrg
   │  ├── PDA.scala ───────────── Pushdown Automata (PDA)의 정의
   │  ├── CFG.scala ───────────── Context-Free Grammar (CFG)의 정의
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  ├── basics.scala ────────── 기본 함수들의 정의
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

**이 숙제의 목표는 `Implementation.scala` 파일에 `pdafs2es`, `pdaes2fs`, 그리고
`cfg2pdaes` 함수를 구현하는 것입니다.**

- [(문제 #1) PDA with Final States to PDA with Empty Stacks (30점)](#문제-1-pda-with-final-states-to-pda-with-empty-stacks-30점)
- [(문제 #2) PDA with Empty Stacks to PDA with Final States (30점)](#문제-2-pda-with-empty-stacks-to-pda-with-final-states-30점)
- [(문제 #3) CFGs to PDA with Empty Stacks (40점)](#문제-3-cfgs-to-pda-with-empty-stacks-40점)
- [부록](#appendix)
  - [PDA의 String 형태](#pda의-string-형태)
  - [CFG의 String 형태](#cfg의-string-형태)


## (문제 #1) PDA with Final States to PDA with Empty Stacks (30점)

첫 번째 문제는 **final state로 정의된 PDA**를 **empty stack으로 정의된 PDA**로
변환하는 `pdafs2es` 함수를 구현하는 것입니다.

```scala
  // Convert a PDA with final states to a PDA with empty stacks
  def pdafs2es(pda: PDA): PDA = ???
```

### 테스트 케이스

`Spec.scala` 파일에 테스트 케이스가 정의되어 있고, `Spec.scala` 파일에 자신만의
테스트 케이스를 추가할 수 있습니다. 테스트 케이스는 `sbt test`를 실행할 수
있습니다.

`PDA` 클래스의 `dump` 함수를 사용하여 구현체를 디버깅할 수 있습니다. 자세한
내용은 [PDA의 String 형태](#pda의-string-형태) 부분을 참고하세요.


## (문제 #2) PDA with Empty Stacks to PDA with Final States (30점)

두 번째 문제는 첫 번째 문제의 정확히 반대 과정으로, **empty stack으로 정의된
PDA**를 **final state로 정의된 PDA**로 변환하는 `pdaes2es` 함수를 구현하는
것입니다.

```scala
  // Convert a PDA with empty stacks to a PDA with final states
  def pdaes2fs(pda: PDA): PDA = ???
```

### 테스트 케이스

`Spec.scala` 파일에 테스트 케이스가 정의되어 있고, `Spec.scala` 파일에 자신만의
테스트 케이스를 추가할 수 있습니다. 테스트 케이스는 `sbt test`를 실행할 수
있습니다.

`PDA` 클래스의 `dump` 함수를 사용하여 구현체를 디버깅할 수 있습니다. 자세한
내용은 [PDA의 String 형태](#pda의-string-형태) 부분을 참고하세요.

## (문제 #3) CFGs to PDA with Empty Stacks (40점)

마지막 문제는 **CFG**를 **empty stack으로 정의된 PDA**로 변환하는 `cfg2pades`
함수를 구현하는 것입니다.

```scala
  // Convert a CFG to a PDA with empty stacks
  def cfg2pdaes(cfg: CFG): PDA = ???
```

### 테스트 케이스

`Spec.scala` 파일에 테스트 케이스가 정의되어 있고, `Spec.scala` 파일에 자신만의
테스트 케이스를 추가할 수 있습니다. 테스트 케이스는 `sbt test`를 실행할 수
있습니다.

테스트 케이스는 CFG의 string 형태를 사용하여 정의되어 있습니다. 이 string 형태
대한 설명은 [CFG의 String 형태](#cfg의-string-형태)를 참고하세요. 또한, `CFG`
클래스의 `dump` 함수를 사용해서 구현체를 확인해볼 수 있습니다.


## 부록

### PDA의 String 형태

다음과 같이, `dump` 함수를 사용하여 PDA의 string 형태를 확인할 수 있습니다:

```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can dump any PDA
    pda1.dump

    ...
  }
  ...
}
```

그 후, `sbt test`를 실행하면, 모든 테스트 케이스의 실행이 끝난 후, PDA의 string
형태와 Scala object가 출력됩니다:

```bash
$ sbt
...
sbt:...> test
...
----------------------------------------
[SCORE] ...
----------------------------------------
...
* A PDA is dumped:
  * String form:
    - states: 0, 1, 2
    - symbols: a, b
    - alphabets: A, C
    - initState: 0
    - initAlphabet: A
    - finalStates: 2
    - trans: {
        0 -<e>-> 1 [A -> A]
        0 -<e>-> 1 [C -> C]
        0 - a -> 0 [A -> CA]
        0 - a -> 0 [C -> CC]
        1 -<e>-> 2 [A -> A]
        1 - b -> 1 [C -> ]
      }
  * Scala object: PDA(HashSet(0, 1, 2),HashSet(a, b),HashSet(0, 2),HashMap((1,Some(b),0) -> Set(), (0,None,2) -> Set((1,List(2))), (2,Some(b),2) -> Set(), (1,Some(a),2) -> Set(), (0,None,0) -> Set((1,List(0))), (2,Some(b),0) -> Set(), (0,Some(b),0) -> Set(), (2,None,0) -> Set(), (2,None,2) -> Set(), (0,Some(a),0) -> Set((0,List(2, 0))), (0,Some(a),2) -> Set((0,List(2, 2))), (1,Some(a),0) -> Set(), (1,None,0) -> Set((2,List(0))), (1,None,2) -> Set(), (2,Some(a),0) -> Set(), (2,Some(a),2) -> Set(), (1,Some(b),2) -> Set((1,List())), (0,Some(b),2) -> Set()),0,0,Set(2))
...
```

### CFG의 String 형태

CFG를 string 형태로 정의할 수 있습니다:

```scala
val cfg: CFG = CFG("""
  'S -> b 'S bb | 'A ;;
  'A -> a 'A | <e> ;;
""")
```

이는 다음과 같이 정의한 것과 동일합니다:

```scala
val A = 0
val S = 18
val cfg: CFG = CFG(
  variables = Set(A, S),
  symbols = Set('a', 'b'),
  start = S,
  productions = Set(
    S -> List('b', S, 'b', 'b'),
    S -> List(A),
    A -> List('a', A),
    A -> List(),
  )
)
```

CFG의 string 형태는 다음과 같이 정의됩니다:
- `'A`는 variable $A$을 의미합니다.
- `<e>`는 비어있는 나열을 의미합니다.
- `a`는 하나의 symbol $\texttt{a}$를 의미합니다.
- `S -> x | y`는 `S -> x ;; S -> y`를 짧게 정의한 것입니다.

다음과 같이, `dump` 함수를 사용하여 CFG의 string 형태를 확인할 수 있습니다:

```scala
class Spec extends SpecBase {

  // The playground for tests
  def afterTest: Unit = {
    ...

    // You can dump any CFG
    cfg.dump

    ...
  }
  ...
}
```

그 후, `sbt test`를 실행하면, 모든 테스트 케이스의 실행이 끝난 후, CFG의 string
형태와 Scala object가 출력됩니다:

```bash
$ sbt
...
sbt:...> test
...
----------------------------------------
[SCORE] ...
----------------------------------------
...
* A CFG is dumped:
  * String form:
    'S -> 'A | b 'S b b ;;
    'A -> <e> | a 'A ;;
  * Scala object: CFG(Set(18, 0),Set(b, a),18,Set((18,List(b, 18, b, b)), (18,List(0)), (0,List(a, 0)), (0,List())))
...
```
