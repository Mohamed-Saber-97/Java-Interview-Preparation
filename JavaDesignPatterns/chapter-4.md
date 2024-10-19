# Factory Method Pattern

## GoF Definition

- It defines an interface for creating an object, but lets subclasses decide which class to instantiate. The Factory Method pattern lets a class defer instantiation to subclasses
- Here you start with an abstract creator class (often called a creator) that defines the basic structure of an application, and the subclasses (that derive from this abstract class) take the responsibility of doing the actual instantiation process
- In database programming, you may need to support different database users. For example, one user may use SQL Server and the other may opt for Oracle. So, when you need to insert data into your database, first you need to create a connection object, such as SqlServerConnection or OracleConnection, and only then can you proceed. If you put the code into an if-else block (or switch statements), you may need to repeat lots of similar code, which isn’t easily maintainable. Also, whenever you start supporting a new type of connection, you need to reopen your code and make modifications. This type of problem can be resolved using the Factory Method pattern.
- The `iterator()` method of the `AbstractCollection<E>` (or `Collection<E>`) is an example of a factory method in Java.
- **_you can take advantage of using an abstract class. How? Ok, let’s suppose you want all the subclasses to follow a common rule that is imposed from the parent (or base) class. It simply means that you need to put the common behaviors/codes inside the parent class_**

## Demonstration

```java
// Animal.java
interface Animal {
    void displayBehavior();
}
```

```java
// Dog.java
class Dog implements Animal {
    public Dog(String color) {
        System.out.println("\nA dog with " + color + " color is created.");
    }

    public void displayBehavior() {
        System.out.println("It says: Bow-Wow.");
        System.out.println("It prefers barking.");
    }
}
```

```java
// Tiger.java
class Tiger implements Animal {
    public Tiger(String color) {
        System.out.println("\nA tiger with " + color + " color is created.");
    }

    public void displayBehavior() {
        System.out.println("Tiger says: Halum.");
        System.out.println("It loves to roam in a jungle.");
    }
}
```

```java
// AnimalFactory.java
abstract class AnimalFactory {
    public void createAndDisplayAnimal(String color) {
        Animal animal;
        animal = createAnimal(color);
        animal.displayBehavior();
    }

    // This is the "factory method"
    // Notice that I defer the instantiation
    // process to the subclasses.
    protected abstract Animal createAnimal();
}
```

```java
// DogFactory.java
class DogFactory extends AnimalFactory {
    // Creating and returning a 'Dog' instance
    @Override
    protected Animal createAnimal(String color) {
        return new Dog(color);
    }
}
```

```java
// TigerFactory.java
class TigerFactory extends AnimalFactory {
    // Creating and returning a 'Tiger' instance
    @Override
    protected Animal createAnimal(String color) {
        return new Tiger(color);
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***Factory Method pattern modified demonstration.***");
        AnimalFactory factory;
        // Create a tiger and display its behavior
        // using TigerFactory.
        factory = new TigerFactory();
        factory.createAndDisplayAnimal("yellow");
        // Create a dog and display its behavior
        // using DogFactory.
        factory = new DogFactory();
        factory.createAndDisplayAnimal("white");
    }
}
```

## Q&A Session

- Why have you separated the createAnimal() method from the client code?
    - If you read the GoF definition, you understand that this is the main purpose: for the subclasses to create specialized objects. If you look carefully, you will also find that only this “creational part” varies across the products.
- What are the advantages of using a factory like this?
    - You are separating the code that varies from the code that does not vary (in other words, the advantages of using the Simple Factory pattern are still present). This helps you maintain the code easily
    - The code is not tightly coupled, so you can add new classes such as Lion and Bear at any time in the system without modifying the existing architecture. In other words, you have followed the Open/ Closed Principle.
    - A client can use a factory method without knowing how the actual type of object is created.
- What are the challenges of using a factory like this?
    - each particular instance is associated with a subclass. It means that even if you need only one particular instance, you are forced to create a new subclass. Obviously, the use of a parameterized factory method does not increase the total number of classes and can help you overcome this kind of difficulty
    - If you do not start with a factory method, it can be a challenging task to refactor existing code. You need to understand the dependencies clearly before you refactor the existing code into the factory method pattern.
- I see that the Factory Method pattern is supporting two parallel hierarchies. Is this understanding correct?
    - Note that the creators and their creations/products form separate hierarchies that run in parallel
- You should always mark the factory method with an abstract keyword so that subclasses can complete them. Is this correct?
    - No. Sometimes you may be interested in a default factory method if the creator has no subclasses. In that case, you cannot mark the factory method with an abstract keyword.
