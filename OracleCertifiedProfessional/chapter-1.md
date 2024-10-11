# Basics of Java Programming

## The Java Ecosystem

* Starting with Java 11, JRE (Java Runtime Environment) is no longer available as a stand-alone bundle and not it's part
  of the JDK (Java Development Kit).
* one needs to install the JDK to both develop and run Java programs.
* he JDK tool **_jlink_** can be used to create a runtime image that includes the program code and the necessary runtime
  support to run the program.
* The Java programming language supports the object-oriented programming paradigm, in which the properties (fields) of
  an object and its behavior (methods) are encapsulated in the object.
* Java also incorporates the procedural programming paradigm as objects communicate through method calls
* Encapsulation exposes only **_what an object does_** and not **_how it does it_**
* Java programs are compiled to bytecode that is interpreted by the JVM

## Classes

* One of the fundamental ways in which we handle complexity is by using abstractions.
* The essence of OOP is modeling abstractions, using classes and objects.
* A `class` denotes a category of objects, and acts as a blueprint for creating objects.
* An **_object_** exhibits the **_properties_** and **_behaviors_** defined by its **_class_**.
* An important distinction is made between the contract and the implementation that a class provides for its objects.
* The contract defines which services are provided,and the implementation defines how these services are provided by the
  class.
* Clients (i.e., other objects) need only know the contract of an object, and not its implementation.
* Constructor is executed when an object is created from the class, and it doesn't create the class

## Objects

* The process of creating objects from a class is called instantiation.
* The object is a concrete instance of the abstraction that the class represents.
* A **_reference value_** is returned when an object is created, and it uniquely denotes a particular object
* A **_variable_** denotes a location in memory where a value can be stored.
* An object reference (or simply reference) is a variable that can store a reference value thus provides a handle to an
  object.
* object can only be manipulated by a reference that holds its reference value. Direct access to the reference value is
  not permitted.
* `Point2D p1 = new Point2D(10, 20);`
    * `p1` reference can refer to objects of class Point2D

## Instance and StaticMembers

* In contrast to the instance variables, the implementation of the methods is shared by all instances of the class.
* Static members in a class can be accessed both by the class name and via object references, but instance members can
  be accessed only by object references.

## Inheritance and Aggregation

* Two fundamental mechanisms for building new classes from existing ones: **_inheritance_** and **aggregation**
* _**private**_ fields in the superclass are accessible to the parent class indirectly through the **_public_** methods
* An **_association_** defines a `static` relationship between objects of two classes.
* One such **_association_**, called **_aggregation_** (also known as **_composition_**), expresses how an object uses
  other objects.
* Java supports aggregation of objects by reference, since objects cannot contain other objects explicitly.

## Sample Java Program

* The `main()` method has `public` access—that is, it is accessible from any class.
* Although a Java source file can contain more than one class declaration, the Java compiler enforces the rule that
  there can only be at the most one class in the source file that has public access
* If there is a public class in the source file, the name of the source file must be the name of the public class with
  **_.java_** as its extension
* It is the bytecode in the class files that is executed when a Java program.
* A Java program is run by the Java Application Launcher, java, which is also part of the JDK
* The java command creates an instance of the JVM that executes the bytecode.
* In order to run a single-file source-code program, the following conditions must be met:
    * The single source file must contain **_all_** source code for the program
    * Although there can be several class declarations in the source file, the first class declaration in the source
      file must provide the `main()` method; that is, it must be the entry point of the program
    * There must not exist `class` files corresponding to the class declarations in the single source file that are
      accessible by the **_java_** command
* Note that no class files are created. The source code is compiled fully in memory and executed. Several public classes
  in the single source file is allowed

## Program Output

* A Java program can send its output to the terminal window using an object called **_standard out_** which can be
  accessed using the `public static final` field out in the `System` class, is an object of the class
  `java.io.PrintStream`.
* The print methods convert a primitive value to a string that represents its literal value, and then print the
  resulting string
* An object is first converted to its text representation by calling its `toString()` method implicitly, if it is not
  already called explicitly on the object.
* The `toString()` method called on a `String` object returns the `String` object itself

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
