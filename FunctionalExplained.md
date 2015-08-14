# Caveats #
As of Java 7, functional programming in Java can only be approximated through awkward and verbose use of anonymous classes.  This is expected to change in Java 8, but Guava is currently aimed at users of Java 5 and above.

_Excessive_ use of Guava's functional programming idioms can lead to verbose, confusing, unreadable, and inefficient code.  These are by far the most easily (and most commonly) abused parts of Guava, and when you go to preposterous lengths to make your code "a one-liner," the Guava team weeps.

Compare the following code:
```
Function<String, Integer> lengthFunction = new Function<String, Integer>() {
  public Integer apply(String string) {
    return string.length();
  }
};
Predicate<String> allCaps = new Predicate<String>() {
  public boolean apply(String string) {
    return CharMatcher.JAVA_UPPER_CASE.matchesAllOf(string);
  }
};
Multiset<Integer> lengths = HashMultiset.create(
  Iterables.transform(Iterables.filter(strings, allCaps), lengthFunction));
```
or the `FluentIterable` version
```
Multiset<Integer> lengths = HashMultiset.create(
  FluentIterable.from(strings)
    .filter(new Predicate<String>() {
       public boolean apply(String string) {
         return CharMatcher.JAVA_UPPER_CASE.matchesAllOf(string);
       }
     })
    .transform(new Function<String, Integer>() {
       public Integer apply(String string) {
         return string.length();
       }
     }));
```
with:
```
Multiset<Integer> lengths = HashMultiset.create();
for (String string : strings) {
  if (CharMatcher.JAVA_UPPER_CASE.matchesAllOf(string)) {
    lengths.add(string.length());
  }
}
```

Even using static imports, even if the Function and the Predicate declarations are moved to a different file, the first implementation is less concise, less readable, and less efficient.

Imperative code should be your _default_, your _first choice_ as of Java 7.  You should not use functional idioms unless you are _absolutely_ sure of one of the following:

  * Use of functional idioms will result in _net_ savings of lines of code for your entire project.  In the example above, the "functional" version used 11 lines, the imperative version 6.  Moving the definition of a function to another file, or a constant, does not help.
  * For efficiency, you need a lazily computed view of the transformed collection and cannot settle for an explicitly computed collection.  Additionally, you have read and reread Effective Java, item 55, and besides following those instructions, you have actually done benchmarking to prove that this version is faster, and can cite  numbers to prove it.

Please be sure, when using Guava's functional utilities, that the traditional imperative way of doing things isn't more readable.  Try writing it out.  Was that so bad?  Was that more readable than the preposterously awkward functional approach you were about to try?

# Functions and Predicates #
This article discusses only those Guava features dealing directly with `Function` and `Predicate`.  Some other utilities are associated with the "functional style," such as concatenation and other methods which return views in constant time.  Try looking in the [collection utilities](CollectionUtilitiesExplained.md) article.

Guava provides two basic "functional" interfaces:
  * `Function<A, B>`, which has the single method `B apply(A input)`.  Instances of `Function` are generally expected to be referentially transparent -- no side effects -- and to be consistent with equals, that is, `a.equals(b)` implies that `function.apply(a).equals(function.apply(b))`.
  * `Predicate<T>`, which has the single method `boolean apply(T input)`.  Instances of `Predicate` are generally expected to be side-effect-free and consistent with equals.

