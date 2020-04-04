## MAP - III : Types of Types

When talking about types in computer science, we could be talking about two types of types. Types from type theory are closely related to the type systems of programming languages. Both of these are fun and playing with them illuminates how any language works and may even help you obtain A3 to [L3-tier](https://www.scala-lang.org/old/node/8610) superpowers.

Previously in the [Mathematics' (An)architecture and Processes (MAP) series](https://risav.dev/mathematics-anarchitecture-and-processes-ck5nahog004z4qps1w98ldfz0), we have briefly touched upon Curry-Howard-Lambek isomorphism, which is a trifecta of independent realizations about, among other things, how propositions are types i.e. claims that are true or false, formal proofs, logical calculus' constraints and formulae are types too. With Scala's types, and Java 11+'s and Typescript's later, we will see what it actually means to say something as abstract as 'propositions are types' and why understanding these esoteric and zen sayings can make you a better programmer - in any language, with any kind of type system.

For getting started with types, we will come up with our working definition of types at each level. We will then iteratively refine our definition and intuitions regarding what a type is. As for how typings look in programming, here's are four simple examples:

```TypeScript

val obligatory = "Hello World!"  
// type inferred to be String based on its value

val goodbye : String = "Goodbye, cruel world..." 
// type explicitly defined to be a String

val emoji: Char = '☺'
// Unicode characters are present in Char type's value space

val heteroList: List[Any] = List(obligatory + goodbye, true, 42)
// String type allows addition operation 
// The composed type List[Any] allows use of any type in heteroList
```

This is certainly just a picture and certainly not the whole picture. However we will get closer to types through this representation of types above.

> ![thisIsNotAPipe](https://upload.wikimedia.org/wikipedia/en/b/b9/MagrittePipe.jpg)

> *This is not a pipe. This is just a representation of a pipe as a picture.*

Almost every programming construct that you can use has a type. Thus there are many types of types.

## 1. Common Data Types: Syntactic and Semantic bits

Let's start with the primitive ones. Types of data values are the easiest types to intuitively understand. At level 1, we will define type as a pre-defined value space for our data possibly along with a set of possible operations on the values in the value space.

At level 1, you may find, for instance, a type that does not have any possible operations or one that is simply an alias to another type. These are merely syntactic types. All types have this syntactic aspect in the sense that a type definition tells the program compiler and the programmer that it adheres to the rules and constraints set by the programming language itself, especially regarding what is and isn't allowed to be done with a certain value.

For instance, if you find a type definition called `Money` in your program, it probably is a syntactic sugar representing some type of fixed point numbering.

The semantic aspect of a type would be about what it means to have a certain type in the first place i.e. what possible actions are possible on the type or what interactions are allowed with other types related to it.

For a utility type like 'Money', the operations could be inherited arithmetic methods for addition or subtraction or a convenience method composed of other operations like interest rate calculation.
_________________________________________________________________

### **Any and Nothing**

All types in Scala are different further derivations of the `Any` type. Informally, you may call it the mother of all types as it is the **top type** in Scala's type hierarchy.

> `Any` of Scala is similar to the `Object` type defined by the class also named `Object` in Java. In TypeScript, there are two top types `any` and `unknown`. `any` just tells the compiler that it should not even bother to do any type checks. So compile time type safety due to aforementioned type syntactics and semantics is effectively disabled for that variable or constant of `any` type. So related bugs may only be discovered if unexpected operations are performed on that value at runtime - during testing (hopefully) or in production. `unknown` type signifies that the actual type is unknown yet but can be narrowed down based on runtime values using a language construct called type guards. 

`Any` has general definitions of `equals`, `hashCode` and `toString` methods. This means that `Any` type in Scala allows for checking whether anything equals another and what its hashed or string representation is, all puns intended.

Type theory and category theory, from which we have type systems in programming, also have something called a **bottom type**. It is called so because it is at the bottom of the type hierarchy i.e. any type has the bottom type as its subtype. Scala's bottom type is called `Nothing` as there is nothing you can assign to a variable or constant of type `Nothing`. If a function throws an exception instead of returning any value, it has nothing for a return type. We will find further uses of this in later posts, especially when we discuss covariance under parametric polymorphism.

_________________________________________________________________

### **AnyVal and AnyRef**

You may directly be using or passing around a value from one memory location to another. These types of values are derived from `AnyVal` type, which itself is a descendant derived directly from the `Any` supertype.

You may also just be referring to where the value is located in your memory space instead of moving those values around. These reference types are descended from the `AnyRef` supertype, which itself descends directly from the `Any` type.

When you look at the documentation for a type, for example [Boolean](https://www.scala-lang.org/api/current/scala/Boolean$.html), might find methods that `box` or `unbox` a value. The `scala.Boolean` is a value type but it can be wrap your boolean value inside a 'box' made of `java.lang.Boolean`, which is a reference type. You might need to do this boxing to access reference type's methods or, in this case, to ensure interoperability with Java clients of your library. Similarly, you can also 'unbox' a reference type into a value type.

_________________________________________________________________

### **Unit and Boolean**

Just like `Nothing` allows nothing as its value, `Unit` allows only one and the same value and `Boolean` allows only two.

`Boolean` type is probably the most well known and well understood of all. It allows its value to be either `true` or `false`. 

`Unit` type only allows one value, which is written as `()`. Therefore it holds no information and is very similar to, but not the same as, `void` used in C/C++/Java. It also should not be confused with aforementioned `Nothing` or with `Null` from the next section. We will return back to this type later when we talk about product types and tuples as it can be interpreted as a 0-tuple. We will also see how it is useful when we talk about abstract data types and algebraic data types.    
_________________________________________________________________

### **Option and Null**

Sometimes a reference whose that points to some value or none of it when the instance of that type is being requested by the program execution. Such references use a null pointer in many languages, which Tony Hoare has famously called his billion dollar mistake. 

The functional and type safe solution to this annoyance would be the the type `Option` or `Optional` (in Java, Scala, Kotlin, Swift, SML, OCaml, Rust) or `Maybe` (in Haskell, Agda, Idris). Depending on the implementation, this type may be [monadic](https://books.underscore.io/scala-with-cats/scala-with-cats.html#what-is-a-monad) or at least provide monadic benefits of type safe operation chaining based on pattern matching of its inner contents.

In Scala, `Option` type has two subtypes `Some` or `None`. Going by the above box metaphor, if you think that in your variable there *maybe some* value or *optionally none* at all then you can it inside an `Option` box. To check for *valuables* in the box, you can simply check the type of the content instead of the value of the content itself. The type is `Some` if something is present or `None` otherwise.

```TypeScript
def theAnswer(s: Int): Option[String] = {
    try {
        Some(
          if (s == 42) "The answer was found" else "Still searching for the answer..."
        )
    } catch {
        case e: Exception => None
    }
}
```
_________________________________________________________________

### **The *Normie* Types**

These are the boring types which will become interesting when we start composing them with others and weaving mind-bending (and hopefully helpful) structures later.

#### I. Byte, Short, Int, Char and String

These types have trivially defined value spaces. Available methods for each type will defer as relevant.

```TypeScript
val u = 42  // defaults to Int type
val b: Byte = 1 //  -2^7 to 2^7-1, inclusive i.e. -128 to 127
val s: Short = 2 //  -2^15 to 2^15-1, inclusive i.e. -32,768 to 32,767
val i: Int = 3  // -2^31 to 2^31-1, inclusive i.e. -2,147,483,648 to 2,147,483,647
val c: Char = 'q' // 
```

A Char can be cast as an Int like you can in most languages.

```TypeScript
scala> val c: Char = 'q'
c: Char = q

scala> val i: Int = c
i: Int = 113

scala> val emoji: Char = '☺'
emoji: Char = ☺

scala> emoji.toInt
res0: Int = 9786
```

A `String` is defined as a sequence of `Char`. You assign a string using quotes.:

```TypeScript

val strQ = "Now I am become Death, the destroyer of worlds - J. Robert Oppenheimer"

val listToStr = List("If I'm not back in five minutes", "just wait longer").toString
// output: List(If I'm not back in five minutes, just wait longer)

```



#### II. Long, Float and Double
#### III. Floating Point vs Fixed Point vs Arbitrary Precision
### The Big* and the Rich*

__________________________________________________________

# Upcoming Topics Under 'Types of Types'

## 2. Functions with Data: Domain and Co-domain Spaces

### Object and Class
### Parameter Types and Return Types

## 3. Abstract Data Types: Types with Structure and Behavior

### Sum and Product Types

  #### Union or Tagged Union
  #### Tuple and Record

### Set and List

  #### Unordered and Non-duplicated
  #### Ordered and Random Accessible

### Map or Associative Array
### Stack and Queue
### Graph and Tree

## 4. Algebraic Data Types and HKTs


Further resources:

1. D. L. Parnas, John E. Shore, David Weiss: Abstract types defined as classes of variables, https://dl.acm.org/doi/10.1145/942574.807133

2. Tony Hoare on his infamous billion dollar mistake: https://www.youtube.com/watch?v=kz7DfbOuvOM

3. Official Scala API Docs and resources: https://www.scala-lang.org/

4. L.G. Meredith, Pro Scala: Monadic Design Patterns for the Web: http://patryshev.com/books/GregoryMeredith.pdf# Types of Types

When talking about types in computer science, we could be talking about two types of types. Types from type theory are closely related to the type systems of programming languages. Both of these are fun and playing with them illuminates how any language works and may even help you obtain A3 to [L3-tier](https://www.scala-lang.org/old/node/8610) superpowers.

Previously in the [Mathematics' (An)architecture series](https://risav.dev/mathematics-anarchitecture-and-processes-ck5nahog004z4qps1w98ldfz0), we have briefly touched upon Curry-Howard-Lambek isomorphism, which is a trifecta of independent realizations about, among other things, how propositions are types i.e. claims that are true or false, formal proofs, logical calculus' constraints and formulae are types too. With Scala's types, and Java 11+'s and Typescript's later, we will see what it actually means to say something as abstract as 'propositions are types' and why understanding these esoteric and zen sayings can make you a better programmer - in any language, with any kind of type system.

For getting started with types, we will come up with our working definition of types at each level. We will then iteratively refine our definition and intuitions regarding what a type is. As for how typings look in programming, here's are four simple examples:

```TypeScript

val obligatory = "Hello World!"  
// type inferred to be String based on its value

val goodbye : String = "Goodbye, cruel world..." 
// type explicitly defined to be a String

val emoji: Char = '☺'
// Unicode characters are present in Char type's value space

val heteroList: List[Any] = List(obligatory + goodbye, true, 42)
// String type allows addition operation 
// The composed type List[Any] allows use of any type in heteroList
```

This is certainly just a picture and certainly not the whole picture. However we will get closer to types through this representation of types above.

> ![thisIsNotAPipe](https://upload.wikimedia.org/wikipedia/en/b/b9/MagrittePipe.jpg)

> *This is not a pipe. This is just a representation of a pipe as a picture.*

Almost every programming construct that you can use has a type. Thus there are many types of types.

## 1. Common Data Types: Syntactic and Semantic bits

Let's start with the primitive ones. Types of data values are the easiest types to intuitively understand. At level 1, we will define type as a pre-defined value space for our data possibly along with a set of possible operations on the values in the value space.

At level 1, you may find, for instance, a type that does not have any possible operations or one that is simply an alias to another type. These are merely syntactic types. All types have this syntactic aspect in the sense that a type definition tells the program compiler and the programmer that it adheres to the rules and constraints set by the programming language itself, especially regarding what is and isn't allowed to be done with a certain value.

For instance, if you find a type definition called `Money` in your program, it probably is a syntactic sugar representing some type of fixed or floating point numbering.

The semantic aspect of a type would be about what it means to have a certain type in the first place i.e. what possible actions are possible on the type or what interactions are allowed with other types related to it.

For a utility type like 'Money', the operations could be inherited arithmetic methods for addition or subtraction or a convenience method composed of other operations like interest rate calculation.

### **Any and Nothing**

All types in Scala are different further derivations of the `Any` type. Informally, you may call it the mother of all types as it is the **top type** in Scala's type hierarchy.

> `Any` of Scala is similar to the `Object` type defined by the class also named `Object` in Java. In TypeScript, there are two top types `any` and `unknown`. `any` just tells the compiler that it should not even bother to do any type checks. So compile time type safety due to aforementioned type syntactics and semantics is effectively disabled for that variable or constant of `any` type. So related bugs may only be discovered if unexpected operations are performed on that value at runtime - during testing (hopefully) or in production. `unknown` type signifies that the actual type is unknown yet but can be narrowed down based on runtime values using a language construct called type guards. 

`Any` has general definitions of `equals`, `hashCode` and `toString` methods. This means that `Any` type in Scala allows for checking whether anything equals another and what its hashed or string representation is, all puns intended.

Type theory and category theory, from which we have type systems in programming, also have something called a **bottom type**. It is called so because it is at the bottom of the type hierarchy i.e. any type has the bottom type as its subtype. Scala's bottom type is called `Nothing` as there is nothing you can assign to a variable or constant of type `Nothing`. If a function throws an exception instead of returning any value, it has nothing for a return type. We will find further uses of this in later posts, especially when we discuss covariance under parametric polymorphism.

### **AnyVal and AnyRef**

You may directly be using or passing around a value from one memory location to another. These types of values are derived from `AnyVal` type, which itself is a descendant derived directly from the `Any` supertype.

You may also just be referring to where the value is located in your memory space instead of moving those values around. These reference types are descended from the `AnyRef` supertype, which itself descends directly from the `Any` type.

When you look at the documentation for a type, for example [Boolean](https://www.scala-lang.org/api/current/scala/Boolean$.html), might find methods that `box` or `unbox` a value. The `scala.Boolean` is a value type but it can be wrap your boolean value inside a 'box' made of `java.lang.Boolean`, which is a reference type. You might need to do this boxing to access reference type's methods or, in this case, to ensure interoperability with Java clients of your library. Similarly, you can also 'unbox' a reference type into a value type.

### **Unit and Boolean**

Just like `Nothing` allows nothing as its value, `Unit` allows only one and the same value and `Boolean` allows only two.

`Boolean` type is probably the most well known and well understood of all. It allows its value to be either `true` or `false`. 

`Unit` type only allows one value, which is written as `()`. Therefore it holds no information and is very similar to, but not the same as, `void` used in C/C++/Java. It also should not be confused with aforementioned `Nothing` or with `Null` from the next section. We will return back to this type later when we talk about product types and tuples as it can be interpreted as a 0-tuple. We will also see how it is useful when we talk about abstract data types and algebraic data types.    


### **Option and Null**

Sometimes a reference whose that points to some value or none of it when the instance of that type is being requested by the program execution. Such references use a null pointer in many languages, which Tony Hoare has famously called his billion dollar mistake. 

The functional and type safe solution to this annoyance would be the the type `Option` or `Optional` (in Java, Scala, Kotlin, Swift, SML, OCaml, Rust) or `Maybe` (in Haskell, Agda, Idris). Depending on the implementation, this type may be [monadic](https://en.wikipedia.org/wiki/Monad_(functional_programming)#An_example:_Maybe) or at least provide monadic benefits of type safe operation chaining based on pattern matching of its inner contents.

In Scala, `Option` type has two subtypes `Some` or `None`. Going by the above box metaphor, if you think that in your variable there *maybe some* value or *optionally none* at all then you can it inside an `Option` box. To check for *valuables* in the box, you can simply check the type of the content instead of the value of the content itself. The type is `Some` if something is present or `None` otherwise. There also exists another trio like these called `Try`, `Success` and `Failure`, which essentially form a more concise syntax when catching exceptions could be needed as opposed to just dealing with presence or absence of values. 

We will explore examples of the usage of these when we discuss pattern matching based on types and control flow structures like try-catch, match-case or for-yield. 

### **The *Normie* Types**

These are the boring types which will become interesting when we start composing them with others and weaving mind-bending (and hopefully helpful) structures later.

#### I. Byte, Short and Int

These types have trivially defined value spaces. Available methods for each type will defer as relevant.

```TypeScript
val u = 42  // defaults to Int type
val b: Byte = 1 // (-2^7 to 2^7-1] i.e. -128 to 127
val s: Short = 2 // (-2^15 to 2^15-1] i.e. -32,768 to 32,767
val i: Int = 3 // (-2^31 to 2^31-1] i.e. -2,147,483,648 to 2,147,483,647
```

#### II. Long, Float and Double

These are pretty much the same as the above numerical types except they allow for bigger ranges and, in case of `Float` and `Double`, fractional values with floating point precision.

```TypeScript
val l: Long = 1 // -2^63 to 2^63-1 inclusive
val f: Float = 2.0 // 32-bit IEEE 754 single-precision float
// 1.40129846432481707e-45 to 3.40282346638528860e+38
val d: Double = 3.0 // 64-bit IEEE 754 double-precision float
// 4.94065645841246544e-324d to 1.79769313486231570e+308d
```

#### III. Char and String

A `Char` is a literal from the Unicode charset. A `Char` can be cast to its corresponding position in Unicode charset as an `Int`, like you can in most languages.

```TypeScript
scala> val c: Char = 'q'
c: Char = q

scala> val i: Int = c
i: Int = 113

scala> val emoji: Char = '☺'
emoji: Char = ☺

scala> emoji.toInt
res0: Int = 9786
```

A `String` is defined as a sequence of `Char`s. You assign a string using quotes.:

```TypeScript

val strQ = "Now I am become Death, the destroyer of worlds - J. Robert Oppenheimer"

val listToStr = List("If I'm not back in five minutes", "just wait longer").toString
// output: List(If I'm not back in five minutes, just wait longer)

```

Scala `String`s rely on `java.lang.String` but are also augmented by hundreds of methods from the `StringOps` class.


#### IV. The Big* and the Rich*

There are `StringOps` equivalents for the other the other value types called `RichBoolean`, `RichByte`, `RichChar`, `RichDouble`, `RichFloat`, `RichInt`, `RichLong` and `RichShort`. These [proxies](http://w3sdesign.com/?gr=s07&ugr=proble) provide additional methods to the above value types. Unlike the case in Java with its primitive and boxed reference types, this pattern allows for keeping the value types lightweight while still providing a rich API and therefore fewer use cases for boxing and unboxing for performance - convenience tradeoffs.

Besides the `Rich`* ones, `scala.math` package provides `BigDecimal` and `BigInt` classes. These are for calculations with floating-point numbers of arbitrary precision starting with a default precision approximately that of IEEE 128-bit floating point numbers.
__________________________________________________________

# Next on 'Types of Types'

## 2. Functions with Data: Domain and Co-domain Spaces

Here we will introduce functions and methods together with objects and classes. We will see how OOP augments the power of functional programming and how best to avoid the many pitfalls of OOP.

### 2.1 Object and Class
### 2.2 Functions and Methods
### 2.3 Parameter Types and Return Types

## 3. Abstract Data Types: Types with Structure and Behavior

Here we will rediscover the good old data structures mostly used in programming through a more formal, functional and scala-ble lens 🧐.

### 3.1 Sum and Product Types

  #### Union or Tagged Union
  #### Tuple and Record

### 3.2 Set and List

  #### Unordered and Non-duplicated
  #### Ordered and Random Accessible

### 3.3 Map or Associative Array
### 3.4 Stack and Queue
### 3.5Graph and Tree

## 4. Algebraic Data Types and HKTs

Here we will (re-)discover universal algebra and ring-like, group-like, lattice-like, module-like and algebra-like algebraic structures relevant to advanced programming.

## 5. A Gentle Introduction to Category Theory

Here we will use the Scala and Type Theory topics thus far to go for a deep dive into Bartoz Milewski's excellent [course on Category Theory ](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/). 

Beware that category theorists are sometimes insufferable and gave birth to memes like, 'A monad is just an endofunctor in the category of monoids. What's the problem?' or this one below:

![meme_category_theory.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1586014084969/1Kio9oLYO.png)

**Further resources:**

1. D. L. Parnas, John E. Shore, David Weiss: Abstract types defined as classes of variables, https://dl.acm.org/doi/10.1145/942574.807133

2. Tony Hoare on his infamous billion dollar mistake: https://www.youtube.com/watch?v=kz7DfbOuvOM

3. Official Scala API Docs and resources: https://www.scala-lang.org/

4. L.G. Meredith, Pro Scala: Monadic Design Patterns for the Web: http://patryshev.com/books/GregoryMeredith.pdf