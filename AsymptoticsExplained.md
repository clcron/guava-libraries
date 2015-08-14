The asymptotics of Guava's utilities are entirely predictable, but are listed here for completeness.

## `List` ##

| Implementation | `add` | `add(i, elem)` |  `remove(i)` | `contains` | Iteration | `size` |
|:---------------|:------|:---------------|:-------------|:-----------|:----------|:-------|
| `ArrayList` (JDK) | _O(1)_ | _O(n)_         | _O(n)_       | _O(n)_     | _O(n)_    | _O(1)_ |
| `LinkedList` (JDK) | _O(1)_ | _O(n)_         | _O(n)_       | _O(n)_     | _O(n)_    | _O(1)_ |
| `CopyOnWriteArrayList` (JDK) | _O(n)_ | _O(n)_         | _O(n)_       | _O(n)_     | _O(n)_    | _O(1)_ |
| `ImmutableList` | N/A   | N/A            | N/A          | _O(n)_     | _O(n)_    | _O(1)_ |
| `ImmutableSet.asList()` | N/A   | N/A            | N/A          | _O(1)_     | _O(n)_    | _O(1)_ |

## `Set` ##

| Implementation | `add` | `remove` | `contains` | Iteration | `size` |
|:---------------|:------|:---------|:-----------|:----------|:-------|
| `HashSet` (JDK) | _O(1)_ | _O(1)_   | _O(1)_     | _O(max n)_ `*` | _O(1)_ |
| `LinkedHashSet` (JDK) | _O(1)_ | _O(1)_   | _O(1)_     | _O(n)_    | _O(1)_ |
| `TreeSet` (JDK) | _O(log n)_ | _O(log n)_ | _O(log n)_ | _O(n)_    | _O(1)_ `**` |
| `CopyOnWriteArraySet` (JDK) | _O(n)_ | _O(n)_   | _O(n)_     | _O(n)_    | _O(1)_ |
| `ImmutableSet` | N/A   | N/A      | _O(1)_     | _O(n)_    | _O(1)_ |
| `ImmutableSortedSet` | N/A   | N/A      | _O(log n)_ | _O(n)_    | _O(1)_ |

`*` `HashSet` iteration takes time proportional to the maximum number of elements the `HashSet` has ever had, not proportional to the current number of elements.

`**` `TreeSet.subSet(...).size()` takes time proportional to the size of the subset.

## `Multiset` ##

Note: _n_ is the number of **distinct** elements in the multiset.

| Implementation | Performs like a... | `size()` | `count(E)` | `add(E, int)` | `remove(E, int)` | `setCount(E, int)` | Iterate through `entrySet()` or `elementSet()` |
|:---------------|:-------------------|:---------|:-----------|:--------------|:-----------------|:-------------------|:-----------------------------------------------|
| `HashMultiset` | `HashMap<E, Integer>` | _O(1)_   | _O(1)_     | _O(1)_        | _O(1)_           | _O(1)_             | _O(max n)_ `*`                                 |
| `LinkedHashMultiset` | `LinkedHashMap<E, Integer>` |  _O(1)_  | _O(1)_     | _O(1)_        | _O(1)_           | _O(1)_             | _O(n)_                                         |
| `TreeMultiset` | `TreeMap<E, Integer>` | _O(1)_ `**` | _O(log n)_ | _O(log n)_    | _O(log n)_       | _O(log n)_         | _O(n)_                                         |
| `ConcurrentHashMultiset` | `ConcurrentHashMap<E, AtomicInteger>` | _O(n)_   | _O(1)_     |  _O(1)_       | _O(1)_           |_O(1)_              | _O(n)_                                         |
| `ImmutableMultiset` | `ImmutableMap<E, Integer>` | _O(1)_   | _O(1)_     | _O(1)_        | _O(1)_           | _O(1)_             | _O(n)_                                         |
| `ImmutableSortedMultiset` | `ImmutableSortedMap<E, Integer>` | _O(1)_   | _O(log n)_ | _O(log n)_    | _O(log n)_       | _O(log n)_         | _O(n)_                                         |

`*` Like `HashMap`, the iteration cost through the `entrySet` is linear in the maximum number of elements the `HashMultiset` has ever had, not the number it has now.

`**` `TreeMultiset.subMultiset().size()` takes time _O(log n)_.

## `Multimap` ##

`k` is the number of distinct keys; `n` is the number of distinct entries; `#(key)` is the number of entries associated with `key`.  Where not specified, the asymptotics are equivalent to the "obvious" implementation based on the "Performs like a..." column.

| Implementation | Performs like a... | `size()` | `get(K)` | `put(K, V)` | `containsEntry(K, V)` | Iterate through `entries()` | Iterate through `asMap().entrySet()` |
|:---------------|:-------------------|:---------|:---------|:------------|:----------------------|:----------------------------|:-------------------------------------|
| `ArrayListMultimap` | `HashMap<K, ArrayList<V>>` | _O(1)_   | _O(1)_   | _O(1)_      | _O(`#(key)`)_         | _O(max k + n)_              | _O(max k)_                           |
| `LinkedListMultimap` | `LinkedHashMap<K, LinkedList<V>>` | _O(1)_   | _O(1)_   | _O(1)_      | _O(`#(key)`)_         | _O(n)_                      | _O(k)_                               |
| `HashMultimap` | `HashMap<K, HashSet<V>>` | _O(1)_   | _O(1)_   | _O(1)_      | _O(1)_                | _O(max n)_                  | _O(max k)_                           |
| `LinkedHashMultimap` | `LinkedHashMap<K, LinkedHashSet<V>>` | _O(1)_   |_O(1)_    |_O(1)_       | _O(1)_                | _O(n)_                      | _O(k)_                               |
| `TreeMultimap` | `TreeMap<K, TreeSet<V>>` | _O(1)_   | _O(log k)_ | _O(log k + log `#(key)`)_ | _O(log k + log `#(key)`)_ | _O(n)_                      | _O(k)_                               |
| `ImmutableListMultimap` | `ImmutableMap<K, ImmutableList<V>>` | _O(1)_   | _O(1)_   | N/A         | _O(#(key))_           | _O(n)_                      | _O(k)_                               |
| `ImmutableSetMultimap` | `ImmutableMap<K, ImmutableSet<V>>` | _O(1)_   | _O(1)_   | N/A         | _O(1)_                | _O(n)_                      | _O(k)_                               |