### Special predicates ###
Characters get their own specialized version of `Predicate`, <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/CharMatcher.html'><code>CharMatcher</code></a>, which is typically more efficient and more useful for those needs. `CharMatcher` already implements `Predicate<Character>`, and can be used correspondingly, while conversion from a `Predicate` to a `CharMatcher` can be done using <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/CharMatcher.html#forPredicate(com.google.common.base.Predicate)'><code>CharMatcher.forPredicate</code></a>.  Consult [the CharMatcher article](StringsExplained#CharMatcher.md) for details.

Additionally, for comparable types and comparison-based predicates, most needs can be fulfilled using the `Range` type, which implements an immutable interval.  The `Range` type implements `Predicate`, testing containment in the range.  For example, `Ranges.atMost(2)` is a perfectly valid `Predicate<Integer>`.  More details on using ranges can be found [in the corresponding article](RangesExplained.md).

### Manipulating Functions and Predicates ###

Simple `Function` construction and manipulation methods are provided in [Functions](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Functions.html), including

| <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Functions.html#forMap(java.util.Map)'><code>forMap(Map&lt;A, B&gt;)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Functions.html#compose(com.google.common.base.Function, com.google.common.base.Function)'><code>compose(Function&lt;B, C&gt;, Function&lt;A, B&gt;)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Functions.html#constant(E)'><code>constant(T)</code></a> |  <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Functions.html#identity()'><code>identity()</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Functions.html#toStringFunction()'><code>toStringFunction()</code></a> |
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|

Consult the Javadoc for details.

There are considerably more construction and manipulation methods available in [Predicates](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Predicates.html), but a sample includes:

| <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#instanceOf(java.lang.Class)'><code>instanceOf(Class)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#assignableFrom(java.lang.Class)'><code>assignableFrom(Class)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#contains(java.util.regex.Pattern)'><code>contains(Pattern)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#in(java.util.Collection)'><code>in(Collection)</code></a> |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#isNull()'><code>isNull()</code></a>                             | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#alwaysFalse()'><code>alwaysFalse()</code></a>                           | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#alwaysTrue()'><code>alwaysTrue()</code></a>                           | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#equalTo(T)'><code>equalTo(Object)</code></a>              |
| <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#compose(com.google.common.base.Predicate, com.google.common.base.Function)'><code>compose(Predicate, Function)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#and(com.google.common.base.Predicate...)'><code>and(Predicate...)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#or(com.google.common.base.Predicate...)'><code>or(Predicate...)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html#not(com.google.common.base.Predicate)'><code>not(Predicate)</code></a> |

Consult the Javadoc for details.

# Using #
Guava provides many tools to manipulate collections using functions and predicates.  These can typically be found in the collection utility classes `Iterables`, `Lists`, `Sets`, `Maps`, `Multimaps`, and the like.

## Predicates ##
The most basic use of predicates is to filter collections.  All Guava filter methods return _views_.

| Collection type | Filter method |
|:----------------|:--------------|
| `Iterable`      | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#filter(java.lang.Iterable, com.google.common.base.Predicate)'><code>Iterables.filter(Iterable, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#filter(com.google.common.base.Predicate)'><code>FluentIterable.filter(Predicate)</code></a> |
| `Iterator`      | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#filter(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.filter(Iterator, Predicate)</code></a> |
| `Collection`    | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Collections2.html#filter(java.util.Collection, com.google.common.base.Predicate)'><code>Collections2.filter(Collection, Predicate)</code></a> |
| `Set`           | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Sets.html#filter(java.util.Set, com.google.common.base.Predicate)'><code>Sets.filter(Set, Predicate)</code></a> |
| `SortedSet`     | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Sets.html#filter(java.util.SortedSet, com.google.common.base.Predicate)'><code>Sets.filter(SortedSet, Predicate)</code></a> |
| `Map`           | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#filterKeys(java.util.Map, com.google.common.base.Predicate)'><code>Maps.filterKeys(Map, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#filterValues(java.util.Map, com.google.common.base.Predicate)'><code>Maps.filterValues(Map, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#filterEntries(java.util.Map, com.google.common.base.Predicate)'><code>Maps.filterEntries(Map, Predicate)</code></a> |
| `SortedMap`     | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#filterKeys(java.util.SortedMap, com.google.common.base.Predicate)'><code>Maps.filterKeys(SortedMap, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#filterValues(java.util.SortedMap, com.google.common.base.Predicate)'><code>Maps.filterValues(SortedMap, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#filterEntries(java.util.SortedMap, com.google.common.base.Predicate)'><code>Maps.filterEntries(SortedMap, Predicate)</code></a> |
| `Multimap`      | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#filterKeys(com.google.common.collect.Multimap, com.google.common.base.Predicate)'><code>Multimaps.filterKeys(Multimap, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#filterValues(com.google.common.collect.Multimap, com.google.common.base.Predicate)'><code>Multimaps.filterValues(Multimap, Predicate)</code></a> | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#filterEntries(com.google.common.collect.Multimap, com.google.common.base.Predicate)'><code>Multimaps.filterEntries(Multimap, Predicate)</code></a> |

`*` A filtered `List` view is omitted, because operations such as `get(int)` could not be supported efficiently.  Instead, use `Lists.newArrayList(Collections2.filter(list, predicate))` to make a copy.

Other than simple filtering, Guava provides a number of additional utilities to manipulate iterables with predicates -- typically in the <a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/Iterables.html'><code>Iterables</code></a> utility class, and as "fluent" methods on a <a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html'><code>FluentIterable</code></a>.

| `Iterables` Signature | Explanation| See also |
|:----------------------|:-----------|:---------|
| <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#all(java.lang.Iterable, com.google.common.base.Predicate)'><code>boolean all(Iterable, Predicate)</code></a> | Do all the elements satisfy the predicate? Lazy: if it finds an element failing the predicate, doesn't iterate further. | <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#all(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.all(Iterator, Predicate)</code></a><br><a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#allMatch(com.google.common.base.Predicate)'><code>FluentIterable.allMatch(Predicate)</code></a> <br>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#any(java.lang.Iterable, com.google.common.base.Predicate)'><code>boolean any(Iterable, Predicate)</code></a> </td><td> Do any of the elements satisfy the predicate? Lazy: only iterates until it finds an element satisfying the predicate. </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#any(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.any(Iterator, Predicate)</code></a><br><a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#anyMatch(com.google.common.base.Predicate)'><code>FluentIterable.anyMatch(Predicate)</code></a> </td></tr>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#find(java.lang.Iterable, com.google.common.base.Predicate)'><code>T find(Iterable, Predicate)</code></a> </td><td> Finds and returns an element satisfying the predicate, or throws a <code>NoSuchElementException</code>. </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#find(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.find(Iterator, Predicate)</code></a><br> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#find(java.lang.Iterable, com.google.common.base.Predicate, T)'><code>Iterables.find(Iterable, Predicate, T default)</code></a><br><a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#find(java.util.Iterator, com.google.common.base.Predicate, T)'><code>Iterators.find(Iterator, Predicate, T default)</code></a> </td></tr>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#tryFind(java.lang.Iterable, com.google.common.base.Predicate)'><code>Optional&lt;T&gt; tryFind(Iterable, Predicate)</code></a> </td><td> Returns an element satisfying the predicate, or <code>Optional.absent()</code>. </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#tryFind(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.tryFind(Iterator, Predicate)</code></a><br><a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#firstMatch(com.google.common.base.Predicate)'><code>FluentIterable.firstMatch(Predicate)</code></a><br><a href='UsingAndAvoidingNullExplained#Optional.md'>Optional</a> </td></tr>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#indexOf(java.lang.Iterable, com.google.common.base.Predicate)'><code>indexOf(Iterable, Predicate)</code></a> </td><td> Returns the index of the first element of the iterable satisfying the predicate, or -1 if no such element could be found. </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#indexOf(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.indexOf(Iterator, Predicate)</code></a> </td></tr>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#removeIf(java.lang.Iterable, com.google.common.base.Predicate)'><code>removeIf(Iterable, Predicate)</code></a> </td><td> Removes all elements satisfying the predicate, using the <code>Iterator.remove()</code> method. </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#removeIf(java.util.Iterator, com.google.common.base.Predicate)'><code>Iterators.removeIf(Iterator, Predicate)</code></a> </td></tr></tbody></table>

<h2>Functions</h2>
By far the most common use of functions is transforming collections.  All Guava transform methods return views of the original collection.<br>
<br>
<table><thead><th> Collection type </th><th> Transform method </th></thead><tbody>
<tr><td> <code>Iterable</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterables.html#transform(java.lang.Iterable, com.google.common.base.Function)'><code>Iterables.transform(Iterable, Function)</code></a> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release12/javadoc/com/google/common/collect/FluentIterable.html#transform(com.google.common.base.Function)'><code>FluentIterable.transform(Function)</code></a> </td></tr>
<tr><td> <code>Iterator</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Iterators.html#transform(java.util.Iterator, com.google.common.base.Function)'><code>Iterators.transform(Iterator, Function)</code></a> </td></tr>
<tr><td> <code>Collection</code> </td><td>  <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Collections2.html#transform(java.util.Collection, com.google.common.base.Function)'><code>Collections2.transform(Collection, Function)</code></a> </td></tr>
<tr><td> <code>List</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Lists.html#transform(java.util.List, com.google.common.base.Function)'><code>Lists.transform(List, Function)</code></a> </td></tr>
<tr><td> <code>Map*</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#transformValues(java.util.Map, com.google.common.base.Function)'><code>Maps.transformValues(Map, Function)</code></a> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#transformEntries(java.util.Map, com.google.common.collect.Maps.EntryTransformer)'><code>Maps.transformEntries(Map, EntryTransformer)</code></a> </td></tr>
<tr><td> <code>SortedMap*</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#transformValues(java.util.SortedMap, com.google.common.base.Function)'><code>Maps.transformValues(SortedMap, Function)</code></a> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.html#transformEntries(java.util.SortedMap, com.google.common.collect.Maps.EntryTransformer)'><code>Maps.transformEntries(SortedMap, EntryTransformer)</code></a> </td></tr>
<tr><td> <code>Multimap*</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#transformValues(com.google.common.collect.Multimap, com.google.common.base.Function)'><code>Multimaps.transformValues(Multimap, Function)</code></a> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#transformEntries(com.google.common.collect.Multimap, com.google.common.collect.Maps.EntryTransformer)'><code>Multimaps.transformEntries(Multimap, EntryTransformer)</code></a> </td></tr>
<tr><td> <code>ListMultimap*</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#transformValues(com.google.common.collect.ListMultimap, com.google.common.base.Function)'><code>Multimaps.transformValues(ListMultimap, Function)</code></a> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimaps.html#transformEntries(com.google.common.collect.ListMultimap, com.google.common.collect.Maps.EntryTransformer)'><code>Multimaps.transformEntries(ListMultimap, EntryTransformer)</code></a> </td></tr>
<tr><td> <code>Table</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Tables.html#transformValues(com.google.common.collect.Table, com.google.common.base.Function)'><code>Tables.transformValues(Table, Function)</code></a> </td></tr></tbody></table>

<code>*</code> <code>Map</code> and <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Multimap.html'><code>Multimap</code></a> have special methods that accept an <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Maps.EntryTransformer.html'><code>EntryTransformer&lt;K, V1, V2&gt;</code></a>, which associates keys with a new value computed from both the original value and the key, instead of just the value.<br>
<br>
<code>**</code> A transform operation for <code>Set</code> is omitted, since an efficient <code>contains(Object)</code> operation could not be supported.  Instead, use <code>Sets.newHashSet(Collections2.transform(set, function))</code> to create a copy of a transformed set.<br>
<br>
<pre><code>List&lt;String&gt; names;<br>
Map&lt;String, Person&gt; personWithName;<br>
List&lt;Person&gt; people = Lists.transform(names, Functions.forMap(personWithName));<br>
</code></pre>

<pre><code>ListMultimap&lt;String, String&gt; firstNameToLastNames;<br>
// maps first names to all last names of people with that first name<br>
<br>
ListMultimap&lt;String, String&gt; firstNameToName = Multimaps.transformEntries(firstNameToLastNames,<br>
  new EntryTransformer&lt;String, String, String&gt; () {<br>
    public String transformEntry(String firstName, String lastName) {<br>
      return firstName + " " + lastName;<br>
    }<br>
  });<br>
</code></pre>

Types that can be "composed" with functions include:<br>
<br>
<table><thead><th> <code>Ordering</code> </th><th> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/collect/Ordering.html#onResultOf(com.google.common.base.Function)'><code>Ordering.onResultOf(Function)</code></a> </th></thead><tbody>
<tr><td> <code>Predicate</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Predicates.html#compose(com.google.common.base.Predicate, com.google.common.base.Function)'><code>Predicates.compose(Predicate, Function)</code></a> </td></tr>
<tr><td> <code>Equivalence</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Equivalence.html#onResultOf(com.google.common.base.Function)'><code>Equivalence.onResultOf(Function)</code></a> </td></tr>
<tr><td> <code>Supplier</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Suppliers.html#compose(com.google.common.base.Function, com.google.common.base.Supplier)'><code>Suppliers.compose(Function, Supplier)</code></a> </td></tr>
<tr><td> <code>Function</code> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Functions.html#compose(com.google.common.base.Function, com.google.common.base.Function)'><code>Functions.compose(Function, Function)</code></a> </td></tr></tbody></table>

Additionally, the <a href='ListenableFutureExplained.md'>ListenableFuture</a> API supports transforming listenable futures.  <code>Futures</code> also provides methods accepting an <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/util/concurrent/AsyncFunction.html'><code>AsyncFunction</code></a>, a variation on <code>Function</code> that allows values to be computed asynchronously.<br>
<br>
<table><thead><th> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/util/concurrent/Futures.html#transform(com.google.common.util.concurrent.ListenableFuture, com.google.common.base.Function)'><code>Futures.transform(ListenableFuture, Function)</code></a> </th><th> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/util/concurrent/Futures.html#transform(com.google.common.util.concurrent.ListenableFuture, com.google.common.base.Function, java.util.concurrent.Executor)'><code>Futures.transform(ListenableFuture, Function, Executor)</code></a> </th></thead><tbody>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/util/concurrent/Futures.html#transform(com.google.common.util.concurrent.ListenableFuture, com.google.common.util.concurrent.AsyncFunction)'><code>Futures.transform(ListenableFuture, AsyncFunction)</code></a> </td><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/util/concurrent/Futures.html#transform(com.google.common.util.concurrent.ListenableFuture, com.google.common.util.concurrent.AsyncFunction, java.util.concurrent.Executor)'><code>Futures.transform(ListenableFuture, AsyncFunction, Executor)</code></a> </td></tr>