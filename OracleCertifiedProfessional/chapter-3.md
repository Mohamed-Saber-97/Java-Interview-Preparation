# Declarations

## Class Declarations

* A static context is defined by static methods, static field initializers, and static initializer blocks
* A non-static context is defined by instance methods, instance field initializers, instance initializer blocks, and constructors.
* One crucial difference between the two contexts is that static code in a class can only refer to other static members in the class, whereas non-static code can refer to any member of the class

## Method Declarations

* A non-void method must either use a return statement to return a value to the caller of the method or throw an exception to terminate its execution
* The parameter names are local to the method, and It is recommended to use the `@param` tag in a Javadoc comment to document the formal parameters of a method
* The type in the parameter declaration cannot be designated by the var reserved type name At compile time, it is impossible to determine the type of the formal parameter
* The signature of a method comprises the name of the method and the types of the formal parameters only
    * `double cubeVolume(double length, double width, double height) {}` has the signature `cubeVolume(double, double, double)`

## Variable Declarations

* A **_variable_** stores a value of a particular type. A variable has a name, a type, and a value associated with it
* variables can store only values of primitive data types and reference values of objects
* Field variables are variables that are declared in type declarations (`classes`, `interfaces`, and `enums`).
* Local variables are variables that are declared in methods, constructors, and blocks and can be declared with the var type name.
* A reference variable can store the reference value of an object, and can be used to manipulate the object denoted by the reference value
* The declaration determines which objects can be referenced by a reference variable
* Default Values for Fields:

|                Data type                 |         Default value         |
|:----------------------------------------:|:-----------------------------:|
|                `boolean`                 |            `false`            |
|                  `char`                  |           `\u0000`            |
| Integer (`byte`, `short`, `int`, `long`) | `0L` for long, `0` for others |
|    Floating-point (`float`, `double`)    |       `0.0F` or `0.0D`        |
|             Reference types              |            `null`             |

* If no explicit initialization is provided for a static variable, it is initialized with the default value of its type when the class is loaded the first time
* if no initialization is provided for an instance variable, it is initialized with the default value of its type when the class is instantiated accordingly in every object created from the class
* The fields of reference types are always initialized with the null reference value if no initialization is provided
* Local variables are not initialized implicitly when they are allocated memory at method invocation that is, when the execution of a method begins
* Local variables must be explicitly initialized before being used. The compiler will report an error only if an attempt is made to use an uninitialized local variable

## Instance Methods and the Object Reference this

* the `this` reference cannot be used in a static context, as static code is not executed in the context of any object

## Method Overloading

* Each method has a signature, which comprises the name of the method plus the types and order of the parameters in the formal parameter list.
* Several method implementations may have the same name, as long as the method signatures differ. This practice is called method **_overloading_**
* Only methods declared in the same class and those that are inherited by the class can be overloaded
* At compile time, the right implementation of an overloaded method is chosen, based on the signature of the method call.

## Constructors

* The only action taken by the default constructor is to call the superclass constructor. This ensures that the inherited state of the object is initialized properly

## Static Member Declarations

* When the class is loaded, static fields are initialized to their default values if no explicit initialization is specified
* **_Shadowing_** of fields by local variables is different from **_hiding_** of fields by field declarations in subclasses
* local variable shadows the static field, the simple name now refers to the local variable. The shadowed static field can of course be accessed using the class name
* Static methods are often used to implement utility classes that provide common and frequently used functions
* Static methods in a superclass cannot be overridden in a subclass as instance methods can, but they can be hidden by static methods in a subclass
* A type parameter of a generic class or interface cannot be used in a `static` method

## Arrays

