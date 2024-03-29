# Common Instructions for Programming Assignments

[English](./README.md) | [한국어](./README.ko.md)

- [Assignments](#assignments)
- [Prerequisites](#prerequisites)
- [Download](#download)
- [Directory Structure](#directory-structure)
- [Restrictions](#restrictions)
- [Exceptions](#exceptions)
- [Testing Your Implementation](#testing-your-implementation)
  - [Writing Test Cases](#writing-test-cases)
  - [Running Test Cases](#running-test-cases)
  - [Interactively Testing Your Implementation](#interactively-testing-your-implementation)
- [Submission](#submission)
- [Grading](#grading)

This is a repository for assignment documents of courses by the [Programming Languages Research Group (PLRG)](https://plrg.korea.ac.kr/) at [Korea University](https://korea.ac.kr).

## Assignments

### Basics

| Name | Explanation |
| :--: | ----------- |
| [scala-tutorial](./scala-tutorial/README.md) | Scala Tutorial |

### [COSE212: Programming Languages](https://plrg.korea.ac.kr/courses/cose212/)

| Name | Explanation |
| :--: | ----------- |
| [ae](./cose212/ae/README.md) | `AE` - Arithmetic Expressions |
| [vae](./cose212/vae/README.md) | `VAE` - `AE` with Variables |
| [f1vae](./cose212/f1vae/README.md) | `F1VAE` - `VAE` with First-Order Functions |
| [fvae](./cose212/fvae/README.md) | `FVAE` - `VAE` with First-Class Functions |
| [fae](./cose212/fae/README.md) | `FAE` - `AE` with First-Class Functions |
| [rfae](./cose212/rfae/README.md) | `RFAE` - `FAE` with Recursion and Conditionals |
| [bfae](./cose212/bfae/README.md) | `BFAE` - `FAE` with Mutable Boxes |
| [mfae](./cose212/mfae/README.md) | `MFAE` - `FAE` with Mutable Variables |
| [lfae](./cose212/lfae/README.md) | `LFAE` - `FAE` with Lazy Evaluation |
| [fae-cps](./cose212/fae-cps/README.md) | `FAE-cps` - `FAE` with Continuation-Passing Style |
| [kfae](./cose212/kfae/README.md) | `KFAE` - `FAE` with First-Class Continuations |
| [tfae](./cose212/tfae/README.md) | `TFAE` - `FAE` with Type System |
| [trfae](./cose212/trfae/README.md) | `TRFAE` - `RFAE` with Type System |
| [atfae](./cose212/atfae/README.md) | `ATFAE` - `TRFAE` with Algebraic Data Types |
| [ptfae](./cose212/ptfae/README.md) | `PTFAE` - `TFAE` with Parametric Polymorphism |
| [stfae](./cose212/stfae/README.md) | `STFAE` - `TFAE` with Subtype Polymorphism |
| [tifae](./cose212/tifae/README.md) | `TIFAE` - `TRFAE` with Type Inference |

#### Projects

| Name | Explanation |
| :--: | ----------- |
| [cobalt](./cose212/cobalt/README.md) | `COBALT` - Comprehension-supported Boolean and Arithmetic Expression with Lists and Tuples |
| [magnet](./cose212/magnet/README.md) | `MAGNET` - Mutable Arithmetic Expressions with Generators and Exceptions |
| [battery](./cose212/battery/README.md) | `BATTERY` - Basic and Algebraic Data Type-supported Typed Expressions with Recursions and Polymorphism |

### [COSE215: Theory of Computation](https://plrg.korea.ac.kr/courses/cose215/)

| Name | Explanation |
| :--: | ----------- |
| [fa-examples](./cose215/fa-examples/README.md) | Finite Automata Examples |
| [equiv-re-fa](./cose215/equiv-re-fa/README.md) | Equivalence of Regular Expressions and Finite Automata |
| [equiv-pda-cfg](./cose215/equiv-pda-cfg/README.md) | Equivalence of Pushdown Automata and Context-Free Grammars |

## Prerequisites

* JDK >= 8 (<https://www.oracle.com/java/technologies/downloads/>)
* sbt (<https://www.scala-sbt.org/download.html>)

## Download

Run `sbt new ku-plrg-classroom/[assignment name].g8` in your terminal. The
assignment directory whose name is the same as the assignment will be created
under the current directory.

> [!WARNING]
>
> You should **REPLACE** `[assignment name]` with the **name of the assignment**
> or copy and paste the command from the document of each assignment.

Below is an example.

```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
...
name [Scala Tutorial]:
```

> [!NOTE]
>
> Please type `Enter` key if you want to use the default directory name.

## Directory Structure

You can find the following directories and files in the assignment directory:

<pre><code>src
├─ main/scala/kuplrg
│  ├─ Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
│  └─ Template.scala
└─ test/scala/kuplrg
   ├─ Spec.scala ──────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
   └─ SpecBase.scala</code></pre>

**DO NOT** edit files other than
`src/main/scala/kuplrg/Implementation.scala` and
`src/test/scala/kuplrg/Spec.scala`.

* `src/main/scala/kuplrg/Implementation.scala`:
You must fill this file to implement the required function(s).
It is enough to edit only this file to finish the assignment.

* `src/main/scala/kuplrg/Template.scala`:
This file contains the definitions of functions that you must implement to
complete the assignment. Please **DO NOT** edit this file.

* `src/main/scala/kuplrg/error.scala`:
This file defines `error` functions. Please refer to the
[Exceptions](#exceptions) section for more details.

* `src/test/scala/kuplrg/Spec.scala`
This file contains test cases. Please refer to the [Writing Test
Cases](#writing-test-cases) and [Running Test Cases](#running-test-cases)
sections to learn how to write and run your own test cases.

* `src/test/scala/kuplrg/SpecBase.scala`
This file contains a basic framework for test cases. You do not need to read or
edit this file.

## Restrictions

You **MUST NOT** use any of the following features in your implementation:

* `var`
* `while`
* `asInstanceOf`
* `isInstanceOf`
* `null`
* `return`
* `throw`
* `try-catch`
* mutable data structures (in `scala.collection.mutable`)

You can use any other features that are not explicitly forbidden. For example,

* You can define helper functions and additional types.
* You can use immutable data structures.
* You can use `for` comprehensions.
* You can mutate mutable variables/fields already defined in the provided code.

The use of the prohibited features will make your code not compile. If your code
compiles successfully, you are not using any prohibited features.

## Exceptions

While you cannot use `throw` and `try-catch` in your implementation, you can
throw an exception with an error message `s` by calling `error(s)` defined in
`Template.scala`. The exception will be of a type `PLError` defined in
`Template.scala`. To omit the message, you can use `error()`, instead of
`error("")`.

```scala
error()       // throws a `PLError` with the message ""
error("foo")  // throws a `PLError` with the message "foo"

def f(x: Int): Int =
  if (x < 0) error("x must be non-negative")
  else x

f(42)         // 42
f(-1)         // throws a `PLError` with the message "x must be non-negative"
```

You can test `PLError` exceptions by using `testExc`. Please refer to the
[Writing Test Cases](#writing-test-cases) section for more details.

## Testing Your Implementation

### Writing Test Cases

You can write your own test cases in `Spec.scala`. You can use the `check`,
`test`, and `testExc` functions to test your implementation:

* The `check` function takes any expression and checks if it normally terminates
    without any exceptions.
* The `test` function takes two expressions whose types are equal to each other
    and checks if they are equal.
* The `testExc` function takes an expression and a string. It checks if the
    expression throws a `PLError` exception whose message contains the given
    string.

For example, you can write the following test cases for the `f` function in
`Spec.scala`:

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

> [!WARNING]
>
> Passing all the provided tests does not guarantee that your implementation is
> correct. So, we **HIGHLY RECOMMEND** adding your own tests to check your
> implementation.

### Running Test Cases

Under the assignment directory, execute `sbt` to start an sbt console.

```
$ sbt
[info] welcome to sbt
...
sbt:...>
```

To run the test cases in `Spec.scala`, execute the `test` command.
Every test case will fail at the beginning.

```
sbt:...> test
...
[error] Failed tests:
[error]         kuplrg.Spec
```

After implementing every function correctly, every test will succeed.

```
sbt:...> test
...
[info] All tests passed.
```

If you want to watch the test results in real time, you can use the `~test`.
Then, `sbt` will run the test cases every time you save the file.

### Interactively Testing Your Implementation

If you want to test your implementation interactively, you can use the `console`
command in sbt. It will start a Scala REPL with the assignment implementation.
You can use any variables and functions defined in `Implementation.scala`.

```
sbt:...> console
...
scala> import kuplrg.*, Implementation.*

scala> ...
```

## Submission

Please use [Blackboard](https://kulms.korea.ac.kr/) to submit your code.
You must submit your `Implementation.scala` of each assignment.

> [!CAUTION]
>
> **If you copy and paste existing code, you will get an F.** You must work on
> each assignment by yourself without getting any help. You can refer to the
> lecture materials, including the lecture slides, or even other materials on
> the Internet. However, **you should be able to explain the details of your
> code. If not, you will get an F.**

We disallow late submissions. If you submit multiple times, only the last
submission will be graded.


## Grading

You can get maximum of 100 points. If your code uses a disallowed feature or
does not compile, you will get 0 points.

The test cases for the grading are similar to, but not the same as, those
provided in `Spec.scala`. You get more points as you pass more test cases. The
execution of each test case does not affect the others, e.g., the other test
cases will be graded properly even when your implementation runs forever for a
certain test case.
