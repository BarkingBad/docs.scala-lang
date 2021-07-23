---
title: "Opaque Type Aliases"
type: section
num: 39
previous-page: /scala3/reference/other-new-features/export
next-page: /scala3/reference/other-new-features/open-classes
---

Opaque types aliases provide type abstraction without any overhead. Example:

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >object MyMath:
</span><span id="1" class="" >
</span><span id="2" class="" >  opaque type Logarithm = Double
</span><span id="3" class="" >
</span><span id="4" class="" >  object Logarithm:
</span><span id="5" class="" >
</span><span id="6" class="" >    // These are the two ways to lift to the Logarithm type
</span><span id="7" class="" >
</span><span id="8" class="" >    def apply(d: Double): Logarithm = math.log(d)
</span><span id="9" class="" >
</span><span id="10" class="" >    def safe(d: Double): Option[Logarithm] =
</span><span id="11" class="" >      if d &gt; 0.0 then Some(math.log(d)) else None
</span><span id="12" class="" >
</span><span id="13" class="" >  end Logarithm
</span><span id="14" class="" >
</span><span id="15" class="" >  // Extension methods define opaque types&apos; public APIs
</span><span id="16" class="" >  extension (x: Logarithm)
</span><span id="17" class="" >    def toDouble: Double = math.exp(x)
</span><span id="18" class="" >    def + (y: Logarithm): Logarithm = Logarithm(math.exp(x) + math.exp(y))
</span><span id="19" class="" >    def * (y: Logarithm): Logarithm = x + y
</span><span id="20" class="" >
</span><span id="21" class="" >end MyMath
</span></code></pre><div class="snippet-meta"><div class="snippet-label">MyMath.scala</div></div></div>

This introduces `Logarithm` as a new abstract type, which is implemented as `Double`.
The fact that `Logarithm` is the same as `Double` is only known in the scope where
`Logarithm` is defined which in the above example corresponds to the object `MyMath`.
Or in other words, within the scope it is treated as type alias, but this is opaque to the outside world
where in consequence `Logarithm` is seen as an abstract type and has nothing to do with `Double`.

The public API of `Logarithm` consists of the `apply` and `safe` methods defined in the companion object.
They convert from `Double`s to `Logarithm` values. Moreover, an operation `toDouble` that converts the other way, and operations `+` and `*` are defined as extension methods on `Logarithm` values.
The following operations would be valid because they use functionality implemented in the `MyMath` object.

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span name="MyMath.scala" id="1" class="hideable include" >object MyMath:
</span><span name="MyMath.scala" id="2" class="hideable include" >
</span><span name="MyMath.scala" id="3" class="hideable include" >  opaque type Logarithm = Double
</span><span name="MyMath.scala" id="4" class="hideable include" >
</span><span name="MyMath.scala" id="5" class="hideable include" >  object Logarithm:
</span><span name="MyMath.scala" id="6" class="hideable include" >
</span><span name="MyMath.scala" id="7" class="hideable include" >    // These are the two ways to lift to the Logarithm type
</span><span name="MyMath.scala" id="8" class="hideable include" >
</span><span name="MyMath.scala" id="9" class="hideable include" >    def apply(d: Double): Logarithm = math.log(d)
</span><span name="MyMath.scala" id="10" class="hideable include" >
</span><span name="MyMath.scala" id="11" class="hideable include" >    def safe(d: Double): Option[Logarithm] =
</span><span name="MyMath.scala" id="12" class="hideable include" >      if d &gt; 0.0 then Some(math.log(d)) else None
</span><span name="MyMath.scala" id="13" class="hideable include" >
</span><span name="MyMath.scala" id="14" class="hideable include" >  end Logarithm
</span><span name="MyMath.scala" id="15" class="hideable include" >
</span><span name="MyMath.scala" id="16" class="hideable include" >  // Extension methods define opaque types&apos; public APIs
</span><span name="MyMath.scala" id="17" class="hideable include" >  extension (x: Logarithm)
</span><span name="MyMath.scala" id="18" class="hideable include" >    def toDouble: Double = math.exp(x)
</span><span name="MyMath.scala" id="19" class="hideable include" >    def + (y: Logarithm): Logarithm = Logarithm(math.exp(x) + math.exp(y))
</span><span name="MyMath.scala" id="20" class="hideable include" >    def * (y: Logarithm): Logarithm = x + y
</span><span name="MyMath.scala" id="21" class="hideable include" >
</span><span name="MyMath.scala" id="22" class="hideable include" >end MyMath
</span><span name="MyMath.scala" id="23" class="hideable include" >
</span><span id="25" class="" >import MyMath.Logarithm
</span><span id="26" class="" >
</span><span id="27" class="" >val l = Logarithm(1.0)
</span><span id="28" class="" >val l2 = Logarithm(2.0)
</span><span id="29" class="" >val l3 = l * l2
</span><span id="30" class="" >val l4 = l + l2
</span></code></pre></div>

