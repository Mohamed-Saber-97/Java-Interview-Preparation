<!-- TOC -->

* [Control Flow](#control-flow)
    * [The switch Statement](#the-switch-statement)
    * [The switch Expression](#the-switch-expression)
    * [The for(:) Statement](#the-for-statement)
    * [The return Statement](#the-return-statement)

<!-- TOC -->
--------

# Control Flow

## The switch Statement

* The selector expression must evaluate to a value whose type must be one of the following:
    * A primitive data type: `char`, `byte`, `short`, or `int`
    * A wrapper type: `Character`, `Byte`, `Short`, or `Integer`
    * An `enum` type
    * The type `String`
* The type of the selector expression cannot be boolean, long, or floating-point.
* The statements in the switch block can have case labels, where each case label specifies one or more case **_constants_**
* The `switch` block must be compatible with the type of the selector expression, otherwise a compile-time error occurs
* If no valid case labels are found and the default label is omitted, the whole switch statement is skipped
* The case constants (CC) in the case labels are constant expressions whose values must be unique, meaning no duplicate values are allowed
* In fact, a case constant must be a compile-time constant expression whose value is assignable to the type of the selector expression

```java
public class SeasonsII {
    public static void main(String[] args) {
        int monthNumber = 11;
        switch ( monthNumber ) {
            case 12, 1, 2 -> System.out.println("Snow in the winter.");
            case 3, 4, 5 -> System.out.println("Green grass in the spring.");
            case 6, 7, 8 -> System.out.println("Sunshine in the summer.");
            case 9, 10, 11 -> {
                switch ( monthNumber ) {
                    case 10 -> System.out.println("Halloween.");
                    case 11 -> System.out.println("Thanksgiving.");
                }
// Always printed for case constants 9, 10, 11:
                System.out.println("Yellow leaves in the fall.");
            }
            default -> throw new IllegalArgumentException(monthNumber + " is not a valid month.");
        }
    }
}
```

```java
public static final String MEDIUM = "Medium";
public static final String HOT = "Hot";
//the compiler cannot guarantee that the value of the reference will not change at runtime
public static String HOT = "Hot"; // Not OK as case label
//it cannot deduce the value at compile time, as the constructor must be run to construct the value.
public static final String HOT = new String("Hot"); // Not OK as case label
```

* Switching on enum values is essentially based on equality comparison of unique integer values that are ordinal values assigned by the compiler to the constants of an enum type.

## The switch Expression

* A switch expression evaluates to a value, as opposed to a switch statement that does not
* The switch expression with the colon notation must be exhaustive, meaning the case labels, and if necessary the default label, must cover all values of the selector expression type.
* Non-exhaustive switch expressions will result in a compile-time error. The default label is typically used to make the switch expression exhaustive

```java
case 1->"ONE";
        case 2->yield "two"; // Compile-time error!
```

```java
void test() {
    case ALARM -> {
        soundTheAlarm();
        callTheFireDepartment();
        yield Status.EVACUATE;
    } // OK
    case ALL_CLEAR -> {
        yield Status.NORMAL; // Compile-time error: not last statement in the block
        standDown();
    }
}
```

```java
public class SeasonsV {
  public static void main(String[] args) {
    int monthNumber = 11;
    Season season = switch ( monthNumber ) { // (2)
      case 12, 1, 2 -> Season.WINTER; // (3)
      case 3, 4, 5 -> Season.SPRING; // (4)
      case 6, 7, 8 -> Season.SUMMER; // (5)
      case 9, 10, 11 -> Season.FALL; // (6)
      default -> throw new IllegalArgumentException(monthNumber + " not a valid month.");
    };
    System.out.println(season);
  }

  enum Season {WINTER, SPRING, SUMMER, FALL} // (1)
}
```

* The switch expression can only evaluate to a single value. Multiple values can be returned by constructing an object with the required values and returning the object as a result of evaluating the
  switch expression. Record classes are particularly suited for this purpose

## The for(:) Statement

The `for(:)` loop has its limitations. Specifically, we cannot change element values,and this kind of loop does not provide any provision for positional access using an index. The `for(:)` loop only
increments by one and always in a forward direction.It does not allow iterations over several arrays simultaneously. Under such circumstances, the `for(;;)` loop can be more convenient.

## The return Statement

* A recommended best practice is to document the value returned by a method in a Javadoc comment using the @return tag.