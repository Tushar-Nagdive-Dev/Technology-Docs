## How `teeing()` Works in Java Streams

`teeing()` is a static method in `Collectors` (introduced in Java 12) that combines two collectors into a single composite collector. It processes each stream element through **both** collectors simultaneously, then merges their results using a user-provided function.

### Signature
```java
public static <T, R1, R2, R> Collector<T, ?, R> teeing(
    Collector<? super T, ?, R1> downstream1,
    Collector<? super T, ?, R2> downstream2,
    BiFunction<? super R1, ? super R2, R> merger
)
```

### Internal Mechanics
1. The `teeing()` collector creates two internal accumulators – one for each downstream collector.
2. For every element in the stream, it is fed to **both** downstream collectors (the element is processed twice, but the stream is traversed only once).
3. After all elements are processed, the two intermediate results (`R1` and `R2`) are passed to the `merger` function.
4. The merger produces the final result `R`.

This is essentially a **single‑pass, dual‑aggregation** mechanism.

### Simple Example
```java
var numbers = List.of(1, 2, 3, 4, 5);

// Compute both sum and count in one pass
Result stats = numbers.stream().collect(teeing(
    Collectors.summingInt(Integer::intValue),   // downstream1 → sum
    Collectors.counting(),                      // downstream2 → count
    (sum, count) -> new Result(sum, count)      // merger
));
```

## Usefulness in Production Scenarios

### 1. Avoiding Multiple Stream Traversals
Large data sources (files, databases, network streams) often cannot be rewound. `teeing()` processes the stream **once** while calculating two different aggregates, saving I/O and time.

```java
// Instead of:
long sum = list.stream().mapToLong(Transaction::amount).sum();
long count = list.stream().count(); // second pass

// Use:
var sumAndCount = transactions.stream().collect(teeing(
    summingLong(Transaction::amount),
    counting(),
    (sum, cnt) -> new long[]{sum, cnt}
));
```

### 2. Computing Multiple Statistics
When you need several derived metrics from the same data – e.g., min, max, average, sum – `teeing()` keeps code concise and efficient.

```java
// Find employee with min salary and max salary in one pass
var minAndMax = employees.stream().collect(teeing(
    minBy(Comparator.comparingDouble(Employee::salary)),
    maxBy(Comparator.comparingDouble(Employee::salary)),
    (min, max) -> new Pair<>(min.get(), max.get())
));
```

### 3. Validating Business Rules
Combine two aggregations to check a condition without storing intermediate collections.

```java
// Check if total amount equals sum of squares (some validation rule)
boolean rulePassed = numbers.stream().collect(teeing(
    summingDouble(n -> n),
    summingDouble(n -> n * n),
    (sum, sumSq) -> Math.abs(sumSq - sum * sum) < 1e-6
));
```

### 4. Enriching Reports
Generate rich summaries like “total and average order value” or “count and distinct count” in one pass.

```java
var orderStats = orders.stream().collect(teeing(
    summingDouble(Order::value),
    averagingDouble(Order::value),
    (total, avg) -> String.format("Total: %.2f, Avg: %.2f", total, avg)
));
```

### 5. Parallel Stream Optimisation
When used with parallel streams, the two downstream collectors can still benefit from parallel execution because each collector maintains its own thread‑safe accumulator. This is more efficient than two separate parallel streams.

### 6. Custom Mergers for Complex DTOs
You can directly assemble domain objects without temporary variables.

```java
record SalesReport(double revenue, long numberOfSales, double averageSale) {}

SalesReport report = sales.stream().collect(teeing(
    summingDouble(Sale::amount),
    teeing(                      // nested teeing for more than two aggregations
        counting(),
        averagingDouble(Sale::amount),
        Pair::new
    ),
    (revenue, pair) -> new SalesReport(revenue, pair.count(), pair.average())
));
```

## Important Caveats
- **Stateful collectors** (e.g., `Collectors.toMap()` that may throw on duplicate keys) can cause surprising results because the two downstream collectors run independently on the same elements.
- The merger function is only called **once**, after the entire stream is consumed.
- While the collector itself is not inherently parallel‑friendly, it works correctly in parallel streams as long as both downstream collectors are concurrent and thread‑safe.

## Summary
`teeing()` shines whenever you need **two different reductions** on the same stream. In production, it reduces code duplication, improves performance by avoiding multiple passes, and clearly expresses intent – especially for reporting, validation, and statistical analysis tasks.


[![Preview](https://img.shields.io/badge/🚀-Preview-blue?style=for-the-badge)](https://htmlpreview.github.io/?https://github.com/Tushar-Nagdive/StackBlueprint/blob/StackTech/spring-boot/spring-generals/visuals/teeing-stream.html)