But the following operations would lead to type errors:

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span name="MyMath.scala" id="1" class="hideable include" >object MyMath:
</span><span name="MyMath.scala" id="2" class="hideable include" >
</span><span name="MyMath.scala" id="3" class="hideable include" >  opaque type Logarithm = Double
</span><span name="MyMath.scala" id="4" class="hideable include" >
</span><span name="MyMath.scala" id="5" class="hideable include" >  object Logarithm:
</span><span name="MyMath.scala" id="6" class="hideable include" >
</span><span name="MyMath.scala" id="7" class="hideable include" >    // These are the two ways to lift to the Logarithm type
</span><span name="MyMath.scala" id="8" class="hideable include" >
</span><span name="MyMath.scala" id="9" class="hideable include" >    def apply(d: Double): Logarithm = math.log(d)
</span><span name="MyMath.scala" id="10" class="hideable include" >
</span><span name="MyMath.scala" id="11" class="hideable include" >    def safe(d: Double): Option[Logarithm] =
</span><span name="MyMath.scala" id="12" class="hideable include" >      if d &gt; 0.0 then Some(math.log(d)) else None
</span><span name="MyMath.scala" id="13" class="hideable include" >
</span><span name="MyMath.scala" id="14" class="hideable include" >  end Logarithm
</span><span name="MyMath.scala" id="15" class="hideable include" >
</span><span name="MyMath.scala" id="16" class="hideable include" >  // Extension methods define opaque types&apos; public APIs
</span><span name="MyMath.scala" id="17" class="hideable include" >  extension (x: Logarithm)
</span><span name="MyMath.scala" id="18" class="hideable include" >    def toDouble: Double = math.exp(x)
</span><span name="MyMath.scala" id="19" class="hideable include" >    def + (y: Logarithm): Logarithm = Logarithm(math.exp(x) + math.exp(y))
</span><span name="MyMath.scala" id="20" class="hideable include" >    def * (y: Logarithm): Logarithm = x + y
</span><span name="MyMath.scala" id="21" class="hideable include" >
</span><span name="MyMath.scala" id="22" class="hideable include" >end MyMath
</span><span name="MyMath.scala" id="23" class="hideable include" >
</span><span id="25" class="" >import MyMath.Logarithm
</span><span id="26" class="" >
</span><span id="27" class="" >val l = Logarithm(1.0)
</span><span id="28" class="snippet-error tooltip" label="Found:    (Snippet0.this.l : Snippet0.this.MyMath.Logarithm)
Required: Double">val d: Double = l       // error: found: Logarithm, required: Double
</span><span id="29" class="snippet-error tooltip" label="Found:    (1.0d : Double)
Required: Snippet0.this.MyMath.Logarithm">val l2: Logarithm = 1.0 // error: found: Double, required: Logarithm
</span><span id="30" class="snippet-error tooltip" label="Found:    (2 : Int)
Required: Snippet0.this.MyMath.Logarithm">l * 2                   // error: found: Int(2), required: Logarithm
</span><span id="31" class="snippet-error tooltip" label="value / is not a member of Snippet0.this.MyMath.Logarithm">l / l2                  // error: `/` is not a member of Logarithm
</span></code></pre></div>

### Bounds For Opaque Type Aliases

