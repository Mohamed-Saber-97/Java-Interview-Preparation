* [Functional-Style Programming](#functional-style-programming)
    * [Functional Interfaces](#functional-interfaces)
        * [Declaring a Functional Interface](#declaring-a-functional-interface)
            * [Overriding Abstract Methods from Multiple Interfaces, Revisited](#overriding-abstract-methods-from-multiple-interfaces-revisited)
        * [Selected Interfaces in the Java SE Platform API](#selected-interfaces-in-the-java-se-platform-api)
    * [Lambda Expressions](#lambda-expressions)
        * [Lambda Parameters](#lambda-parameters)
        * [Lambda Body](#lambda-body)
        * [Type Checking and Execution of Lambda Expressions](#type-checking-and-execution-of-lambda-expressions)
        * [Accessing Members in an Enclosing Class](#accessing-members-in-an-enclosing-class)
        * [Lambda Expressions versus Anonymous Classes](#lambda-expressions-versus-anonymous-classes)
            * [Implementation](#implementation)
    * [Overview of Built-In Functional Interfaces](#overview-of-built-in-functional-interfaces)
    * [Suppliers](#suppliers)
    * [Predicates](#predicates)
    * [Consumers](#consumers)
        * [Two-Arity Specialization of `Consumer<T>`: `BiConsumer<T, U>`](#two-arity-specialization-of-consumert-biconsumert-u)
    * [Functions](#functions)
    * [Two-Arity Specialization of `Function<T, R>`: `BiFunction<T, U, R>`](#two-arity-specialization-of-functiont-r-bifunctiont-u-r)
    * [Extending `Function<T,T>`: `UnaryOperator<T>`](#extending-functiontt-unaryoperatort)
    * [Extending `BiFunction<T,T,T>`: `BinaryOperator<T>`](#extending-bifunctionttt-binaryoperatort)
    * [Currying Functions](#currying-functions)
    * [Method and Constructor References](#method-and-constructor-references)

---------

# Functional-Style Programming

## Functional Interfaces

### Declaring a Functional Interface

- An `interface` can provide explicit `public` `abstract` method declarations for any of the three non-`final` `public` instance methods in the `Object` class (`equals()`, `hashCode()`,
  `toString()`). Including these methods explicitly in an interface should not be attempted, unless there are compelling reasons for doing so. These methods are automatically implemented by every class that implements an interface, since every class directly or indirectly inherits from the `Object` class. *
  *_Therefore, such method declarations are not considered `abstract` in the definition of a functional interface_**

```java
public static <T> boolean testPredicate(T object, Predicate<T> predicate) {
    return predicate.test(object);
}
```

```java
// Class implementation of Predicate<String>.
class PredicateTest implements Predicate<String> {
    public boolean test(String str) {
        return str.startsWith("A"); // (1)
    }
}
```

```java
void test() {
    // An anonymous class implementation of Predicate<String>.
    static Predicate<String> testLength = new Predicate<>() {
        public boolean test(String str) {
            return str.length() < 4; // (2)
        }
    };
```

```text
 // Client code:
    System.out.println(testPredicate("Anna", new PredicateTest())); // true
    System.out.println(testPredicate("Anna", testLength)); // false
}
```

#### Overriding Abstract Methods from Multiple Interfaces, Revisited

```java
interface ContractX {
    void doIt(int i);
}

interface ContractY {
    void doIt(double d);
}

// Compile-time error!
@FunctionalInterface
interface ContractZ extends ContractX, ContractY {
    @Override
    void doIt(int d); // Compile-time error!

    @Override
    void doIt(double d); // Compile-time error!
}
```

```java
interface Feature1<R> {
    void flatten(List<R> plist);
}

interface Feature2<S> {
    void flatten(List<S> plist);
}

@FunctionalInterface
interface Features<T> extends Feature1<T>, Feature2<T> { // (1)
    @Override
    void flatten(List<T> plist); // Can be omitted.
}
```

### Selected Interfaces in the Java SE Platform API

- Such single-abstract-method (SAM) not marked as `@FunctionalInterface`
    - `Comparable<T>` for creating an object that can be compared with another object according to their natural ordering
    - `Iterable<T>` for creating an object that represents a collection in a for(:) loop to iterate over its elements
    - `AutoCloseable` for creating an object that represents a resource that is automatically closed when exiting a `try`-with-resources statement
    - `Readable` for creating an object that is a source for reading characters
- functional interfaces that are intended to be implemented by lambda expressions.
    - `java.util.Comparator<T>` with the method `compare()` for defining criteria for comparing two objects according to their total ordering
    - `java.lang.Runnable` with the method `run()` for defining code to be executed in threads
    - `java.util.concurrent.Callable<V>` with the method `call()` for defining code to be executed in threads, that can return a result and throw an exception

## Lambda Expressions

### Lambda Parameters

- Declared-type parameters: `(Integer a, String y) -> {};`
- Inferred-type parameters: `(a, b) -> {};` or `(var a, var b) -> {};`

```java
void test() {
    () -> {}; // Empty parameter list
    // Single formal parameter:
    (String str) -> {}; // Single declared-type parameter
    (str) -> {}; // Single inferred-type parameter
    str -> {}; // Single inferred-type parameter
    (var str) -> {}; // Single var-type inferred parameter
    // Multiple formal parameters:
    (Integer x, Integer y) -> {}; // Multiple declared-type parameters
    (x, y) -> {}; // Multiple inferred-type parameters
    (var x, var y) -> {}; // Multiple var-type inferred parameters
    // Modifiers and annotations with formal parameters:
    (final int i, int j) -> {}; // Modifier with declared-type parameters
    (final var i, var j) -> {}; // Modifier with var-type inferred parameters
    (@NonNull int i, int j) -> {}; // Annotation with declared-type parameter
    (var i, @Nullable var j) -> {}; // Annotation with var-type inferred parameter
    // Parentheses are mandatory with multiple formal parameters:
    String str ->{}
    ; // Illegal: Missing parentheses
    var str ->{}
    ; // Illegal: Missing parentheses
    Integer x, Integer y -> {}; // Illegal: Missing parentheses
    x, y -> {}; // Illegal: Missing parentheses
    var x, var y -> {}; // Illegal: Missing parentheses
    // All formal parameters must be either declared-type, inferred-type, or
    // var-type inferred parameters.
    (String str, j) -> {}; // Cannot mix declared-type and inferred-type
    (String str, var j) -> {}; // Cannot mix declared-type and var-type inferred
    (var str, j) -> {}; // Cannot mix var-type inferred and inferred-type
    // Modifiers and annotations cannot be used with inferred-type parameters.
    (final str, j) -> {}; // No modifiers with inferred-type parameters
    (str, @NonNull j) -> {}; // No annotations with inferred-type parameters
}
```

### Lambda Body

```java
void test() {
    () -> 2021 // Expression body, non-void return
    () -> null // Expression body, non-void return
    (i, j) -> i + j // Expression body, non-void return
            (i, j) ->i <= j ? i : j // Expression body, non-void return
    str -> str.length() > 3 // Expression body, non-void return
    str -> str != null && !str.equals("") && str.length() > 3 && str.equals(new StringBuilder(str).reverse().toString())// Expression body, non-void return
    val -> System.out.println(val) // Method invocation statement, void return
    sb -> sb.trimToSize() // Method invocation statement, void return
    sb -> sb.append("!") // Method invocation statement, non-void return
    () -> new StringBuilder("?") // Object creation statement, non-void return
    value -> value++ // Increment statement, non-void return
    value -> value *= 2 // Assignment statement, non-void return
    (int i) -> while (i < 10) ++i // Illegal: not an expression but statement
            (x, y) ->return x + y // Illegal: return not allowed in expression
}
```

### Type Checking and Execution of Lambda Expressions

- A lambda expression can only be defined in a context where a functional interface can be used: for example, in an assignment context, a method call context, or a cast context
- The compiler determines the target type that is required in the context where the lambda expression is defined. This target type is always a functional interface type

### Accessing Members in an Enclosing Class

- A lambda expression does not inherit names of members declared in the functional interface it implements, which obviously then cannot be accessed in the lambda body
- If the `this` reference is used in a lambda expression in a non-static context, it refers to the enclosing object, and can be used to access members of this object.

### Lambda Expressions versus Anonymous Classes

#### Implementation

- A lambda expression can only be used to provide implementation of exactly one functional interface. It represents an anonymous function. Unlike an object, it has only behavior and no state
- An anonymous class is restricted to either implementing one interface or extending one class, but it is not restricted to implementing only one abstract method from its supertype
- No separate class file with Java bytecode is created for a lambda expression, in contrast to a separate class file for each anonymous class declared in a source file

## Overview of Built-In Functional Interfaces

- commonly implemented by functions: to get a value (`Supplier<T>`), to test a predicate (`Predicate<T>`), to accept a value but not return a result (`Consumer<T>`), and to apply a function to a value in order to compute a new result (`Function<T, R>`).

| Functional interface (T and R are type parameters) |       Functional       |       method Function        | Arity of function type |
|:--------------------------------------------------:|:----------------------:|:----------------------------:|:----------------------:|
|                   `Supplier<T>`                    |    `get`: `() -> T`    | Provide an instance of a T.  |       Zero-arity       |
|                   `Predicate<T>`                   | `test`: `T -> boolean` | Evaluate a predicate on a T. |       One-arity        |
|                   `Consumer<T>`                    | `accept`: `T -> void`  |    Perform action on a T.    |       One-arity        |
|                  `Function<T, R>`                  |   `apply`: `T -> R`    |    Transform a T to an R.    |       One-arity        |

## Suppliers

| Functional interface (T, U, and R are type parameters) |        Functional method        | Default methods unless otherwise indicated |
|:------------------------------------------------------:|:-------------------------------:|:------------------------------------------:|
|                     `Supplier<T>`                      |        `get`: `() -> T`         |                     -                      |
|                     `IntSupplier`                      |     `getAsInt`: `() -> int`     |                     -                      |
|                     `LongSupplier`                     |    `getAsLong`: `() -> long`    |                     -                      |
|                    `DoubleSupplier`                    |  `getAsDouble`: `() -> double`  |                     -                      |
|                   `BooleanSupplier`                    | `getAsBoolean`: `() -> boolean` |                     -                      |

## Predicates

| Functional interface (T, U, and R are type parameters) |      Functional method       |           Default methods unless otherwise indicated            |
|:------------------------------------------------------:|:----------------------------:|:---------------------------------------------------------------:|
|                     `Predicate<T>`                     |    `test`: `T -> boolean`    | `and()`, `or()`, `negate()`, static `isEqual()`, static `not()` |
|                   `IntPredicate<T>`                    |  `test`: `int  -> boolean`   |                   `and()`, `or()`, `negate()`                   |
|                   `LongPredicate<T>`                   |  `test`: `long  -> boolean`  |                   `and()`, `or()`, `negate()`                   |
|                  `DoublePredicate<T>`                  | `test`: `double  -> boolean` |                   `and()`, `or()`, `negate()`                   |
|                  `BiPredicate<T, U>`                   | `test`: `(T, U) -> boolean`  |                   `and()`, `or()`, `negate()`                   |

## Consumers

| Functional interface (T, U, and R are type parameters) |     Functional method      | Default methods unless otherwise indicated |
|:------------------------------------------------------:|:--------------------------:|:------------------------------------------:|
|                     `Consumer<T>`                      |   `accept`: `T -> void`    |                `andThen()`                 |
|                    `IntConsumer<T>`                    |  `accept`: `int -> void`   |                `andThen()`                 |
|                   `LongConsumer<T>`                    |  `accept`: `long -> void`  |                `andThen()`                 |
|                  `DoubleConsumer<T>`                   | `accept`: `double -> void` |                `andThen()`                 |

### Two-Arity Specialization of `Consumer<T>`: `BiConsumer<T, U>`

| Functional interface (T, U, and R are type parameters) |        Functional method        | Default methods unless otherwise indicated |
|:------------------------------------------------------:|:-------------------------------:|:------------------------------------------:|
|                   `BiConsumer<T, U>`                   |   `accept`: `(T, U) -> void`    |                `andThen()`                 |
|                  `ObjIntConsumer<T>`                   |  `accept`: `(T, int) -> void`   |                     -                      |
|                  `ObjLongConsumer<T>`                  |  `accept`: `(T, long) -> void`  |                     -                      |
|                 `ObjDoubleConsumer<T>`                 | `accept`: `(T, double) -> void` |                     -                      |

## Functions

| Functional interface (T, U, and R are type parameters) |       Functional method        |  Default methods unless otherwise indicated   |
|:------------------------------------------------------:|:------------------------------:|:---------------------------------------------:|
|                    `Function<T, R>`                    |       `apply`: `T -> R`        | `compose()`, `andThen()`, static `identity()` |
|                    `IntFunction<R>`                    |      `apply`: `int -> R`       |                       -                       |
|                   `LongFunction<R>`                    |      `apply`: `long -> R`      |                       -                       |
|                  `DoubleFunction<R>`                   |     `apply`: `double -> R`     |                       -                       |
|                   `ToIntFunction<R>`                   |    `applyAsInt`: `int -> R`    |                       -                       |
|                  `ToLongFunction<R>`                   |   `applyAsLong`: `long -> R`   |                       -                       |
|                 `ToDoubleFunction<R>`                  | `applyAsDouble`: `double -> R` |                       -                       |

| Functional interface (T, U, and R are type parameters) |         Functional method         | Default methods unless otherwise indicated |
|:------------------------------------------------------:|:---------------------------------:|:------------------------------------------:|
|                  `IntToLongFunction`                   |   `applyAsLong`: `int -> long`    |                     -                      |
|                 `IntToDoubleFunction`                  | `applyAsDouble`: `int -> double`  |                     -                      |
|                  `LongToIntFunction`                   |    `applyAsInt`: `long -> int`    |                     -                      |
|                 `LongToDoubleFunction`                 | `applyAsDouble`: `long -> double` |                     -                      |
|                 `DoubleToIntFunction`                  |   `applyAsInt`: `double -> int`   |                     -                      |
|                 `DoubleToLongFunction`                 | `applyAsLong`: `double  -> long`  |                     -                      |

## Two-Arity Specialization of `Function<T, R>`: `BiFunction<T, U, R>`

| Functional interface (T, U, and R are type parameters) |      Functional method      | Default methods unless otherwise indicated |
|:------------------------------------------------------:|:---------------------------:|:------------------------------------------:|
|                 `BiFunction<T, U, R>`                  |   `apply`: `(T, U) -> R`    |                `andThen()`                 |
|                `ToIntBiFunction<T, U>`                 |  `apply`: `(T, U) -> int`   |                     -                      |
|                `ToLongBiFunction<T, U>`                |  `apply`: `(T, U) -> long`  |                     -                      |
|               `ToDoubleBiFunction<T, U>`               | `apply`: `(T, U) -> double` |                     -                      |

| Functional interface (T, U, and R are type parameters) |         Functional method         | Default methods unless otherwise indicated |
|:------------------------------------------------------:|:---------------------------------:|:------------------------------------------:|
|                  `IntToLongFunction`                   |   `applyAsLong`: `int -> long`    |                     -                      |
|                 `IntToDoubleFunction`                  | `applyAsDouble`: `int -> double`  |                     -                      |
|                  `LongToIntFunction`                   |    `applyAsInt`: `long -> int`    |                     -                      |
|                 `LongToDoubleFunction`                 | `applyAsDouble`: `long -> double` |                     -                      |
|                 `DoubleToIntFunction`                  |   `applyAsInt`: `double -> int`   |                     -                      |

## Extending `Function<T,T>`: `UnaryOperator<T>`

| Functional interface (T, U, and R are type parameters) |           Functional method           |  Default methods unless otherwise indicated   |
|:------------------------------------------------------:|:-------------------------------------:|:---------------------------------------------:|
|        `UnaryOperator<T> extends Function<T,T>`        |           `apply`: `T -> T`           | `compose()`, `andThen()`, static `identity()` |
|                   `IntUnaryOperator`                   |     `applyAsInt`: `int` -> `int`      |           `compose()`, `andThen()`            |
|                  `LongUnaryOperator`                   |    `applyAsLong`: `long` -> `long`    |           `compose()`, `andThen()`            |
|                 `DoubleUnaryOperator`                  | `applyAsDouble`: `double` -> `double` |           `compose()`, `andThen()`            |

## Extending `BiFunction<T,T,T>`: `BinaryOperator<T>`

| Functional interface (T, U, and R are type parameters) |               Functional method               |   Default methods unless otherwise indicated    |
|:------------------------------------------------------:|:---------------------------------------------:|:-----------------------------------------------:|
|     `BinaryOperator<T> extends BiFunction<T,T,T>`      |            `apply`: `(T, T) -> T`             | `andThen()`, static `maxBy()`, static `minBy()` | 
|                  `IntBinaryOperator`                   |       `applyAsInt`: (`int, int) -> int`       |                        —                        | 
|                  `LongBinaryOperator`                  |     `applyAsLong`: `(long, long) -> long`     |                        —                        | 
|                 `DoubleBinaryOperator`                 | `applyAsDouble`: `(double, double) -> double` |                        —                        |

## Currying Functions

```java

@FunctionalInterface
interface TriFunction<T, U, V, R> {
    R compute(T t, U u, V v); // (T, U ,V) -> R
}
```

## Method and Constructor References

|              Purpose of method reference              |   Lambda expression/ Method reference    |                                 syntax Comment                                 |
|:-----------------------------------------------------:|:----------------------------------------:|:------------------------------------------------------------------------------:|
|               Designate a static method               |  `(args) -> RefType.staticMethod(args)`  |                                                                                |
|                                                       |         `RefType::staticMethod`          |                                       -                                        |
|  Designate an instance method of a bounded instance   |  `(args) -> expr.instanceMethod(args)`   |               Target reference provided by the method reference                |
|                                                       |          `expr::instanceMethod`          |                                                                                |
| Designate an instance method of an unbounded instance | `(arg0,rest)->arg0.instanceMethod(rest)` | arg0 is of RefType. Target reference provided later when the method is invoked |
|                                                       |        `RefType::instanceMethod`         |                                                                                |
|                Designate a constructor                |     `(args) -> new ClassType(args)`      |                        Deferred creation of an instance                        |
|                                                       |             `ClassType::new`             |                                                                                |
|             Designate array construction              |   `arg -> new ElementType[arg][]...[]`   |                         Deferred creation of an array                          |
|                                                       |       `ElementType[][]...[]::new`        |                                                                                |
