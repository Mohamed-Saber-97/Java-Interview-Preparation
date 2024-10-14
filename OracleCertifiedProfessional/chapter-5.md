# Object-Oriented Programming

## Implementing Inheritance

* If no extends clause is specified in the header of a class declaration, the class implicitly inherits from the `java.lang.Object` class.
* If a superclass member is accessible by its simple name in the subclass (without the use of any extra syntax, like `super`), that member is considered inherited.
* Conversely, private, overridden, and hidden members of the superclass are not inherited.
* Inheritance should not be confused with the existence of such members in the state of a subclass object.
* Since constructors are not members of a class, they are not inherited by a subclass
* Members of the superclass that have private access are not inherited by the subclass and can only be accessed indirectly.
* `private` field of the superclass is not inherited, but exists in the subclass object and is indirectly accessible through `public` methods.
* If their lifetimes are dependent on the lifetime of the aggregate object, then this relationship is called **_composition_**, and implies strong ownership of the parts by the composite object
* A good design strategy advocates that inheritance should be used only if the relationship is-a is unequivocally maintained throughout the lifetime of the objects involved; otherwise, aggregation is
  the best choice
* Code reuse is also best achieved by aggregation when there is no is-a relationship
* However, changing the contract of a superclass can have consequences for the subclasses (called the ripple effect) as well as for clients that are dependent on a particular behavior of the
  subclasses. For this reason, **_aggregation provides stronger encapsulation than inheritance_**
* The new method in the subclass must abide by the following rules of method overriding:
    * The new method definition in the subclass must have the same method signature. Whether parameters in the overriding method should be final is at the discretion of the subclass. A method’s
      signature does not comprise the final modifier of parameters, only their types and order
    * The return type of the overriding method can be a subtype of the return type of the overridden method called covariant return, and it applies only to reference types, not to primitive types.
      Changing the return type to another primitive type will result in a compile-time error. There is no subtype–supertype relationship
      between primitive types
    * The new method definition cannot narrow the accessibility of the method, but it can widen it
    * The new method definition can throw either all or none, or a subset of the checked exceptions (including their subclasses) that are specified in the throws clause of the overridden method in the
      superclass
    * the overriding method may not throw any checked exception that is wider than the checked exceptions thrown by the overridden method. However, the overriding method
      may throw any unchecked exceptions.
* more facts to note about overriding:
    * A subclass must use the keyword super to invoke an overridden method in the superclass
    * A final method cannot be overridden because the modifier final prevents method overriding. An attempt to override a final method will result in a com- pile-time error
    * An abstract method, in contrast, requires the non-abstract subclasses to override the method to provide an implementation
    * The access modifier private for a method means that the method is not accessible outside the class in which it is defined; therefore, a subclass cannot override it. However, a subclass can
      give its own definition of such a method, which may have the same signature as the method in its superclass
    * An instance method in a subclass cannot override a static method in the superclass. The compiler will flag such an attempt as an error. A static method is class specific and not part of any
      object, while overriding methods are invoked on behalf of objects of the subclass. However, a static method in a subclass can hide a static method in the superclass,
      but it cannot hide an instance method in the superclass
    * Constructors, since they are not inherited, cannot be overridden
* Only non-final instance methods in the superclass that are directly accessible from the subclass using their simple name can be overridden
* both non-private instance and static methods can be overloaded in the class they are defined in or are in a subclass of the class they are defined in
* For overloaded and static methods, which method implementation will be executed at runtime is determined at compile time. In contrast, for overridden methods, the method implementation to be
  executed is determined at runtime
* A subclass cannot override inherited fields of the superclass, but it can hide them
* The following distinction between invoking instance methods on an object and accessing fields of an object must be noted. When an instance method is invoked on an object using a reference, it is the
  dynamic type of the reference (i.e., the type of the current object denoted by the reference at runtime), not the declared type of the reference, that determines which method implementation will be
  executed
* When a field of an object is accessed using a reference, it is the declared type of the reference, not the type of the current object denoted by the reference, that determines which field will
  actually be accessed
* Hiding a static method is analogous to overriding an instance method except for one important aspect:  Calls to static methods are bound at compile time as opposed to runtime for calls to overridden
  instance methods, and therefore do not exhibit polymorphic behavior exhibited by calls to overridden instance methods and If the signatures are different, the method name is overloaded, not hidden.*
  A static method in the subclass can only hide a static method in the superclass
