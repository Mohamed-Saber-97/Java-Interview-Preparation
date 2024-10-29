* [Generics](#generics)
    * [Introducing Generics](#introducing-generics)
    * [Generic Types and Parameterized Types](#generic-types-and-parameterized-types)
        * [Generic Types](#generic-types)
            * [Some Restrictions on the Use of Type Parameters in a Generic Type](#some-restrictions-on-the-use-of-type-parameters-in-a-generic-type)
        * [Parameterized Local Variable Type Inference](#parameterized-local-variable-type-inference)
        * [Generic Interfaces](#generic-interfaces)
        * [Extending Generic Types](#extending-generic-types)
        * [Raw Types and Unchecked Warnings](#raw-types-and-unchecked-warnings)
    * [Wildcards](#wildcards)
        * [The Subtype Covariance Problem with Parameterized Types](#the-subtype-covariance-problem-with-parameterized-types)
        * [Wildcard Types](#wildcard-types)
            * [Summary of Subtyping Relationships for Generic Types](#summary-of-subtyping-relationships-for-generic-types)
        * [Subtype Covariance: `? extends` Type](#subtype-covariance--extends-type)
        * [Subtype Contravariance: `? super` Type](#subtype-contravariance--super-type)
        * [Subtype Bivariance: `?`](#subtype-bivariance-)
        * [Some Restrictions on Wildcard Types](#some-restrictions-on-wildcard-types)
    * [Using References of Wildcard Parameterized Types](#using-references-of-wildcard-parameterized-types)
        * [Using Parameterized References to Call Set and Get Methods](#using-parameterized-references-to-call-set-and-get-methods)
    * [Bounded Type Parameters](#bounded-type-parameters)
        * [Multiple Bounds](#multiple-bounds)
    * [Generic Methods and Constructors](#generic-methods-and-constructors)
        * [Declaring Generic Methods](#declaring-generic-methods)

---------------

# Generics

## Introducing Generics

- Generics in Java are a way of providing type information in ADTs so that the compiler can guarantee type-safety of operations at runtime
- Generics are implemented as compile-time transformations, with negligible impact on the JVM
- The generic type declaration is compiled once into a single Java class file, and the use of the generic type is checked against this file.
- Also, no extraneous Java class files are generated for each use of the generic type.
- Since the compiler guarantees type-safety, this eliminates the necessity of explicit type checking and casting at runtime

## Generic Types and Parameterized Types

### Generic Types

- `E` is used for the type of elements in a collection, `K` and `V` are used for the type of the keys and the type of the values in a map, and `T` is used to represent an arbitrary type
- we can only call methods that are inherited from the Object class on the field data, as these methods are inherited by all objects, regardless of their object type
- The scope of the type parameter E of the generic type includes any non-static inner classes, but excludes any static member types—the parameter E cannot be accessed in static context
- It also excludes any nested generic declarations where the same name is redeclared as a formal type parameter. Shadowing of type parameter names should be avoided

```java
class Node<E> {
    private E data; // Data (1)
    private Node<E> next; // Reference to next node (2)

    Node(E data, Node<E> next) { // (3)
        this.data = data;
        this.next = next;
    }

    public E getData() {return this.data;} // (5)

    public void setData(E data) {this.data = data;} // (4)

    public Node<E> getNext() {return this.next;} // (7)

    public void setNext(Node<E> next) {this.next = next;} // (6)

    @Override
    public String toString() { // (8)
        return this.data.toString() + (this.next == null ? "" : ", " + this.next.toString());
    }
}
```

#### Some Restrictions on the Use of Type Parameters in a Generic Type

- A constructor declaration in a generic class cannot specify the formal type parameters of the generic class in its constructor header after the class name

```text
class Node<E> {
    Node<E>(){} // Compile-time error!
}
```

- A formal type parameter cannot be used to create a new instance, as it is not known which concrete type it represents `E ref = new E(); // Compile-time error!`

### Parameterized Local Variable Type Inference

```java
void test() {
    Node<String> node1 = new Node<String>(null, null); // (1) Node of String
    Node<String> node2 = new Node<>(null, null); // (2) Node of String
    var node3 = new Node<String>(null, null); // (3) Node of String
    var node4 = new Node<>(null, null); // (4) Node of Object
}
```

### Generic Interfaces

```java
interface IMonoLink<E> {
    E getData();

    void setData(E data);

    IMonoLink<E> getNext();

    void setNext(IMonoLink<E> next);
}
```

```java
class MonoNode<E> implements IMonoLink<E> {
    private E data; // Data
    private IMonoLink<E> next; // Reference to next node (1)

    MonoNode(E data, IMonoLink<E> next) { // (2)
        this.data = data;
        this.next = next;
    }

    @Override
    public E getData() {return this.data;}

    @Override
    public void setData(E data) {this.data = data;}

    @Override
    public IMonoLink<E> getNext() {return this.next;} // (4)

    @Override
    public void setNext(IMonoLink<E> next) {this.next = next;} // (3)

    @Override
    public String toString() {
        return this.data.toString() + (this.next == null ? "" : ", " + this.next);
    }
}
```

### Extending Generic Types

```java
void test() {
    BiNode<Integer> intBiNode = new BiNode<>(2020, null, null);
    MonoNode<Integer> intMonoNode = intBiNode; // (1)
    MonoNode<Number> numMonoNode = intBiNode; // (2) Compile-time error!
}
```

![Extending Generic Types](/images/ExtendingGenericTypes.png "Extending Generic Types")

```java
class WeirdNode<E> extends MonoNode<E> implements IMonoLink<Integer> {}// Error!
```

### Raw Types and Unchecked Warnings

- parameterized types `Node<String>`, `Node<Integer>`, and `Node<Node<String>>` are all represented at runtime by their raw type `Node`
- the compiler does not create a new class for each parameterized type.
- Only one class (`Node`) exists that has the name of the generic class (`Node<E>`), and the compiler generates only one class file (Node.class) with the Java bytecode for the generic class
- Only reference types (excluding array creation and enumerations) can be used in invocations of generic types

> Generics are implemented in the compiler only. The JVM is oblivious about the use of generic types. It does not differentiate between Node<String> and Node<Integer>, and just knows about the class Node. The compiler translates the generic class by a process known as type erasure; meaning that information about type parameters is erased and casts are inserted to make the program type-safe at runtime. The compiler guarantees that casts added at compile time never fail at runtime, when the program compiles without any unchecked warnings

## Wildcards

### The Subtype Covariance Problem with Parameterized Types

- The reason for the compile-time errors is that `Node<Integer>` and `Node<Double>` are not subtypes of `Node<Number>`, although Integer and Double are subtypes of `Number`
- the array types `Integer[]` and `Double[]` are subtypes of the array type `Number[]`.

![No Subtype Covariance for Parameterized Types](/images/NoSubtypeCovarianceforParameterizedTypes.png "No Subtype Covariance for Parameterized Types" )

### Wildcard Types

- The wildcard `?` by itself represents all types

#### Summary of Subtyping Relationships for Generic Types

|          Name          |      Syntax      |                Semantics                |            Description            |
|:----------------------:|:----------------:|:---------------------------------------:|:---------------------------------:|
|   Subtype covariance   | `? extends` Type |  Any subtype of Type (including Type)   | Bounded wildcard with upper bound |
| Subtype contravariance |  `? super` Type  | Any supertype  of Type (including Type) | Bounded wildcard with lower bound |
|   Subtype bivariance   |       `?`        |                All types                |        Unbounded wildcard         |
|   Subtype invariance   |       Type       |             Only type Type              |      Type parameter/argument      |

### Subtype Covariance: `? extends` Type

```java
Node<? extends Integer> intSubNode = new Node<Integer>(100, null);
Node<? extends Number> numSupNode = intSubNode;
```

### Subtype Contravariance: `? super` Type

```java
Node<? super Number> numSupNode = new Node<Number>(100, null);
Node<? super Integer> numIntSupNode = numSupNode;
```

### Subtype Bivariance: `?`

- `Node<?>` <=> `Node<? extends Object>`

### Some Restrictions on Wildcard Types

- Wildcards cannot be used in instance creation expressions. The actual type parameter in the constructor call must be a concrete type

```java
Node<?> anyNode = new Node<?>(2020, null); // Compile-time error!
Node<? extends Integer> extIntNodeA = new Node<? extends Integer>(0, null); // Compile-time error!
Node<? extends Integer> extIntNodeB = new Node<Integer>(0, null); // OK
```

- Wildcards cannot be used in the header of reference type declarations. Supertypes in the extends and implements clauses cannot have wildcards

```java
interface INode extends Comparable<? extends Node<?>> { /* ... */} // Not OK.

class QuestionableNode<?> { /* ... */} // Not OK.

class SubNode extends Node<?> { /* ... */} // Not OK.

class XNode implements Comparable<?> { /* ... */} // Not OK.
```

- However, nested wildcards are not a problem in a reference type declaration header or in an object creation expression

```java
Node<?> nodeOfAnyNode = new Node<Node<?>>(new Node<Integer>(2020, null), null);

class OddNode extends Node<Node<?>> implements Comparable<Node<?>> { /* ... */}
```

## Using References of Wildcard Parameterized Types

### Using Parameterized References to Call Set and Get Methods

```java
class Node<E> {
    private E data;

    public E getData() {return data;} // (1) Get operation.

    public void setData(E obj) {data = obj;} // (2) Set operation.
}
```

|   Operation   |    `Node`     |            `Node<?>`             |    `Node <? extends Number>`     | `Node <? super Number>` |   `Node<Number>`    |
|:-------------:|:-------------:|:--------------------------------:|:--------------------------------:|:-----------------------:|:-------------------:|
| set/put/write |  Any object   | Cannot put anything except nulls | Cannot put anything except nulls |   `Number` or subtype   | `Number` or subtype |
|   get/read    | `Object` only |          `Object` only           |             `Number`             |      `Object` only      |      `Number`       |

## Bounded Type Parameters

### Multiple Bounds

- `class CmpNode<E extends Number & Serializable> `

## Generic Methods and Constructors

### Declaring Generic Methods

- A generic method (also called polymorphic method) is implemented like an ordinary method, except that one or more formal type parameters are specified immediately preceding the return type
- In the case of a generic constructor, the formal parameters are specified before the class name in the constructor header

```java
static <E> boolean contains(E key, E[] array) {
    for (E element : array)
        if (key.equals(element)) return true;
    return false;
}
```