# Kotlin style guide of FRESH!

## Vision
* Write Kotlin like code. (Lamda, Collections, Extension functions)
  - Use lambda expression
  - Use extension functions (i.g. scope functions)

* Write shorter
  - Code that is difficult to read for shortening doesn't mean anything
  - Bad code if you need to think

## Rule

* Use a formatter in `/.idea/codeStyleSttings.xml`
* Start a new line at right vertica line on android studio. (about 130 characters)   

### Coding rules

* bad  
	not recommend
* not good  
  No problem in Kotlin but it is not coding rules in FRESH!
* good  
  recommend in FRESH!

Both `bad`, `not good` and `good` are correct in Kotlin.  
These definitions are on coding rules.

## Idiom

### Use an IDE suggestion

```Kotlin
val array = ArrayList<Int>()

// bad
array.get(0)    

// good
array[0]


// bad
getActivity().finish()

// good
activity.finish()
```

### Use a smart cast

```Kotlin
fun hoge(value: Boolean?) {
    value ?: return
    if (value) {

    }
}

fun hoge(context: Context) {
    context as Activity
    context.finish() // finish activity
}

// bad
if (hoge !is Hoge) {
    throw IllegalArgumentException("not Hoge!")
}
hoge.foo()

// good
hoge as? Hoge ?: throw IllegalArgumentException("not Hoge!")
hoge.foo()
```

### Line break

* Start a new line at right vertical line on Android studio. (About 130 characters)

![RightVerticalLine][AndroidStudio]

* Start a new line by a symbol which are `,`, `:`, `{`, `=` in case characters over vertical line.

```Kotlin
fun hoge(aaa: Int, bbb: Int, ccc: Int, ddd: Int, eee: Int,
     fff: Int, ggg: Int) {
}

fun hoge(aaa: Int, bbb: Int, ccc: Int, ddd: Int, eee: Int) =
   hoge().apply {
   }

data class Hoge(private val aaa: Int, private val bbb: Int) :
         AbstractHoge() {
}
```

### `it` in Lambda expression applies to wide scope.

```Kotlin
// bad
{ hoge -> 
  hoge?.let { it.foo() }
}

// good
{ 
  it?.let { hoge -> hoge.foo() }
}
```

### Should use a type interface

You can write a type if it is difficult to understand.

* property

```Kotlin
val hoge = 0  // Int
val foo = 10L // Long
val bar = 100f //Float
```

* function

```Kotlin
// return Boolean
fun Context.isConnectToWifi() =
      (getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager)
         .activeNetworkInfo?.type == ConnectivityManager.TYPE_WIFI

// return Point
fun Display.getSize(): Point = Point().apply { getSize(this) }
```

### Don't write for loop

You don't have to write for loop because threre is `forEach` in collections package of Kotlin.

```Kotlin
// not good
for (i in 0..9) {
}

// good
(0..9).forEach {
}

// good (if you want to know index)
(0..9).forEachIndexed { index, value ->
}
```

### Use `to` expression creating `Pair` class

```Kotlin
// bad
val pair = Pair(foo, bar) 

// good 
val pair = foo to bar
```

* `to` is infix function

`public infix fun <A, B> A.to(that: B): Pair<A, B> = Pair(this, that)`

### Use `range`

```Kotlin
val char = 'K'

// bad
if (char >= 'A' && char <= 'Z') print("Hit!")

// good
if (char in 'A'..'Z') print("Hit!")

when (char) {
   in 'A'..'Z' -> print("Hit!")
   else -> return
}
```

### `when`

#### Use `when` in case there are two or more branches of if-else

```Kotlin
// bad
val hoge = 10if (hoge > 10) {

} else if (hoge > 5) {

} else if (hoge > 0) {

} else {

}

// good
when {
    hoge > 10 -> print("10")
    hoge > 5 -> print("0")
    hoge > 0 -> print("0")
    else -> print("else")
}
```

#### Use `is` in case of comparing a class type

```Kotlin
val hoge: Hoge = Hoge()
when (hoge) {
    is Hoge -> {

    }
    else -> {
       
    }
}
```

