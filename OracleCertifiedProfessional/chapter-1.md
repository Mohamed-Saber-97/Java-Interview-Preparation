# Basics of Java Programming

## The Java Ecosystem

### Multi-paradigm Programming

- The Java programming language supports the _object-oriented programming paradigm_, in which the properties of an object and its behavior are encapsulated in the object.
- The properties and the behavior are represented by the fields and the methods of the object, respectively.
- The objects communicate through method calls in a procedural manner. in other words, Java also incorporates the _procedural programming paradigm_.
- Encapsulation exposes only _what_ an object does and not _how_ it does it, so that its implementation can be changed with minimal impact on its clients.

### Architecture-Neutral and Portable Bytecode

- Java programs are compiled to bytecode that is interpreted by the JVM.
- The often-cited slogan “Write once, run everywhere” is true only if a compatible JVM is available for the hardware and software platform.
- The specification of the bytecode is architecture neutral, meaning it is independent of any hardware architecture.

### Simplicity

- Java does not have a preprocessor, and it does not allow pointer handling, user-defined operator overloading, or multiple class inheritance, and it opted for automatic garbage collection.

### Dynamic and Distributed

- The JVM can dynamically load class libraries from the local file system as well as from machines on the network, when those libraries are needed at runtime.

### Robust and Secure

- Runtime index checks for arrays and strings, automatic garbage collection, and elimination of pointers are some of the features of Java that promote reliability.
- the module system further enhances encapsulation and configuration.
- The exception handling feature of Java is without a doubt the main factor that facilitates the development of robust systems.
- The language does not allow direct access to memory.
- A bytecode verifier determines whether any untrusted code loaded in the JVM is safe.
- The sandbox model is used to confine and execute any untrusted code, limiting the damage that such code can cause.

### High Performance and Multithreaded

- The JIT feature monitors the program at runtime to identify performance-critical bytecode (called _hotspots_) that can be optimized and usually translated to machine code to boost performance.

## Classes

- An important distinction is made between the _contract_ and the _implementation_ that a class provides for its objects.
- The contract defines _which_ services are provided, and the implementation defines _how_ these services are provided by the class.
- Clients  (i.e., other objects) need only know the contract of an object, and not its implementation, to avail themselves of the object’s services.

### Declaring Members: Fields and Methods

- a constructor is executed when an object is created from the class.

## Objects

- The process of creating objects from a class is called _instantiation_.
- A _reference value_ is returned when an object is created which uniquely denotes a particular object.
- A _variable_ denotes a location in memory where a value can be stored.
- An _object reference_ (or simply _reference_) is a variable that can store a reference value. objects in Java do not have names, but rather are denoted by references.
- Thus, a reference provides a handle to an object, as it can indirectly denote an object whose reference value it holds.
- In Java, an object can only be manipulated by a reference that holds its reference value.
- Direct access to the reference value is not permitted.
- Two distinct objects can have the same state if their instance variables have the same values.
- The purpose of the constructor call on the right-hand side of the `new` operator is to initialize the fields of the newly created object.

## Instance Members

- Objects communicate by calling methods on each other.
- The method invoked on the object can also return results back to its caller, via a single return value
- _reference.fieldName_ Use of the dot notation is governed by the accessibility of the member

## Static Members

- A static variable will be allocated in the class and initialized to the string specified in its declaration when the class is loaded
- Static members in a class can be accessed both by the class name and via object references, but instance members can be accessed only by object references.

## Inheritance

- There are two fundamental mechanisms for building new classes from existing ones: _inheritance_ and _aggregation_
- The child class is called the _subclass_, and the parent class is called the _superclass_
- The **superclass** class is a **_generalization_**, whereas the class **subclass** is a **_specialization_**.
- A subclass can extend only one superclass
- `private` fields in the superclass are accessible to a _subclass_ object indirectly through the `public` get and set methods

## Aggregation

- An _association_ defines a static relationship between objects of two classes. One such _association_, called _aggregation_ (also known as _composition_), expresses how an object uses other objects.
- Java supports aggregation of objects by reference, since objects cannot contain other objects explicitly

## Sample Java Program

- The JDK provides tools for compiling and running programs. The classes in the Java SE Platform API are already compiled, and the JDK tools know where to find them
- Java source files can be compiled using the _**Java Language Compiler**_, `javac`, which is part of the JDK
- _at the most_ one class in the source file that has `public` access
- If there is a public class in the source file, the name of the source file must be the name of the public class with .java as its extension
- A Java program is run by the **_Java Application Launcher_**, `java`, which is also part of the JDK
- The `java` command creates an instance of the JVM that executes the bytecode.

### Running a Single-File Source-Code Program

- In order to run a single-file source-code program, the following conditions must be met:
    - The single source file must contain **_all_** source code for the program
    - Although there can be several class declarations in the source file, the first class declaration in the source file must provide the `main()` method; that is, it must be the entry point of the program
    - There must not exist `class` files corresponding to the class declarations in the single source file that are accessible by the **_java_** command

- Note that no class files are created. The source code is compiled fully in memory and executed. Several public classes in the single source file is allowed

## Program Output

- A Java program can send its output to the terminal window using an object called **_standard out_** which can be accessed using the `public static final` field out in the `System` class, is an object of the class `java.io.PrintStream`.
- The print methods convert a primitive value to a string that represents its literal value, and then print the resulting string
- An object is first converted to its text representation by calling its `toString()` method implicitly, if it is not already called explicitly on the object.
- The `toString()` method called on a `String` object returns the `String` object itself

|     Parameter Value      | Format Spec | Example Value | String Printed |                                                                  Description                                                                  |
|:------------------------:|:-----------:|:-------------:|:--------------:|:---------------------------------------------------------------------------------------------------------------------------------------------:|
|    **Integer Value**     |    `%d`     |     `123`     |     "123"      |                                                Occupies as many character positions as needed.                                                |
|    **Integer Value**     |    `%6d`    |     `123`     |    "   123"    |           Occupies six character positions and is right-justified. The printed string is padded with leading spaces, if necessary.            |
|    **Integer Value**     |   `%06d`    |     `123`     |    "000123"    |            Occupies six character positions and is right-justified. The printed string is padded with leading zeros, if necessary.            |
| **Floating-Point Value** |    `%f`     |    `4.567`    |   "4.567000"   |                            Occupies as many character positions as needed, but always includes six decimal places.                            |
| **Floating-Point Value** |   `%.2f`    |    `4.567`    |     "4.57"     |    Occupies as many character positions as needed, but includes only two decimal places. The value is rounded in the output, if necessary.    |
| **Floating-Point Value** |   `%6.2f`   |    `4.567`    |   "   4.57"    | Occupies six character positions, including the decimal point, and uses two decimal places. The value is rounded in the output, if necessary. |
|      **Any Object**      |    `%s`     |     `Hi!`     |     "Hi!"      |                             The text representation of the object occupies as many character positions as needed.                             |
|      **Any Object**      |    `%6s`    |     `Hi!`     |    "   Hi!"    |                        The text representation of the object occupies six character positions and is right-justified.                         |
|      **Any Object**      |   `%-6s`    |     `Hi!`     |    "Hi!   "    |                         The text representation of the object occupies six character positions and is left-justified.                         |