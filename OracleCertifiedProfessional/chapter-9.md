# Nested Type Declarations

## Overview of Nested Type Declarations

![](/images/OverviewofTypeDeclarations.png "Overview of Type Declarations")

```java
class TLC { // (1) Top-level class
    static SMI sf = new SMI() {}; // (12) Inner class in static context
    // Anonymous classes (here defined as initializer expressions):
    SMC nsf = new SMC() {}; // (11) Inner class in non-static context

    // Local types in non-static context (analogous for static context):
    void nsm() { // Non-static method
        enum SLE {} // (9) Static local enum
        interface SLI {} // (8) Static local interface

        record SLR() {} // (10) Static local record

        class LC {} // (7) Local class (inner class)
    }

    enum SME {} // (4) Static member enum

    interface SMI {} // (3) Static member interface

    // Static member types:
    static class SMC {} // (2) Static member class

    record SMR() {} // (5) Static member record

    // Non-static member class:
    class NSMC {} // (6) Inner class
}
```

|                                           Type                                           |                     Declaration Context                     |                                        Access Modifiers                                        | Enclosing Instance |                            Direct Access to Enclosing Context                             |
|:----------------------------------------------------------------------------------------:|:-----------------------------------------------------------:|:----------------------------------------------------------------------------------------------:|:------------------:|:-----------------------------------------------------------------------------------------:|
|     Top-level `class`, `interface`, `enum` type, or `record` class (Top-level types)     |                           Package                           |                                  `public` or `package` access                                  |         No         |                                            N/A                                            |
| Static member `class`, `interface`, `enum` type, or `record` class (Static member types) |   As `static` member of a top-level type or a nested type   | All, except when declared in interfaces whose member type declarations are implicitly `public` |         No         |                            Static members in enclosing context                            |
|                         Non-`static` member class (Inner class)                          | As non-`static` member of a top-level type or a nested type |                                              All                                               |        Yes         |                             All members in enclosing context                              |
|                                Local class (Inner class)                                 |             In block with non-`static` context              |                                              None                                              |        Yes         |   All members in enclosing context plus `final` or effectively `final` local variables    |
|                                Local class (Inner class)                                 |               In block with `static` context                |                                              None                                              |         No         | `Static` members in enclosing context plus `final` or effectively `final` local variables |
|                              Anonymous class (Inner class)                               |            As expression in non-`static` context            |                                              None                                              |        Yes         |   All members in enclosing context plus `final` or effectively `final` local variables    |
|                              Anonymous class (Inner class)                               |              As expression in `static` context              |                                              None                                              |         No         | `Static` members in enclosing context plus `final` or effectively `final` local variables |
|          Local `interface`, `enum` type, or `record` class (Static local types)          |       In block with `static` and non-`static` context       |                                              None                                              |         No         |                           `Static` members in enclosing context                           |

## Static Member Types

### Importing Static Member Types

```java
import static smc.ListPool.MyLinkedList.BiNode; // (3) Static import

public class Client2 {
    BiNode objRef2 = new BiNode(); // (4)
}
//class BiListPool implements smc.ListPool.IBiLink { } // (5) Compile-time error!
// Not accessible!
```

- the interface `smc.ListPool.IBiLink` is declared with package access in its enclosing class ListPool in the package smc, and therefore is invisible in other packages, including the default package.

### Accessing Members in Enclosing Context

- Static member classes do not have a `this` reference, as they do not have any notion of an enclosing instance
- This means that any code in a static member class can only directly access static members in its enclosing context
- Trying to access any instance members directly in its enclosing context results in a compile-time error
- a static member class can define both static and instance members, like any other top-level class. However, its code can only directly access static members in its enclosing context

## Anonymous Classes

### Declaring Anonymous Classes

- An anonymous class cannot be declared with an access modifier, nor can it be declared `static`, `final`, or `abstract`

### Extending an Existing Class

```java
new superclass_name<optional_type_arguments> (optional_constructor_arguments)
        {
member_declarations
}
```

- Optional type arguments and constructor arguments can be specified, which are passed to the superclass constructor. Thus the superclass must provide a constructor corresponding to the arguments passed. No extends clause is used in the construct

```java
interface IDrawable { // (1)
    void draw();
}
```

```java
class Shape implements IDrawable { // (2)
    @Override
    public void draw() {System.out.println("Drawing a Shape.");}
}
```

