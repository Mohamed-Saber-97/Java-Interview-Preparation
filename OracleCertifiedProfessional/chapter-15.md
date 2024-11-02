* [Collections: Part II](#collections-part-ii)
    * [The Java Collections Framework](#the-java-collections-framework)
        * [The Core Interfaces](#the-core-interfaces)
    * [Collections](#collections)
        * [Basic Operations](#basic-operations)
        * [Bulk Operations](#bulk-operations)
        * [Iterating Over a Collection](#iterating-over-a-collection)
        * [Streams](#streams)
        * [Array Operations](#array-operations)
    * [Lists](#lists)
        * [The `ArrayList<E>`, `LinkedList<E>`, and `Vector<E>` Classes](#the-arrayliste-linkedliste-and-vectore-classes)
    * [Sets](#sets)
        * [The `HashSet<E>` and `LinkedHashSet<E>` Classes](#the-hashsete-and-linkedhashsete-classes)
    * [Sorted Sets and Navigable Sets](#sorted-sets-and-navigable-sets)
        * [The `SortedSet<E>` Interface](#the-sortedsete-interface)
        * [The `NavigableSet<E>` Interface](#the-navigablesete-interface)
        * [The `TreeSet<E>` Class](#the-treesete-class)
    * [Queues](#queues)
        * [The `Queue<E>` Interface](#the-queuee-interface)
        * [The `PriorityQueue<E>` and `LinkedList<E>` Classes](#the-priorityqueuee-and-linkedliste-classes)
    * [Deques](#deques)
        * [The `Deque<E>` Interface](#the-dequee-interface)
        * [The `ArrayDeque<E>` and `LinkedList<E>` Classes](#the-arraydequee-and-linkedliste-classes)
    * [Maps](#maps)
        * [Basic Key-Based Operations](#basic-key-based-operations)
        * [Creating Unmodifiable Maps](#creating-unmodifiable-maps)
        * [Advanced Key-Based Operations](#advanced-key-based-operations)
        * [Bulk Operations](#bulk-operations-1)
        * [Collection Views](#collection-views)
    * [Map Implementations](#map-implementations)
    * [Sorted Maps and Navigable Maps](#sorted-maps-and-navigable-maps)
        * [The `SortedMap<K, V>` Interface](#the-sortedmapk-v-interface)
        * [The `NavigableMap<K, V>` Interface](#the-navigablemapk-v-interface)
    * [The Collections Class](#the-collections-class)
        * [Unmodifiable Views of Collections](#unmodifiable-views-of-collections)
        * [Ordering Elements in Lists](#ordering-elements-in-lists)
        * [Searching in Collections](#searching-in-collections)
        * [Replacing Elements in Collections](#replacing-elements-in-collections)
    * [The Arrays Class](#the-arrays-class)
        * [Sorting Arrays](#sorting-arrays)
        * [Searching in Arrays](#searching-in-arrays)
        * [Comparing Arrays](#comparing-arrays)
        * [Miscellaneous Utility Methods in the `Arrays` Class](#miscellaneous-utility-methods-in-the-arrays-class)

------------

# Collections: Part II

## The Java Collections Framework

### The Core Interfaces

![](/images/TheCoreInterface.png "The Core Interfaces")

|                 Interface                  |                                                                 Description                                                                  |                    Concrete classes                    |
|:------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------:|
|              `Collection<E>`               |                                  allow a collection of objects to be maintained or handled as a single unit                                  |                           -                            |
|      `List<E> extends Collection<E>`       |                                         maintain a sequence of elements that can contain duplicates                                          |      `ArrayList<E>`, `Vector<E>`, `LinkedList<E>`      |
|       `Set<E> extends Collection<E>`       |                                        represent its mathematical namesake: a set of unique elements.                                        |            `HashSet<E>`, `LinkedHashSet<E>`            |
|       `SortedSet<E> extends Set<E>`        |                provide the required functionality for maintaining a set in which the elements are stored in some sort order.                 |                      `TreeSet<E>`                      |
|   `NavigableSet<E> extends SortedSet<E>`   |          extends and replaces the `SortedSet<E>` interface to maintain a sorted set, and should be the preferred choice in new code          |                      `TreeSet<E>`                      |
|      `Queue<E> extends Collection<E>`      | need to be processed according to some policy—that is, insertion at one end and removal at the other, usually as FIFO (first in, first out). |          `PriorityQueue<E>`, `LinkedList<E>`           |
|        `Deque<E> extends Queue<E>`         |                              maintain a queue whose elements can be processed at both ends: double-ended queue                               |            `ArrayDeque<E>`, `LinkedList<E>`            |
|                 `Map<K,V>`                 |                                        defines operations for maintaining mappings of keys to values                                         | `HashMap<K,V>`, `Hashtable<K,V>`, `LinkedHashMap<K,V>` |
|     `SortedMap<K,V> extends Map<K,V>`      |                                            maps that maintain their mappings sorted in key order                                             |                     `TreeMap<K,V>`                     |
| `NavigableMap<K,V> extends SortedMap<K,V>` |                                     extends and replaces the `SortedMap<K, V>` interface for sorted maps                                     |                     `TreeMap<K,V>`                     |

![](/images/TheCoreCollectionInterface.png)

![](/images/TheCoreMapInterface.png)

- only lists and queues allow duplicates
- The `TreeSet<E>` class and the queue classes `PriorityQueue<E>` and `ArrayDeque<E>` do not allow the `null` value to be stored in the collection
- the classes `Hashtable<K, V>` and `TreeMap<K, V>` disallow the `null` value as a key.
- `Hashtable<K, V>`does not allow the null value to be associated with a key.

| Concrete collections and maps |      Interface       |                  Duplicates                  |               Ordered/ Sorted                | Methods the implementations expect the elements to provide | Data structures on which implementation is based |
|:-----------------------------:|:--------------------:|:--------------------------------------------:|:--------------------------------------------:|:----------------------------------------------------------:|:------------------------------------------------:|
|        `ArrayList<E>`         |      `List<E>`       |                   Allowed                    |               Insertion order                |                         `equals()`                         |                 Resizable array                  |
|        `LinkedList<E>`        | `List<E>`,`Deque<E>` |                   Allowed                    |        Insertion/priority/deque order        |                         `equals()`                         |                   Linked list                    |
|          `Vector<E>`          |      `List<E>`       |                   Allowed                    |               Insertion order                |                         `equals()`                         |                 Resizable array                  |
|         `HashSet<E>`          |       `Set<E>`       |               Unique elements                |                   No order                   |                  `equals()`, `hashCode()`                  |                    Hash table                    |
|  `LinkedHashSet<E>`,`Set<E>`  |       `Set<E>`       |               Unique elements                |               Insertion order                |                  `equals()`, `hashCode()`                  |        Hash table  and doubly linked list        |
|         `TreeSet<E>`          |  `NavigableSet<E>`   |          Unique elements (not null)          |                  Sort order                  |          `equals()`, `hashCode()`, `compareTo()`           |                  Balanced tree                   |
|      `PriorityQueue<E>`       |      `Queue<E>`      |              Allowed (not null)              |      Access according to priority order      |                 `equals()`, `compareTo()`                  |       Priority heap (tree-like structure)        |
|        `ArrayDeque<E>`        |      `Deque<E>`      |              Allowed (not null)              |                 Deque order                  |                         `equals()`                         |                 Resizable array                  |
|        `HashMap<K,V>`         |      `Map<K,V>`      |                 Unique keys                  |                   No order                   |                  `equals()`, `hashCode()`                  |                    Hash table                    |
|     `LinkedHashMap<K,V>`      |      `Map<K,V>`      |                 Unique keys                  | Key insertion order/ Access order of entries |                  `equals()`, `hashCode()`                  |        Hash table and doubly linked list         |
|       `HashTable<K,V>`        |      `Map<K,V>`      | Unique keys (no null key and no null values) |                   No order                   |                  `equals()`, `hashCode()`                  |                    Hash table                    |
|        `TreeMap<K,V>`         | `NavigableMap<K,V>`  |          Unique keys (no null key)           |             Sorted in key order              |           `equals()`, `hashCode()`,`compareTo()`           |                  Balanced tree                   |

## Collections

### Basic Operations

- `int size()`
- `boolean isEmpty()`
- `boolean contains(Object element)`
- `boolean add(E element)`
- `boolean remove(Object element)`

### Bulk Operations

- `boolean containsAll(Collection<?> c)`
- `boolean addAll(Collection<? extends E> c)`
    - requires that the element type of the other collection is the same as, or a subtype of, the element type of the current collection
- `boolean removeAll(Collection<?> c)`
    - can be performed with collections of any type.
- `boolean retainAll(Collection<?> c)` like inner join
    - can be performed with collections of any type.
- `void clear()`

### Iterating Over a Collection

- `Iterator<E> iterator()` From `Iterable<E> interface`
- `default void forEach(Consumer<? super T> action)`, `default void forEachRemaining(Consumer<? super E> action)` From `Iterable<E> interface`
- `boolean hasNext()`, `E next()`, `void remove()` From `Iterable<E> interface`
    - Best practices dictate that the three methods of the iterator should be used in lock-step inside a loop
    - the `next()` method must be called before the `remove()` method for each element in the collection; otherwise, a `java.lang.IllegalStateException` or an `UnsupportedOperationException` is raised at runtime
- `default boolean removeIf(Predicate<? super E> filter)`

### Streams

- `default Stream<E> stream()`, `default Stream<E> parallelStream()`

### Array Operations

- `Object[] toArray()`, `<T> T[] toArray(T[] a)`, `default <T> T[] toArray(IntFunction<T[]> generator)`

```java
IntFunction<String[]> createStrArray = nn -> new String[nn];
String[] strArray5 = strSet.toArray(createStrArray);
String[] strArray6 = strSet.toArray(String[]::new);
String[] strArray7 = strSet.toArray(createStrArray.apply(0));
```

## Lists

- `ListIterator<E> listIterator()`, `ListIterator<E> listIterator(int index)` which is bidirectional iterator for lists

```java
interface ListIterator<E> extends Iterator<E> {
    boolean hasNext();

    boolean hasPrevious();

    E next(); // Element after the cursor

    E previous(); // Element before the cursor

    int nextIndex(); // Index of element after the cursor

    int previousIndex(); // Index of element before the cursor

    void remove(); // Optional

    void set(E o); // Optional

    void add(E o); // Optional
}
```

### The `ArrayList<E>`, `LinkedList<E>`, and `Vector<E>` Classes

- Unlike the `ArrayList<E>` class, the `Vector<E>` class is thread-safe, meaning that concurrent calls to the vector will not compromise its integrity
- The `LinkedList<E>` implementation uses a doubly linked list. Insertions and deletions in a doubly linked list are very efficient.
- The `ArrayList<E>` and `Vector<E>` classes offer comparable performance, but Vectors suffer a performance penalty due to **_synchronization_**.
- Position-based access has **constant-time** performance for the `ArrayList<E>` and `Vector<E>` classes
- position-based access is in linear time for a `LinkedList<E>`, owing to iteration in a doubly linked list
- When frequent insertions and deletions occur inside a list, a LinkedList<E> can be worth considering. In most cases, the ArrayList<E> implementation is the overall best choice for implementing lists.

## Sets

### The `HashSet<E>` and `LinkedHashSet<E>` Classes

- The `HashSet<E>` class implements the `Set<E>` interface. Since this implementation uses a hash table, it offers near-constant-time performance for most operations
- `HashSet()` Constructs a new, empty set
- `HashSet(Collection<? extends E> c)`
- `HashSet(int initialCapacity)`
- `HashSet(int initialCapacity, float loadFactor)`

## Sorted Sets and Navigable Sets

### The `SortedSet<E>` Interface

- `E first()`, `E last()` `NoSuchElementException` if the sorted set is empty
- `SortedSet<E> headSet(<E> toElement)` Range-view operations
- `SortedSet<E> tailSet(<E> fromElement)` Range-view operations
- `SortedSet<E> subSet(<E> fromElement, <E> toElement)` Range-view operations
- `Comparator<? super E> comparator()`

### The `NavigableSet<E>` Interface

- `E pollFirst()`, `E pollLast()` removed elements `return null` if the sorted set is empty
- `NavigableSet<E> headSet(E toElement, boolean inclusive)`
- `NavigableSet<E> tailSet(E fromElement, boolean inclusive)`
- `NavigableSet<E> subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)`
- `E ceiling(E e)`  least element in the navigable set greater than or equal to argument e
- `E floor(E e)` greatest element in the navigable set less than or equal to argument e
- `E higher(E e)` least element in the navigable set strictly greater than argument e
- `E lower(E e)` greatest element in the navigable set strictly less than argument e
- `Iterator<E> descendingIterator()` , `NavigableSet<E> descendingSet()` returns a reverse-order iterator

### The `TreeSet<E>` Class

- `TreeSet()`
- `TreeSet(Comparator<? super E> comparator)`
- `TreeSet(Collection<? extends E> collection)`
- `TreeSet(SortedSet<E> set)`

## Queues

### The `Queue<E>` Interface

- `boolean add(E element)`, `boolean offer(E element)`
    - throws an `IllegalStateException` if the queue is full, but the `offer()` method does not
- `E remove()`, `E poll()` retrieve the head element and remove it from the queue
    - `remove()` method throws a `NoSuchElementException`, but the `poll()` method returns the `null` value
- `E element()`, `E peek()` retrieve the head element, but do not remove it from the queue
    - `element()` method throws a `NoSuchElementException`, but the `peek()` method returns the `null` value

| Operation                   | Throws exception                               | Returns special value                   |
|-----------------------------|------------------------------------------------|-----------------------------------------|
| Insert at the tail          | `add(e)` can throw `IllegalArgumentException`  | `offer(e)` returns `true` or `false`    |
| Remove from the head        | `remove(e)` can throw `NoSuchElementException` | `poll()` returns head element or `null` |
| Examine element at the head | `element()` can throw `NoSuchElementException` | `peek()` returns head element or `null` |

### The `PriorityQueue<E>` and `LinkedList<E>` Classes

- `PriorityQueue()`
- `PriorityQueue(int initialCapacity)`
- `PriorityQueue(Comparator<? super E> comparator)`
- `PriorityQueue(int initialCapacity, Comparator<? super E> comparator)`
- `PriorityQueue(PriorityQueue<? extends E> pq)`
- `PriorityQueue(Collection<? extends E> c)`
- `PriorityQueue(SortedSet<? extends E> set)`

## Deques

### The `Deque<E>` Interface

- A deque can also be used as a _stack_
- `boolean offerFirst(E element)`
- `boolean offerLast(E element)` Queue equivalent: `offer()`
- `void addFirst(E element)` throw an `IllegalStateException` if the element cannot be added
- `void addLast(E element)` Queue equivalent: `add()` throw an `IllegalStateException` if the element cannot be added
- `void push(E element)` Synonym: `addFirst()`
- `E removeFirst()` Queue equivalent: `remove()` throw a `NoSuchElementException` if the deque is empty
- `E removeLast()` throw a `NoSuchElementException` if the deque is empty
- `E pollFirst()` Queue equivalent: `poll()` return `null` if the deque is empty
- `E pollLast()` return `null` if the deque is empty
- `E pop()` Synonym: `removeFirst()`
- `boolean removeFirstOccurence(Object obj)`
- `boolean removeLastOccurence(Object obj)`
- `E getFirst()` Queue equivalent: `element()` throw a `NoSuchElementException` if the deque is empty
- `E getLast()` throw a `NoSuchElementException` if the deque is empty
- `E peekFirst()` Queue equivalent: `peek()` throw a `null` if the deque is empty
- `E peekLast()` throw a `null` if the deque is empty
- `Iterator<E> descendingIterator()`

| Insert at the head |    Insert at the tail     |  Runtime behavior on failure   |
|:------------------:|:-------------------------:|:------------------------------:|
|   `offerFirst()`   | `offerLast()`, `offer()*` |   Returns `false` if `full`    |
|    `addFirst()`    |   `addLast()`, `add()*`   | Throws `IllegalStateException` |

|     Remove from the head     | Remove from the tail |   Runtime behavior on failure   |
|:----------------------------:|:---------------------|:-------------------------------:|
|   `pollFirst()`, `poll()*`   | `pollLast()`         |     Returns `null` if empty     |
| `removeFirst()`, `remove()*` | `removeLast()`       | Throws `NoSuchElementException` |

|    Examine at the head     | Examine at the tail |   Runtime behavior on failure   |
|:--------------------------:|:--------------------|:-------------------------------:|
|  `peekFirst()`, `peek()*`  | `peekLast()`        |     Returns `null` if empty     |
| `getFirst()`, `element()*` | `getLast()`         | Throws `NoSuchElementException` |

### The `ArrayDeque<E>` and `LinkedList<E>` Classes

- The `ArrayDeque<E>` class provides better performance than the `LinkedList<E>` class for implementing FIFO queues, and is also a better choice than the `java.util.Stack` class for implementing stacks.
- `ArrayDeque()`
- `ArrayDeque(int numOfElements)`
- `ArrayDeque(Collection<? extends E> c)`

## Maps

### Basic Key-Based Operations

- `V put(K key, V value)` returns the old value previously associated with the specified key, if any or null
- `default V putIfAbsent(K key, V value)`
- `V get(Object key)`, `default V getOrDefault(Object key, V defaultValue)`
- `V remove(Object key)` returns the value previously associated with the specified key or null
- `default boolean remove(Object key, Object value)`
- `default V replace(K key, V value)` returns the previous value associated with the specified key, or null
- `default boolean replace(K key, V oldValue, V newValue)`
- `boolean containsKey(Object key)`, `boolean containsValue(Object value)`

### Creating Unmodifiable Maps

- `static <K,V> Map.Entry<K,V> entry(K k, V v)` returns an unmodifiable Map

```java
interface Entry<K, V> { // Nested interface in the Map<K, V> interface.
    K getKey();

    V getValue();

    V setValue(V value); // Only if the entry is modifiable.
}
```

- `@SafeVarargs static <K,V> Map<K,V> ofEntries(Map.Entry<? extends K,? extends V>... entries)`
- `static <K,V> Map<K,V> copyOf(Map<? extends K, ? extends V> map)`

### Advanced Key-Based Operations

- `default V merge(K key, V value, BiFunction<? super V,? super V,? extends V> remappingFunc)`
- `default V compute(K key, BiFunction<? super K,? super V,? extends V> remappingFunc)`
- `default V computeIfAbsent(K key, Function<? super K,? extends V> mappingFunc)`
- `default V computeIfPresent(K key, BiFunction<? super K,? super V,? extends V> remappingFunc)`

### Bulk Operations

- `int size()`
- `boolean isEmpty()`
- `void clear()`
- `void putAll(Map<? extends K, ? extends V> map)`
- `default void forEach(BiConsumer<? super K,? super V> action)`
- `default void replaceAll(BiFunction<? super K,? super V,? extends V> func)`

### Collection Views

- `Set<K> keySet()`
- `Collection<V> values()`
- `Set<Map.Entry<K, V>> entrySet()`

## Map Implementations

|                    Map                    | null as key | null as value |            Kind of map            |  Thread-safe?   |
|:-----------------------------------------:|:-----------:|:-------------:|:---------------------------------:|:---------------:|
|              `HashMap<K,V>`               |   Allowed   |    Allowed    |             Unordered             | Not thread-safe |
| `LinkedHashMap<K,V> extends HashMap<K,V>` |   Allowed   |    Allowed    | Key insertion order/ Access order | Not thread-safe |
|             `Hashtable<K,V>`              | Not allowed |  Not allowed  |             Unordered             |   Thread-safe   |
|              `TreeMap<K,V>`               | Not allowed |    Allowed    |          Key-sort order           | Not thread-safe |

- `HashMap()`
- `HashMap(int initialCapacity)`
- `HashMap(int initialCapacity, float loadFactor)`
- `HashMap(Map<? extends K,? extends V> otherMap)`
- `LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder)`

## Sorted Maps and Navigable Maps

### The `SortedMap<K, V>` Interface

- `K firstKey()` Sorted set: `first()` throw a `NoSuchElementException` if the map is empty
- `K lastKey()` Sorted set: `last()` throw a `NoSuchElementException` if the map is empty
- Range View Operations
    - `SortedMap<K,V> headMap(K toKey)` Sorted set: `headSet()`
    - `SortedMap<K,V> tailMap(K fromKey)` Sorted set: `tailSet()`
    - `SortedMap<K,V> subMap(K fromKey, K toKey)` Sorted set: `subSet()`
- `Comparator<? super K> comparator()`

### The `NavigableMap<K, V>` Interface

- `Map.Entry<K, V> pollFirstEntry()` Navigable set: `pollFirst()`  removes and returns the first entry
- `Map.Entry<K, V> pollLastEntry()` Navigable set: `pollLast()`  removes and returns the last entry
- `Map.Entry<K, V> firstEntry()` Examine
- `Map.Entry<K, V> lastEntry()` Examine
- `NavigableMap<K, V> headMap(K toKey, boolean inclusive)` Navigable set: `headSet()`
- `NavigableMap<K, V> tailMap(K fromKey, boolean inclusive)` Navigable set: `tailSet()`
- `NavigableMap<K, V> subMap(K fromKey, boolean fromInclusive, K toKey, boolean toInclusive)` Navigable set: `tailSet()`
- Closest-matches
    - `Map.Entry<K, V> ceilingEntry(K key)`
    - `K ceilingKey(K key)`
    - `Map.Entry<K, V> floorEntry(K key)`
    - `K floorKey(K key)`
    - `Map.Entry<K, V> higherEntry(K key)`
    - `K higherKey(K key)`
    - `Map.Entry<K, V> lowerEntry(K key)`
    - `K lowerKey(K key)`
- Navigation-views
    - `NavigableMap<K, V> descendingMap()`
    - `NavigableSet<K> navigableKeySet()`
    - `NavigableSet<K> descendingKeySet()`

## The Collections Class

### Unmodifiable Views of Collections

- `<E> Collection<E> unmodifiableCollection(Collection<? extends E> c)`
- `<E> List<E> unmodifiableList(List<? extends E> list)`
- `<E> Set<E> unmodifiableSet(Set<? extends E> set)`
- `<E> SortedSet<E> unmodifiableSortedSet(SortedSet<E> sortedSet)`
- `<E> NavigableSet<E> unmodifiableNavigableSet(NavigableSet<E> navSet)`
- `<K, V> Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> map)`
- `<K, V> SortedMap<K, V> unmodifiableSortedMap(SortedMap<K, ? extends V> sm)`
- `<K, V> NavigableMap<K, V> unmodifiableNavigableMap(NavigableMap<K, ? extends V> nm)`

### Ordering Elements in Lists

- `<E extends Comparable<? super E>> void sort(List<E> list)`
- `<E> void sort(List<E> list, Comparator<? super E> comparator)`
- `<E> Comparator<E> reverseOrder()`
- `<E> Comparator<E> reverseOrder(Comparator<E> comparator)`
- `void sort(Comparator<? super E> comparator)`
- `void reverse(List<?> list)`
- `void shuffle(List<?> list)`
- `void shuffle(List<?> list, Random rnd)`
- `void swap(List<?> list, int i, int j)`
- `void rotate(List<?> list, int distance)`

### Searching in Collections

- `<E> int binarySearch(List<? extends Comparable<? super E>> list, E key)`
- `<E> int binarySearch(List<? extends E> list, E key,Comparator<? super E> cmp))`
- `int indexOfSubList(List<?> source, List<?> target)`
- `int lastIndexOfSubList(List<?> source, List<?> target)`
- `<E extends Object & Comparable<? super E>> E max(Collection<? extends E> c)`
- `<E> E max(Collection<? extends E> c, Comparator<? super E> cmp)`
- `<E extends Object & Comparable<? super E>> E min(Collection<? extends E> c)`
- `<E> E min(Collection<? extends E> cl, Comparator<? super E> cmp)`

### Replacing Elements in Collections

- `void replaceAll(UnaryOperator<E> operator)`
- `<E> boolean replaceAll(List<E> list, E oldVal, E newVal)` // Collections
- `<E> boolean addAll(Collection<? super E> collection, E... elements)`
- `<E> void copy(List<? super E> destination, List<? extends E> source)`
- `<E> void fill(List<? super E> list, E element)`
- `<E> List<E> nCopies(int n, E element)`

## The Arrays Class

### Sorting Arrays

- `void sort(type[] array)`
- `void sort(type[] array, int fromIndex, int toIndex)`
- `<E> void sort(E[] array, Comparator<? super E> cmp)`
- `<E> void sort(E[] array, int fromIndex, int toIndex, Comparator<? super E> cmp)`

### Searching in Arrays

- `int binarySearch(type[] array, type key)`
- `int binarySearch(type[] array, int fromIndex, int toIndex, type key)`
- `<E> int binarySearch(E[] array, E key, Comparator<? super E> c)`
- `<E> int binarySearch(E[] array, int fromIndex, int toIndex, E key, Comparator<? super E> c)`

### Comparing Arrays

- `boolean equals(type[] a, type[] b)`
- `int compare(type[] a, type[] b)`
- `<T extends Comparable<? super T>> int compare(T[] a, T[] b)`
- `int mismatch(type[] a, type[] b)`

### Miscellaneous Utility Methods in the `Arrays` Class

- `String toString(type[] array)`
- `String deepToString(Object[] array)`
- `void fill(type[] array, type value)`
- `void fill(type[] array, int fromIndex, int toIndex, type value)`
- `<T> void setAll(T[] array, IntFunction<? extends T> generator)`
- `void setAll(int[] array, IntUnaryOperator generator)`
- `void setAll(long[] array, IntToLongFunction generator)`
- `void setAll(double[] array, IntToDoubleFunction generator)`