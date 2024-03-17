# Scala 튜토리얼

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
```

> [!WARNING]
> 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

- **[기본 데이터 타입 (10점)](#기본-데이터-타입-10점)**
  - [(문제 #1) `sqsum` (5점)](#문제-1-sqsum-5점)
  - [(문제 #2) `concat` (5점)](#문제-2-concat-5점)
- **[함수 (15점)](#함수-15점)**
  - [(문제 #3) `subN` (5점)](#문제-3-subn-5점)
  - [(문제 #4) `twice` (5점)](#문제-4-twice-5점)
  - [(문제 #5) `compose` (5점)](#문제-5-compose-5점)
- **[리스트 (15점)](#리스트-15점)**
  - [(문제 #6) `sumOnlyOdd` (5점)](#문제-6-sumonlyodd-5점)
  - [(문제 #7) `foldWith` (5점)](#문제-7-foldwith-5점)
  - [(문제 #8) `toSet` (5점)](#문제-8-toset-5점)
- **[맵과 집합 (10점)](#맵과-집합-10점)**
  - [(문제 #9) `getOrZero` (5점)](#문제-9-getorzero-5점)
  - [(문제 #10) `setMinus` (5점)](#문제-10-setminus-5점)
- **[트리 (25점)](#트리-25점)**
  - [(문제 #11) `has` (5점)](#문제-11-has-5점)
  - [(문제 #12) `maxDepthOf` (5점)](#문제-12-maxdepthof-5점)
  - [(문제 #13) `mul` (5점)](#문제-13-mul-5점)
  - [(문제 #14) `countLeaves` (5점)](#문제-14-countleaves-5점)
  - [(문제 #15) `postOrder` (5점)](#문제-15-postorder-5점)
- **[논리식 (25점)](#논리식-25점)**
  - [(문제 #16) `countLiterals` (5점)](#문제-16-countliterals-5점)
  - [(문제 #17) `countNots` (5점)](#문제-17-countnots-5점)
  - [(문제 #18) `depth` (5점)](#문제-18-depth-5점)
  - [(문제 #19) `eval` (5점)](#문제-19-eval-5점)
  - [(문제 #20) `getString` (5점)](#문제-20-getstring-5점)

## 기본 데이터 타입 (10점)

#### (문제 #1) `sqsum` (5점)

정수 `x`와 `y`를 받아서, `x`와 `y`의 제곱의 합을 반환하는 함수를 작성하세요.
```scala
test(sqsum(0, 0), 0)
test(sqsum(2, 3), 13)
test(sqsum(-3, 4), 25)
```

#### (문제 #2) `concat` (5점)

두 개의 문자열 `x`와 `y`를 받아서, 두 문자열을 이어붙인 결과를 반환하는 함수를
작성하세요.
```scala
test(concat("Hello ", "World!"), "Hello World!")
test(concat("COSE", "212"), "COSE212")
test(concat("COSE", "215"), "COSE215")
```

## 함수 (15점)

#### (문제 #3) `subN` (5점)

정수 `n`을 받아서, 정수에 `n`을 뺀 결과를 반환하는 함수를 작성하세요.
```scala
test(subN(3)(5), 2)
test(subN(4)(13), 9)
test(subN(243)(-942), -1185)
```

#### (문제 #4) `twice` (5점)

`Int => Int` 타입의 함수 `f`를 받아서, `f`를 두 번 적용하는 함수를 반환하는
함수를 작성하세요.
```scala
test(twice(_ + 3)(1), 7)
test(twice(subN(3))(10), 4)
test(twice(_ * 10)(42), 4200)
```

#### (문제 #5) `compose` (5점)

`Int => Int` 타입의 함수 `f`와 `g`를 받아서, `f`와 `g`를 합성한 함수 (`f ∘ g`)
를 반환하는 함수를 작성하세요.
```scala
test(compose(_ + 3, _ * 2)(1), 5)
test(compose(_ * 10, _ + 1)(42), 430)
test(compose(subN(3), subN(2))(10), 5)
```


## 리스트 (15점)

#### (문제 #6) `sumOnlyOdd` (5점)

정수 리스트 `l`을 받아서, `l`에 있는 홀수들의 합을 반환하는 함수를 작성하세요.
```scala
test(sumOnlyOdd(List(2)), 0)
test(sumOnlyOdd(List(1, 2, 3)), 4)
test(sumOnlyOdd(List(4, 2, 3, 7, 5)), 15)
```

#### (문제 #7) `foldWith` (5점)

`(Int, Int) => Int` 타입의 함수 `f`를 받아서, 정수 리스트 `l`을 `f`를 이용해서
하나의 정수로 합치는 함수를 작성하세요. 시작 값은 0을 사용하세요.
```scala
test(foldWith(_ + _)(List(1, 2, 3)), 6)
// 6 = 0 + 1 + 2 + 3
test(foldWith(_ - _)(List(5, 9, 2, 3)), -19)
// -19 = 0 - 5 - 9 - 2 - 3
test(foldWith(_ * 2 + _)(List(4, 7, 3, 2)), 68)
// 68 = (((0 * 2 + 4) * 2 + 7) * 2 + 3) * 2 + 2
```

#### (문제 #8) `toSet` (5점)

정수 리스트 `l`과 정수 `from`을 받아서, `from` 이상의 인덱스를 가지는 `l`의
원소들을 모은 집합을 반환하는 함수를 작성하세요.
```scala
test(toSet(List(1, 5, 2, 7, 4, 2, 4), 0), Set(1, 2, 4, 5, 7))
// { 1, 5, 2, 7, 4, 2, 4 } = { 1, 2, 4, 5, 7 }
test(toSet(List(1, 5, 2, 7, 4, 2, 4), 2), Set(2, 4, 7))
// { 2, 7, 4, 2, 4 } = { 2, 4, 7 }
test(toSet(List(1, 5, 2, 7, 4, 2, 4), 4), Set(2, 4))
// { 4, 2, 4 } = { 2, 4 }
```


## 맵과 집합 (10점)

#### (문제 #9) `getOrZero` (5점)

문자열을 정수로 매핑하는 맵 `m`과 문자열 `s`를 받아서, `m`에 `s`에 대한 매핑이
있다면 해당 정수를 반환하고, 그렇지 않다면 0을 반환하는 함수를 작성하세요.
```scala
test(getOrZero(Map("Park" -> 3, "Kim" -> 5), "Park"), 3)
test(getOrZero(Map("Park" -> 3, "Kim" -> 5), "Lee"), 0)
test(getOrZero(Map("Park" -> 3, "Kim" -> 5), "Kim"), 5)
```

#### (문제 #10) `setMinus` (5점)

집합 `s1`과 `s2`를 받아서, `s1`에는 속하지만 `s2`에는 속하지 않는 원소들로
만들어진 집합을 반환하는 함수를 작성하세요.
```scala
test(setMinus(Set(1, 2, 3), Set(2, 3, 4)), Set(1))
test(setMinus(Set(1, 2, 3), Set(4, 5, 6)), Set(1, 2, 3))
test(setMinus(Set(1, 2, 3), Set(1, 2, 3, 4)), Set())
```


## 트리 (25점)

`Tree` 타입은 트리를 나타냅니다. 트리는 말단 노드 (`Leaf`) 또는 비말단 노드
(`Branch`)로 구성됩니다. 말단 노드는 값만 가지고 있고, 비말단 노드는 값, 왼쪽
서브트리, 오른쪽 서브트리를 가집니다.

```scala
enum Tree:
  case Leaf(value: Int)
  case Branch(left: Tree, value: Int, right: Tree)
