# User Guide #

The Guava project contains several of Google's core libraries that we rely on in our Java-based projects: collections, caching, primitives support, concurrency libraries, common annotations, string processing, I/O, and so forth.  Each of these tools really do get used every day by Googlers, in production services.

But trawling through Javadoc isn't always the most effective way to learn how to make best use of a library.  Here, we try to provide readable and pleasant explanations of some of the most popular and most powerful features of Guava.

_This wiki is a work in progress, and parts of it may still be under construction._

  * Basic utilities: Make using the Java language more pleasant.
    * [Using and avoiding null](UsingAndAvoidingNullExplained.md): `null` can be ambiguous, can cause confusing errors, and is sometimes just plain unpleasant.  Many Guava utilities reject and fail fast on nulls, rather than accepting them blindly.
    * [Preconditions](PreconditionsExplained.md): Test preconditions for your methods more easily.
    * [Common object methods](CommonObjectUtilitiesExplained.md): Simplify implementing `Object` methods, like `hashCode()` and `toString()`.
    * [Ordering](OrderingExplained.md): Guava's powerful "fluent `Comparator`" class.
    * [Throwables](ThrowablesExplained.md): Simplify propagating and examining exceptions and errors.
  * Collections: Guava's extensions to the JDK collections ecosystem.  These are some of the most mature and popular parts of Guava.
    * [Immutable collections](ImmutableCollectionsExplained.md), for defensive programming, constant collections, and improved efficiency.
    * [New collection types](NewCollectionTypesExplained.md), for use cases that the JDK collections don't address as well as they could: multisets, multimaps, tables, bidirectional maps, and more.
    * [Powerful collection utilities](CollectionUtilitiesExplained.md), for common operations not provided in `java.util.Collections`.
    * [Extension utilities](CollectionHelpersExplained.md): writing a `Collection` decorator?  Implementing `Iterator`?  We can make that easier.
  * [Caches](CachesExplained.md): Local caching, done right, and supporting a wide variety of expiration behaviors.
  * [Functional idioms](FunctionalExplained.md): Used sparingly, Guava's functional idioms can significantly simplify code.
  * Concurrency: Powerful, simple abstractions to make it easier to write correct concurrent code.
    * [ListenableFuture](ListenableFutureExplained.md): Futures, with callbacks when they are finished.
    * [Service](ServiceExplained.md): Things that start up and shut down, taking care of the difficult state logic for you.
  * [Strings](StringsExplained.md): A few extremely useful string utilities: splitting, joining, padding, and more.
  * [Primitives](PrimitivesExplained.md): operations on primitive types, like `int` and `char`, not provided by the JDK, including unsigned variants for some types.
  * [Ranges](RangesExplained.md): Guava's powerful API for dealing with ranges on `Comparable` types, both continuous and discrete.
  * [I/O](IOExplained.md): Simplified I/O operations, especially on whole I/O streams and files, for Java 5 and 6.
  * [Hashing](HashingExplained.md): Tools for more sophisticated hashes than what's provided by `Object.hashCode()`, including Bloom filters.
  * [EventBus](EventBusExplained.md): Publish-subscribe-style communication between components without requiring the components to explicitly register with one another.
  * [Math](MathExplained.md): Optimized, thoroughly tested math utilities not provided by the JDK.
  * [Reflection](ReflectionExplained.md): Guava utilities for Java's reflective capabilities.
  * Tips: Getting your application working the way you want it to with Guava.
    * [Philosophy](PhilosophyExplained.md): what Guava is and isn't, and our goals.
    * [Using Guava in your build](UseGuavaInYourBuild.md), with build systems including Maven, Gradle, and more.
    * [Using ProGuard](UsingProGuardWithGuava.md) to avoid bundling parts of Guava you don't use with your JAR.
    * [Apache Commons equivalents](ApacheCommonCollectionsEquivalents.md), helping you translate code from using Apache Commons Collections.
    * [Compatibility](Compatibility.md), details between Guava versions.
    * [Idea Graveyard](IdeaGraveyard.md), feature requests that have been conclusively rejected.
    * [Friends](FriendsOfGuava.md), open-source projects we like and admire.
    * [HowToContribute](HowToContribute.md), how to contribute to Guava.

**NOTE:** To discuss the contents of this wiki, please just use the guava-discuss mailing list.