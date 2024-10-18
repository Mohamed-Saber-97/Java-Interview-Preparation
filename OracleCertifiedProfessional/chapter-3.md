<!-- TOC -->

* [Declarations](#declarations)
    * [Class Declarations](#class-declarations)
    * [Method Declarations](#method-declarations)
    * [Variable Declarations](#variable-declarations)
        * [Declaring and Initializing Variables](#declaring-and-initializing-variables)
        * [Reference Variables](#reference-variables)
        * [Initial Values for Variables](#initial-values-for-variables)
            * [Initializing Local Variables of Primitive Data Types](#initializing-local-variables-of-primitive-data-types)
            * [Initializing Local Reference Variables](#initializing-local-reference-variables)
            * [Lifetime of Variables](#lifetime-of-variables)
    * [Instance Methods and the Object Reference `this`](#instance-methods-and-the-object-reference-this)
    * [Method Overloading](#method-overloading)
    * [Constructors](#constructors)
    * [Static Member Declarations](#static-member-declarations)
        * [Static Members in Classes](#static-members-in-classes)
        * [Static Fields in Classes](#static-fields-in-classes)
        * [Static Methods in Classes](#static-methods-in-classes)
    * [Arrays](#arrays)
        * [Constructing an Array](#constructing-an-array)
        * [Using an Array](#using-an-array)
        * [Anonymous Arrays](#anonymous-arrays)
        * [Multidimensional Arrays](#multidimensional-arrays)
    * [Parameter Passing](#parameter-passing)
        * [Passing Primitive Data Values](#passing-primitive-data-values)
        * [Passing Reference Values](#passing-reference-values)
        * [`final` Parameters](#final-parameters)
    * [Local Variable Type Inference](#local-variable-type-inference)

<!-- TOC -->
--------

# Declarations

## Class Declarations

- **_static code in a class can only refer to other static members_** in the class, whereas **_non-static code can refer to any member of the class_**.

## Method Declarations

- A non-`void` method must either use a return statement to `return` a value or throw an exception to terminate its execution
- The _type_ in the parameter declaration cannot be designated by the `var` reserved type name. At compile time, it is impossible to determine the type of the formal parameter
- The _signature_ of a method comprises the name of the method and the types of the formal parameters only
    - method: `double cubeVolume(double length, double width, double height) {}`
    - signature: `cubeVolume(double, double, double)`

## Variable Declarations

- A _variable_ stores a value of a particular type. A variable has a name, a type, and a value associated with it.
- variables can store only values of primitive data types and reference values of objects
- Field variables are variables that are declared in _type declarations_ (_classes_, _interfaces_, and _enums_).
- Local variables are variables that are declared in _methods_, _constructors_, and _blocks_. Local variables can be declared with the `var` type name

### Declaring and Initializing Variables

- _declare variables_, meaning they are used to specify the type and the name of variables
- This implicitly determines their memory allocation and the values that can be stored in them.

### Reference Variables

- A _reference variable_ can store the reference value of an object, and can be used to manipulate the object denoted by the reference value
- _reference variable_  declaration specifies the name and the reference type only and determines which objects can be referenced by it

### Initial Values for Variables

- Default Values for Fields:

|                Data type                 |         Default value         |
|:----------------------------------------:|:-----------------------------:|
|                `boolean`                 |            `false`            |
|                  `char`                  |           `\u0000`            |
| Integer (`byte`, `short`, `int`, `long`) | `0L` for long, `0` for others |
|    Floating-point (`float`, `double`)    |       `0.0F` or `0.0D`        |
|             Reference types              |            `null`             |

- If no explicit initialization is provided for a `static` variable, it is initialized with the default value of its type when the class is loaded
- if no initialization is provided for an instance variable, it is initialized with the default value of its type when the class is instantiated.

#### Initializing Local Variables of Primitive Data Types

- Local variables are variables that are declared in _methods_, _constructors_, and _blocks_
- They are not initialized implicitly when they are allocated memory at method invocation—that is, when the execution of a method begins
- Local variables must be explicitly initialized before being used. The compiler will report an error only if an attempt is made to use an uninitialized local variable

#### Initializing Local Reference Variables

- If the reference is set to the value `null`, the program will compile. However, a runtime error (`NullPointerException`) will occur when the code is executed because the reference will not denote
  any object.

#### Lifetime of Variables

- lifetime of variables in the following three contexts:
    - _Instance variables_: Instance variables exist as long as the object they belong to is in use at runtime.
    - _Static variables_: They are created when the class is loaded at runtime and exist as long as the class is available at runtime.
    - _Local variables_: After the execution of the method, constructor, or block completes, local variables are no longer accessible

## Instance Methods and the Object Reference `this`

- The reason is that all instance methods are passed an implicit reference to the _current object_—that is, the object on which the method is being invoked.
- the keyword `this` can be used in any non-`static` context and cannot be used in a `static` context, as `static` code is not executed in the context of any object
- The `this` reference can be used as a normal reference to reference the current object, but the reference cannot be modified—it is a `final` reference

## Method Overloading

- Each method has a _signature_, which comprises the name of the method plus the types and order of the parameters in the formal parameter list
- Only methods declared in the same class and those that are inherited by the class can be overloaded
- At compile time, the right implementation of an overloaded method is chosen, based on the signature of the method call

## Constructors

- following restrictions on constructors should be noted:
    - Modifiers other than an access modifier are not permitted in the constructor header
    - Constructors cannot return a value, and therefore, do not specify a return type, not even void, in the constructor header. But their declaration can use the return statement (without the return
      value) in the constructor body
    - The constructor name must be the same as the class name
- Class names and method names exist in different namespaces. Thus, there are no name conflicts where a method has the same name the constructor

## Static Member Declarations

### Static Members in Classes

- The class need not be instantiated to access its static members. This is in contrast to instance members of the class which can only be accessed by references that actually refer to an instance of
  the class.

### Static Fields in Classes

- Static fields (also called _static variables_ and _class variables_) exist only in the class in which they are defined

### Static Methods in Classes

- Static methods can be overloaded analogous to instance methods
- Static methods in a superclass cannot be overridden in a subclass as instance methods can, but they can be _hidden_ by `static` methods in a subclass
- A type parameter of a generic class or interface cannot be used in a `static` method

## Arrays

- An _array_ is a data structure that defines an indexed collection with a fixed number of data elements that all have the _same type_
- The size of an array is fixed and cannot be changed after the array has been created.
- In Java, arrays are objects. Arrays can be of primitive data types or reference types
- All elements in the array are either of a specific primitive data type or references of a specific reference type
- References in the array can then denote objects of this reference type or its subtypes

### Constructing an Array

- `element_type1[] array_name = new element_type2[array_size];` The minimum value of `array_size` is 0; in other words, zero-length arrays can be constructed in Java and If the array size is negative,
  a
  `NegativeArraySizeException` is thrown at runtime.
- When the array is constructed, all of its elements are initialized to the default value for _element_type2_. This is true for both member and local arrays when they are constructed
- `element_type[] array_name = { array_initialize_list };`

### Using an Array

- `array_name [index_expression]` The index is specified by the _index_expression_, whose value should be promotable to an int value; otherwise, a compile-time error is flagged
- At runtime, the index value is automatically checked to ensure that it is within the array index bounds.
- If the index value is less than 0, or greater than or equal to array_name.length, an `ArrayIndexOutOfBoundsException` is thrown

### Anonymous Arrays

```java
int[] daysInMonth;
daysInMonth ={31,28,31,30,31,30,31,31,30,31,30,31}; // Compile-time error
daysInMonth =new int[]{31,28,31,30,31,30,31,31,30,31,30,31}; // OK
```

### Multidimensional Arrays

- When constructing multidimensional arrays with the new operator, the length of the deeply nested arrays may be omitted

```java
void test() {
    //street, hotel, floor, and room
    HotelRoom[][][][] rooms = new HotelRoom[10][5][][]; // Just streets and hotels
    rooms[0][0] = new HotelRoom[3][]; // 3 floors in 1st hotel on 1st street.
    rooms[0][0][0] = new HotelRoom[8]; // 8 rooms on 1st floor in this hotel.
    rooms[0][0][0][0] = new HotelRoom(); // Initializes 1st room on this floor.
}
```

## Parameter Passing

- It should also be stressed that each invocation of a method has its own copies of the formal parameters, as is the case for any local variables in the method
- The JVM uses a **_stack_** to keep track of method execution and a **_heap_** to manage the objects that are created by the program
- Values of local variables and those passed to the method as parameters, together with any temporary values computed during the execution of the method, are always stored on the **_stack_**
- Thus, only primitive values and reference values are stored on the stack, and only these can be passed as parameters in a method call, but never any object from the heap

### Passing Primitive Data Values

- An actual parameter is an expression that is evaluated first, with the resulting value then being assigned to the corresponding formal parameter at method invocation
- when the actual parameter is a variable of a primitive data type, the value of the variable from the stack is copied to the formal parameter at method invocation
- Since formal parameters are local to the method, any changes made to the formal parameter will not be reflected in the actual parameter after the call completes
- However, for parameter passing, there are no implicit narrowing conversions for integer constant expressions

```java
static void doIt(long i) { /* ... */ }

void test() {
    Integer intRef  = 34;
    Long    longRef = 34L;
    doIt(34); // (1) Primitive widening conversion: long <-- int
    doIt(longRef); // (2) Unboxing: long <-- Long
    doIt(intRef); // (3) Unboxing, followed by primitive widening conversion: long <-- int <-- Integer
}
```

### Passing Reference Values

- If the actual parameter expression evaluates to a reference value, the resulting reference value on the stack is assigned to the corresponding formal parameter reference at method invocation

### `final` Parameters

- A `final` parameter is also known as a _blank final variable_; that is, it is blank (uninitialized) until a value is assigned to it,  (e.g., at method invocation) and then the value in the variable
  cannot be changed during the lifetime of the variable.
- The compiler can treat final variables as constants for code optimization purposes.

## Local Variable Type Inference

- The var restricted type name is allowed in local variable declarations in blocks  (including initializer blocks), constructors, and methods
- A local declaration with var requires an initialization expression, which in the case of local arrays must be either an array creation expression or an anonymous array expression. it should be
  possible to infer both the array element type and the size of the array. It cannot be an array initializer.

```java
public class ValidLVTI {
    // Static initializer block:
    static {
        var slogan = "Keep calm and code Java."; // Allowed in static initializer block
    }
    
    // Instance initializer block:
    {
        var banner = "Keep calm and catch exceptions."; // Allowed in instance initializer block
    }
    
    // Constructor:
    public ValidLVTI() {
        var luckyNumber = 13; // (2) Allowed in a constructor.
    }
    
    // Method:
    public static void main(String[] args) {
        var virus        = "COVID-19"; // Type of virus is String.
        var acronym      = virus.substring(0, 5); // Type of acronym is String.
        var num          = Integer.parseInt(virus.substring(6)); // Type of num is int.
        var obj          = new Object(); // (4) Type of obj is Object.
        var title        = ( String ) null; // (5) Initialization expression type is String. Type of title is String
        var sqrtOfNumber = Math.sqrt(100); // (6) Type of sqrtOfNumber is double, since the method returns a double value.
        var tvSize       = ( short ) 55; // (7) Type of tvSize is short.
        var tvSize2      = 65; // (8) Type of tvSize2 is int.
        var diameter     = 10.0; // (9) Type of diameter is double.
        var radius       = 2.5F; // Type of radius is float.
// Arrays:
        var vowels      = new char[]{'a', 'e', 'i', 'o', 'u'}; // (11a) Type of vowels is char[]. Size is 5
        var zodiacSigns = new String[12]; // (11b) Type of zodiacSigns is String[].Size is 12.
        var a_2x3       = new int[2][3]; // (11c) Type of a_2x3 is int[][]. Size is 2x3.
        var a_2xn       = new int[2][]; // (11d) Type of a_2xn is int[][]. Size is 2x?, where second dimension can be undefined
// The for(:) loop:
        var word1 = ""; // Type of word2 is String.
        for ( var vowel : vowels ) { // Type of vowel is char in the for(:)loop.
            var letter = vowel; // Type of letter is char.
            word1 += letter;
        }
// The for(;;) loop:
        var word2 = ""; // Type of word2 is String.
        for ( var i = 0; i < vowels.length; i++ ) { // Type of i is int in the for loop
            var letter = vowels[i]; // Type of letter is char.
            word2 += letter;
        }
        // switch-statement:
        switch ( virus ) {
            case "Covid-19":
                var flag = "Needs to be tested."; // Type is String.Do testing
                break;
            default: // Do nothing.
        }
    }
}
```

```java
public class InvalidLVTI {
    var javaVendor = "Oracle"; // Not allowed in instance variable declaration.
    static var javaVersion = 11; // Not allowed in static variable declaration.
    
    public static void main(var args) { // Not allowed for method parameters.
        var name; // Not allowed without initialization expression.
        var objRef     = null; // Literal null not allowed.
        var x          = 10.0, y = 20.0, z = 40; // Not allowed in compound declaration.
        var vowelsOnly = {'a', 'e', 'i', 'o', 'u'}; // Array initializer not allowed
        var attendance = new int[]; // Non-empty dimension required.
        var array3Dim  = new String[][ 2][]; // Cannot specify an empty dimension before a non-empty dimension
        var letters[] = new char[]{'a', 'e', 'i', 'o', 'u'}; // var not allowed as element type
        var prompt    = prompt + 1; // Self-reference not allowed in initialization expression
    }
    
    public static var getPlatformName() { // Not allowed as return type.
        return "JDK";
    }
}
```