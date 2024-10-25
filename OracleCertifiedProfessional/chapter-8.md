* [Selected API Classes](#selected-api-classes)
    * [Overview of the `java.lang` Package](#overview-of-the-javalang-package)
        * [Partial Inheritance Hierarchy in the `java.lang` Package](#partial-inheritance-hierarchy-in-the-javalang-package)
        * [Common functionality](#common-functionality)
    * [The `Object` Class](#the-object-class)
    * [The Wrapper Classes](#the-wrapper-classes)
        * [Converting Values among Primitive, Wrapper, and String Types](#converting-values-among-primitive-wrapper-and-string-types)
        * [Common Wrapper Class Utility Methods](#common-wrapper-class-utility-methods)
            * [Converting Primitive Values to Wrapper Objects](#converting-primitive-values-to-wrapper-objects)
            * [Converting Strings to Wrapper Objects](#converting-strings-to-wrapper-objects)
            * [Converting Wrapper Objects to Strings](#converting-wrapper-objects-to-strings)
            * [Converting Primitive Values to Strings](#converting-primitive-values-to-strings)
            * [Converting Wrapper Objects to Primitive Values](#converting-wrapper-objects-to-primitive-values)
            * [Wrapper Comparison, Equality, and Hash Code](#wrapper-comparison-equality-and-hash-code)
        * [Numeric Wrapper Classes](#numeric-wrapper-classes)
            * [Converting Numeric Wrapper Objects to Numeric Primitive Types](#converting-numeric-wrapper-objects-to-numeric-primitive-types)
            * [Converting Strings to Numeric Values](#converting-strings-to-numeric-values)
            * [Converting `Integer` and `Long` Values to Strings in Different Notations](#converting-integer-and-long-values-to-strings-in-different-notations)
        * [The Character Class](#the-character-class)
        * [The Boolean Class](#the-boolean-class)
        * [Converting Strings to Boolean Values](#converting-strings-to-boolean-values)
    * [The String Class](#the-string-class)
        * [Internal Representation of Strings](#internal-representation-of-strings)
        * [Creating and Initializing Strings](#creating-and-initializing-strings)
            * [Immutability](#immutability)
            * [String Internment](#string-internment)
            * [String Constructors](#string-constructors)
        * [The `CharSequence` Interface](#the-charsequence-interface)
        * [Reading Characters from a `String`](#reading-characters-from-a-string)
        * [Reading Lines from a String](#reading-lines-from-a-string)
        * [Comparing Strings](#comparing-strings)
        * [Character Case in a String](#character-case-in-a-string)
        * [Concatenation of Strings](#concatenation-of-strings)
            * [Repeating Strings](#repeating-strings)
        * [Joining of `CharSequence` Objects](#joining-of-charsequence-objects)
        * [Searching for Characters and Substrings in Strings](#searching-for-characters-and-substrings-in-strings)
        * [Extracting Substrings from Strings](#extracting-substrings-from-strings)
        * [Converting Primitive Values and Objects to Strings](#converting-primitive-values-and-objects-to-strings)
        * [Transforming a String](#transforming-a-string)
        * [Indenting Lines in a String](#indenting-lines-in-a-string)
        * [Formatted Strings](#formatted-strings)
        * [Text Blocks](#text-blocks)
            * [Basic Text Blocks](#basic-text-blocks)
            * [Using Escape Sequences in Text Blocks](#using-escape-sequences-in-text-blocks)
            * [Incidental Whitespace](#incidental-whitespace)
    * [The `StringBuilder` Class](#the-stringbuilder-class)
        * [Constructing String Builders](#constructing-string-builders)
            * [String Builder Constructors](#string-builder-constructors)
        * [Searching for Substrings in String Builders](#searching-for-substrings-in-string-builders)
        * [Extracting Substrings from String Builders](#extracting-substrings-from-string-builders)
        * [Constructing Strings from String Builders](#constructing-strings-from-string-builders)
        * [Comparing String Builders](#comparing-string-builders)
        * [Modifying String Builders](#modifying-string-builders)
            * [Appending Characters to a String Builder](#appending-characters-to-a-string-builder)
            * [Inserting Characters in a String Builder](#inserting-characters-in-a-string-builder)
            * [Replacing Characters in a String Builder](#replacing-characters-in-a-string-builder)
            * [Deleting Characters in a String Builder](#deleting-characters-in-a-string-builder)
            * [Reversing Characters in a String Builder](#reversing-characters-in-a-string-builder)
        * [Controlling String Builder Capacity](#controlling-string-builder-capacity)
    * [The Math Class](#the-math-class)
        * [Miscellaneous Rounding Functions](#miscellaneous-rounding-functions)
        * [Exponential Functions](#exponential-functions)
        * [Exact Integer Arithmetic Functions](#exact-integer-arithmetic-functions)
        * [Pseudorandom Number Generator](#pseudorandom-number-generator)
    * [The Random Class](#the-random-class)
        * [Determining the Range](#determining-the-range)
        * [Generating the Same Sequence of Pseudorandom Numbers](#generating-the-same-sequence-of-pseudorandom-numbers)
    * [Using Big Numbers](#using-big-numbers)
        * [BigDecimal Constants](#bigdecimal-constants)
        * [Constructing BigDecimal Numbers](#constructing-bigdecimal-numbers)
        * [Computing with BigDecimal Numbers](#computing-with-bigdecimal-numbers)

-----

# Selected API Classes

## Overview of the `java.lang` Package

- The `java.lang` package is indispensable when programming in Java. It is automatically imported into every source file at compile time
- The package contains:
    - `Object` class that is the superclass of all classes
    - the wrapper classes
    - classes essential for interacting with the JVM (`Runtime`)
    - security (`SecurityManager`)
    - loading classes (`ClassLoader`)
    - dealing with threads (`Thread`)
    - handling exceptions (`Throwable`, `Error`, `Exception`, `RuntimeException`)
    - standard input, output, and error streams (`System`)
    - string handling (`String`, `StringBuilder`)
    - mathematical functions (`Math`)

### Partial Inheritance Hierarchy in the `java.lang` Package

![](/images/PartialInheritanceHierarchy.png)

### Common functionality

## The `Object` Class

- `public boolean equals(Object obj);`
    - returns true only if the two references compared denote the same object. Normally overridden to provide the semantics of object value equality, as is the case for the wrapper classes and the `String` class.
- `public int hashCode()`
    - Returns an `int` value as the _hash value_ of the object. Hash codes are different for different objects yet `Strings` that are equal have the same hash code
- `public String toString()`
    - return a text representation of the object. If a subclass does not override this method **_<class_name>@<hash_code>_** is returned
- `public final Class<?> getClass()`
    - Returns the _runtime_ class of the object, which is represented by an object of the class `java.lang.Class` at runtime
- `protected Object clone() throws CloneNotSupportedException`
    - provides **_shallow copying_** . the subclass must implement the `Cloneable` marker interface to indicate that its objects can be safely cloned. Otherwise, the `clone()` method in the Object class will throw a checked `CloneNotSupportedException`.
- `@Deprecated(since="9") protected void finalize() throws Throwable`
    - This method is called on an object just before it is garbage collected so that any clean-up can be done
- ```java
  public final void wait(long timeout) throws InterruptedException
  public final void wait(long timeout, int nanos) throws InterruptedException
  public final void wait() throws InterruptedException
  public final void notify()
  public final void notifyAll()
  ```

```java
public class ObjectMethods {
    public static void main(String[] args) {
        // Two objects of MyClass.
        MyClass obj1 = new MyClass();
        MyClass obj2 = new MyClass();
        // Two strings.
        String str1 = new String("WhoAmI");
        String str2 = new String("WhoAmI");
        System.out.println("str1: " + str1.toString());//str1: WhoAmI
        System.out.println("str2: " + str2.toString() + "\n");//str2: WhoAmI
        // Method hashCode() overridden in String class.
        // Strings that are equal have the same hash code.
        System.out.println("hash code for str1: " + str1.hashCode());//hash code for str1: -1704812257
        System.out.println("hash code for str2: " + str2.hashCode() + "\n");//hash code for str2: -1704812257
        // Hash codes are different for different MyClass objects.
        System.out.println("hash code for MyClass obj1: " + obj1.hashCode());//hash code for MyClass obj1: 2036368507
        System.out.println("hash code for MyClass obj2: " + obj2.hashCode() + "\n");//hash code for MyClass obj2: 1785210046
        // Method equals() overridden in the String class.
        System.out.println("str1.equals(str2): " + str1.equals(str2));//str1.equals(str2): true
        System.out.println("str1 == str2: " + (str1 == str2) + "\n");//str1 == str2: false
        // Method equals() from the Object class called.
        System.out.println("obj1.equals(obj2): " + obj1.equals(obj2));//obj1.equals(obj2): false
        System.out.println("obj1 == obj2: " + (obj1 == obj2) + "\n");//obj1 == obj2: false
        // The runtime object that represents the class of an object.
        Class<? extends String> rtStringClass = str1.getClass();//Class for str1: class java.lang.String
        Class<? extends MyClass> rtMyClassClass = obj1.getClass();//Class for obj1: class MyClass
        // The name of the class represented by the runtime object.
        System.out.println("Class for str1: " + rtStringClass);//Text representation of str1: WhoAmI
        System.out.println("Class for obj1: " + rtMyClassClass + "\n");//Text representation of obj1: MyClass@7960847b
        // The toString() method is overridden in the String class.
        String textRepStr = str1.toString();
        String textRepObj = obj1.toString();
        System.out.println("Text representation of str1: " + textRepStr);
        System.out.println("Text representation of obj1: " + textRepObj + "\n");
        // Shallow copying of arrays.
        MyClass[] array1 = {new MyClass(), new MyClass(), new MyClass()};
        MyClass[] array2 = array1.clone();
        // Array objects are different, but share the element objects.
        System.out.println("array1 == array2: " + (array1 == array2));//array1 == array2: false
        for (int i = 0; i < array1.length; i++) {
            System.out.println("array1[" + i + "] == array2[" + i + "] : " + (array1[i] == array2[i]));
      /*
              array1[0] == array2[0] : true
              array1[1] == array2[1] : true
              array1[2] == array2[2] : true
       */
        }
        System.out.println();
        // Clone an object of MyClass.
        MyClass obj3 = obj1.clone();
        System.out.println("hash code for cloned MyClass obj3: " + obj3.hashCode());//hash code for cloned MyClass obj3: 1552787810
        System.out.println("obj1 == obj3: " + (obj1 == obj3));//obj1 == obj3: false
    }
}

```

## The Wrapper Classes

- All wrapper classes are `final`, meaning that they cannot be extended
- The objects of all wrapper classes that can be instantiated are _immutable_; in other words, the value in the wrapper object cannot be changed.
- the `Void` class is considered a wrapper class, it does not wrap any primitive value and is not instantiable (i.e., has no `public` constructors).
- wrapper classes constructors are now deprecated

### Converting Values among Primitive, Wrapper, and String Types

![](/images/ConvertingValues.png "Converting Values among Primitive, Wrapper, and String Types")

|  type is  | WrapperType is |                                                   Comments                                                    |
|:---------:|:--------------:|:-------------------------------------------------------------------------------------------------------------:|
| `boolean` |   `Boolean`    |                                                (1a) Autoboxing                                                |
|  `char`   |  `Character`   |                        (2) Not for `Character` type. Can throw `NumberFormatException`                        |
|  `byte`   |     `Byte`     |                                                 (4a) Unboxing                                                 |
|  `short`  |    `Short`     | (5) For numeric wrapper types and `Boolean` only. Can throw `NumberFormatException` for numeric wrapper types |
|   `int`   |   `Integer`    |                          Can throw `NumberFormatException` for numeric wrapper types                          |
|  `long`   |     `Long`     |                                (6b) Not for `byte` and `short` primitive types                                |
|  `float`  |    `Float`     |                                                                                                               |
| `double`  |    `Double`    |                                                                                                               |

### Common Wrapper Class Utility Methods

#### Converting Primitive Values to Wrapper Objects

```java
void test() {
    //Autoboxing 
    Character charObj1 = '\n';
    Boolean boolObj1 = true;
    Integer intObj1 = 2020;
    Double doubleObj1 = 3.14;

    //valueOf(type v) method 
    static WrapperType valueOf (type v)//Method Signature

    Character charObj1 = Character.valueOf('\n');
    Boolean boolObj1 = Boolean.valueOf(true);
    Integer intObj1 = Integer.valueOf(2020);
    Double doubleObj1 = Double.valueOf(3.14);
}
```

#### Converting Strings to Wrapper Objects

- Each wrapper class (except `Character`) defines the `static` method `valueOf(String str)` that returns the wrapper object corresponding to the primitive value represented by the `String` object passed as an argument
- This method for the numeric wrapper types also throws a `NumberFormatException` if the String parameter is not a valid number.

```java
void test() {
    static WrapperType valueOf (String str)//Method Signature

    Boolean boolObj4 = Boolean.valueOf("false");
    Integer intObj3 = Integer.valueOf("1949");
    Double doubleObj2 = Double.valueOf("3.14");
    Double doubleObj3 = Double.valueOf("Infinity");

    static IntegerWrapperType valueOf (String str,int base) throws NumberFormatException//Method Signature
    Byte byteObj1 = Byte.valueOf("1010", 2); // Decimal value 10
    Short shortObj2 = Short.valueOf("12", 8); // Decimal value 10
    Short shortObj3 = Short.valueOf("012", 8); // Decimal value 10
    Short shortObj4 = Short.valueOf("\012", 8); // NumberFormatException
    Integer intObj4 = Integer.valueOf("-a", 16); // Decimal value -10
    Integer intObj6 = Integer.valueOf("-0xa", 16); // NumberFormatException
    Long longObj2 = Long.valueOf("-a", 16); // Decimal value -10L
}
```

#### Converting Wrapper Objects to Strings

- Each wrapper class overrides the `toString()` method from the Object class

```java
void test() {
    String toString ()//Method Signature
    String charStr = charObj1.toString(); // "\n"
    String boolStr = boolObj2.toString(); // "true"
    String intStr = intObj1.toString(); // "2020"
    String doubleStr = doubleObj1.toString(); // "3.14"
}
```

#### Converting Primitive Values to Strings

- Each wrapper class defines a `static` method `toString(type v)` that returns the string corresponding to the primitive value of type, which is passed as an argument

```java
void test() {
    static String toString (type v)//Method Signature
    String charStr2 = Character.toString('\n'); // "\n"
    String boolStr2 = Boolean.toString(true); // "true"
    String intStr2 = Integer.toString(2020); // "2020"
    String doubleStr2 = Double.toString(3.14); // "3.14"
}
```

#### Converting Wrapper Objects to Primitive Values

- Unboxing is a convenient way to unwrap the primitive value in a wrapper object
- Each wrapper class defines a `typeValue()` method that returns the primitive value in the wrapper object

```java
void test() {
    char c = charObj1; // '\n'
    boolean b = boolObj2; // true
    int i = intObj1; // 2020
    double d = doubleObj1; // 3.14

    type typeValue ()//Method Signature

    char c = charObj1.charValue(); // '\n'
    boolean b = boolObj2.booleanValue(); // true
    int i = intObj1.intValue(); // 2020
    double d = doubleObj1.doubleValue(); // 3.14
}
```

#### Wrapper Comparison, Equality, and Hash Code

- Each wrapper class implements the `Comparable<Type>` interface

```java
void test() {
    int compareTo (Type obj2)//Method Signature
    // Comparisons based on objects created earlier
    Character charObj2 = 'a';
    int result1 = charObj1.compareTo(charObj2); // result1 < 0
    int result2 = intObj1.compareTo(intObj3); // result2 > 0
    int result3 = doubleObj1.compareTo(doubleObj2); // result3 == 0
    int result4 = doubleObj1.compareTo(intObj1); // Compile-time error!

    boolean equals (Object obj2)//Method Signature
    // Comparisons based on objects created earlier
    boolean charTest = charObj1.equals(charObj2); // false
    boolean boolTest = boolObj2.equals(Boolean.FALSE); // false
    boolean intTest = intObj1.equals(intObj3); // true
    boolean doubleTest = doubleObj1.equals(doubleObj2); // true
    boolean test = intObj1.equals(Long.valueOf(2020L)); // false. Not same type

    int hashCode ()//Method Signature
    int index = charObj1.hashCode(); // 10 ('\n')
}
```

- The following values are _interned_ when they are wrapped during boxing. That is, only _one_ wrapper object exists in the program for these primitive values when boxing is applied:
    - The `boolean` value `true` or `false`
    - A `byte`
    - A `char` with a Unicode value in the interval [`\u0000`, `\u007f`] (i.e., decimal interval [0, 127])
    - An `int` or `short` value in the interval [-128, 127]

```java
void test() {
    // Reference and object equality
    Byte bRef1 = 10;
    Byte bRef2 = 10;
    System.out.println(bRef1 == bRef2); // true
    System.out.println(bRef1.equals(bRef2)); // true

    Integer iRef1 = 1000;
    Integer iRef2 = 1000;
    System.out.println(iRef1 == iRef2); // false, values not in [-128, 127]
    System.out.println(iRef1.equals(iRef2)); // true
}
```

### Numeric Wrapper Classes

- The numeric wrapper classes `Byte`, `Short`, `Integer`, `Long`, `Float`, and `Double` are all subclasses of the abstract class `Number`
- Each numeric wrapper class defines an assortment of constants like `NumericWrapperType.MIN_VALUE` and `NumericWrapperType.MAX_VALUE`

#### Converting Numeric Wrapper Objects to Numeric Primitive Types

```java
byte byteValue();

short shortValue();

int intValue();

long longValue();

float floatValue()

double doubleValue();
```

```java
Byte byteObj2 = (byte) 16; // Cast mandatory
Integer intObj5 = 42030;
Double doubleObj4 = Math.PI;
short shortVal = intObj5.shortValue(); // (1)
long longVal = byteObj2.longValue();
int intVal = doubleObj4.intValue(); // (2) Truncation
double doubleVal = intObj5.doubleValue();
```

#### Converting Strings to Numeric Values

```java
void test() {
    static type parseType (String str) throws NumberFormatException//Method Signature
    byte value1 = Byte.parseByte("16");
    int value2 = Integer.parseInt("2020"); // parseInt, not parseInteger
    int value3 = Integer.parseInt("7UP"); // NumberFormatException.
    double value4 = Double.parseDouble("3.14");
    double value5 = Double.parseDouble("Infinity");

    static type parseType (String str,int base) throws NumberFormatException//Method Signature
    byte value6 = Byte.parseByte("1010", 2); // Decimal value 10
    short value7 = Short.parseShort("12", 8); // Decimal value 10
    short value8 = Short.parseShort("012", 8); // Decimal value 10
    short value9 = Short.parseShort("\012", 8); // NumberFormatException
    int value10 = Integer.parseInt("-a", 16); // Decimal value -10
    int value11 = Integer.parseInt("-0xa", 16); // NumberFormatException
    long value12 = Long.parseLong("-a", 16); // Decimal value -10L
}
```

#### Converting `Integer` and `Long` Values to Strings in Different Notations

```java
static String toBinaryString(int i)

static String toHexString(int i)

static String toOctalString(int i)

static String toString(int i, int base)

static String toString(int i)
```

### The Character Class

```java
void test() {
    Character.MIN_VALUE;
    Character.MAX_VALUE;
    static int getNumericValue ( char ch)
    static boolean isLowerCase ( char ch)
    static boolean isUpperCase ( char ch)
    static boolean isTitleCase ( char ch)
    static boolean isDigit ( char ch)
    static boolean isLetter ( char ch)
    static boolean isLetterOrDigit ( char ch)
    static boolean isWhitespace ( char ch)
    static char toUpperCase ( char ch)
    static char toLowerCase ( char ch)
    static char toTitleCase ( char ch)
}
```

### The Boolean Class

```java
void test() {
    Boolean.TRUE;
    Boolean.FALSE;
}
```

### Converting Strings to Boolean Values

- returns the `boolean` value `true` only if the `String` argument is equal to the string "true", ignoring the case; otherwise, it returns the `boolean` value false

```java
static boolean parseBoolean(String str)//Method Signature

boolean b1 = Boolean.parseBoolean("TRUE"); // true.
boolean b2 = Boolean.parseBoolean("true"); // true.
boolean b3 = Boolean.parseBoolean("false"); // false.
boolean b4 = Boolean.parseBoolean("FALSE"); // false.
boolean b5 = Boolean.parseBoolean("not true"); // false.
boolean b6 = Boolean.parseBoolean("null"); // false.
boolean b7 = Boolean.parseBoolean(null); // false.
```

## The String Class

### Internal Representation of Strings

- Internally, the character sequence in a `String` object is stored as _an array_ of `byte`
- If all characters in the string can be stored as a single byte per character, they are all encoded in the array with the LATIN-1 encoding scheme—1 byte per character.
- If any character in the sequence requires more than 1 byte, they are all encoded in the array with the UTF-16 encoding scheme—2 bytes per character.
- To keep track of which encoding is used for the characters in the internal byte array, the String class has a private final encoding-flag field named coder which the string methods can consult to correctly interpret the bytes in the internal array.

### Creating and Initializing Strings

#### Immutability

- The String class implements immutable character strings, which are read-only once the string has been created and initialized.
- Objects of the String class are thus thread-safe, as the state of a String object cannot be corrupted through concurrent access by multiple threads.

#### String Internment

- A string literal is a reference to a String object
- The compiler optimizes handling of string literals (and compile-time constant expressions that evaluate to strings): Only one String object is shared by all string-valued constant expressions with the same character sequence
- The compile-time evaluation of the constant expression involving the two string literals results in a string that is already interned

```java
String can1 = 7 + "Up"; // Value of compile-time constant expression: "7Up"
String can2 = "7Up"; // "7Up"
boolean r = can1 == can2; // true
String word = "Up";
String can4 = 7 + word; // Not a compile-time constant expression
```

#### String Constructors

- using a constructor creates a brand-new String object; using a constructor does not **_intern_** the string
- Constructing String objects can also be done from arrays of bytes, arrays of characters, and string builders
- `String()`
    - Creates a new String object, whose content is the empty string, ""
- `String(String str)`
- `String(char[] value)` and `String(char[] value, int offset, int count)`
- `String(StringBuilder builder)`

```java
byte[] bytes = {97, 98, 98, 97};
char[] characters = {'a', 'b', 'b', 'a'};
StringBuilder strBuilder = new StringBuilder("abba");

String byteStr = new String(bytes); // Using array of bytes: "abba"
String charStr = new String(characters); // Using array of chars: "abba"
String buildStr = new String(strBuilder); // Using string builder: "abba"
```

- The `String` method `intern()` allows the contents of a `String` object to be interned. If a string with the same contents is not already in the string pool, it is added; otherwise, the already interned string is returned `strX.intern() == strY.intern()` is true if and only if `strX.equals(strY)` is `true`

### The `CharSequence` Interface

- This interface defines a readable sequence of char values. It is implemented by the `String` and `StringBuilder` classes
- `int length()`
- `default boolean isEmpty()`
- `char charAt(int index)`
- `CharSequence subSequence(int start, int end)` from the index start to the index end-1, inclusive.
- `String toString()`
- `static int compare(CharSequence cs1, CharSequence cs2)`
- `default IntStream chars()`

### Reading Characters from a `String`

- `char[] toCharArray()`
- `void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)`

### Reading Lines from a String

- `Stream<String> lines()` separated by a line terminator

### Comparing Strings

- Characters are compared based on their Unicode values `boolean test = 'a' < 'b';` true since `0x61` < `0x62`
- `boolean equals(Object obj)`, `boolean equalsIgnoreCase(String str2)`
- `int compareTo(String str2)` The `String` class implements the `Comparable<String>` interface

```java
void test() {
    String strA = new String("The Case was thrown out of Court");
    String strB = new String("the case was thrown out of court");
    boolean b1 = strA.equals(strB); // false
    boolean b2 = strA.equalsIgnoreCase(strB); // true
    String str1 = "abba";
    String str2 = "aha";
    int compVal1 = str1.compareTo(str2); // negative value => str1 < str2
}
```

### Character Case in a String

- `String toUpperCase()`, `String toUpperCase(Locale locale)`, `String toLowerCase()`, `String toLowerCase(Locale locale)`
    - original string is returned if none of the characters needs its case changed, but a new String object is returned if any of the characters need their case changed

### Concatenation of Strings

- `String concat(String str)`

```java
void test() {
    String billboard = "Just";
    billboard.concat(" lost in space."); // (1) Returned reference value not stored.
    System.out.println(billboard); // (2) "Just"
    billboard = billboard.concat(" advertise").concat(" here."); // (3) Chaining.
    System.out.println(billboard); // (4) "Just advertise here."
}
```

#### Repeating Strings

- `String repeat(int count)` If this string is empty (i.e., its length is zero) then the empty string is returned

### Joining of `CharSequence` Objects

- `static String join(CharSequence delimiter, CharSequence... elements)`
- `static String join(CharSequence delimiter,Iterable<? extends CharSequence> elements)`
- If either delimiter is `null` or elements is `null`, a `NullPointerException` is thrown. If both are `null`, the method call is ambiguous
- If an element in elements is `null`, the string "null" is added as its text representation.

```java
void test() {
    StringBuilder left = new StringBuilder("Left");
    StringBuilder right = new StringBuilder("Right");
    ArrayList<CharSequence> charSeqList = new ArrayList<>();
    charSeqList.add(right);
    charSeqList.add(left); // Add StringBuilder objects.
    charSeqList.add("Right");
    charSeqList.add("Left"); // Add String objects.
    String resultStr2 = "<" + String.join("; ", charSeqList) + ">";
    System.out.println(resultStr2); // <Right; Left; Right; Left>
}
```

### Searching for Characters and Substrings in Strings

- These methods search forward toward the end of the string. In other words, the index of the _first_ occurrence of the character or substring is found. If the search is unsuccessful, the value -1 is returned
- `int indexOf(int ch)`, `int indexOf(int ch, int fromIndex)`
- `int indexOf(String str)`, `int indexOf(String str, int fromIndex)`
    - If the index argument is negative, the index is assumed to be 0
    - If the index argument is greater than the length of the string, it is effectively considered to be equal to the length of the string, resulting in the value -1 being returned.
- `int lastIndexOf(int ch)`, `int lastIndexOf(int ch, int fromIndex)`
- `int lastIndexOf(String str)`, `int lastIndexOf(String str, int fromIndex)`
    - search is backward toward the start of the string. In other words, the index of the last occurrence of the character or substring is found
- `String replace(char oldChar, char newChar)`,`String replace(CharSequence target, CharSequence replacement)`
    - The current string is returned if no occurrences of the oldChar can be found.
- `boolean contains(CharSequence cs)`
- `boolean startsWith(String prefix)`, `boolean startsWith(String prefix, int index)`
- `boolean endsWith(String suffix)`

### Extracting Substrings from Strings

- `boolean isBlank()` true if the string is empty or contains only whitespace
- `String strip()`, `String stripLeading()`, `String stripTrailing()`
    - It is recommended to use the strip methods to remove leading and trailing whitespace in a string instead of `String trim()`
- `String substring(int startIndex)`, `String substring(int startIndex, int endIndex)` the last character in the substring is at index endIndex-1 and If the index value is not valid, an `IndexOutOfBoundsException` is thrown.

### Converting Primitive Values and Objects to Strings

- `static String valueOf(Object obj)`
- `static String valueOf(char[] charArray)`
- `static String valueOf(boolean b)`
- `static String valueOf(char c)`
- `static String valueOf(int i)`
- `static String valueOf(long l)`
- `static String valueOf(float f)`
- `static String valueOf(double d)`
    - there are no valueOf() methods that accept a byte or a short

### Transforming a String

- The `transform()` method allows a function to be applied to a string to compute a result
- `<R> R transform(Function<? super String,? extends R> f)`

```java
void test() {
    String message = "Take me to your leader!";
    String tongueSpeake = message.transform(s -> new StringBuilder(s).reverse().toString().toUpperCase());
    System.out.println(tongueSpeake); // !REDAEL RUOY OT EM EKAT
}
```

### Indenting Lines in a String

- `String indent(int n)`
    - Adjusts the indentation of each line of this string based on the value of n, and normalizes line termination characters
    - This string is conceptually separated into lines using the `String.lines()` method. Each line is then adjusted depending on the value of n, and then terminated with a line feed ("\n"). The resulting lines are then concatenated and the final string returned
    - If n > 0 then n spaces are inserted at the beginning of each line. This also applies to an empty line
    - If n == 0 then the line remains unchanged. However, line terminators are still normalized
    - If n < 0 then at most n whitespace characters are removed from the beginning of each line, depending on the number of leading whitespace characters in the line.

```java
void test() {
    String cmds = " Attention!\r Quick march!\n\n Eyes left!"; //" Attention!\r Quick march!\n\n Eyes left!"
    String str1 = cmds.indent(0); //" Attention!\n Quick march!\n\n Eyes left!\n"
    String str2 = cmds.indent(3); //" Attention!\n Quick march!\n \n Eyes left!\n"
    String str3 = cmds.indent(-1); //"Attention!\n Quick march!\n\n Eyes left!\n"
    String str4 = cmds.indent(-2); //"Attention!\nQuick march!\n\n Eyes left!\n"
    String str5 = cmds.indent(-3); //"Attention!\nQuick march!\n\nEyes left!\n"
}
```

### Formatted Strings

- `static String format(String format, Object... args)`, `String formatted(Object... args)`
- `public String[] split(String regex)`, `public String[] split(String regex, int limit)`

### Text Blocks

#### Basic Text Blocks

- The content of a text block begins with the sequence of characters that immediately begins after the line terminator of the opening delimiter and ends with the first double quote of the closing delimiter. Note that the line terminator of the opening delimiter is not part of the text block’s content.

#### Using Escape Sequences in Text Blocks

- using the \n escape sequence will be interpreted as literally inserting a newline character in the text
- ending a line explicitly in the text block with a newline character will result in an empty line being inserted into the resulting string
- `\` is used for preventing the termination of the current line so that it is joined with the next line, and it's only allowed in text blocks
- `\s` escape sequence to retain trailing whitespace

#### Incidental Whitespace

- the lines in the text blocks have all been left-justified
- The least indented lines end up with no indentation.
- `String stripIndent()`
    - incidental whitespace removed from the beginning and end of every line

## The `StringBuilder` Class

- Although there is a close relationship between objects of the `String` and `StringBuilder` classes, these are two independent `final` classes, both directly extending the `Object` class both classes implement the `CharSequence` interface and the `Comparable` interface
- `String` references cannot be stored (or cast) to `StringBuilder` references, and vice versa
- the `StringBuilder` class does not override the `equals()` and `hashCode()` methods from the `Object` class, the contents of string builders should be converted to `String` objects for equality comparison and to compute a hash value
- The compiler uses string builders to implement string concatenation with the + operator in String-valued non-constant expressions

### Constructing String Builders

#### String Builder Constructors

- The capacity of a string builder is the maximum number of characters that a string builder can accommodate before its size is automatically augmented.
- `StringBuffer` supports the same operations as the `StringBuilder` class, but the `StringBuffer` class is _thread-safe_
- The initial capacity of the string builder is set to the length of the argument sequence, plus room for 16 more characters.
- `StringBuilder()`
- `StringBuilder(String str)`
- `StringBuilder(CharSequence charSeq)`
- `StringBuilder(int initialCapacity)`

```java
void test() {
    StringBuilder strBuilder1 = new StringBuilder("Phew!"); // "Phew!", capacity 21
    StringBuilder strBuilder2 = new StringBuilder(10); // "", capacity 10
    StringBuilder strBuilder3 = new StringBuilder(); // "", capacity 16
}
```

### Searching for Substrings in String Builders

- `int indexOf(String substr)`, `int indexOf(String substr, int fromIndex)`
- `int lastIndexOf(String str)`, `int lastIndexOf(String str, int fromIndex)`

### Extracting Substrings from String Builders

- `String substring(int startIndex)`, `String substring(int startIndex, int endIndex)`

### Constructing Strings from String Builders

- `String toString()`

### Comparing String Builders

- `int compareTo(StringBuilder anotherSB)`

### Modifying String Builders

- the methods in this subsection return the reference value of the modified string builder, making it convenient to chain calls to these methods

#### Appending Characters to a String Builder

- `StringBuilder append(Object obj)`
    - The obj argument is converted to a string as if by the static method call `String.valueOf(obj)`
- `StringBuilder append(String str)`
- `StringBuilder append(CharSequence charSeq)`
- `StringBuilder append(CharSequence charSeq, int start, int end)`
- `StringBuilder append(char[] charArray)`
- `StringBuilder append(char[] charArray, int offset, int length)`
- `StringBuilder append(char c)`
- `StringBuilder append(boolean b)`
- `StringBuilder append(int i)`
- `StringBuilder append(long l)`
- `StringBuilder append(float f)`
- `StringBuilder append(double d)`

#### Inserting Characters in a String Builder

- `StringBuilder insert(int offset, Object obj)`
- `StringBuilder insert(int dstOffset, CharSequence seq)`
- `StringBuilder insert(int dstOffset, CharSequence seq, int start, int end)`
- `StringBuilder insert(int offset, String str)`
- `StringBuilder insert(int offset, char[] charArray)`
- `StringBuilder insert(int offset, type c)`

#### Replacing Characters in a String Builder

- `void setCharAt(int index, char ch)`
- `StringBuilder replace(int start, int end, String replacement)`
    - the start index (inclusive) and the end index (exclusive)

#### Deleting Characters in a String Builder

- `StringBuilder deleteCharAt(int index)`
- `StringBuilder delete(int start, int end)`
    - the start index (inclusive) and the end index (exclusive)

#### Reversing Characters in a String Builder

- `StringBuilder reverse()`

### Controlling String Builder Capacity

- `int capacity()`
    - Returns the current capacity of the string builder, meaning the number of characters the current builder can accommodate without allocating a new, larger array to hold characters
- `void ensureCapacity(int minCapacity)`
    - Ensures that there is room for at least a minCapacity number of characters
- `void trimToSize()`
- `void setLength(int newLength)`

## The Math Class

- The `final` class `java.lang.Math` defines a set of static methods to support common mathematical functions
- `Math.E`
- `Math.PI`

### Miscellaneous Rounding Functions

- `static int abs(int i)`
- `static long abs(long l)`
- `static float abs(float f)`
- `static double abs(double d)`
- `static int min(int a, int b)`, `static int max(int a, int b)`
- `static long min(long a, long b)`, `static long max(long a, long b)`
- `static float min(float a, float b)`, `static float max(float a, float b)`
- `static double min(double a, double b)`,`static double max(double a, double b)`
- `static double ceil(double d)`, `static double floor(double d)`
- `static int round(float f)`, `static long round(double d)`

### Exponential Functions

- `static double pow(double d1, double d2)`
- `static double exp(double d)`
- `static double log(double d)`
- `static double sqrt(double d)`

### Exact Integer Arithmetic Functions

- if it is important to detect overflow errors, the `Math` class provides methods that report overflow errors by throwing an `ArithmeticException` when performing integer arithmetic operations
- `static int absExact(int a)`, `static long absExact(long a)`
- `static int addExact(int x, int y)`
- `static long addExact(long x, long y)`
- `static int subtractExact(int x, int y)`
- `static long subtractExact(long x, long y)`
- `static int multiplyExact(int x, int y)`
- `static long multiplyExact(long x, int y)`
- `static long multiplyExact(long x, long y)`
- `static int incrementExact(int a)`, `static int decrementExact(int a)`
- `static long incrementExact(long a)`, `static long decrementExact(long a)`
- `static int negateExact(int a)`, `static long negateExact(long a)`
- `static int toIntExact(long value)`

### Pseudorandom Number Generator

- `static double random()`
    - Returns a random number greater than or equal to 0.0 and less than 1.0
- `int diceValue = 1 + (int)(Math.random() * 6.0);` A dice roll in range `[1 .. 6]`

## The Random Class

- The `java.util.Random` class implements the `java.util.random.RandomGenerator` interface that defines the common protocol for pseudorandom number generators (PRNGs) in Java.
- constructors: `Random()`, `Random(long seed)`
- `int nextInt()`, `int nextInt(int bound)` bound (exclusive)
- `long nextLong()`
- `float nextFloat()`
- `double nextDouble()`
- `boolean nextBoolean()`

### Determining the Range

```java
void test() {
    number = 2 + generator.nextInt(11); // Random integer in the interval [2, 12]
    //values in the interval to have a distance greater than 1
    //3*generator.nextInt(5) always returns a value in the set {0, 3, 6, 9, 12}
    number = 2 + 3 * generator.nextInt(5);// Random integer in the set {2, 5, 8, 11, 14}
}
```

### Generating the Same Sequence of Pseudorandom Numbers

- This is because the pseudorandom number generator is based on the time of the system clock. This is obviously different each time the program is run
- If we want to generate the same sequence of pseudorandom numbers each time the program is run, we can specify a seed in the call to the Random constructor. The seed is usually a prime number

## Using Big Numbers

### BigDecimal Constants

- `BigDecimal.ZERO`, `BigDecimal.ONE`, `BigDecimal.TEN`

### Constructing BigDecimal Numbers

- `BigDecimal(int value)`
- `BigDecimal(long value)`
- `BigDecimal(double value)`
- `BigDecimal(String strValue)`
- `static BigDecimal valueOf(long value)`
- `static BigDecimal valueOf(double value)`

### Computing with BigDecimal Numbers

- `BigDecimal add(BigDecimal val)`
- `BigDecimal subtract(BigDecimal val)`
- `BigDecimal multiply(BigDecimal val)`
- `BigDecimal divide(BigDecimal val)`
- `BigDecimal remainder(BigDecimal val)`
- `BigDecimal abs()`
- `BigDecimal negate()`
- `BigDecimal pow(int n)`