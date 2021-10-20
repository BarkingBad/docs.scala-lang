---
title: "Experimental definitions"
next-page: /scala3/reference/changed-features
num: 51
type: section
previous-page: /scala3/reference/other-new-features/type-test
---

<!-- THIS FILE HAS BEEN GENERATED BY SCALADOC PREPROCESSOR.
    The whole process of generation the docs can be found under this README: https://github.com/lampepfl/dotty/blob/master/docs/README.md
    The source file can be found here https://github.com/lampepfl/dotty/edit/master/docs/docs/reference/other-new-features/experimental-defs.md
    NOTE THAT ANY CHANGES TO THIS FILE WILL BE OVERRIDEN BY PREPROCESSOR.
-->

## Experimental definitions

The `@experimental` annotation allows the definition of an API that is not guaranteed backward binary or source compatibility.
This annotation can be placed on term or type definitions.

### References to experimental definitions

Experimental definitions can only be referenced in an experimental scope. Experimental scopes are defined as follows.

(1) The RHS of an experimental `def`, `val`, `var`, `given` or `type` is an experimental scope.

<details>
<summary>Examples</summary>

<div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental
</span><span id="3" class="" >def x = ()
</span><span id="4" class="" >
</span><span id="5" class="" >def d1 = x // error: value x is marked @experimental and therefore ...
</span><span id="6" class="" >@experimental def d2 = x
</span><span id="7" class="" >
</span><span id="8" class="" >val v1 = x // error: value x is marked @experimental and therefore ...
</span><span id="9" class="" >@experimental val v2 = x
</span><span id="10" class="" >
</span><span id="11" class="" >var vr1 = x // error: value x is marked @experimental and therefore ...
</span><span id="12" class="" >@experimental var vr2 = x
</span><span id="13" class="" >
</span><span id="14" class="" >lazy val lv1 = x // error: value x is marked @experimental and therefore ...
</span><span id="15" class="" >@experimental lazy val lv2 = x
</span></code></pre></div><div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental
</span><span id="3" class="" >val x = ()
</span><span id="4" class="" >
</span><span id="5" class="" >@experimental
</span><span id="6" class="" >def f() = ()
</span><span id="7" class="" >
</span><span id="8" class="" >@experimental
</span><span id="9" class="" >object X:
</span><span id="10" class="" >  def fx() = 1
</span><span id="11" class="" >
</span><span id="12" class="" >def test1: Unit =
</span><span id="13" class="" >  f() // error: def f is marked @experimental and therefore ...
</span><span id="14" class="" >  x // error: value x is marked @experimental and therefore ...
</span><span id="15" class="" >  X.fx() // error: object X is marked @experimental and therefore ...
</span><span id="16" class="" >  import X.fx
</span><span id="17" class="" >  fx() // error: object X is marked @experimental and therefore ...
</span><span id="18" class="" >
</span><span id="19" class="" >@experimental
</span><span id="20" class="" >def test2: Unit =
</span><span id="21" class="" >  // references to f, x and X are ok because `test2` is experimental
</span><span id="22" class="" >  f()
</span><span id="23" class="" >  x
</span><span id="24" class="" >  X.fx()
</span><span id="25" class="" >  import X.fx
</span><span id="26" class="" >  fx()
</span></code></pre></div><div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental type E
</span><span id="3" class="" >
</span><span id="4" class="" >type A = E // error type E is marked @experimental and therefore ...
</span><span id="5" class="" >@experimental type B = E
</span></code></pre></div><div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental class A
</span><span id="3" class="" >@experimental type X
</span><span id="4" class="" >@experimental type Y = Int
</span><span id="5" class="" >@experimental opaque type Z = Int
</span><span id="6" class="" >
</span><span id="7" class="" >def test: Unit =
</span><span id="8" class="" >  new A // error: class A is marked @experimental and therefore ...
</span><span id="9" class="" >  val i0: A = ??? // error: class A is marked @experimental and therefore ...
</span><span id="10" class="" >  val i1: X = ??? // error: type X is marked @experimental and therefore ...
</span><span id="11" class="" >  val i2: Y = ??? // error: type Y is marked @experimental and therefore ...
</span><span id="12" class="" >  val i2: Z = ??? // error: type Y is marked @experimental and therefore ...
</span><span id="13" class="" >  ()
</span></code></pre></div><div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >@experimental
</span><span id="1" class="" >trait ExpSAM {
</span><span id="2" class="" >  def foo(x: Int): Int
</span><span id="3" class="" >}
</span><span id="4" class="" >def bar(f: ExpSAM): Unit = {} // error: error form rule 2
</span><span id="5" class="" >
</span><span id="6" class="" >def test: Unit =
</span><span id="7" class="" >  bar(x =&gt; x) // error: reference to experimental SAM
</span><span id="8" class="" >  ()
</span></code></pre></div>

