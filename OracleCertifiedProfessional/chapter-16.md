* [Streams](#streams)
    * [Introduction to Streams](#introduction-to-streams)
    * [Stream Basics](#stream-basics)
        * [Composing Stream Pipelines](#composing-stream-pipelines)
        * [Executing Stream Pipelines](#executing-stream-pipelines)
        * [Comparing Collections and Streams](#comparing-collections-and-streams)
        * [Overview of API for Data Processing Using Streams](#overview-of-api-for-data-processing-using-streams)
    * [Building Streams](#building-streams)
        * [Aspects to Consider When Creating Streams](#aspects-to-consider-when-creating-streams)
            * [Sequential or Parallel Stream](#sequential-or-parallel-stream)
            * [Ordered or Unordered Stream](#ordered-or-unordered-stream)
            * [`static` factory methods for building streams](#static-factory-methods-for-building-streams)
        * [Using Generator Functions to Build Infinite Streams](#using-generator-functions-to-build-infinite-streams)
    * [Intermediate Stream Operations](#intermediate-stream-operations)
        * [Filtering](#filtering)
        * [Examining Elements in a Stream](#examining-elements-in-a-stream)
        * [Mapping: Transforming Streams](#mapping-transforming-streams)
        * [Flattening Streams](#flattening-streams)
        * [Sorted Streams](#sorted-streams)
    * [The Optional Class](#the-optional-class)
        * [Creating an Optional](#creating-an-optional)
        * [Querying an `Optional`](#querying-an-optional)
    * [Terminal Stream Operations](#terminal-stream-operations)
        * [Consumer Action on Stream Elements](#consumer-action-on-stream-elements)
        * [Matching Elements](#matching-elements)
        * [Finding the First or Any Element](#finding-the-first-or-any-element)
        * [Counting Elements](#counting-elements)
        * [Finding Min and Max Elements](#finding-min-and-max-elements)
        * [Implementing Functional Reduction: The `reduce()` Method](#implementing-functional-reduction-the-reduce-method)
        * [Implementing Mutable Reduction: The `collect()` Method](#implementing-mutable-reduction-the-collect-method)
        * [Collecting](#collecting)
        * [Functional Reductions Exclusive to Numeric Streams](#functional-reductions-exclusive-to-numeric-streams)
    * [Collectors](#collectors)

----------------

# Streams

## Introduction to Streams

- An aggregate operation performs a task on the stream as a whole rather than on an individual element of the stream.
- Examples of stream operations accepting implementation of functional interfaces
    - Generating elements of the stream using a `Supplier`
    - Converting the elements in the stream according to a mapping defined by a `Function`
    - Filtering the elements in the stream according to some criteria defined by a `Predicate`
    - Sorting the elements in the stream using a `Comparator`
    - Performing actions for each of the elements in the stream with the help of a `Consumer`

## Stream Basics

- Stream operations are characterized either as intermediate operations or terminal operations
- An intermediate operation always returns a new stream that is, it transforms its input stream to an output stream
- A terminal operation either causes a side effect or computes a result

### Composing Stream Pipelines

- Stream operations can be chained together to compose a stream pipeline in which stream components are specified in the following order:
    - An operation on a data source for building the initial stream
    - Zero or more intermediate operations to transform one stream into another
    - A single mandatory terminal operation in order to execute the pipeline and produce some result or side effect

### Executing Stream Pipelines

- Apart from the fact that an intermediate operation always returns a new stream and a terminal operation never does, another crucial difference between these two kinds of operations is the way in which they are executed
- Intermediate operations use lazy execution; that is, they are executed on demand.
- Terminal operations use eager execution; that is, they are executed immediately when the terminal operation is invoked on the stream
- This means the intermediate operation will never be executed unless a terminal operation is invoked on the output stream of the intermediate operation, whereupon the intermediate operations will start to execute and pull elements from the stream created on the data source
- there was only one pass over the elements in the stream.

### Comparing Collections and Streams

- Collections are data structures that can be used to store and retrieve elements. Streams are data structures that do not store their elements, but process them by expressing computations on them through operations like `filter()` and `collect()`.
- Typically, operations are provided to add or remove elements from a collection. However, no elements can be added or removed from a stream—that is, streams are **_immutable_** and A stream operation does not mutate its data source
- A collection can be used in the program as long as there is a reference to it. However, a stream cannot be reused once it is consumed. It must be re-created on the data source in order to be reused

### Overview of API for Data Processing Using Streams

- All streams implement the AutoCloseable interface, meaning they should be closed after use in order to facilitate resource management during execution

![The Core Stream Interfaces](/images/TheCoreStreamInterface.png "The Core Stream Interfaces")

## Building Streams

### Aspects to Consider When Creating Streams

#### Sequential or Parallel Stream

- A sequential stream is one whose elements are processed sequentially when the stream pipeline is executed by a single thread
- A parallel stream is split into multiple sub-streams that are processed in parallel by multiple instances of the stream pipeline being executed by multiple threads, and their intermediate results combined to create the final result
- `BaseStream.sequential()` or `BaseStream.parallel()`

#### Ordered or Unordered Stream

- Processing of unordered parallel streams may have better performance than for ordered parallel streams in identical stream pipelines when the ordering constraint is removed, as maintaining the order might carry a performance penalty

#### `static` factory methods for building streams

- `static <T> Stream<T> empty()`
- `static <T> Stream<T> of(T t)`
- `static <T> Stream<T> of(T... varargs)` creates a finite sequential ordered stream
- `static <T> Stream<T> ofNullable(T t)` Only in the `Stream<T>` interface which Creates a singleton stream that has a single element
- `static <T> Stream<T> generate(Supplier<T> supplier)` Creates an infinite sequential unordered stream where each element is generated by the specified supplier
- `static <T> Stream<T> iterate(T s, UnaryOperator<T> uop)` Creates an infinite sequential ordered stream
- `static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)`
    - ordered only if both input streams are ordered
    - parallel if either input stream is parallel
    - finite only if both input streams are finite

### Using Generator Functions to Build Infinite Streams

- `generate()` creates infinite sequential streams that are unordered
- `iterate()` creates infinite sequential streams that are ordered

## Intermediate Stream Operations

- An intermediate operation is not performed on all elements of the stream before performing the next operation on all elements resulting from the previous stream
- the intermediate operations are performed back-to-back on each element in the stream.
- Moving intermediate operations such as `filter()`, `distinct()`, `dropWhile()`, `limit()`, `skip()`, and `takeWhile()` earlier in the pipeline can be beneficial, as they all decrease the size of the input stream

### Filtering

- `Stream<T> filter(Predicate<? super T> predicate)`
- `default Stream<T> takeWhile(Predicate<? super T> predicate)`
    - If the stream is ordered, it returns a stream consisting of the longest prefix of elements taken from this stream that match the given predicate
    - If the stream is unordered, and some (but not all) of the elements of this stream match the given predicate, then the behavior of this operation is nondeterministic; it is free to take any subset of matching elements (which includes the empty set).
- `default Stream<T> dropWhile(Predicate<? super T> predicate)`
- `Stream<T> distinct()`
- `Stream<T> skip(long n)` stream has fewer than n elements, an empty stream is returned
- `Stream<T> limit(long maxSize)`

### Examining Elements in a Stream

- `Stream<T> peek(Consumer<? super T> action)`

### Mapping: Transforming Streams

- `<R> Stream<R> map(Function<? super T,? extends R> mapper)` does not change the stream size, but it can change the stream element type
- `IntStream mapToInt(ToIntFunction<? super T> mapper)`
- `LongStream mapToLong(ToLongFunction<? super T> mapper)`
- `DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper)`

### Flattening Streams

- `<R> Stream<R> flatMap(Function<? super T,? extends Stream<? extends R>> mapper)`
- `IntStream flatMapToInt(Function<? super T,? extends IntStream> mapper)`
- `LongStream flatMapToLong(Function<? super T,? extends LongStream> mapper)`
- `DoubleStream flatMapToDouble(Function<? super T,? extends DoubleStream> mapper)`

### Sorted Streams

- `Stream<T> sorted()`
- `Stream<T> sorted(Comparator<? super T> cmp)`
- `Stream<T> unordered()`

| Intermediate operation |    Stateful/ Stateless    | Can change stream size | Can change stream type | Encounter order |
|:----------------------:|:-------------------------:|:----------------------:|:----------------------:|:---------------:|
|       `distinct`       |         Stateful          |          Yes           |           No           |    Unchanged    | 
|       `dropWhile       |         Stateful          |          Yes           |           No           |    Unchanged    |
|        `filter`        |         Stateless         |          Yes           |           No           |    Unchanged    | 
|       `flatMap`        |         Stateless         |          Yes           |          Yes           | Not guaranteed  | 
|        `limit`         | Stateful, short-circuited |          Yes           |           No           |    Unchanged    |
|         `map`          |         Stateless         |           No           |          Yes           | Not guaranteed  | 
|       `mapMulti`       |         Stateless         |          Yes           |          Yes           | Not guaranteed  |
|       `parallel`       |         Stateless         |           No           |           No           |    Unchanged    | 
|         `peek`         |         Stateless         |           No           |           No           |    Unchanged    | 
|      `sequential`      |         Stateless         |           No           |           No           |    Unchanged    | 
|         `skip`         |         Stateful          |          Yes           |           No           |    Unchanged    |
|        `sorted`        |         Stateful          |           No           |           No           |     Ordered     | 
|      `takeWhile`       | Stateful, short-circuited |          Yes           |           No           |    Unchanged    |
|      `unordered`       |         Stateless         |           No           |           No           | Not guaranteed  |

## The Optional Class

### Creating an Optional

- The `Optional<T>` class models the absence of a value by a special singleton returned by the `Optional.empty()`
- `static <T> Optional<T> empty()`
- `static <T> Optional<T> of(T nonNullValue)`
- `static <T> Optional<T> ofNullable(T value)`

### Querying an `Optional`

- `T get()`
- `boolean isPresent()`
- `void ifPresent(Consumer<? super T> consumer)`
- `T orElse(T other)`
- `T orElseGet(Supplier<? extends T> other)`
- `<X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X extends Throwable`

## Terminal Stream Operations

### Consumer Action on Stream Elements

- `void forEach(Consumer<? super T> action)`
- `void forEachOrdered(Consumer<? super T> action)`

### Matching Elements

- `boolean anyMatch(Predicate<? super T> predicate)`
- `boolean allMatch(Predicate<? super T> predicate)`
- `boolean noneMatch(Predicate<? super T> predicate)`

### Finding the First or Any Element

- `Optional<T> findFirst()`
- `Optional<T> findAny()`

### Counting Elements

- `long count()`

### Finding Min and Max Elements

- `Optional<T> min(Comparator<? super T> cmp)`
- `Optional<T> max(Comparator<? super T> cmp)`

### Implementing Functional Reduction: The `reduce()` Method

- `Optional<T> reduce(BinaryOperator<T> accumulator)`
- `T reduce(T identity, BinaryOperator<T> accumulator)`

```java
void test() {
    int totNumOfTracks = CD.cdList.stream().mapToInt(CD::noOfTracks).reduce(0, (sum, numOfTracks) -> sum + numOfTracks);
}
```

### Implementing Mutable Reduction: The `collect()` Method

- `<R,A> R collect(Collector<? super T,A,R> collector)`

### Collecting

- `Object[] toArray()`
- `<A> A[] toArray(IntFunction<A[]> generator)`
- `default List<T> toList()`

### Functional Reductions Exclusive to Numeric Streams

- `numtype sum()`
- `OptionalDouble average()`
- `SummaryStatistics summaryStatistics()`

## Collectors

- `static <T,C extends Collection<T>> Collector<T,?,C> toCollection(Supplier<C> collectionFactory)`
- `static <T> Collector<T,?,List<T>> toList()`
- `static <T> Collector<T,?,List<T>> toUnmodifiableList()`
- `static <T> Collector<T,?,Set<T>> toSet()`
- `static <T> Collector<T,?,Set<T>> toUnmodifiableSet()`
- `static <T,K,U> Collector<T,?,Map<K,U>> toMap(Function<? super T,? extends K> keyMapper,Function<? super T,? extends U> valueMapper)`
    - `Map<String, Year> mapTitleToYear = CD.cdList.stream().collect(Collectors.toMap(CD::title, CD::year));`
- `static Collector<CharSequence,?,String> joining()`
- `static Collector<CharSequence,?,String> joining(CharSequence delimiter)`
- `static <T> Collector<T,?,Long> counting()`
- `static <T> Collector<T,?,Optional<T>> maxBy(Comparator<? super T> cmp)`
- `static <T> Collector<T,?,Optional<T>> minBy(Comparator<? super T> cmp)`