* Analogous to accessing fields, the static method invocation is determined by the declared type of the reference
* In summary, a subclass can do any of the following:
    * Access inherited members using their simple names
    * Override a non-final instance method with the same signature as in the superclass
    * Overload a static or instance method with the same name as in the superclass
    * Hide a static non-final method with the same signature as in the superclass
    * Hide a static or instance field with the same name as in the superclass
    * Declare new members that are not in the superclass
    * Declare constructors that can invoke constructors in the superclass implicitly, or use the `super()` construct with the appropriate arguments to invoke them explicitly

|             Comparison criteria             |                                                                        Overriding of instance methods/Hiding of `static` methods                                                                        |                   Overloading of instance and `static` methods                    |
|:-------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------:|
|                 Method name                 |                                                                                            Must be the same                                                                                             |                                 Must be the same                                  |
|                Argument list                |                                                                                            Must be the same                                                                                             |                                 Must be different                                 |
|                 Return type                 |                                                                                Can be the same type or a covariant type                                                                                 |                                 Can be different                                  |
|                throws clause                |                                Can be restrictive about checked exceptions thrown.Must not throw new checked exceptions, but can include subclasses of exceptions thrown                                |                                 Can be different                                  |
|                Accessibility                |                                                                          Can make it less restrictive,but not more restrictive                                                                          |                                 Can be different                                  |
|               final modifier                |                             A private instance/static method cannot be overridden/hidden. No compile-time error if private instance/static method is defined in a subclass                              |               Can be overloaded in the same class or in a subclass                |
|              private modifier               |                             A private instance/static method cannot be overridden/hidden. No compile-time error if private instance/static method is defined in a subclass                              |                        Can be overloaded in the same class                        |
|             Declaration context             |                                                                 An instance/static method can only be overridden/ hidden in a subclass                                                                  | An instance or static method can be overloaded in the same class or in a subclass |
| Binding of method call/polymorphic behavior | Calls to overridden instance methods are bound at runtime, and therefore exhibit polymorphic behavior. Calls to hidden static methods are bound at compile time and do not exhibit polymorphic behavior |      Calls are bound at compile time and do not exhibit polymorphic behavior      |

## The Object Reference super

* The `this` reference can be used in non-static code to refer to the current object. The keyword `super`, in contrast, can be used in non-static code to access fields and invoke methods from the
  superclass
* The keyword super provides a reference to the current object as an instance of its superclass
* In method invocations with super, the method from the superclass is invoked regardless of what the actual type of the current object is or whether the current class overrides the method

## Abstract Classes and Methods

* The keyword abstract is used in the following contexts in Java:
    * Declaring abstract classes
    * Declaring abstract methods in classes, in interfaces, and in enum types
* Like a normal class, an abstract class can declare class members, constructors, and initializers
* A class that is declared `absract` cannot be instantiated, regardless of whether it has abstract methods or not
* Creating an object of a subclass results in the fields of all its superclasses, whether these classes are abstract or not, to be created and initialized—that is, they exist in the subclass object
* A class cannot be declared both final and abstract—that would be a contradiction in terms: A final class cannot be extended, but an abstract class is incomplete or considered to be incomplete and
  must be extended
* An abstract class should not be used to implement a class that cannot be instantiated. The recommended practice is to only provide a zero-argument constructor that is private, thus making sure that
  it is never invoked in the class
* In many ways abstract classes and interfaces are similar, and interfaces can be used with advantage in many cases. However, if private state should be maintained with instance members, then abstract
  classes are preferred, as interfaces do not have any notion of state
* When overriding an abstract method from the superclass, the notation @Override should always be used in the overriding method in the subclass. The compiler will issue an error if the override
  criteria are not satisfied
* The accessibility of an abstract method declared in a top-level class cannot be private, as subclasses would not be able to override the method and provide an implementation. Thus, an abstract
  method
  in a top-level class can only have public, protected, or package accessibility

## Final Declarations

* The keyword final can be used in the following contexts in Java:
    * Declaring final classes
    * Declaring final members in classes, and in enum types
    * Declaring final local variables in methods and blocks
    * Declaring final static variables in interfaces
* A class can be declared final to indicate that it cannot be extended—that is, one cannot define subclasses of a final class
* Interfaces are implicitly abstract, and therefore cannot be declared as final
* The Java SE Platform API includes many final classes—for example, the java.lang.String class and the wrapper classes for primitive values
* A final method in a class is a concrete method (i.e., has an implementation) and cannot be overridden or hidden in any subclass. Any normal class can declare a final method. The class need not be
  declared final