</details>

(2.) The signatures of an experimental `def`, `val`, `var`, `given` and `type`, or constructors of `class` and `trait` are experimental scopes.

<details>
<summary>Examples</summary>

<div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental def x = 2
</span><span id="3" class="" >@experimental class A
</span><span id="4" class="" >@experimental type X
</span><span id="5" class="" >@experimental type Y = Int
</span><span id="6" class="" >@experimental opaque type Z = Int
</span><span id="7" class="" >
</span><span id="8" class="" >def test1(
</span><span id="9" class="" >  p1: A, // error: class A is marked @experimental and therefore ...
</span><span id="10" class="" >  p2: List[A], // error: class A is marked @experimental and therefore ...
</span><span id="11" class="" >  p3: X, // error: type X is marked @experimental and therefore ...
</span><span id="12" class="" >  p4: Y, // error: type Y is marked @experimental and therefore ...
</span><span id="13" class="" >  p5: Z, // error: type Z is marked @experimental and therefore ...
</span><span id="14" class="" >  p6: Any = x // error: def x is marked @experimental and therefore ...
</span><span id="15" class="" >): A = ??? // error: class A is marked @experimental and therefore ...
</span><span id="16" class="" >
</span><span id="17" class="" >@experimental def test2(
</span><span id="18" class="" >  p1: A,
</span><span id="19" class="" >  p2: List[A],
</span><span id="20" class="" >  p3: X,
</span><span id="21" class="" >  p4: Y,
</span><span id="22" class="" >  p5: Z,
</span><span id="23" class="" >  p6: Any = x
</span><span id="24" class="" >): A = ???
</span><span id="25" class="" >
</span><span id="26" class="" >class Test1(
</span><span id="27" class="" >  p1: A, // error
</span><span id="28" class="" >  p2: List[A], // error
</span><span id="29" class="" >  p3: X, // error
</span><span id="30" class="" >  p4: Y, // error
</span><span id="31" class="" >  p5: Z, // error
</span><span id="32" class="" >  p6: Any = x // error
</span><span id="33" class="" >) {}
</span><span id="34" class="" >
</span><span id="35" class="" >@experimental class Test2(
</span><span id="36" class="" >  p1: A,
</span><span id="37" class="" >  p2: List[A],
</span><span id="38" class="" >  p3: X,
</span><span id="39" class="" >  p4: Y,
</span><span id="40" class="" >  p5: Z,
</span><span id="41" class="" >  p6: Any = x
</span><span id="42" class="" >) {}
</span><span id="43" class="" >
</span><span id="44" class="" >trait Test1(
</span><span id="45" class="" >  p1: A, // error
</span><span id="46" class="" >  p2: List[A], // error
</span><span id="47" class="" >  p3: X, // error
</span><span id="48" class="" >  p4: Y, // error
</span><span id="49" class="" >  p5: Z, // error
</span><span id="50" class="" >  p6: Any = x // error
</span><span id="51" class="" >) {}
</span><span id="52" class="" >
</span><span id="53" class="" >@experimental trait Test2(
</span><span id="54" class="" >  p1: A,
</span><span id="55" class="" >  p2: List[A],
</span><span id="56" class="" >  p3: X,
</span><span id="57" class="" >  p4: Y,
</span><span id="58" class="" >  p5: Z,
</span><span id="59" class="" >  p6: Any = x
</span><span id="60" class="" >) {}
</span></code></pre></div>

</details>

(3.) The `extends` clause of an experimental `class`, `trait` or `object` is an experimental scope.

<details>
<summary>Examples</summary>

