
## Functions
You can create functions with `def`.

### Variant 1

     scala> def divider(i:Int, j:Int): Int = {
          | val value = i/j
          | println(value)
          | value
          | }
     divider: (i: Int, j: Int)Int

     scala> divider(10,2)
     5
     res0: Int = 5

### Variant 2

     scala> def divider(i:Int, j:Int): Int = i/j
     divider: (i: Int, j: Int)Int

     scala> divider(10,2)
     res4: Int = 5

## Creating multiline anonymous function

### Variant 1

	scala> val myFuncAno = (x:Int) => {
	     | println("hello")
	     | x+1
	     | }
	myFuncAno: Int => Int = $$Lambda$761/428798670@4cbd03e7
     
	scala> myFuncAno
	res0: Int => Int = $$Lambda$761/428798670@4cbd03e7
     
	scala> myFuncAno(1)
	hello
	res1: Int = 2

### Variant 2

	scala> val myFuncAno = { i:Int =>
	     | println("hello")
	     | i+2
	     | }
	myFuncAno: Int => Int = $$Lambda$776/878511419@3d0cac1f
     
	scala> myFuncAno
	res2: Int => Int = $$Lambda$776/878511419@3d0cac1f
     
	scala> myFuncAno(2)
	hello
	res3: Int = 4

## Multiple argument function to curried function

	scala> def divider(i:Int, j:Int): Int = i/j
	divider: (i: Int, j: Int)Int
     
	scala> val divide = (divider _).curried
	divide: Int => (Int => Int) = scala.Function2$$Lambda$811/27317011@13908f9c
     
	scala> val remain = divide(10)
	remain: Int => Int = scala.Function2$$Lambda$812/866589987@23c05889
     
	scala> remain(2)
	res4: Int = 5

Curried functions applies params starting with first param onwards.

## `class`

    scala> class Parenthesis() {
         | def print() = println("hello")
         | }
    defined class Parenthesis
     
    scala> val instance = new Parenthesis()
    instance: Parenthesis = Parenthesis@58ec7116
     
    scala> instance.print()
    hello

Also, to be able to access constructor params, we need to define them as `var` or `val`.

    scala> class Calculator(val brand: String, val model: String)
    defined class Calculator
     
    scala> val calc = new Calculator("HP", "30B")
    calc: Calculator = Calculator@5a48d186
     
    scala> calc.brand
    res3: String = HP

## Optional Parenthesis

When calling a function or a class constructor, parenthesis become optional if no argument is to be passed.

    scala> class Parenthesis() {
         | def print() = println("hello")
         | }
    defined class Parenthesis
     
    scala> val instance = new Parenthesis
    instance: Parenthesis = Parenthesis@58ec7116
     
    scala> instance.print
    hello
     
    scala> instance.print()
    hello


## Constructor initialization

	scala> class Calculator(brand: String) {
     |   /**
     |    * A constructor.
     |    */
     |   val color: String = if (brand == "TI") {
     |     "blue"
     |   } else if (brand == "HP") {
     |     "black"
     |   } else {
     |     "white"
     |   }
     |
     |   // An instance method.
     |   def print() = println(color)
     | }
	defined class Calculator
     
	scala> val calc = new Calculator("Casio")
	calc: Calculator = Calculator@eebd983
     
	scala> calc.print()
	white
     
    scala> calc.color
	res6: String = white

## Abstract Classes

Same as in Java.

## `trait`

`trait` is collections of fields and behaviors.

    scala> trait Car {
         |   val brand: String
         | }
    defined trait Car
     
    scala> trait Shiny {
         |   val shineRefraction: Int
         | }
    defined trait Shiny
   
Now you can use a trait to define a class by defining the values or behaviours.

    scala> class BMW extends Car {
         |   val brand = "BMW"
         | }
    defined class BMW

You can also extend multiple `traits` using the `with` keyword.

    scala> class BMW extends Car with Shiny {
         |   val brand = "BMW"
         |   val shineRefraction = 12
         | }
    defined class BMW

### Trait vs an Abstract Class

* Favor using traits. It’s handy that a class can extend several traits; a class can extend only one class.
* If you need a constructor parameter, use an abstract class. Abstract class constructors can take parameters; trait constructors can’t.

## apply()

Functions are Objects. The syntactic sugar of apply helps unify the duality of object and functional programming. You can pass classes around and use them as functions and functions are just instances of classes under the covers.

    scala> class Foo {}
    defined class Foo
     
    scala> object FooMaker {
         |   def apply() = new Foo
         | }
    defined module FooMaker
      
    scala> val newFoo = FooMaker()
    newFoo: Foo = Foo@7625b23b


    scala> object addOne extends Function1[Int, Int] {
     |   def apply(m: Int): Int = m + 1
     | }
    defined module addOne
      
    scala> addOne(1)
    res7: Int = 2

In the above example, `extends Function1[Int, Int]` is equivalent to `extends (Int => Int)`

## object

Values and functions cannot be outside of a class or object.

    scala> object colorHolder {
         |   val BLUE = "Blue"
         |   val RED = "Red"
         | }
    defined object colorHolder
      
    scala> colorHolder.BLUE
    res3: String = Blue

## Matching based on switch

### Based on value

    scala> val value = 1
    value: Int = 1
     
    scala> value match {
         |   case 1 => "one"
         |   case 2 => "two"
         |   case _ => "some other number"
         | }
    res4: String = one

### Matching on class members

Brute way of matching class members:

    scala> class Calculator(val brand: String, val model: String)
    defined class Calculator
      
    scala> def calcType(calc: Calculator) = calc match {
         |   case _ if calc.brand == "HP" && calc.model == "20B" => "financial"
         |   case _ if calc.brand == "HP" && calc.model == "48G" => "scientific"
         |   case _ if calc.brand == "HP" && calc.model == "30B" => "business"
         |   case _ => "unknown"
         | }
    calcType: (calc: Calculator)String

**NOTE:** Use `val` in the class constructor argument, to be able to access the members publicly as `calc.brand`, etc.

## Exceptions

Similar to Java