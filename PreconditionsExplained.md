# Preconditions #
Guava provides a number of precondition checking utilities.  We strongly recommend importing these statically.  ([How to do this easily in Eclipse](http://piotrjagielski.com/blog/working-with-static-imports-in-eclipse/).)

Each method has three variants:
  * No extra arguments.  Any exceptions are thrown without error messages.
  * An extra `Object` argument.  Any exception is thrown with the error message `object.toString()`.
  * An extra `String` argument, with an arbitrary number of additional `Object` arguments.  This behaves something like printf, but for GWT compatibility and efficiency, it only allows `%s` indicators.  Example:
```
checkArgument(i >= 0, "Argument was %s but expected nonnegative", i);
checkArgument(i < j, "Expected i < j, but %s > %s", i, j);
```

| Signature (not including extra args) | Description | Exception thrown on failure |
|:-------------------------------------|:------------|:----------------------------|
| <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkArgument(boolean)'><code>checkArgument(boolean)</code></a> | Checks that the `boolean` is `true`.  Use for validating arguments to methods.  | `IllegalArgumentException`  |
| <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkNotNull(T)'><code>checkNotNull(T)</code></a> | Checks that the value is not null.  Returns the value directly, so you can use `checkNotNull(value)` inline. | `NullPointerException`      |
| <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkState(boolean)'><code>checkState(boolean)</code></a> | Checks some state of the object, not dependent on the method arguments.  For example, an `Iterator` might use this to check that `next` has been called before any call to `remove`. | `IllegalStateException`     |
| <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkElementIndex(int, int)'><code>checkElementIndex(int index, int size)</code></a> | Checks that `index` is a valid _element_ index into a list, string, or array with the specified size.  An element index may range from 0 inclusive to `size` **exclusive**.  You don't pass the list, string, or array directly; you just pass its size.<br>Returns <code>index</code>.  <table><thead><th> <code>IndexOutOfBoundsException</code> </th></thead><tbody>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkPositionIndex(int, int)'><code>checkPositionIndex(int index, int size)</code></a> </td><td> Checks that <code>index</code> is a valid <i>position</i> index into a list, string, or array with the specified size.  A position index may range from 0 inclusive to <code>size</code> <b>inclusive</b>.  You don't pass the list, string, or array directly; you just pass its size.<br>Returns <code>index</code>.  </td><td> <code>IndexOutOfBoundsException</code> </td></tr>
<tr><td> <a href='http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/base/Preconditions.html#checkPositionIndexes(int, int, int)'><code>checkPositionIndexes(int start, int end, int size)</code></a> </td><td> Checks that <code>[start, end)</code> is a valid sub range of a list, string, or array with the specified size.  Comes with its own error message.  </td><td> <code>IndexOutOfBoundsException</code> </td></tr></tbody></table>

We preferred rolling our own preconditions checks over e.g. the comparable utilities from Apache Commons for a few reasons.  Piotr Jagielski <a href='http://piotrjagielski.com/blog/google-guava-vs-apache-commons-for-argument-validation/'>discusses</a> why he prefers our utilities, but briefly:<br>
<br>
<ul><li>After static imports, the Guava methods are clear and unambiguous.  <code>checkNotNull</code> makes it clear what is being done, and what exception will be thrown.<br>
</li><li><code>checkNotNull</code> returns its argument after validation, allowing simple one-liners in constructors: <code>this.field = checkNotNull(field)</code>.<br>
</li><li>Simple, varargs "printf-style" exception messages.  (This advantage is also why we recommend continuing to use <code>checkNotNull</code> over <a href='http://docs.oracle.com/javase/7/docs/api/java/util/Objects.html#requireNonNull(java.lang.Object,java.lang.String)'>Objects.requireNonNull</a> introduced in JDK 7.)</li></ul>

We recommend that you split up preconditions into distinct lines, which can help you figure out which precondition failed while debugging.  Additionally, you should provide helpful error messages, which is easier when each check is on its own line.