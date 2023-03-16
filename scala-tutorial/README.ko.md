# Scala 튜토리얼

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 프로젝트를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

- [기본 타입 (10점)](#기본-타입-10점)
  - [(문제 #1) `volumeOfCuboid` (5점)](#문제-1-volumeofcuboid-5점)
  - [(문제 #2) `concat` (5점)](#문제-2-concat-5점)
- [함수 (15점)](#함수-15점)
  - [(문제 #3) `mulN` (5점)](#문제-3-muln-5점)
  - [(문제 #4) `twice` (5점)](#문제-4-twice-5점)
  - [(문제 #5) `compose` (5점)](#문제-5-compose-5점)
- [리스트 (10점)](#리스트-10점)
  - [(문제 #6) `double` (5점)](#문제-6-double-5점)
  - [(문제 #7) `product` (5점)](#문제-7-product-5점)
- [맵 (5점)](#맵-10점)
  - [(문제 #8) `getOrNotFound` (5점)](#문제-8-getornotfound-5점)
- [대수적 데이터 타입 (60점)](#대수적-데이터-타입-60점)
  - [(문제 #9) `depth` (15점)](#문제-9-depth-15점)
  - [(문제 #10) `sum` (15점)](#문제-10-sum-15점)
  - [(문제 #11) `countLeaves` (15점)](#문제-11-countleaves-15점)
  - [(문제 #12) `flatten` (15점)](#문제-12-flatten-15점)

## 기본 타입 (10점)

### (문제 #1) `volumeOfCuboid` (5점)

직육면체의 세 변의 길이를 나타내는 양의 정수 `a`, `b`, `c`를 받아서, 그
직육면체의 부피를 반환하는 함수를 작성하세요.
```scala
test(volumeOfCuboid(1, 3, 5), 15)
test(volumeOfCuboid(2, 3, 4), 24)
```

### (문제 #2) `concat` (5점)

두 문자열 `x`와 `y`를 받아서, 그 두 문자열을 이어붙인 문자열을 반환하는 함수를
작성하세요.
```scala
test(concat("x", "y"), "xy")
test(concat("abc", "def"), "abcdef")
```

## 함수 (15점)

### (문제 #3) `mulN` (5점)

정수 `n`을 받아서, 정수에 `n`을 곱하는 함수를 반환하는 함수를 작성하세요.
```scala
test(mulN(3)(5), 15)
test(mulN(4)(5), 20)
```

### (문제 #4) `twice` (5점)

`Int => Int` 타입의 함수 `f`를 받아서, `f`를 두 번 적용하는 함수를 반환하는
함수를 작성하세요.
```scala
test(twice(mulN(3))(5), 45)
test(twice(mulN(4))(5), 80)
```

### (문제 #5) `compose` (5점)

`Int => Int` 타입의 함수 `f`와 `g`를 받아서, `f`와 `g`를 합성한 함수 (`f ∘ g`)
를 반환하는 함수를 작성하세요.
```scala
test(compose(mulN(3), mulN(4))(5), 60)
test(compose(mulN(4), mulN(5))(5), 100)
```

## 리스트 (10점)

### (문제 #6) `double` (5점)

정수 리스트 `l`을 받아서, 그 리스트의 각 원소를 두 배한 리스트를 반환하는 함수를 작성하세요.
```scala
test(double(List(1, 2, 3)), List(2, 4, 6))
test(double(double(List(1, 2, 3, 4, 5))), List(4, 8, 12, 16, 20))
```

### (문제 #7) `product` (5점)

정수 리스트 `l`을 받아서, 그 리스트의 원소들의 곱을 반환하는 함수를 작성하세요.
```scala
test(product(List(1, 2, 3)), 6)
test(product(List(4, 2, 3, 7, 5)), 840)
```

## 맵 (10점)

### (문제 #8) `getOrNotFound` (5점)

문자열을 정수로 매핑하는 맵 `m`과 문자열 `s`를 받아서, `m`에 `s`에 대한 매핑이
있으면 그 매핑을, 그렇지 않으면 `"Not Found"`라는 메시지를 포함하는 오류를
던지는 함수를 작성하세요.

> :warning: `throw`를 직접 사용할 수 없습니다. 대신 `Template.scala`에 정의된
`error`를 사용하고, 자세한 내용은 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽어주세요.

```scala
val m: Map[String, Int] = Map("Park" -> 1, "Kim" -> 2)
test(getOrNotFound(m, "Park"), 1)
test(getOrNotFound(m, "Kim"), 2)
testExc(getOrNotFound(m, "Ryu"), "Not Found")
testExc(getOrNotFound(m, "Hong"), "Not Found")
```

## 대수적 데이터 타입 (60점)

`Tree` 타입은 트리를 나타내는 대수적 데이터 타입입니다.
`Tree` 타입은 `Branch` 타입 또는 `Leaf` 타입 중 하나입니다.
- `Branch` 타입은 정수 값(`value: Int`)과 자식 트리들(`children: List[Tree]`)을
  가집니다.
- `Leaf` 타입은 정수 값(`value: Int`)을 가집니다.

> :warning: `Branch`는 적어도 하나 이상의 자식을 가지고 있다고 가정합니다. (`branch.children.length >= 1`).

```scala
sealed trait Tree
case class Branch(value: Int, children: List[Tree]) extends Tree
case class Leaf(value: Int) extends Tree
```

> :warning: `Tree` 타입은 이미 `Template.scala`에 정의되어 있습니다. **절대로** `Tree`
타입을 `Implentation.scala`에 다시 정의하지 마세요.

예를 들어, 다음은 `Tree` 타입의 예제들입니다.
```scala
//     1
//   / | \
//  2  3  4
val t1: Tree = Branch(1, List(Leaf(2), Leaf(3), Leaf(4)))

//    1
//   / \
//  2   3
//     / \
//    4   5
val t2: Tree = Branch(1, List(Leaf(2), Branch(3, List(Leaf(4), Leaf(5)))))

//    5
//    |
//    4
//    |
//    3
//   / \
//  2   1
val t3: Tree = Branch(5, List(Branch(4, List(Branch(3, List(Leaf(2), Leaf(1)))))))
```

### (문제 #9) `depth` (15점)

하나의 트리 `t`를 받아서, 그 트리의 깊이를 반환하는 함수를 작성하세요. 트리의
깊이는 트리의 루트에서 가장 먼 `Leaf`까지의 거리입니다. 예를 들어, `t1`의 깊이는
1, `t2`의 깊이는 2, `t3`의 깊이는 3입니다.
```scala
test(depth(t1), 1)
test(depth(t2), 2)
test(depth(t3), 3)
```

### (문제 #10) `sum` (15점)

하나의 트리 `t`를 받아서, 그 트리의 모든 노드의 값을 더한 값을 반환하는 함수를
작성하세요. 예를 들어, `t1`의 합은 10, `t2`의 합은 15, `t3`의 합은 15입니다.
```scala
test(sum(t1), 10)
test(sum(t2), 15)
test(sum(t3), 15)
```

### (문제 #11) `countLeaves` (15점)

하나의 트리 `t`를 받아서, 그 트리의 모든 `Leaf` 노드의 개수를 반환하는 함수를
작성하세요. 예를 들어, `t1`의 `Leaf` 노드의 개수는 3, `t2`의 `Leaf` 노드의
개수는 3, `t3`의 `Leaf` 노드의 개수는 2입니다.
```scala
test(countLeaves(t1), 3)
test(countLeaves(t2), 3)
test(countLeaves(t3), 2)
```

### (문제 #12) `flatten` (15점)

하나의 트리 `t`를 받아서, 그 트리의 모든 노드의 값을 전위 순회(pre-order
traversal)한 결과를 리스트로 반환하는 함수를 작성하세요. 예를 들어, `t1`의 전위
순회 결과는 `List(1, 2, 3, 4)`, `t2`의 전위 순회 결과는 `List(1, 2, 3, 4, 5)`,
`t3`의 전위 순회 결과는 `List(5, 4, 3, 2, 1)`입니다.
```scala
test(flatten(t1), List(1, 2, 3, 4))
test(flatten(t2), List(1, 2, 3, 4, 5))
test(flatten(t3), List(5, 4, 3, 2, 1))
```
