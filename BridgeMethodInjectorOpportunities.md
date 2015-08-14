# Where might we use the [Bridge Method Injector](http://bridge-method-injector.infradna.com/) in Guava? #

We're not yet sure whether we want to use it at all, but part of making that decision will be having a list of possible uses in front of us.

Note that any of these changes might introduce source incompatibilities (e.g., for people who call ImmutableSet.of(immediateFuture(...))).

  * Iterables.(various) -> FluentIterable
  * ImmutableMultiset.elementSet -> ImmutableSet
  * Iterators.forArray, singletonIterator -> UnmodifiableListIterator
  * Splitter.split -> List (or FluentIterable if we could depend on collect from base)
  * Sets.filter (etc.) -> SetView
  * Futures.immediate\*Future -> ListenableScheduledFuture?