<div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental def x = 2
</span><span id="3" class="" >
</span><span id="4" class="" >@experimental class A1(x: Any)
</span><span id="5" class="" >class A2(x: Any)
</span><span id="6" class="" >
</span><span id="7" class="" >
</span><span id="8" class="" >@experimental class B1 extends A1(1)
</span><span id="9" class="" >class B2 extends A1(1) // error: class A1 is marked @experimental and therefore marked @experimental and therefore ...
</span><span id="10" class="" >
</span><span id="11" class="" >@experimental class C1 extends A2(x)
</span><span id="12" class="" >class C2 extends A2(x) // error def x is marked @experimental and therefore
</span></code></pre></div>

</details>

(4.) The body of an experimental `class`, `trait` or `object` is an experimental scope.

<details>
<summary>Examples</summary>

<div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental def x = 2
</span><span id="3" class="" >
</span><span id="4" class="" >@experimental class A {
</span><span id="5" class="" >  def f = x // ok because A is experimental
</span><span id="6" class="" >}
</span><span id="7" class="" >
</span><span id="8" class="" >@experimental class B {
</span><span id="9" class="" >  def f = x // ok because A is experimental
</span><span id="10" class="" >}
</span><span id="11" class="" >
</span><span id="12" class="" >@experimental object C {
</span><span id="13" class="" >  def f = x // ok because A is experimental
</span><span id="14" class="" >}
</span><span id="15" class="" >
</span><span id="16" class="" >@experimental class D {
</span><span id="17" class="" >  def f = {
</span><span id="18" class="" >    object B {
</span><span id="19" class="" >      x // ok because A is experimental
</span><span id="20" class="" >    }
</span><span id="21" class="" >  }
</span><span id="22" class="" >}
</span></code></pre></div>

</details>

(5.) Annotations of an experimental definition are in experimental scopes.

<details>
<summary>Examples</summary>

<div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental class myExperimentalAnnot extends scala.annotation.Annotation
</span><span id="3" class="" >
</span><span id="4" class="" >@myExperimentalAnnot // error
</span><span id="5" class="" >def test: Unit = ()
</span><span id="6" class="" >
</span><span id="7" class="" >@experimental
</span><span id="8" class="" >@myExperimentalAnnot
</span><span id="9" class="" >def test: Unit = ()
</span></code></pre></div>

</details>

(6.) Any code compiled using a _Nightly_ or _Snapshot_ version of the compiler is considered to be in an experimental scope.
Can use the `-Yno-experimental` compiler flag to disable it and run as a proper release.

In any other situation, a reference to an experimental definition will cause a compilation error.

### Experimental inheritance

All subclasses of an experimental `class` or `trait` must be marked as `@experimental` even if they are in an experimental scope.
Anonymous classes and SAMs of experimental classes are considered experimental.

We require explicit annotations to make sure we do not have completion or cycles issues with nested classes. This restriction could be relaxed in the future.

### Experimental overriding

For an overriding member `M` and overridden member `O`, if `O` is non-experimental then `M` must be non-experimental.

This makes sure that we cannot have accidental binary incompatibilities such as the following change.

```diff
class A:
  def f: Any = 1
class B extends A:
-  @experimental def f: Int = 2
```

### Test frameworks

Tests can be defined as experimental. Tests frameworks can execute tests using reflection even if they are in an experimental class, object or method.

<details>
<summary>Examples</summary>

Test that touch experimental APIs can be written as follows

<div class="snippet" ><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >import scala.annotation.experimental
</span><span id="1" class="" >
</span><span id="2" class="" >@experimental def x = 2
</span><span id="3" class="" >
</span><span id="4" class="" >class MyTests {
</span><span id="5" class="" >  /*@Test*/ def test1 = x // error
</span><span id="6" class="" >  @experimental /*@Test*/ def test2 = x
</span><span id="7" class="" >}
</span><span id="8" class="" >
</span><span id="9" class="" >@experimental
</span><span id="10" class="" >class MyExperimentalTests {
</span><span id="11" class="" >  /*@Test*/ def test1 = x
</span><span id="12" class="" >  /*@Test*/ def test2 = x
</span><span id="13" class="" >}
</span></code></pre></div>

</details>
