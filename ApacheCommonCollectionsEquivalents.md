# <a href='http://commons.apache.org/collections/apidocs/org/apache/commons/collections/CollectionUtils.html'>CollectionUtils</a> (<a href='http://svn.apache.org/viewvc/commons/proper/collections/trunk/src/java/org/apache/commons/collections/CollectionUtils.java?view=markup'>source</a>) #

<dl>
<dt><pre><code>void *addAll*(Collection, Enumeration)</code></pre></dt>
<dd>Iterators.addAll(collection, Iterators.forEnumeration(enumeration))</dd>
<dt><pre><code>void *addAll*(Collection, Iterator)</code></pre></dt>
<dd>Iterators.addAll(collection, iterator)</dd>
<dt><pre><code>void *addAll*(Collection, Object[])</code></pre></dt>
<dd>Collections.addAll(collection, array) (JDK)</dd>
<dt><pre><code>boolean *addIgnoreNull*(Collection, Object)</code></pre></dt>
<dd>if (o != null) { collection.add(o); }</dd>
<dt><pre><code>int *cardinality*(Object, Collection)</code></pre></dt>
<dd>Iterables.frequency(collection, object)</dd>
<dt><pre><code>Collection *collect*(Collection, Transformer)</code></pre></dt>
<dd>newArrayList(Collections2.transform(input, function))</dd>
<dt><pre><code>Collection *collect*(Collection, Transformer, Collection)</code></pre></dt>
<dd>output.addAll(Collections2.transform(input, function))</dd>
<dt><pre><code>Collection *collect*(Iterator, Transformer)</code></pre></dt>
<dd>newArrayList(Iterators.transform(input, function))</dd>
<dt><pre><code>Collection *collect*(Iterator, Transformer, Collection)</code></pre></dt>
<dd>Iterators.addAll(output, Iterators.transform(input, function))</dd>
<dt><pre><code>boolean *containsAny*(Collection coll1, Collection coll2)</code></pre></dt>
<dd><code>!Collections.disjoint(coll1, coll2)</code> (JDK)</dd>
<dt><pre><code>int *countMatches*(Collection, Predicate)</code></pre></dt>
<dd>Iterables.size(Iterables.filter(collection, predicate))</dd>
<dt><pre><code>Collection *disjunction*(Collection, Collection)</code></pre></dt>
<dd>Sets.symmetricDifference(set1, set2)</dd>
<dt><pre><code>boolean *exists*(Collection, Predicate)</code></pre></dt>
<dd>Iterables.any(collection, predicate)</dd>
<dt><pre><code>void *filter*(Collection, Predicate)</code></pre></dt>
<dd>Iterables.removeIf(collection, not(predicate)) (see also Iterables.transform, which creates a view instead of mutating the input)</dd>
<dt><pre><code>Object *find*(Collection, Predicate)</code></pre></dt>
<dd>Iterables.find(collection, predicate)</dd>
<dt><pre><code>void *forAllDo*(Collection, Closure)</code></pre></dt>
<dd>for (Object o : collection) { closure.execute(o); }</dd>
<dt><pre><code>Object *get*(Object, int)</code></pre></dt>
<dd>Iterables.get(o, index), supplemented with calls to entrySet(), forEnumeration(), etc.</dd>
<dt><pre><code>Map *getCardinalityMap*(Collection)</code></pre></dt>
<dd>ImmutableMultiset.copyOf(collection)</dd>
<dt><pre><code>Object *index*(Object, int)</code></pre></dt>
<dd>Iterables.get(o, index), supplemented with calls to keySet(), forEnumeration(), etc.</dd>
<dt><pre><code>Object *index*(Object, Object)</code></pre></dt>
<dd>Iterables.get(o, index), supplemented with calls to entrySet(), forEnumeration(), etc.</dd>
<dt><pre><code>Collection *intersection*(Collection, Collection)</code></pre></dt>
<dd>Sets/Multisets.intersection(a, b)</dd>
<dt><pre><code>boolean *isEmpty*(Collection)</code></pre></dt>
<dd>collection == null || collection.isEmpty()</dd>
<dt><pre><code>boolean *isEqualCollection*(Collection, Collection)</code></pre></dt>
<dd>If both are Sets or Multisets, use equals(); otherwise ImmutableMultiset.copyOf(a).equals(ImmutableMultiset.copyOf(b)</dd>
<dt><pre><code>boolean *isFull*(Collection)</code></pre></dt>
<dd>No equivalent--no BoundedCollection type.</dd>
<dt><pre><code>boolean *isNotEmpty*(Collection)</code></pre></dt>
<dd>collection != null && !collection.isEmpty()</dd>
<dt><pre><code>boolean *isProperSubCollection*(Collection, Collection)</code></pre></dt>
<dd>No equivalent--check that a.size() < b.size() and then use the check described below.</dd>
<dt><pre><code>boolean *isSubCollection*(Collection, Collection)</code></pre></dt>
<dd><code>Multisets.containsOccurrences(ImmutableMultiset.copyOf(coll1), ImmutableMultiset.copyOf(coll2))</code></dd>
<dt><pre><code>int *maxSize*(Collection)</code></pre></dt>
<dd>No equivalent--no BoundedCollection type.</dd>
<dt><pre><code>Collection *predicatedCollection*(Collection, Predicate)</code></pre></dt>
<dd>Constraints.constrainedCollection/List/Set/etc.</dd>
<dt><pre><code>Collection *removeAll*(Collection, Collection)</code></pre></dt>
<dd>newArrayList(Iterables.filter(collection, Predicates.not(Predicates.in(remove))))</dd>
<dt><pre><code>Collection *retainAll*(Collection, Collection)</code></pre></dt>
<dd>newArrayList(Iterables.filter(collection, Predicates.in(retain)))</dd>
<dt><pre><code>void *reverseArray*(Object[])</code></pre></dt>
<dd>Lists.reverse(Arrays.asList(array)) (returns an inverse List view without modifying array)</dd>
<dt><pre><code>Collection *select*(Collection, Predicate)</code></pre></dt>
<dd>newArrayList(Iterables.filter(collection, predicate))</dd>
<dt><pre><code>void *select*(Collection, Predicate, Collection)</code></pre></dt>
<dd>Iterables.addAll(output, Iterables.filter(input, predicate))</dd>
<dt><pre><code>Collection *selectRejected*(Collection, Predicate)</code></pre></dt>
<dd>newArrayList(Iterables.filter(collection, Predicates.not(predicate)))</dd>
<dt><pre><code>void *selectRejected*(Collection, Predicate, Collection)</code></pre></dt>
<dd>Iterables.addAll(output, Iterables.filter(input, Predicates.not(predicate)))</dd>
<dt><pre><code>int *size*(Object)</code></pre></dt>
<dd>Collection/Map.size(), array.length, Iterables/Iterators.size (with forEnumeration() if necessary)</dd>
<dt><pre><code>boolean *sizeIsEmpty*(Object)</code></pre></dt>
<dd>Collection/Map.isEmpty(), array.length == 0, Iterables/Iterators.isEmpty (with forEnumeration() if necessary)</dd>
<dt><pre><code>Collection *subtract*(Collection, Collection)</code></pre></dt>
<dd>No equivalent--create an ArrayList containing a and then call remove on it for each element in b.</dd>
<dt><pre><code>Collection *synchronizedCollection*(Collection)</code></pre></dt>
<dd>Collections.synchronizedCollection(collection) (JDK)</dd>
<dt><pre><code>void *transform*(Collection, Transformer)</code></pre></dt>
<dd>No equivalent for transforming a Collection in place... not very useful. Prefer transformed views (Lists/Collections2.transform) or copies of them.</dd>
<dt><pre><code>Collection *transformedCollection*(Collection, Transformer)</code></pre></dt>
<dd>No equivalent for transforming Objects that are added to a Collection... a ForwardingCollection could easily handle this, though.</dd>
<dt><pre><code>Collection *typedCollection*(Collection, Class)</code></pre></dt>
<dd>Collections.checkedCollection/Set/List/etc. (JDK)</dd>
<dt><pre><code>Collection *union*(Collection, Collection)</code></pre></dt>
<dd>Sets.union(a, b)</dd>
<dt><pre><code>Collection *unmodifiableCollection*(Collection)</code></pre></dt>
<dd>Collections.unmodifiableCollection/Set/List/etc. (JDK) Consider ImmutableCollection types if you want immutability.</dd>
</dl>

# Additions #

I can't find a way to make the Wiki world-writable, so post any additions as comments.  I'll check for new ones every so often and approve their authors as Wiki editors.