* A method that is declared final allows the compiler to do code optimization by replacing its method call with the code of its method body, a technique
  called inlining
* A subclass cannot override or hide a private method from its superclass. However, it can define a new method using the same method signature as the private method, without regard to the other
  criteria for overriding: the return type and the `throws` clause. Defining a new method using the same method signature is impossible with a final method which cannot be overridden or hidden.
  Declaring a private method as final in addition is redundant
* Although a constructor cannot be overridden, it also cannot be declared final
* A final variable is a variable whose value cannot be changed during its lifetime once it has been initialized
* the compiler checks that a final variable is assigned only once before it is accessed, and when in doubt, the compiler issues an error
* A blank final variable is a final variable whose declaration does not specify an initializer expression
* A final field must be explicitly initialized only once with an initializer expression, either in its declaration or in an initializer block. A final instance field can also be initialized in a
  constructor.
* After the class completes initialization, the final static fields of the class are all guaranteed to be initialized. After a constructor completes execution, the final instance fields of the current
  object are all guaranteed to be initialized. The compiler ensures that the class provides the appropriate code to initialize the final fields
* All static initializer blocks are executed at class initialization. Note that a blank final static field must be initialized in a static initializer block; otherwise, the compiler will issue an
  error
* Note that a blank final instance field must be initialized either in an instance initializer block or in a constructor; otherwise, the code will not compile
* Analogous to a non-final field, a final field with the same name can be hidden in a subclass—in contrast to final methods that can neither be overridden nor hidden
* Fields defined in an interface are implicitly final static fields as are the enum constants of an enum type
* A final local variable need not be initialized in its declaration—that is, it can be a blank final local variable—but it must be initialized in the code before it is accessed. However, the compiler
  does not complain as long as the local variable is not accessed—this is in contrast to final fields that must be explicitly assigned a value, whether they are accessed or not

## Interfaces

* An interface defines a contract for services that classes can implement. Objects of such classes guarantee that this contract will be honored
* Before diving into interfaces, an overview of the inheritance relationship between classes can be useful. The extends clause in a class definition only allows linear inheritance between classes—that
  is, a subclass can only extend one superclass. A superclass reference can refer to objects of its own type and of its subclasses strictly according to the linear inheritance hierarchy. Note that
  this inheritance relationship between classes comprises both inheritance of type (i.e., a subclass inherits the type of its superclass and can act as such) and inheritance of implementation (i.e.,
  a subclass inherits methods and fields from its superclass). Since this relationship is linear, it rules out multiple inheritance of implementation, in which a subclass can inherit implementation
  directly from more than one direct superclass
* As we shall see in this section, interfaces not only allow new named reference types to be introduced, but their usage can result in both multiple inheritance of type and multiple inheritance of
  implementation. As we shall also see, multiple inheritance of type does not pose any problems, but multiple inheritance of implementation does and is disallowed by the compiler
* Interfaces with empty bodies can be used as markers to tag classes as having a certain property or behavior. Such interfaces are also called ability interfaces

| Member declarations | Abstract instance method | Default instance method |    Static method    | Private instance method | Private Static method |
|:-------------------:|:------------------------:|:-----------------------:|:-------------------:|:-----------------------:|:---------------------:|
|   Access modifier   |   Implicitly `public`    |   Implicitly `public`   | Implicitly `public` |   `private` mandatory   |  `private` mandatory  |
| non-access modifier |  Implicitly `abstract`   |   `default` mandatory   | `static` mandatory  |          None           |  `static` mandatory   |
|     Implemented     |            No            |           Yes           |         Yes         |           Yes           |          Yes          |
|  can be inherited   |    If not overridden     |    If not overridden    |         No          |           No            |          No           |

| Member declarations |            Constant             | Member Type declarations |
|:-------------------:|:-------------------------------:|:------------------------:|
|   Access modifier   |       Implicitly `public`       |   Implicitly `public`    |
| non-access modifier | Implicitly `static` and `final` |   Implicitly `static`    |
|     Implemented     |               Yes               |           Yes            |
|  can be inherited   |          If not hidden          |      If not hidden       |

* An `interface` that has no direct superinterfaces implicitly includes a `public abstract` method declaration for each public instance method from the `java.lang.Object` class (e.g., `equals()`,
  `toString()`,
  `hashCode()`). These methods are not inherited from the java.lang.Object class, as only abstract method declarations are included in the interface. Their inclusion allows these methods to be called
  using an `interface` reference, and their implementation is always guaranteed at runtime as they are either inherited or overridden by all classes.
