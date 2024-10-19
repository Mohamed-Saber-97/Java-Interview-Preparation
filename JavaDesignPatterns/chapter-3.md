# Simple Factory Pattern

## Definition

- It creates an object without exposing the instantiation logic to the client.

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
    public Dog() {
        System.out.println("\nA dog is created.");
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
    public Tiger() {
        System.out.println("\nA tiger is created.");
    }

    public void displayBehavior() {
        System.out.println("Tiger says: Halum.");
        System.out.println("It loves to roam in a jungle.");
    }
}
```

```java
// AnimalFactory.java
class AnimalFactory {
    public Animal createAnimal(String animalType) {
        Animal animal;
        if (animalType.equals("dog")) {
            animal = new Dog();
        } else if (animalType.equals("tiger")) {
            animal = new Tiger();
        } else {
            System.out.println("You can create either a 'dog' or a 'tiger'. ");
            throw new IllegalArgumentException("Unknown animal cannot be instantiated.");
        }
        return animal;
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("*** Simple Factory Demonstration. * **");
        AnimalFactory factory = new AnimalFactory();
        Animal animal = factory.createAnimal("dog");
        animal.displayBehavior();
        animal = factory.createAnimal("tiger");
        animal.displayBehavior();
    }
}
```

## Q&A Session

- The analysis section shows that you are passing String parameters, and you do not ensure the type safety in this application. Is this correct?
    - it is ok to use enums to guard this situation.
- In this example, I see that the clients are delegating the object’s creation through the Simple Factory pattern. But instead of this, they could directly create the objects with the new operator. Is this correct?
    - In this context, you need to remember the key reasons for using the Simple Factory pattern:
        - One of the key object-oriented design principles is to separate the parts of your code that are most likely to change from the rest
        - In this case, only the creation process for objects changes. You can assume that these animals must tell something about themselves, and that part of the code does not need to vary inside the client code. So, in the future, if there is any change required in the creation process, you need to change only the createAnimal() method of the AnimalFactory class. The client code will not be affected because of those changes
        - You do not want to put lots of if-else blocks (or switch statements) inside the client code. This makes your client code clumsy
        - How you are creating the objects is hidden inside the client code. This kind of abstraction promotes security
        - sometimes the life-cycle management of the created objects must be centralized to ensure consistent behavior within the application. This cannot be done if the client is free to create a concrete object the way they wish
- What are the challenges associated with this pattern
    - If you want to add a new animal or delete an existing animal, you need to modify the createAnimal() method. This process violates the Open/Closed Principle
      (which basically says that a code module should be open for extension but closed for modification) of the SOLID principles
- Can you make the factory class static?
    - In general, I do NOT vote for static factory class because they are basically promoters for global states, which are not ideal for object-oriented programming
- You could make the createAnimal() method static. Is this correct?
    - Yes. In fact, this is a common technique and is often termed as a static factory. The only advantage of this approach is that you can avoid instantiating objects from a class to use a method. But if you need to consider subclassing, you face troubles