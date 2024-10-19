<!-- TOC -->

* [Understanding SOLID Principles](#understanding-solid-principles)
    * [Single Responsibility Principle](#single-responsibility-principle)
        * [Initial Program](#initial-program)
        * [Better Program](#better-program)
    * [Open/Closed Principle](#openclosed-principle)
        * [Initial Program](#initial-program-1)
        * [Better Program](#better-program-1)
    * [Liskov Substitution Principle](#liskov-substitution-principle)
        * [Initial Program](#initial-program-2)
        * [Better Program](#better-program-2)
    * [Interface Segregation Principle](#interface-segregation-principle)
        * [Initial Program](#initial-program-3)
        * [Better Program](#better-program-3)
    * [Dependency Inversion Principle](#dependency-inversion-principle)
        * [Initial Program](#initial-program-4)
        * [Better Program](#better-program-4)
    * [Summary](#summary)

<!-- TOC -->
--------

# Understanding SOLID Principles

## Single Responsibility Principle

- A class acts like a container that can hold many things such as data, properties, or methods. If you put in too much data or methods that are not related to each other, you end up with a bulky class that can create problems in the future
- Suppose you create a class with multiple methods that do different things. changes in one method can impact the other related method(s) in the class. **_a class should have only one reason to change._**
- you can divide a big problem into smaller chunks based on different responsibilities and put each of these small parts into separate classes. the next question is, what do we mean by responsibility? in simple words, **_responsibility is a reason for a change_** and do not to confuse this principle with the principle that says a function should do one, and only one, thing.

### Initial Program

```java
// Employee.java

import java.util.Random;

class Employee {
    public String firstName, lastName, empId;
    public double experienceInYears;

    public Employee(String firstName, String lastName, double experience) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.experienceInYears = experience;
    }

    public void displayEmpDetail() {//shows the employee's name and their working experience in years.
        System.out.println("The employee name: " + lastName + "," + firstName);
        System.out.println("This employee has " + experienceInYears + " years of experience.");
    }

    public String checkSeniority(double experienceInYears) { //evaluates whether an employee is a senior person.
        return experienceInYears > 5 ? "senior" : "junior";
    }

    public String generateEmpId(String empFirstName) { // method generates an employee id using string concatenation.
        int random = new Random().nextInt(1000);
        empId = empFirstName.substring(0, 1) + random;
        return empId;
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("*** A demo without SRP.***"); //*** A demo without SRP.***
        Employee robin = new Employee("Robin", "Smith", 7.5);
        showEmpDetail(robin);
        // The employee name: Smith,Robin
        // This employee has 7.5 years of experience.
        // The employee id: R446
        // This employee is a senior employee
    }

    private static void showEmpDetail(Employee emp) {
        emp.displayEmpDetail();
        System.out.println("The employee id: " + emp.generateEmpId(emp.firstName));
        System.out.println("This employee is a " + emp.checkSeniority(emp.experienceInYears) + " employee.");
    }
}
```

### Better Program

```java
// Employee.java

import java.util.Random;

class Employee {
    public String firstName, lastName, empId;
    public double experienceInYears;

    public Employee(String firstName, String lastName, double experience) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.experienceInYears = experience;
    }

    public void displayEmpDetail() {//shows the employee's name and their working experience in years.
        System.out.println("The employee name: " + lastName + "," + firstName);
        System.out.println("This employee has " + experienceInYears + " years of experience.");
    }
}
```

```java
// EmployeeIdGenerator.java

import java.util.Random;

class EmployeeIdGenerator {
    String empId;

    public String generateEmpId(String empFirstName) {
        int random = new Random().nextInt(1000);
        empId = empFirstName.substring(0, 1) + random;
        return empId;
    }
}
```

```java
// SeniorityChecker.java
class SeniorityChecker {
    public String checkSeniority(double experienceInYears) {
        return experienceInYears > 5 ? "senior" : "junior";
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("*** A demo that follows the SRP.***");
        Employee robin = new Employee("Robin", "Smith", 7.5);
        showEmpDetail(robin);
        // The employee name: Smith,Robin
        // This employee has 7.5 years of experience.
        // The employee id: R446
        // This employee is a senior employee
    }

    private static void showEmpDetail(Employee emp) {
        // Display employee detail
        emp.displayEmpDetail();
        // Generate the ID
        EmployeeIdGenerator idGenerator = new EmployeeIdGenerator();
        String empId = idGenerator.generateEmpId(emp.firstName);
        System.out.println("The employee id: " + empId);
        // Check the seniority level
        SeniorityChecker seniorityChecker = new SeniorityChecker();
        System.out.println("This employee is a " + seniorityChecker.checkSeniority(emp.experienceInYears) + " employee.");
    }
}
```

## Open/Closed Principle

- **_A software artifact should be open for extension but closed for modification_**
    - Any modular decomposition technique must satisfy the Open-Closed Principle. Modules should be both open and closed
    - A module is said to be open if it is still available for extension. For example, it should be possible to expand its set of operations or add fields to its data structures
    - A module is said to be closed if it is available for use by other modules. This assumes that the module has been given a well-defined, stable description (its interface in the sense of information hiding). At the implementation level, closure for a module also implies that you may compile it, perhaps store it in a library, and make it available for others (its clients) to use
    - He explains that openness is useful for software developers because they can’t foresee all the elements that a module may need in the future. But the “closed” modules will satisfy project managers because they want to complete the project instead of waiting for everyone to complete their parts

- **_A class is closed, since it may be compiled, stored in a library, base lined,and used by client classes. But it is also open, since any new class may use it as a parent, adding new features. When a descendant class is defined, there is no need to change the original or to disturb its clients_**
- But inheritance promotes tight coupling. In programming, we like to remove these tight couplings. The new proposal uses abstract base classes that use the protocols instead of a superclass to allow different implementations.These protocols are closed for modification, and they provide another level of abstraction that enables loose coupling
- Now you are following the SRP. If in the future the examining authority changes the distinction criteria
- you do not touch the Student class. So, this part is closed for modification. This solves one part of the problem
- every time the distinction criteria changes, you need to modify the `evaluateDistinction()` method in the DistinctionDecider class. **_So, this class is not closed for modification_**
- OCP can be achieved in different ways, but abstraction is the heart of this principle
- You understand that in the future you may need to consider a different stream, such as commerce. How do you choose a stream? It is based on the subject chosen by a student, right? So, in the upcoming example, you make the Student class `abstract`
- if you want component A to protect from component B, component B should depend on component A. But why do we give component A such importance? It is because we may want to put the most important rules in it.

### Initial Program

```java
// Student.java
class Student {
    String name;
    String regNumber;
    String department;
    double score;

    public Student(String name, String regNumber, double score, String dept) {
        this.name = name;
        this.regNumber = regNumber;
        this.score = score;
        this.department = dept;
    }

    @Override
    public String toString() {
        return ("Name: " + name + "\nReg Number: " + regNumber + "\nDept:" + department + "\nMarks:" + score + "\n*******");
    }
}
```

```java
// DistinctionDecider.java

import java.util.Arrays;
import java.util.List;

class DistinctionDecider {
    List<String> science = Arrays.asList("Comp.Sc.", "Physics");
    List<String> arts = Arrays.asList("History", "English");

    public void evaluateDistinction(Student student) {
        if (science.contains(student.department)) {
            if (student.score > 80) {
                System.out.println(student.regNumber + " has received a distinction in science.");
            }
        }
        if (arts.contains(student.department)) {
            if (student.score > 70) {
                System.out.println(student.regNumber + " has received a distinction in arts.");
            }
        }
    }
}
```

```java
// Client.java

import java.util.ArrayList;
import java.util.List;

class Client {
    public static void main(String[] args) {
        System.out.println("*** A demo without OCP.***");
        List<Student> enrolledStudents = enrollStudents();
        // Display all results.
        System.out.println("===Results:===");
        for (Student student : enrolledStudents) {
            System.out.println(student);
        }
        System.out.println("===Distinctions:===");
        DistinctionDecider distinctionDecider = new DistinctionDecider();
        // Evaluate distinctions.
        for (Student student : enrolledStudents) {
            distinctionDecider.evaluateDistinction(student);
        }
    }

    private static List<Student> enrollStudents() {
        Student sam = new Student("Sam", "R1", 81.5, "Comp.Sc.");
        Student bob = new Student("Bob", "R2", 72, "Physics");
        Student john = new Student("John", "R3", 71, "History");
        Student kate = new Student("Kate", "R4", 66.5, "English");
        List<Student> students = new ArrayList<Student>();
        students.add(sam);
        students.add(bob);
        students.add(john);
        students.add(kate);
        return students;
    }
}
```

### Better Program

```java
abstract class Student {
    String name;
    String regNumber;
    double score;
    String department;

    public Student(String name, String regNumber, double score) {
        this.name = name;
        this.regNumber = regNumber;
        this.score = score;
    }

    @Override
    public String toString() {
        return ("Name: " + name + "\nReg Number: " + regNumber + "\nDept:" + department + "\nMarks:" + score + "\n*******");
    }
}
```

```java
public class ArtsStudent extends Student {
    public ArtsStudent(String name, String regNumber, double score, String dept) {
        super(name, regNumber, score);
        this.department = dept;
    }
}
```

```java
public class ScienceStudent extends Student {
    public ScienceStudent(String name, String regNumber, double score, String dept) {
        super(name, regNumber, score);
        this.department = dept;
    }
}
```

```java
// DistinctionDecider.java
interface DistinctionDecider {
    void evaluateDistinction(Student student);
}
```

```java
// ScienceDistinctionDecider.java
class ScienceDistinctionDecider implements DistinctionDecider {
    @Override
    public void evaluateDistinction(Student student) {
        if (student.score > 80) {
            System.out.println(student.regNumber + " has received a distinction in science.");
        }
    }
}
```

```java
// ArtsDistinctionDecider.java
class ArtsDistinctionDecider implements DistinctionDecider {
    @Override
    public void evaluateDistinction(Student student) {
        if (student.score > 70) {
            System.out.println(student.regNumber + " has received a distinction in arts.");
        }
    }
}
```

```java
// Client.java

import java.util.ArrayList;
import java.util.List;

class Client {
    public static void main(String[] args) {
        System.out.println("*** A demo that follows the OCP. * **");
        List<Student> scienceStudents = enrollScienceStudents();
        List<Student> artsStudents = enrollArtsStudents();
        // Display all results.
        System.out.println("===Results:===");
        for (Student student : scienceStudents) {
            System.out.println(student);
        }
        for (Student student : artsStudents) {
            System.out.println(student);
        }
        // Evaluate distinctions.
        DistinctionDecider scienceDistinctionDecider = new ScienceDistinctionDecider();
        DistinctionDecider artsDistinctionDecider = new ArtsDistinctionDecider();
        System.out.println("===Distinctions:===");
        for (Student student : scienceStudents) {
            scienceDistinctionDecider.evaluateDistinction(student);
        }
        for (Student student : artsStudents) {
            artsDistinctionDecider.evaluateDistinction(student);
        }
    }

    private static List<Student> enrollScienceStudents() {
        Student sam = new ScienceStudent("Sam", "R1", 81.5, "Comp.Sc.");
        Student bob = new ScienceStudent("Bob", "R2", 72, "Physics");
        List<Student> scienceStudents = new ArrayList<Student>();
        scienceStudents.add(sam);
        scienceStudents.add(bob);
        return scienceStudents;
    }

    private static List<Student> enrollArtsStudents() {
        Student john = new ArtsStudent("John", "R3", 71, "History");
        Student kate = new ArtsStudent("Kate", "R4", 66.5, "English");
        List<Student> artsStudents = new ArrayList<Student>();
        artsStudents.add(john);
        artsStudents.add(kate);
        return artsStudents;
    }
}
```

## Liskov Substitution Principle

- **_The LSP says that you should be able to substitute a parent (or base)  type with a subtype_**. It means that in a program segment, you can use a derived class instead of its base class without altering the correctness of the program

### Initial Program

```java
// Payment.java
interface Payment {
    void previousPaymentInfo();

    void newPayment();
}
```

```java
// RegisteredUserPayment.java
class RegisteredUserPayment implements Payment {
    String name;

    public RegisteredUserPayment(String userName) {
        this.name = userName;
    }

    @Override
    public void previousPaymentInfo() {
        System.out.println("Retrieving " + name + "'s last payment details.");
    }

    @Override
    public void newPayment() {
        System.out.println("Processing " + name + "'s current payment request.");
    }
}
```

```java
class GuestUserPayment implements Payment {
    String name;

    public GuestUserPayment() {
        this.name = "guest";
    }

    @Override
    public void previousPaymentInfo() {
        throw new UnsupportedOperationException();
    }

    @Override
    public void newPayment() {
        System.out.println("Processing " + name + "'s current payment request.");
    }
}
```

```java
// PaymentHelper.java

import java.util.ArrayList;
import java.util.List;

public class PaymentHelper {
    List<Payment> payments = new ArrayList<Payment>();

    public void addUser(Payment user) {
        payments.add(user);
    }

    public void showPreviousPayments() {
        payments.forEach(payment::previousPaymentInfo);
        System.out.println("------");
    }

    public void processNewPayments() {
        payments.forEach(payment::newPayment);
        System.out.println("------");
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***A demo without LSP.***\n");
        PaymentHelper helper = new PaymentHelper();
        // Instantiating two registered users
        RegisteredUserPayment robinPayment = new RegisteredUserPayment("Robin");
        RegisteredUserPayment jackPayment = new RegisteredUserPayment("Jack");
        // Adding the users to the helper
        helper.addUser(robinPayment);
        helper.addUser(jackPayment);
        GuestUserPayment guestUser = new GuestUserPayment();
        helper.addUser(guestUser);//Error when invoking method on this one
        // Processing the payments using
        // the helper class.
        // You can see the problem now.
        helper.showPreviousPayments();
        helper.processNewPayments();
    }
}
```

### Better Program

```java
// PreviousPayment.java
interface PreviousPayment {
    void previousPaymentInfo();
}
```

```java
// NewPayment.java
interface NewPayment {
    void newPayment();
}
```

```java
// RegisteredUserPayment.java
class RegisteredUserPayment implements NewPayment, PreviousPayment {
    String name;

    public RegisteredUserPayment(String userName) {
        this.name = userName;
    }

    @Override
    public void previousPaymentInfo() {
        System.out.println("Retrieving " + name + "'s last payment details.");
    }

    @Override
    public void newPayment() {
        System.out.println("Processing " + name + "'s current payment request.");
    }
}
```

```java
// GuestUserPayment.java
class GuestUserPayment implements NewPayment {
    String name;

    public GuestUserPayment() {
        this.name = "guest";
    }

    @Override
    public void newPayment() {
        System.out.println("Processing " + name + "'s current payment request.");
    }
}
```

```java
// PaymentHelper.java

import java.util.ArrayList;
import java.util.List;

class PaymentHelper {
    List<PreviousPayment> previousPayments = new ArrayList<>();
    List<NewPayment> newPayments = new ArrayList<>();

    public void addPreviousPayment(PreviousPayment previousPayment) {
        previousPayments.add(previousPayment);
    }

    public void addNewPayment(NewPayment newPaymentRequest) {
        newPayments.add(newPaymentRequest);
    }

    public void showPreviousPayments() {
        previousPayments.forEach(payment::previousPaymentInfo ());
        System.out.println("------");
    }

    public void processNewPayments() {
        newPayments.forEach(NewPayment::newPayment ());
        System.out.println("***********");
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***A demo that follows the LSP. * **\n ");
        PaymentHelper helper = new PaymentHelper();
        // Instantiating two registered users.
        RegisteredUserPayment robin = new RegisteredUserPayment("Robin");
        RegisteredUserPayment jack = new RegisteredUserPayment("Jack");
        // Instantiating a guest user's payment.
        GuestUserPayment guestUser1 = new GuestUserPayment();
        // Consolidating the previous payment's info to
        // the helper.
        helper.addPreviousPayment(robin);
        helper.addPreviousPayment(jack);
        // Consolidating new payment requests to
        // the helper
        helper.addNewPayment(robin);
        helper.addNewPayment(jack);
        helper.addNewPayment(guestUser1);
        // Retrieve all the previous payments
        // of registered users.
        helper.showPreviousPayments();
        // Process all new payment requests
        // from all users.
        helper.processNewPayments();
    }
}
```

## Interface Segregation Principle

- You often see a fat interface that contains many methods. A class that implements the interface may not need all these methods. So why does the interface contain all these methods? One possible answer is to support some of the implementing classes of this interface. This is the area the Interface Segregation Principle focuses on
- The idea is that **_a client should not depend on a method that it does not use_**.
- Note the following points before you proceed further:
    - client means any class that uses another class (or interface).
    - The word “interface” of the interface segregation principle is not limited to a Java interface. the same concept applies to any supertype, such as an abstract class or a simple parent class
    - The ISP suggests that your class should not depend on interface methods that it does not use

### Initial Program

```java
// Printer.java
interface Printer {
    void printDocument();

    void sendFax();
}
```

```java
// BasicPrinter.java
class BasicPrinter implements Printer {
    @Override
    public void printDocument() {
        System.out.println("The basic printer prints a document.");
    }

    @Override
    public void sendFax() {
        throw new UnsupportedOperationException();
    }
}
```

```java
// AdvancedPrinter.java
class AdvancedPrinter implements Printer {
    @Override
    public void printDocument() {
        System.out.println("The advanced printer prints a document.");
    }

    @Override
    public void sendFax() {
        System.out.println("The advanced printer sends a fax.");
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***A demo without ISP.***");
        Printer printer = new AdvancedPrinter();
        printer.printDocument();
        printer.sendFax();
        printer = new BasicPrinter();
        printer.printDocument();
        // printer.sendFax(); // Will throw error
    }
}
```

### Better Program

```java
// Printer.java
interface Printer {
    void printDocument();
}
```

```java
// FaxDevice.java
interface FaxDevice {
    void sendFax();
}
```

```java
// BasicPrinter.java
class BasicPrinter implements Printer {
    @Override
    public void printDocument() {
        System.out.println("The basic printer prints a document.");
    }
}
```

```java
// AdvancedPrinter.java
class AdvancedPrinter implements Printer, FaxDevice {
    @Override
    public void printDocument() {
        System.out.println("The advanced printer prints a document.");
    }

    @Override
    public void sendFax() {
        System.out.println("The advanced printer sends a fax.");
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***A demo that follows ISP.***");
        Printer printer = new BasicPrinter();
        printer.printDocument();
        printer = new AdvancedPrinter();
        printer.printDocument();
        FaxDevice faxDevice = new AdvancedPrinter();
        faxDevice.sendFax();
    }
}
```

- There is an alternative technique to implementing the isp. this can be done using the “delegation” technique. the discussion of this is beyond the scope of this book. But remember the following point: delegations increase the runtime (it can be small, but it is nonzero for sure) of an application, which can affect the performance of the application. also, based on a particular design, a delegated call can create some additional objects. Unnecessary creation of objects can cause trouble for you because they occupy memory blocks. so, to make an application that should work properly using a very small amount of memory (such as a realtime embedded system), you need to be careful before you create an object

## Dependency Inversion Principle

- The DIP covers two important things:
    - A high-level concrete class should not depend on a low-level concrete class. Instead, both should depend on abstractions
    - Abstractions should not depend upon details. Instead, the details should depend upon abstractions
- It means you should avoid creating a concrete low-level class inside a high-level class. Instead, you should use abstract classes or interfaces. As a result, you remove the tight coupling between the classes
- The program is simple, but it suffers from the following issues:
    - The top-level class (UserInterface) has too much dependency on the bottom-level class (OracleDatabase). These two classes are tightly coupled. So, in the future, if the OracleDatabase class changes (for example, when you change the signature of the saveEmpIdInDatabase(...) method), you need to adjust the UserInterface class.
    - The low-level class should be available before you write the top-level class. So, you are forced to complete the low-level class before you write or test the high-level class
    - What if you use a different database? For example, you may want to switch from the Oracle database to a MySQL database, or you may need to support both

### Initial Program

```java
// UserInterface.java
class UserInterface {
    private OracleDatabase oracleDatabase;

    public UserInterface() {
        this.oracleDatabase = new OracleDatabase();
    }

    public void saveEmployeeId(String empId) {
        // Assuming that this is a valid data.
        // So, storing it in the database.
        oracleDatabase.saveEmpIdInDatabase(empId);
    }
}
```

```java
// OracleDatabase.java
class OracleDatabase {
    public void saveEmpIdInDatabase(String empId) {
        System.out.println("The id: " + empId + " is saved in the Oracle database.");
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***A demo without DIP.***");
        UserInterface userInterface = new UserInterface();
        userInterface.saveEmployeeId("E001");
    }
}
```

### Better Program

```java
// Database.java
interface Database {
    void saveEmpIdInDatabase(String empId);
}
```

```java
// OracleDatabase.java
class OracleDatabase implements Database {
    @Override
    public void saveEmpIdInDatabase(String empId) {
        System.out.println("The id: " + empId + " is saved in the Oracle database.");
    }
}
```

```java
// MySQLDatabase.java
class MySQLDatabase implements Database {
    @Override
    public void saveEmpIdInDatabase(String empId) {
        System.out.println("The id: " + empId + " is saved in the MySQL database.");
    }
}
```

```java
// UserInterface.java
class UserInterface {
    Database database;

    public UserInterface(Database database) {
        this.database = database;
    }

    public void saveEmployeeId(String empId) {
        database.saveEmpIdInDatabase(empId);
    }
}
```

```java
// Client.java
class Client {
    public static void main(String[] args) {
        System.out.println("***A demo that follows the DIP.***");
        // Using Oracle now
        Database database = new OracleDatabase();
        UserInterface userInterface = new UserInterface(database);
        userInterface.saveEmployeeId("E001");
        // Using MySQL now
        database = new MySQLDatabase();
        userInterface = new UserInterface(database);
        userInterface.saveEmployeeId("E002");
    }
}
```

## Summary

- the OCP as the most important object-oriented design principle. The OCP suggests that software entities (a class, module, method, etc.)
  should be open for extension, but closed for modification. The idea is if you do not touch running code, you do not break it. For the new features, you add new code but do not disturb the existing code. It helps you save time on retesting the entire workflow again; instead, you focus on the newly added code and test that part
- In many cases, when you violate the OCP, you break the SRP too.
- The idea of the LSP is you should be able to substitute a parent (or base) type with a subtype. It is your responsibility to write true polymorphic code using the LSP. Using this principle, you can avoid the long tail of if-else chains and make your code OCP compliant too.
- The idea behind the ISP is that a client should not depend on a method that it does not use. This is why you may need to split a fat interface into multiple interfaces to make a better solution
- The DIP suggests two important points. First, a high-level concrete class should not depend on a low-level concrete class. Instead, both should depend on abstractions. Second, the abstractions should not depend upon details. Instead, the details should depend upon abstractions