Opaque type aliases can also come with bounds. Example:

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >object Access:
</span><span id="1" class="" >
</span><span id="2" class="" >  opaque type Permissions = Int
</span><span id="3" class="" >  opaque type PermissionChoice = Int
</span><span id="4" class="" >  opaque type Permission &lt;: Permissions &amp; PermissionChoice = Int
</span><span id="5" class="" >
</span><span id="6" class="" >  extension (x: Permissions)
</span><span id="7" class="" >    def &amp; (y: Permissions): Permissions = x | y
</span><span id="8" class="" >  extension (x: PermissionChoice)
</span><span id="9" class="" >    def | (y: PermissionChoice): PermissionChoice = x | y
</span><span id="10" class="" >  extension (granted: Permissions)
</span><span id="11" class="" >    def is(required: Permissions) = (granted &amp; required) == required
</span><span id="12" class="" >  extension (granted: Permissions)
</span><span id="13" class="" >    def isOneOf(required: PermissionChoice) = (granted &amp; required) != 0
</span><span id="14" class="" >
</span><span id="15" class="" >  val NoPermission: Permission = 0
</span><span id="16" class="" >  val Read: Permission = 1
</span><span id="17" class="" >  val Write: Permission = 2
</span><span id="18" class="" >  val ReadWrite: Permissions = Read | Write
</span><span id="19" class="" >  val ReadOrWrite: PermissionChoice = Read | Write
</span><span id="20" class="" >
</span><span id="21" class="" >end Access
</span></code></pre><div class="snippet-meta"><div class="snippet-label">Access.scala</div></div></div>

The `Access` object defines three opaque type aliases:

- `Permission`, representing a single permission,
- `Permissions`, representing a set of permissions with the meaning "all of these permissions granted",
- `PermissionChoice`, representing a set of permissions with the meaning "at least one of these permissions granted".

Outside the `Access` object, values of type `Permissions` may be combined using the `&` operator,
where `x & y` means "all permissions in `x` *and* in `y` granted".
Values of type `PermissionChoice` may be combined using the `|` operator,
where `x | y` means "a permission in `x` *or* in `y` granted".

Note that inside the `Access` object, the `&` and `|` operators always resolve to the corresponding methods of `Int`,
because members always take precedence over extension methods.
Because of that, the `|` extension method in `Access` does not cause infinite recursion.
Also, the definition of `ReadWrite` must use `|`,
even though an equivalent definition outside `Access` would use `&`.

All three opaque type aliases have the same underlying representation type `Int`. The
`Permission` type has an upper bound `Permissions & PermissionChoice`. This makes
it known outside the `Access` object that `Permission` is a subtype of the other
two types.  Hence, the following usage scenario type-checks.

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span name="Access.scala" id="1" class="hideable include" >object Access:
</span><span name="Access.scala" id="2" class="hideable include" >
</span><span name="Access.scala" id="3" class="hideable include" >  opaque type Permissions = Int
</span><span name="Access.scala" id="4" class="hideable include" >  opaque type PermissionChoice = Int
</span><span name="Access.scala" id="5" class="hideable include" >  opaque type Permission &lt;: Permissions &amp; PermissionChoice = Int
</span><span name="Access.scala" id="6" class="hideable include" >
</span><span name="Access.scala" id="7" class="hideable include" >  extension (x: Permissions)
</span><span name="Access.scala" id="8" class="hideable include" >    def &amp; (y: Permissions): Permissions = x | y
</span><span name="Access.scala" id="9" class="hideable include" >  extension (x: PermissionChoice)
</span><span name="Access.scala" id="10" class="hideable include" >    def | (y: PermissionChoice): PermissionChoice = x | y
</span><span name="Access.scala" id="11" class="hideable include" >  extension (granted: Permissions)
</span><span name="Access.scala" id="12" class="hideable include" >    def is(required: Permissions) = (granted &amp; required) == required
</span><span name="Access.scala" id="13" class="hideable include" >  extension (granted: Permissions)
</span><span name="Access.scala" id="14" class="hideable include" >    def isOneOf(required: PermissionChoice) = (granted &amp; required) != 0
</span><span name="Access.scala" id="15" class="hideable include" >
</span><span name="Access.scala" id="16" class="hideable include" >  val NoPermission: Permission = 0
</span><span name="Access.scala" id="17" class="hideable include" >  val Read: Permission = 1
</span><span name="Access.scala" id="18" class="hideable include" >  val Write: Permission = 2
</span><span name="Access.scala" id="19" class="hideable include" >  val ReadWrite: Permissions = Read | Write
</span><span name="Access.scala" id="20" class="hideable include" >  val ReadOrWrite: PermissionChoice = Read | Write
</span><span name="Access.scala" id="21" class="hideable include" >
</span><span name="Access.scala" id="22" class="hideable include" >end Access
</span><span name="Access.scala" id="23" class="hideable include" >
</span><span id="25" class="" >object User:
</span><span id="26" class="" >  import Access.*
</span><span id="27" class="" >
</span><span id="28" class="" >  case class Item(rights: Permissions)
</span><span id="29" class="" >
</span><span id="30" class="" >  val roItem = Item(Read)  // OK, since Permission &lt;: Permissions
</span><span id="31" class="" >  val rwItem = Item(ReadWrite)
</span><span id="32" class="" >  val noItem = Item(NoPermission)
</span><span id="33" class="" >
</span><span id="34" class="" >  assert(!roItem.rights.is(ReadWrite))
</span><span id="35" class="" >  assert(roItem.rights.isOneOf(ReadOrWrite))
</span><span id="36" class="" >
</span><span id="37" class="" >  assert(rwItem.rights.is(ReadWrite))
</span><span id="38" class="" >  assert(rwItem.rights.isOneOf(ReadOrWrite))
</span><span id="39" class="" >
</span><span id="40" class="" >  assert(!noItem.rights.is(ReadWrite))
</span><span id="41" class="" >  assert(!noItem.rights.isOneOf(ReadOrWrite))
</span><span id="42" class="" >end User
</span></code></pre></div>

