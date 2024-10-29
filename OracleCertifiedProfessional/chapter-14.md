* [Object Comparison](#object-comparison)
    * [The Objects Class](#the-objects-class)
    * [The `equals()` and `hashCode()` Methods](#the-equals-and-hashcode-methods)
    * [Implementing the `java.lang.Comparable<E>` Interface](#implementing-the-javalangcomparablee-interface)
        * [The code can go from](#the-code-can-go-from)
        * [to](#to)
    * [Implementing the `java.util.Comparator<E>` Interface](#implementing-the-javautilcomparatore-interface)

----

# Object Comparison

## The Objects Class

- `static boolean equals(Object obj1, Object obj2)`
- `static int hash(Object... values)`,`static int hashCode(Object obj)`
- `static <T> int compare(T t1, T t2, Comparator<? super T> cmp)`

## The `equals()` and `hashCode()` Methods

- `public boolean equals(Object obj)` is object reference equality
- _**Reflexive**_: For any reference self, `self.equals(self)` is always `true`
- _**Symmetric**_: For any references x and y, if `x.equals(y)` is true, then `y.equals(x)` is `true`
- _**Transitive**_: For any references x, y, and z, if both `x.equals(y)` and `y.equals(z)` are true, then `x.equals(z)` is `true`
- **_Consistent_**: For any references x and y, multiple invocations of `x.equals(y)` will always return the same result, provided the objects referenced by these references have not been modified to affect the equals comparison
- **_`null` comparison_**: For any non-null reference obj, the call `obj.equals(null)` always returns `false`
- Equal objects must produce equal hash codes

## Implementing the `java.lang.Comparable<E>` Interface

- The `Comparable<E>` interface is implemented by the objects of the class—in other words, the class provides the implementation for comparing its objects according to its natural ordering
- `int compareTo(E other)` It throws a `NullPointerException` if the argument is `null`.
- Objects implementing this interface can be used as:
    - Elements in a sorted set
    - Keys in a sorted map
    - Elements in lists that are sorted using the `Collections.sort()` or `List.sort()` method
    - Elements in arrays that are sorted using the overloaded `Arrays.sort()` methods
- The natural ordering for `String` objects (and Character objects) is lexicographical ordering—that is, their comparison is based on the Unicode value of each corresponding character in the strings
- The natural ordering for objects of a numerical wrapper class is in ascending order of the values of the corresponding numerical primitive type
- the natural ordering for objects of the Boolean class, a Boolean object representing the value false is less than a Boolean object representing the value true

### The code can go from

```java

@Override
public int compareTo(VersionNumber vno) {
    if (this.release != vno.release) return Integer.compare(this.release, vno.release);
    if (this.revision != vno.revision) return Integer.compare(this.revision, vno.revision);
    return Integer.compare(this.patch, vno.patch);
}
```

### to

```java

@Override
public int compareTo(VersionNumber vno) {
    return Comparator.comparingInt(VersionNumber::getRelease) // (10a)
                     .thenComparingInt(VersionNumber::getRevision) // (11a)
                     .thenComparingInt(VersionNumber::getPatch) // (12a)
                     .compare(this, vno); // (13a)
}
```

## Implementing the `java.util.Comparator<E>` Interface

- The `java.util.Comparator<E>` interface is a functional interface and is designated as such with the `@FunctionalInterface` annotation in the Java SE API documentation— in other words, it is intended to be implemented by lambda expressions
- `int compare(E o1, E o2)`
- `default Comparator<T> reversed()` => `(a, b) -> this.compare(b, a)`
- `static <T extends Comparable<? super T>> Comparator<T> naturalOrder()` => `(a, b) -> a.compareTo(b)`
- `static <T extends Comparable<? super T>> Comparator<T> reverseOrder()` => `(a, b) -> b.compareTo(a)`
- `static <T> Comparator<T> nullsFirst(Comparator<? super T> cmp)`, `static <T> Comparator<T> nullsLast(Comparator<? super T> cmp)`
    - Return a null-friendly comparator that considers null to be either less than non-null or greater than non-null, respectively. These are useful comparators for sorting or searching in collections and maps when nulls are considered as actual values
- `static <T, U> Comparator<T>  comparing(Function<? super T,? extends U> func, Comparator<? super U> cmp)`
    - `(a, b) -> cmp.compare(func.apply(a), func.apply(b))`
- `static <T, U extends Comparable<? super U>> Comparator<T> comparing(Function<? super T,? extends U> func)`
    - `(a, b)-> func.apply(a).compareTo(func.apply(b))`
- `default Comparator<T> thenComparing(Comparator<? super T> cmp)`