* abstract methods cannot be declared as static, because they comprise the contract fulfilled by the objects of the class implementing the interface. Abstract methods are always implemented as
  instance methods
* A **_sub-interface_** inherits from its superinterfaces all members of those **_super-interfaces_**, except for the following:
    * Any `abstract` or `default` methods that it overrides from its superinterfaces
    * Any `static` methods declared in its superinterfaces
    * Any `static` constants that it hides from its superinterfaces
    * Any `static` member types that it hides from its superinterfaces
* a default method in a top-level interface always has public access, whether the keyword public is specified or not.
* No other non-access modifiers, such as `abstract`, `final`, or `static`, are allowed in a `default` method declaration
* The default method can also be overridden by providing an abstract method declaration

```java
interface ISlogan {
    default void printSlogan() { // (1) Default method.
        System.out.println("Happiness is getting certified!");
    }
}

interface INewSlogan extends ISlogan {
    @Override
    abstract void printSlogan(); // (2) overrides (1) with abstract method.
}

abstract class JavaMaster implements ISlogan {
    @Override
    public abstract void printSlogan(); // (3) overrides (1) with abstract method.
}
```

* Augmenting an existing interface with a default method does not pose this problem. Classes that implement the old interface will work with the augmented interface without modifying or
  recompiling them. Thus interfaces can evolve without affecting classes that worked with the old interface. This is also true for static methods added to existing interfaces
* Private methods in an interface are governed by the following rules:
    * a private method must be declared with the private modifier in its header
    * A private method can only be accessed inside the interface itself
    * A private method cannot be declared with the abstract or the default modifier, but can be declared either as a private instance method or a private static method
    * A private static method can be invoked in any non-abstract method in the interface
    * A private instance method can only be invoked in default and private instance methods of the interface, and not in any static methods of the interface
* if the client is a class that implements this interface or is an interface that extends this interface, then the client can also access such constants directly by their simple names. Such a client
  inherits the interface constants

## Arrays and Subtyping

* All reference types are subtypes of the Object type. This applies to classes, interfaces, enums, and array types, as these are all reference types
* arrays of reference types are also subtypes of the array type `Object[]`, but arrays of primitive data types are not. Note that the array type Object[] is also a subtype of the Object type
* a non-generic reference type is a subtype of another non-generic reference type, the corresponding array types also have an analogous subtype–supertype relationship. This is called the subtype
  covariance relationship
* There is no subtype–supertype relationship between a type and its corresponding array type

## Reference Values and Conversions

* The rule of thumb for the primitive data types is that widening conversions are permitted, but narrowing conversions require an explicit cast.
* The rule of thumb for reference values is that widening conversions up the type hierarchy are permitted, but narrowing conversions down the hierarchy require an explicit cast
* In other words, conversions that are from a subtype to its supertypes are allowed, but other conversions require an explicit cast or are otherwise illegal.
* There is no notion of promotion for reference values.

## Reference Value Assignment Conversions

* the following conversions are permitted:
    * Widening primitive and reference conversions (long ← int, Object ← String)
    * Boxing conversion of primitive values, followed by optional widening reference conversion (Integer ← int, Number ← Integer ← int)
    * Unboxing conversion of a primitive value wrapper object, followed by optional widening primitive conversion (long ← int ← Integer)
    * Narrowing conversion for constant expressions of non-long integer types, with optional boxing (Byte ← byte ← int)
    * widening conversion cannot be followed by any boxing conversion, but the converse is permitted

## Method Invocation Conversions Involving References

* The algorithm used by the compiler for the resolution of overloaded methods incorporates the following phases:
    * The compiler performs overload resolution without permitting boxing, unboxing, or the use of a variable arity call
    * If phase (1) fails, the compiler performs overload resolution allowing boxing and unboxing, but excluding the use of a variable arity call
    * If phase (2) fails, the compiler performs overload resolution combining a variable arity call, boxing, and unboxing

## Reference Casting and the instanceof Operator

* The literal null can be cast to any reference type.
* The instanceof type comparison operator returns true if the left-hand operand can be a subtype of the right-hand operand
* It always returns false if the left-hand operand is null
* This is the reason why cast conversions are said to be unsafe, as they may throw a ClassCastException at runtime
* the instanceof type comparison operator effectively determines whether the reference value in the reference on the left-hand side refers to an object whose class is a subtype of the type specified
  on the right-hand side