```

> [!WARNING]
> `Tree` 타입은 이미 `Template.scala`에 정의되어 있습니다. **절대로** `Tree`
> 타입을 `Implentation.scala`에 다시 정의하지 마세요.

예를 들어, 다음은 `Tree` 타입의 예제들입니다.
```scala
import Tree.*

//  8
val tree1: Tree = Leaf(8)

//    4
//   / \
//  5   2
//     / \
//    8   3
val tree2: Tree = Branch(Leaf(5), 4, Branch(Leaf(8), 2, Leaf(3)))

//    7
//   / \
//  2   3
//     / \
//    5   1
//   / \
//  1   8
val tree3: Tree = Branch(Leaf(2), 7, Branch(Branch(Leaf(1), 5, Leaf(8)), 3, Leaf(1)))
```

#### (문제 #11) `has` (5점)

하나의 정수 `value`를 받아서, 트리가 해당 정수를 가지고 있는지 여부를 반환하는
함수를 작성하세요.
```scala
test(has(8)(tree1), true)
test(has(7)(tree2), false)
test(has(1)(tree3), true)
```

#### (문제 #12) `maxDepthOf` (5점)

하나의 정수 `value`를 받아서, 트리에서 해당 정수를 가진 노드 중 가장 깊은 노드의
깊이를 반환하는 함수를 작성하세요. 여기서 노드의 깊이는 최상위 노드에서 해당
노드까지의 간선의 수를 의미합니다. 만약 트리에 해당 정수를 가진 노드가 없다면
`None`을 반환해야 합니다.
```scala
test(maxDepthOf(8)(tree1), Some(0))
test(maxDepthOf(7)(tree2), None)
test(maxDepthOf(1)(tree3), Some(3))
```

#### (문제 #13) `mul` (5점)

하나의 트리 `t`를 받아서, `t`에 있는 모든 값들의 곱을 반환하는 함수를
작성하세요.
```scala
test(mul(tree1), 8)
test(mul(tree2), 960)
test(mul(tree3), 1680)
```

#### (문제 #14) `countLeaves` (5점)

하나의 트리 `t`를 받아서, `t`에 있는 말단 노드의 수를 반환하는 함수를
작성하세요.
```scala
test(countLeaves(tree1), 1)
test(countLeaves(tree2), 3)
test(countLeaves(tree3), 4)
```

#### (문제 #15) `postOrder` (5점)

하나의 트리 `t`를 받아서, `t`에 있는 모든 값들을 [후위 순회
순서](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%A6%AC_%EC%88%9C%ED%9A%8C)로
담은 리스트를 반환하는 함수를 작성하세요.
```scala
test(postOrder(tree1), List(8))
test(postOrder(tree2), List(5, 8, 3, 2, 4))
test(postOrder(tree3), List(2, 1, 8, 5, 1, 3, 7))
```

## 논리식 (25점)

`BE` 타입은 논리식을 나타냅니다. 논리식은 리터럴 (`True`, `False`), 이진 논리
연산자 (`And`, `Or`), 그리고 단항 논리 연산자 (`Not`)로 구성됩니다.

```scala
enum BE:
  case True
  case False
  case And(left: BE, right: BE)
  case Or(left: BE, right: BE)
  case Not(expr: BE)
