# `MAGNET` - Mutable Arithmetic Expressions with Generators and Exceptions

[English](./README.md) | [한국어](./README.ko.md)

다음과 같이 `sbt`를 이용하여 템플릿 코드를 다운로드 받으세요:
```bash
sbt new ku-plrg-classroom/magnet.g8
```

> :warning: 아직 [공용 지침서](https://github.com/ku-plrg-classroom/docs/blob/main/README.ko.md)를 읽지 않았다면, 이 문서부터 읽어주세요.

템플릿 코드는 다음과 같은 파일들을 포함합니다:
<pre><code>magnet
└─ src
   ├─ main/scala/kuplrg
   │  ├── MAGNET.scala ────────── MAGNET의 정의와 파서들
   │  ├── Implementation.scala ── <b style='color:red;'>[[ 이 파일을 수정하고 제출하세요. ]]</b>
   │  ├── Template.scala ──────── 구현해야 할 함수들의 템플릿
   │  └── error.scala ─────────── `error` 함수의 정의
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ 이 파일에 테스트 케이스를 추가하세요. ]]</b>
      └─ SpecBase.scala ───────── 테스트 케이스의 공통 기능</code></pre>

`MAGNET` 언어는 제너레이터(generator)와 예외(exception)를 지원하는 가변(mutable)
산술 표현식 언어입니다.  이 과제에서는 `interp`과 `bodyOfSquares` 함수를
구현합니다.

## `MAGNET` 언어의 명세

`MAGNET` 언어의 문법(syntax)과 의미가(semantics)가 궁금하면,
[`magnet-spec.pdf`](./magnet-spec.pdf) 참조하세요.


### 실행 오류

만약 `MAGNET` 표현식의 의미가 정의되지 않는다면, `reduce` 함수는 `error`
함수를 호출하여 실행 오류를 발생시켜야 합니다.  예를 들어, 다음과 같은 표현식은
실행 오류를 발생시켜야 합니다:
```scala
eval("1 + true")
```
하지만, 오류 메시지는 검사 대상이 아니며, 오직 `error` 함수를 호출하여 오류를
발생시키는지 여부만 검사합니다.


## (문제 #1) `reduce` (80 점)

`eval` 함수는 `MAGNET` 언어의 인터프리터(interpreter)로, 문자열 형태의 표현식을
인자로 받아 `reduce` 함수를 반복적으로 호출하여 계산을 수행합니다:
```scala
def eval(str: String, debug: Boolean = false): String =
  def aux(st: State): Value =
    if (debug) println("\n" + "-" * 40 + "\n" + st.str)
    reduce(st) match
      case State(Nil, List(v), _, _) =>
        if (debug) println("\n" + "-" * 40 + "\n" + "Result: " + v)
        v
      case st => aux(st)
  val initSt = State(IEval(Map(), Expr(str)) :: Nil, Nil, Map(), Map())
  aux(initSt).str
```
> 추가적으로, `eval` 함수는 `debug` 모드를 지원하며, 각 단계에서 인터프리터의
> 상태를 출력합니다.  예를 들어, 다음과 같은 코드는 해당 표현식을 계산하는
> 과정을 출력합니다:
> ```scala
> eval("1 + 2 * 3", debug = true)
> ```
> `reduce` 함수의 구현이 완료되면, 다음과 같은 출력을 얻을 수 있습니다:
> ```
> ----------------------------------------
> * cont   : eval[_ |- (1 + (2 * 3))] :: []
> * stack  : []
> * handler:
> * mem    : []
> 
> ----------------------------------------
> * cont   : eval[_ |- 1] :: eval[_ |- (2 * 3)] :: (+) :: []
> * stack  : []
> * handler:
> * mem    : []
> 
> ...
> 
> ----------------------------------------
> Result: 7
> ```

`reduce` 함수는 `MAGNET` 언어의 상태를 인자로 받아, 다음 상태를 반환합니다:
```scala
def reduce(st: State): State = ???
```

> 다음의 함수들이 `Implementation.scala` 파일에 정의되어 있으니, 필요하다면
> `reduce` 함수 구현 시 사용하셔도 됩니다:
> ```scala
> def malloc(mem: Mem, n: Int): List[Addr] =
>   val a = malloc(mem)
>   (0 until n).toList.map(a + _)
>
> def malloc(mem: Mem): Addr = mem.keySet.maxOption.fold(0)(_ + 1)
>
> def lookup(env: Env, x: String): Addr =
>   env.getOrElse(x, error(s"free identifier: $x"))
>
> def lookup(handler: Handler, x: Control): KValue =
>   handler.getOrElse(x, error(s"invalid control operation: $x"))
>
> def eq(l: Value, r: Value): Boolean = (l, r) match
>   case (UndefV, UndefV) => true
>   case (NumV(l), NumV(r)) => l == r
>   case (BoolV(l), BoolV(r)) => l == r
>   case (IterV(l), IterV(r)) => l == r
>   case (ResultV(lv, ld), ResultV(rv, rd)) => eq(lv, rv) && ld == rd
>   case _ => false
> ```

**`Implementation.scala` 파일에 `reduce` 함수를 구현하세요.**


## (문제 #2) `bodyOfSquares` (20 점)

`reduce` 함수의 모든 테스트를 통과한 후, `bodyOfSquares` 함수를 구현합니다.

> :warning: 만약 `reduce` 함수의 테스트를 하나라도 통과하지 못한다면,
> `bodyOfSquares` 함수는 테스트되지 않습니다.  따라서, `bodyOfSquares` 함수를
> 구현하기 전에 `reduce` 함수의 모든 테스트를 통과해야 합니다.

`bodyOfSquares` 함수는 `MAGNET` 언어로 작성된 `squares` 함수의 몸체를
의미합니다.
```scala
def squareSumExpr(n: Int, m: Int): String = s"""
  function* squares(from, to) {
    $bodyOfSquares
  }
  var sum = 0;
  for (x of squares($n, $m)) { sum += x; };
  sum
"""

def bodyOfSquares(n: Int): String = ???
```
`squares` 함수는 `from`에서 `to`까지의 숫자들의 제곱을 생성하는 제너레이터
함수입니다.

> :warning: `squares` 함수는 일반 함수가 아닌 이 언어에서 추가된 **제너레이터**
> 함수인 점에 유의하세요. `MAGNET` 언어의 [명세](./magnet-spec.pdf)와
> `Spec.scala`에 주어진 테스트를 참조하여 제너레이터 함수의 의미를 이해해
> 보세요.

**`Implementation.scala` 파일에 `bodyOfSquares` 함수를 구현하세요.**