* At runtime, it is the type of the actual object denoted by the reference on the left-hand side that is compared with the type specified on the right-hand side.
* In other words, what matters at runtime is the type of the actual object denoted by the reference, not the declared or static type of the reference
* The following facts should be noted:
    * The literal null is not an instance of any reference type
    * An instance of a superclass is not an instance of its subclass
    * An instance of a class is not an instance of a totally unrelated class
    * An instance of a class is not an instance of an interface type that the class does not implement
    * Any array of a non-primitive type is an instance of both Object and Object[] types
* The instanceof operator can also be used for pattern matching
* However, as opposed to the instanceof type comparison operator, the compiler flags an error if the destination type is not a subtype of the static type of the left-hand operand.

## Polymorphism

* A supertype reference can denote an object of its subtypes—conversely, a subtype object can act as an object of its supertypes. This is because there is an is-a relationship between a subtype and
  its supertypes
* When a non-private instance method is called, the method definition executed is determined by dynamic method lookup, based on the type of the object denoted by the reference at runtime
* Dynamic method lookup (also known as late binding, dynamic binding, and virtual method invocation) is the process of determining which method definition a method call signature denotes at runtime
* A private instance method does not exhibit polymorphic behavior. A call to such a method can occur only within the class and gets bound to the private method implementation at compile time
* Overloaded instance methods do not exhibit polymorphic behavior, as their calls are bound at compile time, unless an overloaded method is also overridden and invoked by a supertype reference
* Static methods also do not exhibit polymorphic behavior, as these methods do not belong to objects

## Enum Types

* Enum constants are actually final, static variables of the enum type, and they are implicitly initialized with instances of the enum type when the enum type is loaded at runtime
* we cannot create new constants (i.e., objects) of the enum type. An attempt to do so at results in a compile-time error
* When the enum type is loaded at runtime, the constructor is run for each enum constant, passing the argument values specified for the enum constant
* the only access modifier allowed for a constructor is private, as a constructor is understood to be implicitly declared private if no access modifier is specified
* Static initializer blocks can also be declared in an enum type, analogous to those in a class

```java
public enum Meal {
    BREAKFAST(7, 30), LUNCH(12, 15), DINNER(19, 45); // (1)
    // Fields for the meal time: (3)
    private int hh;
    private int mm;
    
    // Non-zero argument constructor (2)
    Meal(
            int hh,
            int mm
        ) {
        this.hh = hh;
        this.mm = mm;
    }
    
    // Instance methods: (4)
    public int getHour() { return this.hh; }
    
    public int getMins() { return this.mm; }
    
    public String getTimeString() { // "hh:mm"
        return String.format("%02d:%02d", this.hh, this.mm);
    }
}
```

* All enum types are subtypes of the java.lang.Enum class which implements the default behavior. All enum types are comparable and serializable

```java
//Static Methods
//Returns an array containing the enum constants of this enum type, in the order they are specified
static EnumTypeName[] values() //Method Signature

//Returns the enum constant with the specified name. An IllegalArgumentException is thrown if the specified name does not match the name of an enum constant
static EnumTypeName valueOf(String name) //Method Signature

//Instance Methods
// The natural order _of the enum constants in an enum type is according to their ordinal values (see the ordinal() method below)
// The compareTo() method in the Comparable interface
final int compareTo(E o) //Method Signature

//Returns true if the specified object is equal to this enum constant 
final boolean equals(Object other)

//Returns a hash code for this enum constant 
final int hashCode()

//Returns the name of this enum constant, exactly as it is declared in its enum declaration
final String name()

// Returns the ordinal value of this enum constant (i.e., its position in its enum type declaration). 
// The first enum constant is assigned an ordinal value of zero
// If the ordinal value of an enum constant is less than the ordinal value of another enum constant of the same enum type, 
// the former occurs before the latter in the enum type declaration
final int ordinal()
```

* the equality test implemented by the equals() method is based on reference equality (==) of the enum constants, not on object value equality
* Comparing two enum references for equality means determining whether they store the reference value of the same enum constant—that is, whether the references are aliases
* An enum type cannot be explicitly extended using the extends clause
* An enum type is implicitly final, unless it contains constant-specific class bodies.
* If it declares constant-specific class bodies, it is implicitly extended. No matter what, it cannot be explicitly declared final.
* An enum class is either implicitly final if its declaration contains no enum constants that have a class body, or implicitly sealed if its declaration contains at least one enum constant that has a
  class body
* Since an enum type can be either implicitly final or implicitly sealed, it can implement a sealed interface—in which case, it must be specified in the permits clause of the sealed interface