On the other hand, the call `roItem.rights.isOneOf(ReadWrite)` would give a type error
since `Permissions` and `PermissionChoice` are different, unrelated types outside `Access`.

### Opaque Type Members on Classes

While typically, opaque types are used together with objects to hide implementation details of a module, they can also be used with classes.

For example, we can redefine the above example of Logarithms as a class.

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span id="0" class="" >class Logarithms:
</span><span id="1" class="" >
</span><span id="2" class="" >  opaque type Logarithm = Double
</span><span id="3" class="" >
</span><span id="4" class="" >  def apply(d: Double): Logarithm = math.log(d)
</span><span id="5" class="" >
</span><span id="6" class="" >  def safe(d: Double): Option[Logarithm] =
</span><span id="7" class="" >    if d &gt; 0.0 then Some(math.log(d)) else None
</span><span id="8" class="" >
</span><span id="9" class="" >  def mul(x: Logarithm, y: Logarithm) = x + y
</span></code></pre><div class="snippet-meta"><div class="snippet-label">Logarithms.scala</div></div></div>

Opaque type members of different instances are treated as different:

<div class="snippet"><div class="buttons"></div><pre><code class="language-scala"><span name="Logarithms.scala" id="1" class="hideable include" >class Logarithms:
</span><span name="Logarithms.scala" id="2" class="hideable include" >
</span><span name="Logarithms.scala" id="3" class="hideable include" >  opaque type Logarithm = Double
</span><span name="Logarithms.scala" id="4" class="hideable include" >
</span><span name="Logarithms.scala" id="5" class="hideable include" >  def apply(d: Double): Logarithm = math.log(d)
</span><span name="Logarithms.scala" id="6" class="hideable include" >
</span><span name="Logarithms.scala" id="7" class="hideable include" >  def safe(d: Double): Option[Logarithm] =
</span><span name="Logarithms.scala" id="8" class="hideable include" >    if d &gt; 0.0 then Some(math.log(d)) else None
</span><span name="Logarithms.scala" id="9" class="hideable include" >
</span><span name="Logarithms.scala" id="10" class="hideable include" >  def mul(x: Logarithm, y: Logarithm) = x + y
</span><span name="Logarithms.scala" id="11" class="hideable include" >
</span><span id="13" class="" >val l1 = new Logarithms
</span><span id="14" class="" >val l2 = new Logarithms
</span><span id="15" class="" >val x = l1(1.5)
</span><span id="16" class="" >val y = l1(2.6)
</span><span id="17" class="" >val z = l2(3.1)
</span><span id="18" class="" >l1.mul(x, y) // type checks
</span><span id="19" class="snippet-error tooltip" label="Found:    (Snippet0.this.z : Snippet0.this.l2.Logarithm)
Required: Snippet0.this.l1.Logarithm">l1.mul(x, z) // error: found l2.Logarithm, required l1.Logarithm
</span></code></pre></div>

In general, one can think of an opaque type as being only transparent in the scope of `private[this]`.

[More details](opaques-details.md)