#### Use `range` in case comparing Int values

```Kotlin
val hoge = 10
when (hoge) {
    in 0..4 -> print("0..4")
    in 5..10 -> print("5..10")
}
```

### Don't start a new line in variable declaration using if-else 

```Kotlin
val foo: Int = 5

// bad
val bar = if (foo > 10) {
    "kotlin"
} else {
    "java"
}

// good
val bar = if(foo > 10) "kotlin" else "java"
```

## Null

### Don't use `!!`

Don't use `!!`, it will erase the benefits of Kotlin.  
You use it only when you want to explicitly raise a Null Pointer Exception.

### Use a scope function in case of checking a null value

```Kotlin
class Hoge {
    fun fun1() {}
    fun fun2() {}
    fun fun3() = true
}

var hoge: Hoge? = null

// not good
if (hoge != null) {
    hoge.fun1()
} else {
    val hoge = Hoge()
    hoge.fun1()
    hoge.fun2()
}

// good
hoge?.run { 
   fun1()
} ?: run {
    hoge = Hoge().apply {
        fun1()
        fun2()
    }
}

// good
if (hoge != null && hoge.fun3()) {
    hoge.fun1()
} else {
    hoge = Hoge().apply {
       fun1()
       fun2()
    }
}
```

## Property

### Property rules

* Use non-null value as much as possible
* Don't use a m-prefix
* Property priority
  - non-null & val
  - non-null & var
  - nullable & var

Use a `lateinit` or `Delegates.notNull()` if you cannot set a initial value.  
`lateinit` is better than `Delegates.notNull()` because `Delegates.notNull()` uses reflection.
But primitive values is not applied to `lateinit`.

```Kotlin
// non-null & val
private val hoge: Hoge = Hoge()
private val drawablePadding: Int by lazy { activity.resources.getDimensionPixelSize(R.dimen.drawable_padding) }

// non-null & var
private lateinit var hoge: Hoge
private var hoge: Hoge = Delegates.notNull()

// nullable & var
private var hoge: Hoge? = null
```

### Constant

Use `const val` for `static final` in Java.
But primitive values is not applied to `const`.

Write a `companion object` block under class declaration.


## Scope function

### Should use scope function

```Kotlin
class Hoge {
    fun fun1() {}
    fun fun2() {}
}

// bad
val hoge = Hoge()
hoge.fun1()
hoge.fun2()

// good
val hoge = Hoge().apply {
   fun1()
   fun2()
}
```

You don't need to use `with` because you can substitute `run`, `let`, `apply`, `also`.

### Don't declare variables that used only once

You can express using a scope function.  
But it becomes hard to read in case it excessibely use.  
You should declare variables if you need to think.

```Kotlin
class Foo

class Bar(val foo: Foo)

class Hoge {
    fun fun1() {}
    fun fun2(bar: Bar) {}
    fun fun3(foo: Foo) {}
}

// bad
val hoge = Hoge()
val foo = Foo()
val bar = Bar(foo)
hoge.fun1()
hoge.fun2(bar)
hoge.fun3(foo)

// good
Hoge().run {    
   fun1()
   Foo().let {        
       fun2(Bar(it))
       fun3(it)
   }
}
```

### which use `run` or `let`

* Use `let` to substitute into functions
* Use `run` to use outside functions

```Kotlin
class Foo

class Bar(val foo: Foo)

class Hoge {
    fun fun1() {}
    fun fun2(bar: Bar) {}
    fun fun3(foo: Foo) {}
}

// not good
Hoge().let {
   it.fun1()
   Foo().run {
       it.fun2(Bar(this))
       it.fun3(this)
   }
}

// good
Hoge().run {    
   fun1()
   Foo().let {        
       fun2(Bar(it))
       fun3(it)
   }
}
```

## Function

### Omit a type of return value

Basically, we omit a type of return value as declaration variables using type interface.  
You should write it in case we cannot guess from a method name.  