```java
522CHAPTER 9:
NESTED TYPE
DECLARATIONS Example 9.14
Defining Anonymous
Classes

// File: AnonClassClient.java
interface IDrawable { // (1)
    void draw();
}

//_____________________________________________________________________________
class Shape implements IDrawable { // (2)
    @Override
    public void draw() {System.out.println("Drawing a Shape.");}
}

//_____________________________________________________________________________
class Painter { // (3) Top-level Class
    public static IDrawable createIDrawable() { // (7) Static Method
        return new IDrawable() { // (8) Implements interface at (1)
            @Override
            public void draw() {
                System.out.println("Drawing a new IDrawable.");
            }
        };
    }

    public Shape createShape() { // (4) Non-static Method
        return new Shape() { // (5) Extends superclass at (2)
            @Override
            public void draw() {System.out.println("Drawing a new Shape.");}
        };
    }
}
```

```java
public class AnonClassClient {
    public static void main(String[] args) { // (9)
        IDrawable[] drawables = { // (10)
                new Painter().createShape(), // (11) Non-static anonymous class
                Painter.createIDrawable(), // (12) Static anonymous class
                new Painter().createIDrawable() // (13) Static anonymous class
        };
        for (IDrawable aDrawable : drawables) // (14)
            aDrawable.draw();
        System.out.println("Anonymous Class Names:");
        System.out.println(drawables[0].getClass().getName());// (15)
        System.out.println(drawables[1].getClass().getName());// (16)
    }
}
```

### Instantiating Anonymous Classes

#### Accessing Local Declarations in the Enclosing Block

- An anonymous class can only access (effectively) final variables in its enclosing local context

#### Accessing Members in the Enclosing Class

```java
abstract class Base { // (1) Superclass
    protected int nsf1;

    abstract void printValue();
}
```

```java
class TLCWithAnonClasses { // (2) Top level Class
    private int nsf1; // Non-static field
    private int nsf2; // Non-static field
    private static int sf = 5; // Static field

    public void nonStaticMethod(final int fp) { // (3) Non-static Method
        // Local variables:
        int flv = 10; // (4) Effectively final local variable
        final int hlv = 20; // (5) Final local variable (constant variable)
        int nflv1 = 30; // (6) Non-final local variable
        int nflv3; // (7) Non-final local variable declaration
        nflv1 = 40; // (8) Not effectively final local variable
        Base baseRef = new Base() { // (9) Non-static anonymous class
            // Static fields: Accessing local declarations in the enclosing block:
            static int sff1 = fp; // (10) Final param from enclosing method
            static int sff2 = flv; // (11) Effect. final variable from enclosing method
            // static int sf1 = nflv1; // (12) Not effect. final from enclosing method
            // Instance fields: Accessing local declarations in the enclosing block:
            int f1 = fp; // (13) Final param from enclosing method
            int f2 = flv; // (14) Effectively final variable from enclosing method
            // int f3 = nflv1; // (15) Not effectively final from enclosing method
            // int f4 = nflv2; // (16) nflv2 cannot be resolved: not decl-before-use
            // int f5 = nflv3; // (17) Not definitely assigned: not initialized
            int hlv; // (18) Shadows local variable at (5)
            // Accessing member declarations inherited from superclass:
            int f6 = nsf1; // (19) Inherited from superclass
            int f7 = this.nsf1; // (20) Inherited from superclass
            int f8 = super.nsf1; // (21) Inherited from superclass
            // Accessing (hidden) member declarations in the enclosing class:
            int f9 = TLCWithAnonClasses.this.nsf1; // (22) In enclosing object
            int f10 = nsf2; // (23) Instance field in enclosing object
            int f11 = sf; // (24) Static field from enclosing class

            {nsf1 = fp;} // (25) Non-static initializer block

            @Override
            void printValue() { // (26) Instance method
                System.out.println("Instance field nsf1: " + nsf1);// (27)
            }
        };
        int nflv2 = 70;
        nflv3 = 80;
        baseRef.printValue(); // (28) Invoke method on anonymous object
    }

    public static final Base baseField = new Base() { // (29) Static anonymous class
        // Accessing (hidden) member declarations in the enclosing class:
        // int f1 = TLCWithAnonClasses.this.nsf1; // (30) Not OK. No enclosing object
        // int f2 = nsf2; // (31) Not OK. No enclosing object
        {nsf1 = sf;} // (32) Non-static initializer block

        @Override
        void printValue() { // (33) Instance method
            System.out.println("Instance field nsf1: " + nsf1);// (34)
        }
    };
}
```

```java
public class AnonClient {
    public static void main(String[] args) {
        new TLCWithAnonClasses().nonStaticMethod(100); // (35)
        TLCWithAnonClasses.baseField.printValue(); // (36)
    }
}
```