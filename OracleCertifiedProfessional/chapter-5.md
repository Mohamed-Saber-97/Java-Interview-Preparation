<!-- TOC -->

* [Object-Oriented Programming](#object-oriented-programming)
    * [Implementing Inheritance](#implementing-inheritance)
        * [Relationships: is-a and has-a](#relationships-is-a-and-has-a)
        * [The Subtype–Supertype Relationship](#the-subtypesupertype-relationship)
        * [Overriding Instance Methods](#overriding-instance-methods)
        * [Covariant return in Overriding Methods](#covariant-return-in-overriding-methods)
        * [Overriding versus Overloading](#overriding-versus-overloading)
        * [Hiding Fields](#hiding-fields)
        * [Hiding Static Methods](#hiding-static-methods)
        * [Overriding, Hiding, and Overloading of Methods](#overriding-hiding-and-overloading-of-methods)
    * [Chaining Constructors Using `this()` and `super()`](#chaining-constructors-using-this-and-super)
        * [The `super()` Constructor Call](#the-super-constructor-call)
    * [Abstract Classes and Methods](#abstract-classes-and-methods)
        * [Abstract Classes](#abstract-classes)
            * [Declaring an `abstract` Class](#declaring-an-abstract-class)
            * [Extending an `abstract` Class](#extending-an-abstract-class)
        * [Abstract Methods in Classes](#abstract-methods-in-classes)
            * [Overriding an `abstract` Method](#overriding-an-abstract-method)
    * [Final Declarations](#final-declarations)
        * [Final Classes](#final-classes)
        * [Final Methods in Classes](#final-methods-in-classes)
        * [Final Variables](#final-variables)
        * [Final Fields in Classes](#final-fields-in-classes)
        * [Final Local Variables in Methods](#final-local-variables-in-methods)
    * [Interfaces](#interfaces)
        * [Defining Interfaces](#defining-interfaces)
            * [Summary of Member Declarations in an Interface](#summary-of-member-declarations-in-an-interface)
            * [Summary of Member Declarations in an Interface](#summary-of-member-declarations-in-an-interface-1)
        * [Implementing Interfaces](#implementing-interfaces)
        * [Extending Interfaces](#extending-interfaces)
        * [Default Methods in Interfaces](#default-methods-in-interfaces)
            * [Overriding Default Methods](#overriding-default-methods)
            * [Interface Evolution](#interface-evolution)
        * [Static Methods in Interfaces](#static-methods-in-interfaces)
        * [Private Methods in Interfaces](#private-methods-in-interfaces)
        * [Constants in Interfaces](#constants-in-interfaces)
    * [Arrays and Subtyping](#arrays-and-subtyping)
        * [Arrays and Subtype Covariance](#arrays-and-subtype-covariance)
    * [Reference Value Assignment Conversions](#reference-value-assignment-conversions)
    * [Method Invocation Conversions Involving References](#method-invocation-conversions-involving-references)
        * [Overloaded Method Resolution](#overloaded-method-resolution)
    * [Reference Casting and the `instanceof` Operator](#reference-casting-and-the-instanceof-operator)
        * [The `instanceof` Type Comparison Operator](#the-instanceof-type-comparison-operator)
    * [Polymorphism](#polymorphism)
    * [Enum Types](#enum-types)
        * [Declaring Enum Constructors and Members](#declaring-enum-constructors-and-members)
        * [Implicit Static Methods for Enum Types](#implicit-static-methods-for-enum-types)
        * [Extending Enum Types: Constant-Specific Class Bodies](#extending-enum-types-constant-specific-class-bodies)
    * [Record Classes](#record-classes)
        * [Restrictions on Record Declarations](#restrictions-on-record-declarations)
        * [Augmenting Basic Record Class Declaration](#augmenting-basic-record-class-declaration)
        * [Record Constructors](#record-constructors)
            * [Normal Canonical Record Constructor](#normal-canonical-record-constructor)
            * [Compact Canonical Record Constructor](#compact-canonical-record-constructor)
            * [Normal Non-Canonical Record Constructors](#normal-non-canonical-record-constructors)
            * [Generic Record Classes](#generic-record-classes)
    * [Sealed Classes and Interfaces](#sealed-classes-and-interfaces)
        * [Sealed Classes](#sealed-classes)
        * [Sealed Interfaces](#sealed-interfaces)
            * [Enum and Record Types as Permitted Direct Subtypes](#enum-and-record-types-as-permitted-direct-subtypes)
        * [Deriving Permitted Direct Subtypes of a Sealed Supertype](#deriving-permitted-direct-subtypes-of-a-sealed-supertype)

<!-- TOC -->
-----

# Object-Oriented Programming

## Implementing Inheritance

- Inheritance of members is closely tied to their declared _accessibility_
- If a superclass member is accessible by its simple name in the subclass (without the use of any extra syntax, like
  `super`), that member is considered inherited.
- Conversely, **_private_**, **_overridden_**, **_and hidden members of the superclass_** are _not_ inherited
- The private fields of the _superclass_ is not inherited, but exists in the subclass object and is indirectly accessible through `public` methods.
- Members that have package accessibility in the superclass are also not inherited by subclasses in other packages, as these members are accessible by their simple names only in subclasses within the same package as the superclass.
- Since constructors are _not_ members of a class, they are _not_ inherited by a subclass.

### Relationships: is-a and has-a

- Since a subclass inherits from its superclass, a subclass object **_is-a_** superclass object, and can be used wherever an object of the superclass can be used.
- In Java, a composite object cannot contain other objects. It can only store **_reference values_** of its constituent objects in its fields
- Constituent objects can be shared between objects. If their lifetimes are dependent on the lifetime of the aggregate object, then this relationship is called **_composition_**, and implies strong ownership of the parts by the composite object
- However, changing the contract of a superclass can have consequences for the subclasses (called the **_ripple effect_**) as well as for clients that are dependent on a particular behavior of the subclasses. **_For this reason, aggregation provides stronger encapsulation than inheritance_**.

### The Subtype–Supertype Relationship

- The subclass–superclass relationship allows _single inheritance of type_, meaning that the subclass inherits the
  _type_ of its direct superclass.
- This is in contrast to a class that implements several interfaces, resulting in _multiple inheritance of type_—that is, a class inherits the _type_ of all interfaces it implements
- Note that the compiler only knows about the `static` _type_ (also called the _declared type_) of the reference and ensures that only methods from this type can be called using the reference.
- However, at runtime, the reference will refer to an object of the subtype when the call to the method is executed
- It is the _type of the object_ that the reference refers to at runtime that determines which method is executed
- The type of the object that the reference refers to at runtime is called the _dynamic type_ (a.k.a. the _runtime type_
  or the _object type_) of the reference.

### Overriding Instance Methods

- The overridden method in the superclass is **_not_** inherited by the subclass. When the method is invoked on an object of the subclass, it is the method implementation in the subclass that is executed. The new method in the subclass must abide by the following rules of method overriding:
    - The new method definition in the subclass must have the same method signature. A method’s signature does not comprise the final modifier of parameters, only their types and order
    - The return type of the overriding method can be a subtype of the return _type_ of the overridden method called **_covariant return_**
    - The new method definition cannot _narrow_ the accessibility of the method, but it can _widen_ it
    - The new method definition can throw either all or none, or a subset of the checked exceptions (including their subclasses) that are specified in the throws clause of the overridden method in the superclass —that is, the overriding method may not throw any checked exception that is wider than the checked exceptions thrown by the overridden method. However, the overriding method may throw any unchecked exceptions.
- The criteria for overriding methods also apply to interfaces, where a sub-interface can override `abstract` and
  `default` method declarations from its super-interfaces
- Here are a few more facts to note about overriding:
    - A subclass must use the keyword `super` to invoke an overridden method in the superclass
    - A `final` method cannot be overridden because the modifier `final` prevents method overriding. An attempt to override a `final` method will result in a compile-time error.
    - An `abstract` method, in contrast, requires the non-abstract subclasses to override the method to provide an implementation
    - `private` method means that the method is not accessible outside the class in which it is defined; therefore, a subclass cannot override it.
    - However, a subclass can give its own definition of such a method, which may have the same signature as the method in its superclass
    - A subclass within the same package as the superclass can override any non-`final` methods declared in the superclass.
    - However, a subclass in a different package can override only the non-`final` methods that are declared as either
      `public` or `protected` in the superclass
    - An instance method in a subclass cannot override a `static` method in the superclass. The compiler will flag such an attempt as an error.
    - A `static` method is class specific and not part of any object, while overriding methods are invoked on behalf of objects of the subclass.
    - However, a `static` method in a subclass can hide a `static` method in the superclass, but it cannot hide an instance method in the superclass
    - Constructors, since they are not inherited, cannot be overridden

### Covariant return in Overriding Methods

- covariant return applies only to reference types, not to primitive types

### Overriding versus Overloading

- Method overriding always requires the same method signature (name and parameter types) and the same or covariant return types
- Overloading occurs when the method names are the same, but the parameter lists differ.
- Only non-final instance methods in the superclass that are directly accessible from the subclass using their simple name can be overridden
- In contrast, both non-private instance and static methods can be overloaded in the class they are defined in or are in a subclass of the class they are defined in.
- For overloaded and static methods, which method implementation will be executed at runtime is determined at *
  *_compile time_**
- In contrast, for overridden methods, the method implementation to be executed is determined at **_runtime_**

### Hiding Fields

- A subclass cannot override inherited _fields_ of the superclass, but it can _hide_ them
- If this is the case, the fields in the superclass cannot be accessed in the subclass by their simple names; therefore, they are not inherited by the subclass
- the keyword super can be used in non-`static` code in the subclass declaration to access hidden `static` fields
- When a field of an object is accessed using a reference, it is the declared type of the reference, not the type of the current object denoted by the reference, that determines which field will actually be accessed.
- The field is `static` in the subclass, but not in the superclass. The declared type of the fields need not be the same either—only the field name matters in the hiding of fields

### Hiding Static Methods

- Only instance methods in an object can be overridden
- However, a `static` method in a subclass can hide a `static` method from the superclass
- Hiding a static method is analogous to overriding an instance method except for one important aspect:  Calls to static methods are bound at compile time as opposed to runtime for calls to overridden instance methods, and therefore do not exhibit polymorphic behavior exhibited by calls to overridden instance methods.
- When hiding a static method, the compiler will flag an error if the signatures are the same, but the other requirements regarding the return type, throws clause, and accessibility are not met. If the signatures are different, the method name is overloaded, not hidden
- A static method in the subclass can only hide a static method in the superclass.
- Analogous to an overridden instance method, a hidden superclass static method is not inherited
- Analogous to accessing hidden static fields, a hidden static method in the superclass can always be invoked by using the superclass name or by using the keyword super in non-static code in the subclass declaration
- In summary, a subclass can do any of the following:
    - Access inherited members using their simple names
    - Override a non-final instance method with the same signature as in the superclass
    - Overload a static or instance method with the same name as in the superclass
    - Hide a static non-final method with the same signature as in the superclass
    - Hide a static or instance field with the same name as in the superclass
    - Declare new members that are not in the superclass
    - Declare constructors that can invoke constructors in the superclass implicitly, or use the `super()` construct with the appropriate arguments to invoke them explicitly

### Overriding, Hiding, and Overloading of Methods

|             Comparison criteria             |                                                                         Overriding of instance methods/Hiding of static methods                                                                         |                    Overloading of instance and static methods                     |
|:-------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------:|
|                 Method name                 |                                                                                            Must be the same                                                                                             |                                 Must be the same                                  |
|                Argument list                |                                                                                            Must be the same                                                                                             |                               Must be the different                               |
|                 Return type                 |                                                                                Can be the same type or a covariant type                                                                                 |                                 Can be different                                  |
|               `throws` clause               |                               Can be restrictive about checked exceptions thrown. Must not throw new checked exceptions, but can include subclasses of exceptions thrown                                |                                 Can be different                                  |
|                Accessibility                |                                                                         Can make it less restrictive, but not more restrictive                                                                          |                                 Can be different                                  |
|              `final` modifier               |                                A final instance/static method cannot be overridden/hidden. Compile-time error if final instance/`static` method is defined in a subclass                                |               Can be overloaded in the same class or in a subclass                |
|             `private` modifier              |                             A private instance/static method cannot be overridden/hidden. No compile-time error if private instance/static method is defined in a subclass                              |                     Can be overloaded only in the same class                      |
|             Declaration context             |                                                                 An instance/static method can only be overridden/ hidden in a subclass                                                                  | An instance or static method can be overloaded in the same class or in a subclass |
| Binding of method call/polymorphic behavior | Calls to overridden instance methods are bound at runtime, and therefore exhibit polymorphic behavior. Calls to hidden static methods are bound at compile time and do not exhibit polymorphic behavior |      Calls are bound at compile time and do not exhibit polymorphic behavior      |

## Chaining Constructors Using `this()` and `super()`

### The `super()` Constructor Call

- The `super()` construct has the same restrictions as the `this()` construct: If used, the `super()` call must occur as the first statement in a constructor, and it can only be used in a constructor declaration
- This implies that `this()` and `super()` calls cannot both occur in the same constructor.

## Abstract Classes and Methods

### Abstract Classes

#### Declaring an `abstract` Class

- If a class has one or more `abstract` methods, it must be declared as `abstract`, as it is _incomplete_
- Like a normal class, an `abstract` class can declare class members, constructors, and initializers
- A class that is declared `absract` cannot be instantiated, regardless of whether it has abstract methods or not

#### Extending an `abstract` Class

- subclasses of the abstract class have to take a stand and provide implementations of any inherited abstract methods before objects can be created.
- Analogous to a normal class, an abstract class can only extend a single non-final class that can be either concrete or abstract
- A final class cannot be extended, but an abstract class is incomplete or considered to be incomplete and must be extended
- An abstract class should not be used to implement a class that cannot be instantiated. The recommended practice is to only provide a zero-argument constructor that is private, thus making sure that it is never invoked in the class
- **_In many ways abstract classes and interfaces are similar, and interfaces can be used with advantage in many cases. However, if private state should be maintained with instance members, then abstract classes are preferred, as interfaces do not have any notion of state_**

### Abstract Methods in Classes

#### Overriding an `abstract` Method

- When overriding an `abstract` method from the superclass, the notation `@Override` should always be used in the overriding method in the subclass. The compiler will issue an error if the override criteria are not satisfied
- an abstract method in a top-level class can only have `public`, `protected`, or package accessibility
- Since `static` methods cannot be overridden, declaring an `abstract` `static` method makes no sense, and the compiler will report an error

## Final Declarations

### Final Classes

- A class can be declared `final` to indicate that it cannot be extended. In other words, the class behavior cannot be changed by extending the class
- This implies that one cannot override or hide any methods declared in such a class

### Final Methods in Classes

- A subclass cannot override or hide a private method from its superclass. However, it can define a new method using the same method signature as the private method, without regard to the other criteria for overriding
- Defining a new method using the same method signature is impossible with a final method which cannot be overridden or hidden
- Although a constructor cannot be overridden, it also cannot be declared `final`
- The `java.lang.Object` class—the superclass of all classes—defines a number of final methods (`notify()`,
  `notifyAll()`, and `wait()`—mostly to synchronize the running of threads) that all objects inherit, but cannot override.

### Final Variables

- A blank final variable is a final variable whose declaration does not specify an initializer expression
- A constant variable is a final variable of either a primitive type or type String that is initialized with a constant expression. Constant variables can be used as case labels of a switch statement

### Final Fields in Classes

- A final field must be explicitly initialized only once with an initializer expression, either in its declaration or in an initializer block
- A final instance field can also be initialized in a constructor.
- After the class completes initialization, the final static fields of the class are all guaranteed to be initialized
- After a constructor completes execution, the final instance fields of the current object are all guaranteed to be initialized.
- Analogous to a non-final field, a final field with the same name can be hidden in a subclass—in contrast to final methods that can neither be overridden nor hidden
- Fields defined in an interface are implicitly final `static` fields, as are the `enum` constants of an `enum` type.

### Final Local Variables in Methods

- A final local variable need not be initialized in its declaration, but it must be initialized in the code before it is accessed
- Local variables in certain contexts are considered to be implicitly declared final: an exception parameter of a multi-catch clause and a local variable that refers to a resource in a try-with-resources statement

## Interfaces

- Note that this inheritance relationship between classes comprises both inheritance of type (i.e., a subclass inherits the type of its superclass and can act as such) and inheritance of implementation (i.e., a subclass inherits methods and fields from its superclass).
- _interfaces_ usage can result in both **_multiple inheritance of type_** and *
  *_multiple inheritance of implementation_**
- As we shall also see, multiple inheritance of type does not pose any problems, but multiple inheritance of implementation does and is disallowed by the compiler.

### Defining Interfaces

- An `interface` is abstract by definition, which means that it cannot be instantiated
- interfaces are meant to be implemented by classes, interface members implicitly have `public` access and the `public`
  modifier can be omitted
- Interfaces with empty bodies can be used as markers to tag classes as having a certain property or behavior

#### Summary of Member Declarations in an Interface

| Member declarations | Abstract instance method | Default instance method |    Static method    | Private instance method | Private static method |
|:-------------------:|:------------------------:|:-----------------------:|:-------------------:|:-----------------------:|:---------------------:|
|   Access modifier   |   Implicitly `public`    |   Implicitly `public`   | Implicitly `public` |   `private` mandatory   |  `private` mandatory  |
| Non-access modifier |  Implicitly `abstract`   |   `default` mandatory   | `static` mandatory  |          None           |  `static` mandatory   |
|     Implemented     |            No            |           Yes           |         Yes         |           Yes           |          Yes          |
|  Can be inherited   |    If not overridden     |    If not overridden    |         No          |           No            |          No           |

#### Summary of Member Declarations in an Interface

| Member declarations |            Constant             | Member type declaration |
|:-------------------:|:-------------------------------:|:-----------------------:|
|   Access modifier   |       Implicitly `public`       |   Implicitly `public`   |
| Non-access modifier | Implicitly `static` and `final` |   Implicitly `static`   |
|     Implemented     |               Yes               |           Yes           |
|  Can be inherited   |          If not hidden          |      If not hidden      |

### Implementing Interfaces

- Note that abstract methods cannot be declared as static, because they comprise the contract fulfilled by the objects of the class implementing the interface.
- Abstract methods are always implemented as instance methods
- Even so, regardless of how many interfaces a class implements directly or indirectly, it provides just a single implementation of any abstract method declared in multiple interfaces
- If the methods in the two interfaces had the same signatures but different return types, the Worker class would not be able to implement both interfaces

### Extending Interfaces

- A sub-interface inherits from its superinterfaces all members of those super-interfaces, except for the following:
    - Any abstract or default methods that it overrides from its superinterfaces
    - Any static methods declared in its superinterfaces
    - Any static constants that it hides from its superinterfaces
    - Any static member types that it hides from its superinterfaces
- Thinking in terms of types, every reference type in Java is a subtype of the `java.lang.Object` class. In turn, any interface type is also a subtype of the `Object` class, but it does not inherit any implementation from the `Object`
  class
- an interface that has no direct superinterfaces implicitly declares a `public` abstract method for each `public`
  instance method in the `Object` class

### Default Methods in Interfaces

- No other non-access modifiers, such as `abstract`, `final`, or `static`, are allowed in a `default` method declaration

#### Overriding Default Methods

- The `default` method can also be overridden by providing an `abstract` method declaration

#### Interface Evolution

- Augmenting an existing interface with an abstract method will break all classes that implement this interface, as they will no longer implement the old interface. These classes will need to be modified and recompiled in order to work with the augmented interface
- Augmenting an existing interface with a default method does not pose this problem. Classes that implement the old interface will work with the augmented interface without modifying or recompiling them. Thus interfaces can evolve without affecting classes that worked with the old interface. This is also true for static methods added to existing interfaces

### Static Methods in Interfaces

- a static method in a top-level interface always has public access, whether the keyword public is specified or not
- Static methods in an interface cannot be inherited, unlike static methods in classes

### Private Methods in Interfaces

- Private methods in an interface are governed by the following rules:
    - Not surprisingly, a private method must be declared with the private modifier in its header
    - A private method can only be accessed inside the interface itself
    - A private method cannot be declared with the abstract or the default modifier, but can be declared either as a private instance method or a private static method
    - A private static method can be invoked in any non-abstract method in the interface
    - A private instance method can only be invoked in default and private instance methods of the interface, and not in any static methods of the interface

### Constants in Interfaces

- constants are considered to be public, static, and final
- if the client is a class that implements this interface or is an interface that extends this interface, then the client can also access such constants directly by their simple names. Such a client inherits the interface constants

## Arrays and Subtyping

### Arrays and Subtype Covariance

- the following facts are apparent:
    - All reference types are subtypes of the `Object` type. This applies to classes, interfaces, enums, and array types, as these are all reference types
    - All arrays of reference types are also subtypes of the array type `Object[]`, but arrays of primitive data types are not. Note that the array type `Object[]` is also a subtype of the `Object` type
    - There is no subtype–supertype relationship between a type and its corresponding array type
    - array of subtype objects cannot possibly contain objects of its supertype

## Reference Value Assignment Conversions

- the following conversions are permitted:
    - Widening primitive and reference conversions (`long` ← `int`, `Object` ← `String`)
    - Boxing conversion of primitive values, followed by optional widening reference conversion (`Integer` ← `int`, `Number` ← `Integer` ← `int`)
    - Unboxing conversion of a primitive value wrapper object, followed by optional widening primitive conversion (`long` ← `int` ← `Integer`)
    - Narrowing conversion for constant expressions of non-long integer types, with optional boxing (`Byte` ← `byte` ← `int`)
- Note that these rules imply that a widening conversion cannot be followed by any boxing conversion, but the converse is permitted

## Method Invocation Conversions Involving References

- The conversions for reference value assignment are also applicable to _method invocation conversions_, except for the narrowing conversion for constant expressions of non-`long` integer type

### Overloaded Method Resolution

- The algorithm used by the compiler for the resolution of overloaded methods incorporates the following phases:
    - The compiler performs overload resolution without permitting boxing, unboxing, or the use of a variable arity call
    - If phase (1) fails, the compiler performs overload resolution allowing boxing and unboxing, but excluding the use of a variable arity call
    - If phase (2) fails, the compiler performs overload resolution combining a variable arity call, boxing, and unboxing

## Reference Casting and the `instanceof` Operator

### The `instanceof` Type Comparison Operator

- The literal null can be cast to any reference type
- Both the type cast expression and the `instanceof` type comparison operator require a compile-time check and a runtime check
- The compile-time check determines whether there is a subtype–supertype relationship between the source and destination types.
- At runtime, the reference expression evaluates to a reference value of an object
- Note that if the result of the `instanceof` type comparison operator is `false`, the cast involving the operands will throw a `ClassCastException`.
- the `instanceof` type comparison operator effectively determines whether the reference value in the reference on the left-hand side refers to an object whose class is a subtype of the type specified on the right-hand side
- At runtime, it is the type of the actual object denoted by the reference on the left-hand side that is compared with the type specified on the right-hand side
- In other words, what matters at runtime is the type of the actual object denoted by the reference, not the declared or static type of the reference.
- The following facts should be noted:
    - The literal null is not an instance of any reference type and `instanceof` always returns false
    - An instance of a superclass is not an instance of its subclass
    - An instance of a class is not an instance of a totally unrelated class
    - An instance of a class is not an instance of an interface type that the class does not implement
    - Any array of a non-primitive type is an instance of both `Object` and `Object[]` types

## Polymorphism

- A supertype reference can denote an object of its subtypes—conversely, a subtype object can act as an object of its supertypes. This is because there is an is-a relationship between a subtype and its supertypes
- When a non-private instance method is called, the method definition executed is determined by _dynamic method lookup_, based on the _type of the object_ denoted by the reference at runtime
- It is performed starting in the class of the object on which the method is invoked, and moves up the inheritance hierarchy to find the supertype with the appropriate method definition
- Overloaded instance methods do not exhibit polymorphic behavior, as their calls are bound at compile time, unless an overloaded method is also overridden and invoked by a supertype reference
- Static methods also do not exhibit polymorphic behavior, as these methods do not belong to objects

## Enum Types

### Declaring Enum Constructors and Members

```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

```java
public enum Meal {
    // Each enum constant defines a constant-specific class body
    BREAKFAST(7, 30) { // (1)

        @Override
        public double mealPrice(Day day) { // (2)
            double breakfastPrice = 10.50;
            if (day.equals(Day.SATURDAY) || day == Day.SUNDAY) breakfastPrice *= 1.5;
            return breakfastPrice;
        }

        @Override
        public String toString() { // (3)
            return "Breakfast";
        }
    }, // (4)
    LUNCH(12, 15) {
        @Override
        public double mealPrice(Day day) { // (5)
            double lunchPrice = 20.50;
            switch (day) {
                case SATURDAY:
                case SUNDAY:
                    lunchPrice *= 2.0;
            }
            return lunchPrice;
        }

        @Override
        public String toString() {
            return "Lunch";
        }
    }, DINNER(19, 45) {
        @Override
        public double mealPrice(Day day) { // (6)
            double dinnerPrice = 25.50;
            if (day.compareTo(Day.SATURDAY) >= 0 && day.compareTo(Day.SUNDAY) <= 0) dinnerPrice *= 2.5;
            return dinnerPrice;
        }
    };

    // Instance fields: Time for the meal.
    private int hh;
    private int mm;

    // Enum constructor:
    Meal(int hh, int mm) {
        this.hh = hh;
        this.mm = mm;
    }

    // Abstract method implemented in constant-specific class bodies.
    abstract double mealPrice(Day day); // (7)

    // Instance methods:
    public int getHour() {return this.hh;}

    public int getMins() {return this.mm;}

    public String getTimeString() { // "hh:mm"
        return String.format("%02d:%02d", this.hh, this.mm);
    }
}
```

### Implicit Static Methods for Enum Types

```java
//Returns an array containing the enum constants of this enum type, in the order they are specified
static EnumTypeName[] values();//Method Signature

//Returns the enum constant with the specified name. 
//An IllegalArgumentException is thrown if the specified name does not match the name of an enum constant
static EnumTypeName valueOf(String name);//Method Signature

//The natural order of the enum constants in an enum type is according to their ordinal values
final int compareTo(E o);//Method Signature

//Returns the ordinal value of this enum constant 
final int ordinal();//Method Signature
```

### Extending Enum Types: Constant-Specific Class Bodies

- Constant-specific class bodies define anonymous classes
- Constructors, abstract methods, and static methods cannot be declared in a constant specific class body

## Record Classes

#### Restrictions on Record Declarations

- A record class cannot have an explicit extends clause to declare a direct superclass, not even its direct superclass Record
- As a record class is implicitly final and being final also means that a record class cannot be declared abstract
- **_the name of a record component in the component list cannot be the same as the name of the methods in the `Object` class, except `equals`_**

### Augmenting Basic Record Class Declaration

- Constructors can be customized, as can the get methods corresponding to the component fields
- New instance methods can be declared, as can static members and initializers, but new instance fields and initializers are not allowed in the record body.

### Record Constructors

- A normal canonical record constructor, if declared, supersedes the implicit canonical record constructor
- If a normal canonical record constructor is not specified, a compact canonical record constructor can be declared. In this case, the implicit canonical record constructor is generated. Note that either the normal canonical record constructor or the compact canonical record constructor can be specified, but not both
- Any number of non-canonical record constructors can be specified, whether any canonical constructor is provided

#### Normal Canonical Record Constructor

- few other restrictions:
    - It must be at least as accessible as the record class. For example, for a top-level record class that has public accessibility, it must be declared public
    - It cannot be generic or have a throws clause in its header. In other words, any checked exception thrown by the constructor must be handled in the body of the constructor
    - It cannot invoke another record constructor with the `this()` expression to chain constructors locally, as is the case with normal constructors
- record is always deserialized using the canonical constructor

```java
public record CD(String artist, String title, int noOfTracks, Year year, Genre genre) {
    // Normal canonical record constructor
    public CD(String artist, String title, int noOfTracks, Year year, Genre genre) {
        artist = artist.strip();
        title = title.strip();
        noOfTracks = noOfTracks < 0 ? 0 : noOfTracks;
        year = year.compareTo(Year.of(2022)) > 0 ? Year.of(2022) : year;
        genre = genre == null ? Genre.OTHER : genre;
        this.artist = artist;
        this.title = title;
        this.noOfTracks = noOfTracks;
        this.year = year;
        this.genre = genre;
    }
}
```

#### Compact Canonical Record Constructor

- It eliminates the need for a parameter list and the initialization of the component fields
- The parameter list is derived from the component list declared in the record header, and the initialization of all component fields is left to the implicit canonical record constructor
- The implicit canonical record constructor is called before exiting from the compact canonical constructor; therefore, a return statement is not allowed in the compact constructor.
- The compact constructor primarily functions as a validator for the data values before they are assigned to the component fields by the implicit canonical record constructor

```java
public record CD(String artist, String title, int noOfTracks, Year year, Genre genre) {
    // Static fields with some ready-made CDs: (10)
    public final static CD cd0 = new CD(" Jaav", "Java Jive", 8, Year.of(2017), Genre.POP);
    public final static CD cd2 = new CD("Funkies ", " Lambda Dancing ", 10, Year.of(2024), null);
    public final static CD cd4 = new CD("Genericos", "Hot Generics", -5, Year.of(2018), Genre.JAZZ);

    // Compact canonical record constructor
    public CD {
        artist = artist.strip();
        title = title.strip();
        year = year.compareTo(Year.of(2022)) > 0 ? Year.of(2022) : year;
        genre = genre == null ? Genre.OTHER : genre;
        // this.artist = artist; // Compile-time error!
    }

    // New instance methods:
    public boolean isPop() {return this.genre == Genre.POP;}

    public boolean isJazz() {return this.genre == Genre.JAZZ;}

    public boolean isOther() {return this.genre == Genre.OTHER;}

    // Customize a get method:
    @Override
    public String title() {
        return this.title.toUpperCase();
    }

    // Declare a nested type:
    public enum Genre {POP, JAZZ, OTHER}
}
```

#### Normal Non-Canonical Record Constructors

- constructors whose signature is not the same as that of the canonical constructor

```java
public record CD(String artist, String title, int noOfTracks, Year year, Genre genre) {
    // A non-canonical record constructor
    public CD() {
        this(" Anonymous ", " No title ", 0, Year.of(2022), Genre.OTHER); // (6)
    }
}
```

#### Generic Record Classes

- `record Container<T>(T item) { /* Empty body */}`
    - `Container<String> p0 = new Container<>("Hi");`
    - `Container<Integer> p1 = new Container<>(Integer.valueOf(10));`

## Sealed Classes and Interfaces

### Sealed Classes

- A sealed class is declared using the modifier sealed and the optional permits clause in the class header
- The permits clause is optional if a sealed class and its permitted direct subclasses are declared in the same compilation unit
- If a class specifies the permits clause, the class must be declared sealed.
- The permits clause, if specified, must appear after any extends and implements clauses in the class header
- `public abstract sealed class Book permits PrintedBook, Ebook, Audiobook {/*(1)*/}`
    - Each permitted direct subclass must extend its direct sealed superclass
    - Each permitted direct subclass must be declared either sealed, non-sealed, or final. This avoids any unwanted subclassing to the inheritance hierarchy of a sealed class
    - Note that a class cannot be declared non-sealed unless it is a permitted direct subclass of a direct sealed superclass
    - Each permitted direct subclass must be accessible by the direct sealed superclass. If the sealed superclass is in a named module, the permitted subclasses must also be in the same module. Note that this does not mean that they have to be in the same package in the named module

### Sealed Interfaces

- Analogous to sealed classes, sealed interfaces can also be defined. However, a sealed interface can have both sub-interfaces and subclasses as its permitted direct subtypes
- Any permitted subclass of a sealed interface must be declared either sealed, non-sealed or final, but any permitted sub-interface can only be declared either sealed or non-sealed. The modifier final is not allowed for interfaces

#### Enum and Record Types as Permitted Direct Subtypes

- an enum type is either implicitly final (has no enum constants that have a class body) or implicitly sealed (has at least one enum constant with a class body that constitutes an implicitly declared direct subclass

```java
enum CD implements MediaStorage {CD_ROM, CD_R, CD_W} // (2) Implicitly final

enum DVD implements MediaStorage {DVD_R {}, DVD_RW} // (3) Implicitly sealed

sealed interface MediaStorage permits CD, DVD {} // (1) Sealed interface

sealed interface MediaStorage permits CD, DVD, HardDisk {}// (1a) Sealed interface

record HardDisk(double capacity) implements MediaStorage {}// (4) Implicitly final
```

### Deriving Permitted Direct Subtypes of a Sealed Supertype

- The permits clause of a sealed class or interface can be omitted, if all its permitted direct subtypes are declared in the same compilation unit—that is, are in the same source file
- Since only one reference type can be declared public in a compilation unit, the accessibility of the other type declarations cannot be public

```java
sealed interface Subscribable {} // Permitted subtypes are derived.

non-sealed interface VIPSubcribable extends Subscribable {}

// File: Book.java (compilation unit)
public abstract sealed class Book {} // Permitted subclasses are derived.

non-sealed class PrintedBook extends Book {}

final class Ebook extends Book implements Subscribable {}

final class Audiobook extends Book implements Subscribable {}
```