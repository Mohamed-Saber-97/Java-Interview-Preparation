* [Object Lifetime](#object-lifetime)
    * [Reachable Objects](#reachable-objects)
        * [Memory Organization at Runtime](#memory-organization-at-runtime)
    * [Invoking Garbage Collection Programmatically](#invoking-garbage-collection-programmatically)
    * [Initializers](#initializers)
    * [Field Initializer Expressions](#field-initializer-expressions)
        * [Declaration Order of Initializer Expressions](#declaration-order-of-initializer-expressions)
        * [Exception Handling and Initializer Expressions](#exception-handling-and-initializer-expressions)
    * [Static Initializer Blocks](#static-initializer-blocks)
        * [Declaration Order of Static Initializers](#declaration-order-of-static-initializers)
        * [Exception Handling in Static Initializer Blocks](#exception-handling-in-static-initializer-blocks)
    * [Instance Initializer Blocks](#instance-initializer-blocks)
        * [Declaration Order of Instance Initializers](#declaration-order-of-instance-initializers)
        * [Exception Handling and Instance Initializer Blocks](#exception-handling-and-instance-initializer-blocks)
    * [Constructing Initial Object State](#constructing-initial-object-state)
        * [Initialization Anomaly under Object State Construction](#initialization-anomaly-under-object-state-construction)

-----

# Object Lifetime

## Reachable Objects

- Java provides thread-based multitasking, meaning that several threads can be executing concurrently in the JVM, each doing its own task
- A thread is alive if it has not completed its execution. Each live thread has its own JVM stack
- The JVM stack contains activation frames of methods that are currently active. Local references declared in a method can always be found in the method’s activation frame, stored on the JVM stack associated with the thread in which the method is called
- An object in the heap is said to be reachable if it is referenced by any local reference in a JVM stack
- Likewise, any object that is denoted by a reference in a reachable object is said to be reachable

### Memory Organization at Runtime

![](/images/MemoryOrganizationAtRuntime.png "Memory Organization at Runtime")

## Invoking Garbage Collection Programmatically

- The program can request that garbage collection be performed, but there is no way to force garbage collection to be activated `System.gc();`
- A Java application has a unique Runtime object that can be used by the application to interact with the JVM
- `static Runtime getRuntime()`
    - Returns the Runtime object associated with the current application
- `void gc()`
    - It is recommended to use the more convenient `static` method `System.gc()`
- `long freeMemory()`
    - Returns the amount of free memory (bytes) in the JVM that is available for new objects.
- `long totalMemory()`
    - Returns the total amount of memory (bytes) available in the JVM, including both memory occupied by current objects and memory available for new objects.

## Initializers

## Field Initializer Expressions

### Declaration Order of Initializer Expressions

- When an object is created using the new operator, instance initializer expressions are executed in the order in which the instance fields are declared in the class

```java
class NonStaticInitializers {
    int length = 10; // (1)
    int width = 10; // (4)
    //double area = length * width; // (2) Not OK. Illegal forward reference.
    double area = length * this.width; // (3) OK, but width has default value 0.
    int height; // (6)
    int sqSide = height = 20; // (5) OK. Legal forward reference.
}
```

- The declaration-before-reading rule is equally applicable to static initializer expressions when static fields are referenced by their simple names
- methods are not subject to the same access rules as initializer expressions

### Exception Handling and Initializer Expressions

- Initializer expressions in named classes and interfaces must not result in any uncaught checked exception
- If any checked exception is thrown during execution of an initializer expression, it must be caught and handled by code called from the initializer expression
- This restriction does not apply to instance initializer expressions in anonymous classes

## Static Initializer Blocks

### Declaration Order of Static Initializers

- Local variables in static and instance initializer blocks can be declared with the reserved type name `var`
- Initializer blocks are not members of a class, and they cannot have a return statement because they cannot be called directly.
- When a class is initialized, the initializer expressions in static field declarations and static initializer blocks are executed in the order in which they are specified in the class

```java
public class ScheduleV1 {
    //static int numOfDays = 7 * numOfWeeks; // (1) Compile-time error! Simple name.
    static int numOfWeeks = 52; // (2)
}
```

```java
public class ScheduleV3 {
    static int numOfWeeks = 52; // (2) 52
    static int numOfDays = 7 * ScheduleV3.numOfWeeks; // (1) 0
```

- During class initialization, such final static fields are always initialized first, before any other initializers are executed.
- Such a constant field never gets initialized with the default value of its type at runtime

### Exception Handling in Static Initializer Blocks

- Exception handling in static initializer blocks is no different from that in static initializer expressions: Uncaught checked exceptions cannot be thrown.
- Code in initializers cannot throw checked exceptions. A static initializer block cannot be called directly.
- Therefore, any checked exceptions must be caught and handled in the body of the static initializer block; otherwise, the compiler will issue an error.

## Instance Initializer Blocks

### Declaration Order of Instance Initializers

- Analogous to the other initializers discussed earlier, an instance initializer block cannot make a forward reference to a field by its simple name in a read operation as that would violate the declaration-before-reading rule. However, using the `this` keyword to access a field is not a problem

```java
public class InstanceInitializersII {
    final int k = 50; // (9) Final instance field with constant expression.
    // Instance field declarations.
    int i; // (7) Field declaration without initializer expression.
    int j = 100; // (8) Field declaration with initializer expression.

    { //Instance initializer with forward references. (1)
        i = j = 10; // (2) Permitted.
        int result = this.i * this.j; // (3) i is 10, j is 10.
        System.out.println(this.i); // (4) 10
        System.out.println(this.j); // (5) 10
        System.out.println(this.k); // (6) 50
    }
}
```

### Exception Handling and Instance Initializer Blocks

- Exception handling in instance initializer blocks differs from that in static initializer blocks in the following aspect: The execution of an instance initializer block can result in an uncaught checked exception, provided the exception is declared in the throws clause of every constructor in the class. Static initializer blocks cannot allow this, since no constructors are involved in class initialization. Instance initializer blocks in anonymous classes have even greater freedom: They can throw any exception

## Constructing Initial Object State

```java
class SuperclassA {
    public SuperclassA() { // (1) Superclass constructor
        System.out.println("Constructor in SuperclassA");
    }
}
```

```java
class SubclassB extends SuperclassA {
    int value = initializerExpression(); // (6) Instance field declaration

    { // (4) Instance initializer block
        System.out.println("Instance initializer block in SubclassB");
        value = 2; // (5)
    }

    SubclassB() { // (2) No-argument constructor
        this(3);
        System.out.println("No-argument constructor in SubclassB");
    }

    SubclassB(int i) { // (3) Non-zero argument constructor
        System.out.println("Non-zero argument constructor in SubclassB");
        value = i;
    }

    private int initializerExpression() { // (7)
        System.out.println("Instance initializer expression in SubclassB");
        return 1;
    }
}
```

```java
public class ObjectConstruction {
    public static void main(String[] args) {
        SubclassB objRef = new SubclassB(); // (8)
        System.out.println("value: " + objRef.value);
    }
}
```

```text
Constructor in SuperclassA
Instance initializer block in SubclassB
Instance initializer expression in SubclassB
Non-zero argument constructor in SubclassB
No-argument constructor in SubclassB
value: 3
```

### Initialization Anomaly under Object State Construction

```java
class SuperclassA {
    protected int superValue; // (1)

    SuperclassA() { // (2)
        System.out.println("Constructor in SuperclassA");
        this.doValue(); // (3)
    }

    void doValue() { // (4)
        this.superValue = 911;
        System.out.println("superValue (from SuperclassA): " + this.superValue);
    }
}
```

```java
class SubclassB extends SuperclassA {
    private int value = 800; // (5)

    SubclassB() { // (6)
        System.out.println("Constructor in SubclassB");
        this.doValue();
        System.out.println("superValue (from SuperclassA): " + this.superValue);
    }

    @Override
    void doValue() { // (7)
        System.out.println("value (from SubclassB): " + this.value);
    }
}
```

```java
public class ObjectInitialization {
    public static void main(String[] args) {
        System.out.println("Creating an object of SubclassB.");
        new SubclassB(); // (8)
    }
}
```

```text
Output from the program:
Creating an object of SubclassB.
Constructor in SuperclassA
value (from SubclassB): 0
Constructor in SubclassB
value (from SubclassB): 800
superValue (from SuperclassA): 0
```