```Kotlin
fun createIntent(context: Context, foo: Int, bar: Boolean) = 
Intent(context, HogeActivity::class.java).apply {    
   putExtra("foo", foo)
   putExtra("bar", bar)
}

fun hoge(value: Int): String = when (value) {
   in 0..10 -> "foo"
   in 100..500 -> "bar"
   else -> "else"
}
```


### Don't name `setHoge`, `getHoge`

Don't name `setHoge`, `getHoge` at property and function because it is used in Kotlin language.

```Kotlin
// bad
fun setHoge() {
}
```

### Use named arguments for function overloading

```Kotlin
// bad
class Hoge {
   fun hoge() {
      print("hoge")
   }

   fun hoge(prefix: String) {
       print(prefix + "hoge")
   }
}

// good
class Hoge {
   fun hoge(prefix: String = "") {
      print(prefix + "hoge")
   }
}
```

### Use typealias

```Kotlin
// bad
interface CallBackListener {
   fun onHoge(foo: String, bar: Int)
}

// caller
var callback: CallBackListener? = null
callback?.onHoge("foo", 100)

// callee
val callback = object : CallBackListener {
  override fun onHoge(foo: String, bar: Int) {
    print("$foo : $bar")
  }
}

// good
typealias CallBackListener = (foo: String, bar: Int) -> Unit

// caller
var callback: CallBackListener? = null
callback?.invoke("foo", 100)

// callee
val callback = { foo, bar -> 
  print("$foo : $bar")
}
```

### Extension functions

* Use it as utility functions in Java
* It is not necessary to use extension functions in case it is defined wide scope.
 i.g. String
 In case we recommend the private extension functions.

* Extension class name : `HogeExt.kt`

* Structure of package 

```
main
 |-data
 |-ui
   |-util
     |-ext
 |-util
   |-ext
```

### Private extension functions

Functions taking only one object as an argument replace private extension functions. (not necessary)

```Kotlin
enum class Hoge() {
    FOO,
    BAR,
    NONE
}

// not good
fun toHoge(arg: String): Hoge {
    if (!arg.startsWith("hoge")) {
        return Hoge.NONE    
    }
    return when (arg) {
       "hogeFoo" -> Hoge.FOO
       "hogeBar" -> Hoge.BAR
       else -> Hoge.NONE
    }
}

// good
fun String.toHoge(): Hoge {
    if (!startsWith("hoge")) {
        return Hoge.NONE    
    }
    return when (this) {
       "hogeFoo" -> Hoge.FOO
       "hogeBar" -> Hoge.BAR
       else -> Hoge.NONE    }
    }
}
```

### Don't create extension functions in same class.

```Kotlin
class Hoge {

   // bad
   fun Hoge.foo() {
   }

   // good
   fun foo() {
   }
}
```


## Others

### Rx

```Kotlin
// bad
Observable.just(10)
       .map { 
          it * 3 
       } 
       .filter {
          val rand = Random()
          it % rand.nextInt(10) == 0
       }        
       .map { it.toFloat() }
       .withLatestFrom(Observable.just("hoge"), BiFunction { num: Float, hoge: String -> num to hoge })
       .subscribe ({ print(it) }, { print(it) })

// bad
// You must start a new line as operators
Observable.just(10).map { it * 3 } 
       .filter {
          val rand = Random()
          it % rand.nextInt(10) == 0
       }.map { it.toFloat() }
       .withLatestFrom(Observable.just("hoge"),
          BiFunction { num: Float, hoge: String -> num to hoge })
       .subscribe ({
          print(it) 
       }, {
          print(it)
       })

// good
Observable.just(10)
        .map { it * 3 } // one line
        .filter { // line break in case of multiple lines
           val rand = Random()
           it % rand.nextInt(10) == 0
        }        
        .map(Int::toFloat) // method reference
        .withLatestFrom(Observable.just("hoge")) { num, hoge -> num to hoge } // Use rx-kotlin
        .subscribe ({ // start a new line in subscribe block if it implements `onError`
           print(it) 
        }, {
           print(it)
        })
```

# License

```
Copyright (C) 2017 CyberAgent, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[AndroidStudio]: /arts/AndroidStudio.jpg
