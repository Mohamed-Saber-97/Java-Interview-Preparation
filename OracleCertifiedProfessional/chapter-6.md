<!-- TOC -->

* [Access Control](#access-control)
    * [Design Principle: Encapsulation](#design-principle-encapsulation)
    * [Java Source File Structure](#java-source-file-structure)
    * [Packages](#packages)
        * [Importing Reference Types](#importing-reference-types)
    * [Searching for Classes on the Class Path](#searching-for-classes-on-the-class-path)
    * [Access Modifiers](#access-modifiers)
        * [Access Modifiers for Top-Level Type Declarations](#access-modifiers-for-top-level-type-declarations)
        * [Access Modifiers for Class Members](#access-modifiers-for-class-members)
            * [`public` Members](#public-members)
            * [`protected` Members](#protected-members)
            * [Additional Remarks on Accessibility](#additional-remarks-on-accessibility)
    * [Scope Rules](#scope-rules)
        * [Class Scope for Members](#class-scope-for-members)
    * [Implementing Immutability](#implementing-immutability)

<!-- TOC -->
-----

# Access Control

## Design Principle: Encapsulation

- Encapsulation is achieved through **_information hiding_**
    - Method or block declaration
    - Reference type declaration
    - Package declaration
    - Module declaration
-

## Java Source File Structure

- A Java source file can have the following elements that, if present, must be specified in the following order:
    - An optional package declaration to specify a package name
    - Zero or more import declarations. Since import declarations introduce type or static member names in the source code, they must be placed before any type declarations
    - Any number of top-level type declarations. Class, enum, and interface declarations are collectively known as type declarations. Since these declarations belong to the same package, they are said to be defined at the top level, which is the package level

## Packages

#### Importing Reference Types

- An import declaration does not recursively import subpackages, as such nested packages are autonomous packages
- The declaration also does not result in inclusion of the source code of the types; rather, it simply imports type names (i.e., it makes type names available to the code in a compilation unit
- Import statements are not present in the compiled code, as all type names in the source code are replaced with their fully qualified names by the compiler

## Searching for Classes on the Class Path

|         Operation          |                         Command                         |
|:--------------------------:|:-------------------------------------------------------:|
| Compiling non-modular code | `javac --class-path classpath -d directory sourceFiles` |
| Compiling non-modular code |  `javac -classpath classpath -d directory sourceFiles`  |
| Compiling non-modular code |     `javac -cp classpath -d directory sourceFiles`      |
| Executing non-modular code |    `java --class-path classpath qualifiedClassName`     |
| Executing non-modular code |     `java -classpath classpath qualifiedClassName`      |
| Executing non-modular code |         `java -cp classpath qualifiedClassName`         |

## Access Modifiers

### Access Modifiers for Top-Level Type Declarations

- only `public` and _package-private_ are allowed

### Access Modifiers for Class Members

#### `public` Members

- Public access is the least restrictive of all the access modifiers
- A public member is accessible from anywhere, both in the package containing its class and by other packages where its class is accessible
- Subclasses can access their inherited public members by their simple names, and all clients can access public members in an instance of the class Superclass1

#### `protected` Members

- A protected member is accessible in all classes in the same package, and by all subclasses of its class in any package where its class is accessible
- a subclass in another package can only access protected instance members in the superclass via references of its own type—that is, protected instance members that are inherited by objects of the subclass.

| Member Access | In the defining class | In a subclass in the same package | In a Non-subclass in the same package | In a subclass in a different package | In a Non-subclass in a different package |
|:-------------:|:---------------------:|:---------------------------------:|:-------------------------------------:|:------------------------------------:|:----------------------------------------:|
|   `public`    |          Yes          |                Yes                |                  Yes                  |                 Yes                  |                   Yes                    |
|  `protected`  |          Yes          |                Yes                |                  Yes                  |                 Yes                  |                    No                    |
|   `package`   |          Yes          |                Yes                |                  Yes                  |                  No                  |                    No                    |
|   `private`   |          Yes          |                No                 |                  No                   |                  No                  |                    No                    |

#### Additional Remarks on Accessibility

- Access modifiers that can be specified for a class member apply equally to constructors. However, when no constructor is specified, the default constructor inserted by the compiler implicitly follows the accessibility of the class
- Accessibility of members declared in an enum type is analogous to those declared in a class, except for enum constants that are always public and constructors that are always private -A protected member in an enum type is only accessible in its own package, since an enum type is implicitly final
- In contrast, the accessibility of members declared in an interface is always implicitly public. Omission of the public access modifier in this context does not imply package accessibility

## Scope Rules

### Class Scope for Members

- The golden rule is that static code can only access other static members by their simple names
- Static code is not executed in the context of an object, so the references this and super are not available
- An object has knowledge of its class, so static members are always accessible in a non-static context

## Implementing Immutability

- Shared resources are typically implemented using `synchronized` code in order to guarantee thread safety of the shared resource. However, if the shared resource is an immutable object, thread safety comes for free.
- Any method that seemingly modifies the state of an immutable object is in fact returning a new immutable object with the state modified appropriately based on the original object. Primitive values are of course always immutable
- There are certain guidelines that can help to avoid common pitfalls when implementing immutable classes:
- It should not be possible to extend the class
    - A straightforward approach is to declare the class as final
    - declare the constructor as private and provide static factory methods to construct instances
- All fields should be declared final and private

- Check the consistency of the object state at the time the object is created
- No set methods (a.k.a. setter or mutator methods) should be provided
- A client should not be able to access mutable objects referred to by any fields in the class

```java
public class WeeklyStatsV2 { // (1) Class is not final.

    private WeeklyStatsV2(String description, int weekNumber, int[] stats) { // (5a) Private constructor
        this.description = description;
        this.weekNumber = weekNumber;
        this.stats = Arrays.copyOf(stats, stats.length); // Create a private copy.
    }

    // (5b) Static factory method to construct objects.
    public static WeeklyStatsV2 getNewWeeklyStats(String description, int weekNumber, int[] stats) {
        if (weekNumber <= 0 || weekNumber > 52) {
            throw new IllegalArgumentException("Invalid week number: " + weekNumber);
        }
        if (stats.length != 7) {
            throw new IllegalArgumentException("Stats not for whole week: " + Arrays.toString(stats));
        }
        return new WeeklyStatsV2(description, weekNumber, stats);
    }
}
```