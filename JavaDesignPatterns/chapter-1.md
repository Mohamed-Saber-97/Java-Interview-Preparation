# A Brief Overview of GoF Design Patterns

## Creational patterns

- These patterns abstract the instantiation process. You make the systems independent of how their objects are composed, created, and represented. In these patterns, you should have a basic concern:
  “Where should I place the ‘new’ keyword in my application?” This decision can determine the degree of coupling of your classes. The following five patterns belong to this category:

1. ### Singleton pattern
2. ### Prototype pattern
3. ### Factory Method pattern
4. ### Builder pattern
5. ### Abstract Factory pattern

## Structural patterns

- Here you focus on how classes and objects can be composed to form a relatively large structure. They generally use inheritance or composition to group different interfaces or implementations. Your choice of composition over inheritance (and vice versa) can affect the flexibility of your software. The following seven patterns fall into this category:

1. ### Proxy pattern
2. ### Flyweight pattern
3. ### Composite pattern
4. ### Bridge pattern
5. ### Facade pattern
6. ### Decorator pattern
7. ### Adapter pattern

## Behavioral patterns

- Here you concentrate on algorithms and the assignment of responsibilities among objects. You also need to focus on the communication between them and how the objects are interconnected. The following eleven patterns fall into this category:

1. ### Observer pattern
2. ### Strategy pattern
3. ### Template Method pattern
4. ### Command pattern
5. ### Iterator pattern
6. ### Memento pattern
7. ### State pattern
8. ### Mediator pattern
9. ### Chain of Responsibility pattern
10. ### Visitor pattern
11. ### Interpreter pattern

## Classification of the GoF Design Patterns

- The GoF made another classification based on scope. It examines whether the pattern primarily focuses on the classes or its objects. Class patterns deal with classes and subclasses. They use inheritance mechanisms, so they are static and fixed at compile time. Object patterns deal with objects that can change at run time. So, object patterns are dynamic

| Scope  |  Purpose   |         Pattern         |
|:------:|:----------:|:-----------------------:|
| Class  | Creational |     Factory Method      |
| Class  | Structural |         Adapter         |
| Class  | Behavioral |     Template Method     |
| Class  | Behavioral |       Interpreter       |
| Object | Creational |        Singleton        |
| Object | Creational |        Prototype        |
| Object | Creational |         Builder         |
| Object | Creational |    Abstract Factory     |
| Object | Structural |         Adapter         |
| Object | Structural |          Proxy          |
| Object | Structural |        Flyweight        |
| Object | Structural |        Composite        |
| Object | Structural |         Bridge          |
| Object | Structural |         Facade          |
| Object | Structural |        Decorator        |
| Object | Behavioral |        Observer         |
| Object | Behavioral |        Strategy         |
| Object | Behavioral |         Command         |
| Object | Behavioral |        Iterator         |
| Object | Behavioral |         Memento         |
| Object | Behavioral |          State          |
| Object | Behavioral |        Mediator         |
| Object | Behavioral |         Visitor         |
| Object | Behavioral | Chain of Responsibility |

## Q&A Session

- What are the differences between class patterns and object patterns?
	- In general, class patterns focus on static relationships, but object patterns can focus on dynamic relationships. As the names suggest, class patterns focus on classes and their subclasses, and object patterns focus on the object’s relationships

|  Purpose   |                                     Class patterns                                     |                            Object patterns                            |
|:----------:|:--------------------------------------------------------------------------------------:|:---------------------------------------------------------------------:|
| Creational |                        Can defer object creation to subclasses                         |              Can defer object creation to another object              |
| Structural | They focus on the composition of classes(primarily using the concept of inheritance).  |         They focus on the different ways to assemble objects.         |
| Behavioral | Describes the algorithms and execution flows. They also use the inheritance mechanism. | Describes how different objects can work together and complete a task |

- Can I combine two or more patterns in an application?
	- Yes. In real-world scenarios, this type of activity is common.
- Any general advice before I jump into the topics?
	- Program to a supertype (abstract class/interface), not an implementation.
	- Prefer composition to inheritance in most cases.
	- Try to make a loosely coupled system.
	- Segregate code that is likely to vary from the rest of your code.
	- Encapsulate what varies.