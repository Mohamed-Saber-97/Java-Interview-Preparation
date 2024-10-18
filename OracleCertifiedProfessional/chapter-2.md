<!-- TOC -->

* [Basic Elements, Primitive Data Types, and Operators](#basic-elements-primitive-data-types-and-operators)
    * [Basic Language Elements](#basic-language-elements)
        * [Identifiers](#identifiers)
        * [Keywords](#keywords)
            * [Keywords in Java](#keywords-in-java)
            * [Contextual keywords](#contextual-keywords)
            * [Reserved Keywords Not Currently in Use](#reserved-keywords-not-currently-in-use)
            * [Reserved Literals in Java](#reserved-literals-in-java)
        * [Literals](#literals)
            * [Integer Literals](#integer-literals)
            * [Floating-Point Literals](#floating-point-literals)
            * [Underscores in Numerical Literals](#underscores-in-numerical-literals)
            * [Character Literals](#character-literals)
            * [String Literals](#string-literals)
        * [Comments](#comments)
    * [Primitive Data Types](#primitive-data-types)
        * [Summary of Primitive Data Types](#summary-of-primitive-data-types)
    * [Conversions](#conversions)
        * [Widening and Narrowing Primitive Conversions](#widening-and-narrowing-primitive-conversions)
        * [Widening and Narrowing Reference Conversions](#widening-and-narrowing-reference-conversions)
        * [Boxing and Unboxing Conversions](#boxing-and-unboxing-conversions)
    * [Type Conversion Contexts](#type-conversion-contexts)
        * [Assignment Context](#assignment-context)
        * [Method Invocation Context](#method-invocation-context)
        * [Casting Context of the Unary Type Cast Operator (type)](#casting-context-of-the-unary-type-cast-operator-type)
        * [Numeric Promotion Context](#numeric-promotion-context)
            * [Unary Numeric Promotion](#unary-numeric-promotion)
            * [Binary Numeric Promotion](#binary-numeric-promotion)
    * [Precedence and Associativity Rules for Operators](#precedence-and-associativity-rules-for-operators)
    * [Evaluation Order of Operands](#evaluation-order-of-operands)
        * [Left-Hand Operand Evaluation First](#left-hand-operand-evaluation-first)
        * [Operand Evaluation before Operation Execution](#operand-evaluation-before-operation-execution)
        * [Left-to-Right Evaluation of Argument Lists](#left-to-right-evaluation-of-argument-lists)
    * [The Simple Assignment Operator =](#the-simple-assignment-operator-)
        * [Assigning References](#assigning-references)
        * [Multiple Assignments](#multiple-assignments)
        * [Type Conversions in an Assignment Context](#type-conversions-in-an-assignment-context)
    * [Arithmetic Operators: *, /, %, +, -](#arithmetic-operators------)
        * [Range of Numeric Values](#range-of-numeric-values)
            * [Integer Arithmetic](#integer-arithmetic)
            * [Floating-Point Arithmetic](#floating-point-arithmetic)
            * [Remainder Operator: %](#remainder-operator-)
        * [Numeric Promotions in Arithmetic Expressions](#numeric-promotions-in-arithmetic-expressions)
        * [Arithmetic Compound Assignment Operators: *=, /=, %=, +=, -=](#arithmetic-compound-assignment-operators------)
    * [The Binary String Concatenation Operator +](#the-binary-string-concatenation-operator-)
    * [Variable Increment and Decrement Operators: ++, --](#variable-increment-and-decrement-operators----)
        * [The Increment Operator ++](#the-increment-operator-)
        * [The Decrement Operator --](#the-decrement-operator---)
    * [Relational Operators: <, <=, >, >=](#relational-operators----)
    * [Equality](#equality)
        * [Primitive Data Value Equality: ==, !=](#primitive-data-value-equality--)
        * [Object Reference Equality: ==, !=](#object-reference-equality--)
        * [Object Value Equality](#object-value-equality)
    * [The Conditional Operator ?:](#the-conditional-operator-)
    * [Other Operators: new, [], instanceof, ->](#other-operators-new--instanceof--)

<!-- TOC -->
-----------------------------

# Basic Elements, Primitive Data Types, and Operators

## Basic Language Elements

### Identifiers

- Java programs are written in the Unicode character set characters allowed in identifier names are interpreted according to this character set.
- As one would expect, the characters A to Z and a to z are letters and the characters 0 to 9 are digits
- A _connecting punctuation character_ (such as _underscore_ `_`) and any _currency symbol_ (such as `$`, `¢`, `¥`, or `£`) are also allowed as letters in identifier names
- Note also that the underscore (`_`) on its own is not a legal identifier name, but a keyword
- Examples of Legal Identifiers: `number, Number, sum_$, bingo, $$_100, _007, mål, grüß`
- Examples of Illegal Identifiers: `48chevy, all@hands, grand-sum, _`

### Keywords

- All Java keywords are lowercase, and incorrect usage results in compile-time errors

#### Keywords in Java

- A keyword cannot be used as an identifier.

|  keyword   |  keyword  | keyword  |  keyword   |     keyword      |  keyword   |  keyword  |   keyword    |    keyword     |   keyword   |
|:----------:|:---------:|:--------:|:----------:|:----------------:|:----------:|:---------:|:------------:|:--------------:|:-----------:|
| `abstract` | `default` |   `if`   | `private`  |      `this`      |  `assert`  |   `do`    | `implements` |  `protected`   |   `throw`   |
| `boolean`  | `double`  | `import` |  `public`  |     `throws`     |  `break`   |  `else`   | `instanceof` |    `return`    | `transient` |
|   `byte`   |  `enum`   |  `int`   |  `short`   |      `try`       |   `case`   | `extends` | `interface`  |    `static`    |   `void`    |
|  `catch`   |  `final`  |  `long`  | `strictfp` |    `volatile`    |   `char`   | `finally` |   `native`   |    `super`     |   `while`   |
|  `class`   |  `float`  |  `new`   |  `switch`  | `_` (underscore) | `continue` |   `for`   |  `package`   | `synchronized` |             |

#### Contextual keywords

- A contextual keyword cannot be used as an identifier in certain contexts

|  keyword  | keyword  |  keyword   |   keyword    | 
|:---------:|:--------:|:----------:|:------------:|
| `exports` | `opens`  | `requires` |    `uses`    |
| `permits` | `sealed` |   `var`    | `non-sealed` |
|   `to`    |  `with`  |   `open`   |   `record`   |
|  `yield`  | `module` | `provides` | `transitive` |

#### Reserved Keywords Not Currently in Use

| keyword |
|:-------:|
| `const` |
| `goto`  |

#### Reserved Literals in Java

| keyword |
|:-------:|
| `null`  |
| `true`  |
| `false` |

### Literals

- A literal denotes a constant value; in other words, the value that a literal represents remains unchanged in the program
- Literals represent numerical (integer or floating-point), character, boolean, and string values. In addition, the literal `null` represents the `null` reference.
    - Integer: `2000`, `0`, `-7`
    - Floating-point: `3.14`, `-3.14`, `.5`, `0.5`
    - Character: `'a'` `'A'` `'0'` `':'` `'-'` `')'`
    - Boolean: `true`, `false`
    - String: `"abba"` `"3.14"` `"for"` `"a piece of the action"`

#### Integer Literals

- Integer data types comprise the following primitive data types: `int`, `long`, `byte`, and `short`
- The default data type of integer literal is always `int`

#### Floating-Point Literals

- Floating-point data types come in two flavors: `float` and `double`
- The default data type of a floating-point literal is `double`

#### Underscores in Numerical Literals

- Any number of underscores can be inserted **_between_** the digits that make up the numerical literal

#### Character Literals

- A character literal is quoted in single quotes ('). All character literals have the primitive data type `char`
- A Unicode character can always be specified as a four-digit hexadecimal number (i.e., 16 bits) with the prefix `\u`
- We can also use the escape sequence \ddd to specify a character literal as an octal value
- The number of digits must be three or fewer, and the octal value cannot exceed `\377`
- Most used Character literals and their Unicode:
    - `'A' = '\u0041' corresponds to 65 in ASCII`
    - `'Z' = '\u005a' corresponds to 90 in ASCII`
    - `'a' = '\u0061' corresponds to 97 in ASCII`
    - `'z' = '\u007a' corresponds to 122 in ASCII`

#### String Literals

- A string literal is a sequence of characters that must be enclosed in double quotes and must occur on a single line.
- All string literals are objects of the class `String`

### Comments

- Java supports three types of comments: `//`, `/* */` and `/** */`
- The `javadoc` utility recognizes several tags, including those shown here:

|         Tag         |                                          Meaning                                          |
|:-------------------:|:-----------------------------------------------------------------------------------------:|
|      `@author`      |                                   Identifies the author                                   |
|      `{@code}`      |         Displays information as-is, without processing HTML styles, in code font.         |
|    `@deprecated`    |                      Specifies that a program element is deprecated.                      |
|    `{@docRoot}`     |          Specifies the path to the root directory of the current documentation.           |
|    `@exception`     |                Identifies an exception thrown by a method or constructor.                 |
|      `@hidden`      |                 Prevents an element from appearing in the documentation.                  |
|     `{@index}`      |                              Specifies a term for indexing.                               |
|   `{@inheritDoc}`   |                     Inherits a comment from the immediate superclass.                     |
|      `{@link}`      |                         Inserts an in-line link to another topic.                         |
|   `{@linkplain}`    | Inserts an in-line link to another topic, but the link is displayed in a plain-text font. |
|    `{@literal}`     |                Displays information as-is, without processing HTML styles.                |
|      `@param`       |                                  Documents a parameter.                                   |
|     `@provides`     |                         Documents a service provided by a module.                         |
|      `@return`      |                            Documents a method’s return value.                             |
|       `@see`        |                            Specifies a link to another topic.                             |
|      `@serial`      |                          Documents a default serializable field.                          |
|    `@serialData`    |      Documents the data written by the `writeObject()` or `writeExternal()` methods       |
|   `@serialField`    |                        Documents an `ObjectStreamField` component.                        |
|      `@since`       |                 States the release when a specific change was introduced.                 |
|    `{@summary}`     |                              Documents a summary of an item.                              |
| `{@systemProperty}` |                         States that a name is a system property.                          |
|      `@throws`      |                                   Same as `@exception`.                                   |
|       `@uses`       |                          Documents a service needed by a module                           |
|     `{@value}`      |             Displays the value of a constant, which must be a `static` field              |
|     `@version`      |                        Specifies the version of a program element                         |

```java
import java.io.*;

/**
 * This class demonstrates documentation comments.
 * @author Mohamed Saber
 * @version 1.2
 */
public class SquareNum {
    /**
     * This method demonstrates square().
     * @param args Unused.
     * @throws IOException On input error.
     * @see IOException
     */
    public static void main(String[] args)
            throws IOException {
        SquareNum ob = new SquareNum();
        double    val;
        System.out.println("Enter value to be squared: ");
        val = ob.getNumber();
        val = ob.square(val);
        System.out.println("Squared value is " + val);
    }
    
    /**
     * This method returns the square of num.
     * This is a multiline description. You can use as many lines as you like.
     * @param num The value to be squared.
     * @return num squared.
     */
    public double square(double num) {
        return num * num;
    }
    
    /**
     * This method inputs a number from the user.
     * @return The value input as a double.
     * @throws IOException On input error.
     * @see IOException
     */
    public double getNumber()
            throws IOException {
// create a BufferedReader using System.in
        InputStreamReader isr    = new InputStreamReader(System.in);
        BufferedReader    inData = new BufferedReader(isr);
        String            str;
        str = inData.readLine();
        return ( new Double(str) ).doubleValue();
    }
}
```

## Primitive Data Types

- Primitive data types in Java can be divided into three main categories:
    - _Integral types_: represent signed integers (`byte`, `short`, `int`, `long`) and unsigned character values (`char`)
    - _Floating-point types_: (`float`, `double`): represent fractional signed numbers
    - _Boolean type_ (`boolean`): represents logical values

### Summary of Primitive Data Types

| Data type |  Width (bits)  |             Minimum value, maximum value              | Wrapper class |
|:---------:|:--------------:|:-----------------------------------------------------:|:-------------:|
| `boolean` | Not applicable |                      true, false                      |   `Boolean`   |
|  `byte`   |       8        |           –2<sup>7</sup>, 2<sup>7</sup> – 1           |    `Byte`     |
|  `short`  |       16       |           –2<sup>15</sup>, 2<sup>15</sup>-1           |    `Short`    |
|  `char`   |       16       |                      0x0, 0xfff                       |  `Character`  |
|   `int`   |       32       |           –2<sup>31</sup>, 2<sup>31</sup>-1           |   `Integer`   |
|  `long`   |       64       |           –2<sup>63</sup>, 2<sup>63</sup>-1           |    `Long`     |
|  `float`  |       32       | ±1.40129846432481707e-45f, ±3.402823476638528860e+38f |    `Float`    |
| `double`  |       64       | ±4.94065645841246544e-324, ±1.79769313486231570e+308  |   `Double`    |

## Conversions

### Widening and Narrowing Primitive Conversions

- _widening primitive conversion_ is when the value of a _narrower_ data type can be converted to a value of a _wider_ data type
- all conversions between `char` and the two integer types `byte` and `short` are considered _narrowing primitive conversions_
- narrowing conversions are done in two steps: first converting the source value to the `int` type, and then converting the `int` value to the target type
- Regardless of any loss of magnitude or precision, widening and narrowing primitive conversions **_never_** result in a runtime exception

### Widening and Narrowing Reference Conversions

- Conversions _up_ the _type hierarchy_ are called _widening reference conversions_ (also called _upcasting_). Such a conversion converts from a subtype to a supertype:
  `Object obj = "Upcast me"; // (1) Widening: Object <----- String` Conversions _down_ the type hierarchy represent _narrowing reference conversions_ (also called _down casting_):
  `String str = (String) obj; // (2) Narrowing requires cast: String <----- Object`
- The compiler will reject casts that are not legal or will issue an _unchecked warning_ under certain circumstances if type-safety cannot be guaranteed
- Widening reference conversions do not require any runtime checks and never result in an exception during execution. This is not the case for narrowing reference conversions, which require a runtime
  check and can throw a `ClassCastException` if the conversion is not legal.

### Boxing and Unboxing Conversions

```java
Integer iRef = 10; // (1) Implicit boxing: Integer <----- int
Double dRef = Double.valueOf(3.14); // (2) Explicit boxing: Double <----- double
int i = iRef; // (3) Implicit unboxing: int <----- Integer
double d = dRef.doubleValue(); // (4) Explicit unboxing: double <----- Double
```

- Unboxing a wrapper reference that has the `null` value results in a `NullPointerException`

## Type Conversion Contexts

### Assignment Context

- `byte b = 10; // Narrowing conversion: byte <--- int`

### Method Invocation Context

-Method invocation conversions do not include the implicit narrowing conversion performed for _non-long_ integral constant expressions.

```java
// Assignment: (1) Implicit narrowing followed by (2) boxing.
Character space1 = 32; // Character <-(2)-- char <-(1)-- int

// Invocation of method with signature: valueOf(char)

// Call signature: valueOf(int)
Character space2 = Character.valueOf(32); // Compile-time error!
// Call signature: valueOf(char)
Character space3 = Character.valueOf(( char ) 32); // OK!
```

### Casting Context of the Unary Type Cast Operator (type)

- Java, being a _strongly_ typed language, checks for _type compatibility_ (i.e., it checks whether a type can substitute for another type in a given context) at compile time. However, some checks are
  possible only at runtime (e.g., which type of object a reference actually denotes during execution).
- The _cast operator_ (_type_) is applied to the value of the _expression_. At runtime, a cast results in a new value of _type_, which best represents the value of the _expression_ in the old type.

```java
long l = ( long ) 10; // Widening primitive conversion: long <--- int
int i = ( int ) l; // (1) Narrowing primitive conversion: int <--- long
Integer iRef = ( Integer ) i; // Boxing: Integer <--- int
Object obj = ( Object ) "7Up"; // Widening ref conversion: Object <--- String
String str = ( String ) obj; // (2) Narrowing ref conversion: String <--- Object
i =(int)iRef; // Unboxing: int <--- Integer
```

- Casting between primitive data types and reference types is not permitted, except where boxing and unboxing is applicable.
- Boolean values cannot be cast to other data values, and vice versa.
- The reference literal `null` can be cast to any reference type

### Numeric Promotion Context

#### Unary Numeric Promotion

- Unary numeric promotion proceeds as follows:
    - the single operand is of type `Byte`, `Short`, `Character`, or `Integer`, it is unboxed. If the resulting value is narrower than `int`, it is promoted to a value of type `int` by a widening
      conversion.
    - Otherwise, if the single operand is of type `Long`, `Float`, or `Double`, it is unboxed
    - Otherwise, if the single operand is of a type narrower than `int`, its value is promoted to a value of type `int` by a widening conversion
    - Otherwise, the operand remains unchanged.
- In other words, _**unary numeric promotion results in an operand value that is either `int` or wider**_.
- Unary numeric promotion is applied in the following expressions:
    - Operand of the unary arithmetic operators `+` and `-`
    - Array creation expression; for example, `new int[20]`, where the dimension expression (in this case, 20) must evaluate to an `int` value
    - Indexing array elements; for example, `objArray['a']`, where the index expression (in this case, 'a') must evaluate to an `int` value

#### Binary Numeric Promotion

- Binary numeric promotion implicitly applies appropriate widening primitive conversions so that the widest numeric type of a pair of operands is always at least `int`
- If `T` is wider than int, both operands are converted to `T`; otherwise, both operands are converted to `int`
- This means that **_the resulting type of the operands is at least `int`_**
- Binary numeric promotion is applied in the following expressions:
    - Operands of the arithmetic operators `*`, `/`, `%`, `+`, and `-`
    - Operands of the relational operators `<`, `<=`, `>`, and `>=`
    - Operands of the numerical equality operators `==` and `!=`
    - Operands of the conditional operator `? :`, under certain circumstances

## Precedence and Associativity Rules for Operators

- The following remarks apply to the table:
    - Operators are shown with decreasing precedence.
    - Operators within the same row have the same precedence.
    - Parentheses `()` can override precedence and associativity.
    - Unary operators require one operand.
    - **_All binary operators, except for relational and assignment operators, associate from left to right. Relational operators are non-associative._**
    - Unary postfix increment/decrement, unary operators, assignment operators, and the ternary conditional operator associate from right to left.

|                          Type                          |                       Operator                        |  Associativity  |
|:------------------------------------------------------:|:-----------------------------------------------------:|:---------------:|
| Array element access, member access, method invocation |                 [expression] . (args)                 |  Left to Right  |
|                Unary postfix operators                 |               expression++ expression--               |  Left to Right  |
|                 Unary prefix operators                 | ~ ! ++expression --expression +expression -expression |  Right to Left  |
|             Unary prefix creation and cast             |                      new (type)                       |  Right to Left  |
|                     Multiplicative                     |                         * / %                         |  Left to Right  |
|                        Additive                        |                          + -                          |  Left to Right  |
|                         Shift                          |                       << >> >>>                       |  Left to Right  |
|                       Relational                       |                 < <= > >= instanceof                  | Non-Associative |
|                        Equality                        |                         == !=                         |  Left to Right  |
|                  Bitwise/logical AND                   |                           &                           |  Left to Right  |
|                  Bitwise/logical XOR                   |                           ^                           |  Left to Right  |
|                   Bitwise/logical OR                   |                          \|                           |  Left to Right  |
|                    Conditional AND                     |                          &&                           |  Left to Right  |
|                     Conditional OR                     |                         \|\|                          |  Left to Right  |
|                      Conditional                       |                          ?:                           |  Right to Left  |
|                     Arrow operator                     |                          ->                           |  Right to Left  |
|                       Assignment                       |        = += -= *= /= %= <<= >>= >>>= &= ^= \|=        |  Right to Left  |

## Evaluation Order of Operands

### Left-Hand Operand Evaluation First

- The left-hand operand of a binary operator is fully evaluated before the right-hand operand is evaluated
- If evaluation of the left-hand operand of a binary operator throws an exception , we cannot rely on the presumption that the right-hand operand has been evaluated

```java
void test() {
    int b = 10;
    System.out.println(( b = 3 ) + b);
}
/*
(b=3) + b
    3 + b           b is assigned the value 3
    3 + 3
    6
 */
```

### Operand Evaluation before Operation Execution

- Java guarantees that all operands of an operator are fully evaluated before the actual operation is performed. This rule does not apply to the short-circuit conditional operators `&&`, `||`, and
  `?:`.

### Left-to-Right Evaluation of Argument Lists

- In a method or constructor invocation, each argument expression in the argument list is fully evaluated before any argument expression to its right

## The Simple Assignment Operator =

- `variable = expression`
    - The target _variable_ and the source _expression_ must be assignment compatible
    - The target variable must also have been declared.
    - variables can store either primitive values or reference values, _expression_ evaluates to either a primitive value or a reference value

### Assigning References

- Copying reference values by assignment creates _aliases_

### Multiple Assignments

- Multiple assignments are equally valid with references
- `int v1 = v2 = 2016; // Only v1 is declared. Compile-time error!`

### Type Conversions in an Assignment Context

- _implicit narrowing_ primitive conversions on assignment can occur in cases where _all_ the following conditions are fulfilled:
    - The source is a _**constant**_ expression of type `byte`, `short`, `char`, or `int`
    - The target type is of type `byte`, `short`, or `char`
    - The value of the source is determined to be in the range of the target type at compile time
- A _constant expression_ is an expression that denotes either a primitive or a `String` literal; it is composed of operands that can be only _literals_ or _constant variables_, and operators that can
  be
  evaluated only at compile time (e.g., arithmetic and numerical comparison operators, but not increment/decrement operators and method calls).
- A _constant variable_ is a `final` variable of either a primitive type or the String type that is initialized with a constant expression
- Here are some examples that illustrate how the conditions mentioned previously affect narrowing primitive conversions:

```java
void test() {
    int        result     = 100; // Not a constant variable. Not declared final.
    final char finalGrade = 'A'; // Constant variable. ’A’
    System.out.printf("%d%n%s%n%d%n%.2f%n%b%n%d%n%d%n",
                      2022, // Constant expression. 2022
                      "Trust " + "me!", // Constant expression. "Trust me"
                      2 + 3 * 4, // Constant expression. 14
                      Math.PI * Math.PI * 10.0, // Constant expression. 98.70
                      finalGrade == 'A', // Constant expression. true
                      Math.min(2020, 2021), // Not constant expression. Method call.
                      ++result // Not constant expression. Increment operator.
                     );
    
    short     s1 = 10; // int value in range.
    short     s2 = 'a'; // char value in range.
    char      c1 = 32; // int value in range.
    char      c2 = ( byte ) 35; // byte value in range. (int value in range, without cast.)
    byte      b1 = 40; // int value in range.
    byte      b2 = ( short ) 40; // short value in range. (int value in range, without cast.)
    final int i1 = 20; // Constant variable
    byte      b3 = i1; // final value of i1 in range.
}
```

- All other narrowing primitive conversions will produce a compile-time error on assignment and will explicitly require a cast. Here are some examples:

```java
final int i4 = 200; // i4 is a constant variable.
final int i5; // i5 is not a constant variable.
// Conditions not fulfilled for implicit narrowing primitive conversions.
// A cast is required.
int i2 = -20; // i2 is not a constant variable. i2 is not final.
final int i3 = i2; // i3 is not a constant variable, since i2 is not.
char c3 = ( char ) i3; // Final value of i3 not determinable at compile time.
short s3 = ( short ) i2; // Not constant expression.
char c4 = ( char ) i2; // Not constant expression.
byte b4 = ( byte ) 128; // int value not in range.
byte b5 = ( byte ) i4; // Value of constant variable i4 is not in range.
i5 =100; // Initialized at runtime.
short s4 = ( short ) i5; // Final value of i5 not determinable at compile time.
```

- Floating-point values are truncated when cast to integral values.

```java
// The value is truncated to fit the size of the target type.
float huge = ( float ) 1.7976931348623157d; // double to float.
long giant = ( long ) 4415961481999.03D; // (1) double to long.
int big = ( int ) giant; // (2) long to int.
short small = ( short ) big; // (3) int to short.
byte tiny = ( byte ) small; // (4) short to byte.
char symbol = ( char ) 112.5F; // (5) float to char.
```

- The discussion of numeric assignment conversions also applies to numeric parameter values at method invocation, except for the narrowing conversions, which always require a cast
- The following examples illustrate boxing and unboxing in an assignment context:

```java
Boolean boolRef = true; // Boxing.
Byte bRef = 2; // Constant in range: narrowing, then boxing.
// Byte bRef2 = 257; // Constant not in range. Compile-time error!
short s = 10; // Narrowing from int to short.
// Integer iRef1 = s; // short not assignable to Integer.
Integer iRef3 = ( int ) s; // Explicit widening with cast to int and boxing
boolean bv1 = boolRef; // Unboxing.
byte b1 = bRef; // Unboxing.
int iVal = bRef; // Unboxing and widening.
Integer iRefVal = null; // Always allowed.
// int j = iRefVal; // NullPointerException at runtime.
if(iRef3 !=null)iVal =iRef3; // Avoids exception at runtime.
```

## Arithmetic Operators: *, /, %, +, -

### Range of Numeric Values

#### Integer Arithmetic

- Integer arithmetic always returns a value that is in range, except in the case of **_integer division by zero_** and **_remainder by zero_**, which cause an `ArithmeticException`

#### Floating-Point Arithmetic

- adding or multiplying two very large floating-point numbers can result in an out-of-range value that is represented by **_infinity_**
- Attempting float- ing-point division by zero also returns **_infinity_** `System.out.println( 4.0 / 0.0); // Prints: Infinity` or `System.out.println(-4.0 / 0.0); // Prints: -Infinity`
- Floating-point arithmetic can also result in **_underflow_** to zero
- Negative zero compares equal to positive zero; in other words, `(-0.0 == 0.0)` is `true`.
- calculating the **_square root of –1_** or **_(floating-point) dividing zero by zero_** results in a `NaN`
- Any operation involving `NaN` produces `NaN`. Any comparison (except inequality, !=) involving `NaN` and any other value  (including `NaN`) returns `false`.
- An inequality comparison of `NaN` with another value (including `NaN`) always returns `true`

#### Remainder Operator: %

- A shortcut to evaluating the remainder involving negative operands is the following: Ignore the signs of the operands, calculate the remainder, and negate the remainder if the dividend is negative
- An `ArithmeticException` is thrown if the divisor evaluates to zero

```java
int r0 = 7 % 7; // 0
int r1 = 7 % 5; // 2
long r2 = 7L % -5L; // 2L
int r3 = -7 % 5; // -2
long r4 = -7L % -5L; // -2L
boolean relation = -7L == ( -7L / -5L ) * -5L + r4; // true

double dr0 = 7.0 % 7.0; // 0.0
float fr1 = 7.0F % 5.0F; // 2.0F
double dr1 = 7.0 % -5.0; // 2.0
float fr2 = -7.0F % 5.0F; // -2.0F
double dr2 = -7.0 % -5.0; // -2.0
boolean fpRelation = dr2 == ( -7.0 ) - ( -5.0 ) * ( long ) ( -7.0 / -5.0 ); // true
float fr3 = -7.0F % 0.0F; // NaN
```

### Numeric Promotions in Arithmetic Expressions

```java
Byte b = 10; // Constant in range: narrowing and boxing on assignment.
Short s = 20; // Constant in range: narrowing and boxing on assignment.
char c = 'z'; // 122 (\u007a)
int i = s * b; // Values in s and b promoted to int: unboxing, widening.
long n = 20L + s; // Value in s promoted to long: unboxing, widening.
// value in c are promoted to int, followed by implicit
// widening conversion of int to float on assignment.
float r = s + c; // Value in s is unboxed. This short value and the char
// widening conversion of float to double on assignment.
double d = r + i; // Value in i promoted to float, followed by implicit
```

### Arithmetic Compound Assignment Operators: *=, /=, %=, +=, -=

- `type variable op= expression` = `variable = (type) ((variable) op (expression))`

## The Binary String Concatenation Operator +

- Creation of temporary `String` objects might be necessary to store the results of performing successive string concatenations in a `String`-valued expression
- For a `String`-valued **_constant expression_** ((1), (5), (6), and (7)), the compiler computes such an expression at compile time, and the result is treated as a string literal in the program.
- The compiler uses a **_string builder_** to avoid the overhead of temporary `String` objects when applying the string concatenation operator (+) in `String`-valued non-constant expressions ((2), (
  3), and (
  4)),

```java
void test() {
    String strVal  = "" + 2020; // (1) "2020"
    String theName = " Uranium";
    theName = " Pure" + theName; // (2) " Pure Uranium"
    String trademark1 = 100 + "%" + theName; // (3) "100% Pure Uranium"
    String trademark2 = 100 + '%' + theName; // (4) "137 Pure Uranium"
    System.out.println("We can put two and two together and get " + 2 + 2); // (5)
    System.out.println("We can put two and two together and get " + ( 2 + 2 )); // (6)
    System.out.println("2 * 2 = " + 2 * 2); // (7) 2 * 2 = 4
}
```

## Variable Increment and Decrement Operators: ++, --

### The Increment Operator ++

- The prefix increment operator has the following semantics: `++i` adds 1 to the value in `i`, and stores the new value in `i`. It returns the new value as the value of the expression

```java
void test() {
    i += 1;
    return i;
}
```

- The postfix increment operator has the following semantics: `j++` adds 1 to the value in `j`, and stores the new value in `j`. It returns the original value that was in `j` as the value of the
  expression

```java
void test() {
    result = j;
    j += 1;
    return result;
}
```

### The Decrement Operator --

- The prefix decrement operator has the following semantics: `--i` subtracts 1 from the value of `i`, and stores the new value in `i`. It returns the new value as the value of the expression

```java
void test() {
    i -= 1;
    return i;
}
```

- The postfix decrement operator has the following semantics: `j--` subtracts 1 from the value of `j`, and stores the new value in `j`. It returns the original value that was in `j` as the value of
  the expression

```java
void test() {
    result = j;
    j -= 1;
    return result;
}
```

## Relational Operators: <, <=, >, >=

- All relational operators are binary operators and their operands are numeric expressions.
- Binary numeric promotion is applied to the operands of these operators
- The evaluation results in a `boolean` value
- Relational operators are non-associative. Mathematical expressions like `a ≤ b ≤ c` must be written using relational and boolean logical/conditional operators.

```java
void test() {
    double  hours          = 45.5;
    Double  time           = 18.0; // Boxing of double value.
    boolean overtime       = hours >= 35; // true. Binary numeric promotion: double <-- int.
    boolean beforeMidnight = time < 24.0;// true. Unboxing of value in time reference.
    char    letterA        = 'A';
    boolean order          = letterA < 'a'; // true. Binary numeric promotion: int <-- char.
    
    int     a       = 1, b = 7, c = 10;
    boolean illegal = a <= b <= c; // (1) Illegal. Compile-time error!
    boolean valid2  = a <= b && b <= c; // (2) OK.
}
```

## Equality

### Primitive Data Value Equality: ==, !=

```java
void test() {
    int     year    = 2002;
    boolean isEven  = year % 2 == 0; // true.
    boolean compare = '1' == 1; // false. Binary numeric promotion applied.
    boolean test    = compare == false; // true.
    
    int a, b, c;
    a = b = c = 5;
    boolean illegal = a == b == c; // (1) Illegal.
    boolean valid2  = a == b && b == c; // (2) Legal.
    boolean valid3  = a == b == true; // (3) Legal.
}
```

### Object Reference Equality: ==, !=

- Note that only when the type of **_both_** operands is either a reference type or the `null` type do these operators test for object reference equality. Otherwise, they test for primitive data
  equality.

```java
void test() {
    Pizza   pizzaA = new Pizza("Sweet&Sour"); // new object
    Pizza   pizzaB = new Pizza("Sweet&Sour"); // new object
    Pizza   pizzaC = new Pizza("Hot&Spicy"); // new object
    String  banner = "Come and get it!"; // new object
    boolean test   = banner == pizzaA; // (1) Compile-time error
    boolean test1  = pizzaA == pizzaB; // false
    boolean test2  = pizzaA == pizzaC; // false
    pizzaA = pizzaB; // Denote the same object; are aliases
    boolean test3 = pizzaA == pizzaB; // true
    
    Integer iRef = 10;
    boolean b1   = iRef == null; // Object reference equality
    boolean b2   = iRef == 10; // (1) Primitive data value equality
    boolean b3   = null == 10; // Compile-time error!
}
```

### Object Value Equality

- The `Object` class provides the method `public boolean equals(Object obj)`, which can be **_overridden_** to give the right semantics of **_object value equality_**
- The default implementation of this method in the `Object` class returns `true` only if the object is compared with itself, as if the equality operator `==` had been used to compare **_aliases_** of
  an object
- Certain classes in the Java SE API override the `equals()` method, such as `java.lang.String` and the **_wrapper_** classes for the primitive data types
- For two `String` objects, value equality means they contain **_identical character sequences_**
- For the wrapper classes, value equality means the wrapper objects have the **_same primitive value_** and are of the **_same wrapper type_**

## The Conditional Operator ?:

- The conditional expression is not an expression statement. The following code will not compile: `(i < j) ? i : j; // Compile-time error!`

## Other Operators: new, [], instanceof, ->

```java
Pizza myPizza = new Pizza();
boolean test1 = myPizza instanceof Pizza; // true.
boolean test2 = "Pizza" instanceof Pizza; // Compile-time error. Incompatible operand types.
boolean test3 = null instanceof Pizza; // Always false. null is not an instance.
```