```

> [!WARNING]
> `BE` 타입은 이미 `Template.scala`에 정의되어 있습니다. **절대로**
> `BE` 타입을 `Implentation.scala`에 다시 정의하지 마세요.

예를 들어, 다음은 `BE` 타입의 예제들입니다.
```scala
import BE.*

// true | false
val be1: BE = Or(True, False)

// (!(true | false) & !(false | true))
val be2: BE = And(Not(Or(True, False)), Not(Or(False, True)))

// (!((false | !true) & false) & (true & !false))
val be3: BE = And(Not(And(Or(False, Not(True)), False)), And(True, Not(False)))
```

#### (문제 #16) `countLiterals` (5점)

하나의 논리식 `expr`를 받아서, `expr`에 있는 리터럴의 수를 반환하는 함수를
작성하세요. 여기서 리터럴은 `True`와 `False`를 의미합니다.
```scala
test(countLiterals(be1), 2)
test(countLiterals(be2), 4)
test(countLiterals(be3), 5)
```

#### (문제 #17) `countNots` (5점)

하나의 논리식 `expr`를 받아서, `expr`에 있는 `Not` 연산자의 수를 반환하는 함수를
작성하세요.
```scala
test(countNots(be1), 0)
test(countNots(be2), 2)
test(countNots(be3), 3)
```

#### (문제 #18) `depth` (5점)

하나의 논리식 `expr`를 받아서, `expr`의 깊이를 반환하는 함수를 작성하세요.
여기서 논리식의 깊이는 논리식에 있는 연산자의 중첩 수를 의미합니다.
```scala
test(depth(be1), 1)
test(depth(be2), 3)
test(depth(be3), 5)
```

#### (문제 #19) `eval` (5점)

하나의 논리식 `expr`를 받아서, `expr`의 결과를 반환하는 함수를 작성하세요.
```scala
test(eval(be1), true)
test(eval(be2), false)
test(eval(be3), true)
```

#### (문제 #20) `getString` (5점)

하나의 논리식 `expr`를 받아서, `expr`의 문자열 표현을 반환하는 함수를
작성하세요:
- `True`는 `true`로 표현됩니다.
- `False`는 `false`로 표현됩니다.
- `And(left, right)`는 `(<left> & <right>)`로 표현됩니다.
- `Or(left, right)`는 `(<left> | <right>)`로 표현됩니다.
- `Not(expr)`는 `!<expr>`로 표현됩니다.
```scala
test(getString(be1), "(true | false)")
test(getString(be2), "(!(true | false) & !(false | true))")
test(getString(be3), "(!((false | !true) & false) & (true & !false))")
```