* An array is a data structure that defines an indexed collection with a fixed number of data elements that all have the same type
* References in the array can then denote objects of this reference type or its subtypes
* `element_type[] array_name = new element_type[array_size];` or `element_type[] array_name = { array_initialize_list };` or `element_type[] array_name = new element_type[] { array_initialize_list };`
* The minimum value of `array_size` is 0; in other words, zero-length arrays can be constructed in Java. If the array size is negative, a `NegativeArraySizeException` is thrown at runtime
* When the array is constructed, all of its elements are initialized to the default value. This is true for both member and local arrays when they are constructed
* The list can also be legally terminated by a comma `Topping[] pizzaToppings = { new Topping("cheese"), new Topping("tomato"), };`
* The **_index_** should be promotable to an `int` value; otherwise, a compile-time error is flagged
* At runtime, the index value is automatically checked to ensure that it is within the array index bounds.
* If the **_index_** value is less than 0, or greater than or equal to `array_name.length`, an `ArrayIndexOutOfBoundsException` is thrown

```java
int[] daysInMonth;
daysInMonth ={31,28,31,30,31,30,
        31,31,30,31,30,31}; // Compile-time error
daysInMonth =new int[]{31,28,31,30,31,30,31,31,30,31,30,31}; // OK
```

* Since an array element can be an object reference and arrays are objects, array elements can themselves refer to other arrays `element_type[][]...[] array_name;`

```java
Pizza[][] pizzaGalore = {
        {new Pizza(), null, new Pizza()}, // 1. row is an array of 3 elements.
        {null, new Pizza()}, // 2. row is an array of 2 elements.
        new Pizza[1], // 3. row is an array of 1 element.
        {}, // 4. row is an array of 0 elements.
        null // 5. row is not constructed.
};
```

* When constructing multidimensional arrays with the new operator, the length of the deeply nested arrays may be omitted

```java
void test() {
    //street, hotel, floor, and room, respectively
    HotelRoom[][][][] rooms = new HotelRoom[10][5][][]; // Just streets and hotels
    rooms[0][0] = new HotelRoom[3][]; // 3 floors in 1st hotel on 1st street.
    rooms[0][0][0] = new HotelRoom[8]; // 8 rooms on 1st floor in this hotel.
    rooms[0][0][0][0] = new HotelRoom(); // Initializes 1st room on this floor.
}
```

## Parameter Passing

* Objects communicate by calling methods on each other. A method call is used to invoke a method on an object
* each invocation of a method has its own copies of the formal parameters, as is the case for any local variables in the method
* The JVM uses a **_stack_** to keep track of method execution and a **_heap_** to manage the objects that are created by the program
* Values of local variables and those passed to the method as parameters, together with any temporary values computed during the execution of the method, are always stored on the stack
* only primitive values and reference values are stored on the stack, and only these can be passed as parameters in a method call, but never any object from the heap
* In Java, all parameters are passed by value—that is, an actual parameter is evaluated and its value from the stack is assigned to the corresponding formal parameter

|              Data type of the formal parameter              |                 Value passed                 |
|:-----------------------------------------------------------:|:--------------------------------------------:|
|                     Primitive data type                     | Primitive data value of the actual parameter |
| Reference type (i.e., class, interface,array, or enum type) |   Reference value of the actual parameter    |

```java
int i = 4;

leftRight(
        i++,
        i
        );//(4,5) not (4,4)
```

## Variable Arity Methods

* Only one variable arity parameter is permitted in the formal parameter list, and it is always the last parameter in the list
* To overload a variable arity method, it is not enough to change the type of the variable arity parameter to an explicit array type. The compiler will complain if an attempt is made to overload the
  method `transmit()`, as shown in the following code:

```java
//Both methods above have the signature transmit(String[])
public static void transmit(String... data) { } // Compile-time error!

public static void transmit(String[] data) { } // Compile-time error!
```

## Local Variable Type Inference

* The var restricted type name is allowed in local variable declarations in blocks  (including initializer blocks), constructors, and methods
* A local declaration with var requires an initialization expression, which in the case of local arrays must be either an array creation expression or an anonymous array expression. it should be
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