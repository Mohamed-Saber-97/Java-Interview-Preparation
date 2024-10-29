* [Collections, Part I: `ArrayList<E>`](#collections-part-i-arrayliste)
    * [Lists](#lists)
        * [Overview of the Java Collections Framework](#overview-of-the-java-collections-framework)
    * [Declaring References and Constructing `ArrayLists`](#declaring-references-and-constructing-arraylists)
        * [Creating Unmodifiable Lists](#creating-unmodifiable-lists)
    * [Modifying an `ArrayList<E>`](#modifying-an-arrayliste)
        * [Adding Elements](#adding-elements)
        * [Replacing Elements](#replacing-elements)
        * [Removing Elements](#removing-elements)
        * [Modifying Capacity](#modifying-capacity)
    * [Querying an ArrayList<E>](#querying-an-arrayliste)
    * [Iterating Over an `ArrayList<E>`](#iterating-over-an-arrayliste)
    * [Converting an `ArrayList<E>` to an Array](#converting-an-arrayliste-to-an-array)
    * [Creating List Views](#creating-list-views)
        * [Comparing Unmodifiable Lists and List Views](#comparing-unmodifiable-lists-and-list-views)

-------

# Collections, Part I: `ArrayList<E>`

## Lists

### Overview of the Java Collections Framework

- elements is an ArrayList are maintained in the order they are inserted in the list, known as the insertion order
- The elements in such a list are therefore ordered, but they are not sorted, as it is not the values of the elements that determine their ranking in the list
- `Collection<E>` interface extends the `Iterable<E>` interface, so all collections in this framework can be traversed using the `for(:)` loop
- interface`java.lang.Iterable<E>` <-- interface`Collection<E>` <-- interface`List<E>` <-- interface`ArrayList<E>`
- The `ArrayList<E>` class is a dynamically resizable implementation of the `List<E>` interface using arrays (also known as dynamic arrays), providing fast random access (i.e., position-based access in **_constant time_**) and fast list traversal—very much like using an ordinary array.
- The `ArrayList<E>` class is not thread-safe; that is, its integrity can be jeopardized by concurrent access

## Declaring References and Constructing `ArrayLists`

- The zero-argument constructor creates an empty list with the initial capacity of 10.
- The capacity of a list refers to how many elements it can contain at any given time, not how many elements are actually in the list (which is called the size).
- The `ArrayList<E>` constructors
    - `ArrayList()`, `ArrayList(int initialCapacity)`, `ArrayList(Collection<? extends E> c)`

### Creating Unmodifiable Lists

- Although duplicates are allowed, unmodifiable lists do not allow `null` elements, and will result in a `NullPointerException` if an attempt is made to create them with the `null` elements.
- `static <E> List<E> of(E e1, E e2, E e3, E e4, E e5, E e6, E e7, E e8, E e9, E e10)`
- `static <E> List<E> ofCopy(E e1, E e2, E e3, E e4, E e5, E e6, E e7, E e8, E e9, E e10)`
- `@SafeVarargs static <E> List<E> of(E... elements)`
    - The `@SafeVarargs` annotation suppresses the heap pollution warning in the method declaration and also the unchecked generic array creation warning at the call sites
- `static <E> List<E> copyOf(Collection<? extends E> collection)`
    - If the specified collection is subsequently modified, the returned list will not reflect such modifications

```java
void test() {
    List<String> list = List.of("Tom", "Dick", "Harriet");
    // list.add("Harry"); // UnsupportedOperationException
    // list.remove(2); // UnsupportedOperationException
    // list.set(0, "Tommy"); // UnsupportedOperationException
    System.out.println(list); // [Tom, Dick, Harriet]
    System.out.println(list instanceof ArrayList); // false
}
```

```java
void test() {
    String[] strArray = {"Tom", "Dick", "Harriet"};
    List<String> strList = List.of(strArray); // (1) List of String
    List<String[]> strArrayList = List.<String[]>of(strArray); // (2) List of String[]
    System.out.println(strList); // [Tom, Dick, Harriet]
    System.out.println(strArrayList); // [[Ljava.lang.String;@3b22cdd0]
}
```

## Modifying an `ArrayList<E>`

### Adding Elements

- `boolean add(E element)`, `void add(int index, E element)` From `List<E>` interface
- `boolean addAll(Collection<? extends E> c)`, `boolean addAll(int index, Collection<? extends E> c)` From `List<E>` interface

### Replacing Elements

- `E set(int index, E element)` From `List<E>` interface
- `default void replaceAll(UnaryOperator<E> operator)` From `List<E>` interface

### Removing Elements

- `void clear()` From `List<E>` interface
- `E remove(int index)` From `List<E>` interface
- `boolean remove(Object element)` From `List<E>` interface
    - does not throw an exception if an element value is `null`, or if it is passed a `null` value
- `boolean removeAll(Collection<?> c)` From `List<E>` interface
- `boolean removeIf(Predicate<? super E> filter)` From `Collection<E>` interface
- The `remove(int)` removes the element at the specified index. The method `remove(Object)` needs to search the list and compare the argument object with elements in the list for object value equality. This test requires that the argument object override the `equals()` method from the Object class, which merely determines reference value equality. The String class provides the appropriate
  `equals()` method. However, the following code will not give the expected result because the `StringBuilder` class does not provide its own `equals()` method

### Modifying Capacity

- `void trimToSize()` Trims the capacity of this list to its current size
- `void ensureCapacity(int minCapacity)` From `List<E>` interface and Ensures that the capacity of this list is large enough to hold at least the number of elements specified by the minimum capacity

## Querying an ArrayList<E>

- `int size()` From `List<E>` interface
- `boolean isEmpty()` From `List<E>` interface
- `E get(int index)` From `List<E>` interface
- `boolean contains(Object element)` From `List<E>` interface
- `int indexOf(Object o)`, `int lastIndexOf(Object o)` From `List<E>` interface
    - if element doesn't exist the value –1 is returned
- `List<E> subList(int fromIndex, int toIndex)` From `List<E>` interface
    - Returns a view of the list, which consists of the sublist of the elements from the index `fromIndex` to the index `toIndex`-1
- `boolean equals(Object o)` From `List<E>` interface
    - true if and only if the specified object is also a list, both lists have the same size, and all corresponding pairs of elements in the two lists are equal according to object value equality.

## Iterating Over an `ArrayList<E>`

```java
void test() {
    for (String str : strList) {
        if (str.length() <= 3) {
            strList.remove(str); // Throws ConcurrentModificationException
        }
    }
}
```

## Converting an `ArrayList<E>` to an Array

- `Object[] toArray()` From `Collection<E>` interface
    - `Object[] objArray = strList.toArray();`
- `<T> T[] toArray(T[] a)` From `Collection<E>` interface
    - if the length of the array is greater than the number of elements in the collection — the element found immediately after storing the elements of the collection is set to the `null` value before the array is returned
    - If the array is too small, a new array of type `T` and appropriate size is created.
    - If `T` is not a supertype of the runtime type of every element in the collection, an `ArrayStoreException` is thrown
    - `String[] strArray = strList.toArray(new String[0]);`
- `default <T> T[] toArray(IntFunction<T[]> generator)` From `Collection<E>` interface

## Creating List Views

- `@SafeVarargs <E> List<E> asList(E... elements)` From `Arrays` class
    - Returns a fixed-size list view that is backed by the array corresponding to the variable arity parameter elements
    - The iterator for a list view does not support the `remove()` method.
- the `Collections.addAll()` method provides better performance when adding a few elements to an existing collection

### Comparing Unmodifiable Lists and List Views

- The `Arrays.asList()` method returns a fixed-size list view that is backed by the array passed as an argument so that any changes made to the array are reflected in the view list as well
- This is not true of the `List.of()` and `List.ofCopy()` methods, as they create unmodifiable lists which are not backed by any argument array
- The list view returned by the `Arrays.asList()` method is _mutable_, but it cannot be structurally modified. In contrast, the unmodifiable list returned by the `List.of()` method is immutable
- The `Arrays.asList()` method allows null elements, whereas the `List.of()` and `List.ofCopy()` methods do not