## Record Classes

* New instance methods can be declared, as can static members and initializers, but new instance fields and initializers are not allowed in the record body.
* Here are a few other restrictions to note regarding the normal canonical constructor:
    * must be at least as accessible as the record class. For example, for a top-level record class that has public accessibility, it must be declared public
    * It cannot be generic or have a throws clause in its header. In other words, any checked exception thrown by the constructor must be handled in the body of the constructor.
    * It cannot invoke another record constructor with the `this()` expression to chain constructors locally, as is the case with normal constructors

* You can also create overloaded constructors that take a completely different list of parameters.
* The first line of an overloaded constructor must be an explicit call to another constructor via `this()`.
* Also, unlike compact constructors, you can only transform the data on the first line. After the first line, all the fields will already be assigned, and the object is immutable.
* While you can add methods, static fields, and other data types, you cannot add instance fields outside the record declaration, even if they are private. Doing so the purpose of using a record and
  could break immutability!
* Records also do not support instance initializers. All initialization for the fields of a record must happen in a constructor.

```java
public record CD(
        String artist, String title, int noOfTracks, // (1)
        Year year, Genre genre
) {
    public CD(
            String artist,
            String title,
            int noOfTracks,
            Year year,
            Genre genre
             ) {
        this.artist = artist.strip();
        this.title = title.strip();
        this.noOfTracks = noOfTracks < 0 ? 0 : noOfTracks;
        this.year = year.compareTo(Year.of(2022)) > 0 ? Year.of(2022) : year;
        this.genre = genre == null ? Genre.OTHER : genre;
    }
}
```

```java
//Compact Record Constructor and Non-Canonical Constructor
public record CD(
                String artist, String title, int noOfTracks, // (1)
                Year year, Genre genre
        ) {
    public CD {
        artist = artist.strip();
        //this.artist = artist.strip(); compile time error
        title = title.strip();
        noOfTracks = noOfTracks < 0 ? 0 : noOfTracks;
        year = year.compareTo(Year.of(2022)) > 0 ? Year.of(2022) : year;
        genre = genre == null ? Genre.OTHER : genre;
    }
    
    // A non-canonical record constructor (5)
    public CD() {
        this(" Anonymous ", " No title ", 0, Year.of(2022), Genre.OTHER); // (6)
    }
}
```

## Sealed Classes and Interfaces

* A sealed class is a class that restricts which other classes may directly extend it.
* sealed class needs to be declared (and compiled) in the same package as its direct subclasses.
* The subclasses must each extend the sealed class.
* Every class that **_directly_** extends a `sealed` class must specify exactly one of the following three modifiers: `final`, `sealed`, or `non-sealed`.
* `permits` clause is optional and can be omitted if the classes both at the same file or a subclass is inner-class.

|       Location of direct subclasses       |      `permits` clause       |
|:-----------------------------------------:|:---------------------------:|
| In a different file from the sealed class |          Required           |
|   In the same file as the sealed class    | Permitted, but not required |
|     Nested inside of the sealed class     | Permitted, but not required |

```java
// Snake.java
public sealed class Snake { }

final class Cobra extends Snake { }

// OR
// Snake.java
public sealed class Snake {
    final class Cobra extends Snake { }
}
```

```java
public sealed class Bear permits Kodiak, Panda { }

public final class Kodiak extends Bear { }

public non-sealed class Panda extends Bear { }//Can be extended by by unspecified classes
```

```java
public sealed class Snake permits Cobra { //DOES NOT COMPILE
    final class Cobra extends Snake { }
}

public sealed class Snake permits Snake.Cobra {// COMPILEs
    
    final class Cobra extends Snake { }
}
```

* interfaces that extend a sealed interface can only be marked `sealed` or `non-sealed`. They cannot be marked final.
* One distinct feature of a sealed interface is that the permits list can apply to a class that implements the interface or an interface that extends the interface.
* Classes that implements `sealed` interface should either be `final`, `sealed` or `non-sealed`
* Sealed Class Rules:
    * Sealed classes are declared with the sealed and permits modifiers.
    * Sealed classes must be declared in the same package or named module as their direct subclasses.
    * Direct subclasses of sealed classes must be marked final, sealed, or non-sealed.
    * The permits clause is optional if the sealed class and its direct subclasses are declared within the same file or the subclasses are nested within the sealed class.
    * Interfaces can be sealed to limit the classes that implement them or the interfaces that extend them.
