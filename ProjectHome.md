

# Migrated to GitHub #

Guava has migrated to a new location at [GitHub](https://github.com/google/guava). Please use it for all new issue reports, etc.


---


The Guava project contains several of Google's core libraries that we rely on in our Java-based projects: collections, caching, primitives support, concurrency libraries, common annotations, string processing, I/O, and so forth.

The latest release is [18.0](Release18.md), released August 25, 2014.

## Start using Guava ##

You can download a JAR at:

  * [guava-18.0.jar](http://search.maven.org/remotecontent?filepath=com/google/guava/guava/18.0/guava-18.0.jar)
  * [guava-gwt-18.0.jar](http://search.maven.org/remotecontent?filepath=com/google/guava/guava-gwt/18.0/guava-gwt-18.0.jar) (for GWT users)

Guava is also available in Maven Central under the following identifiers:

  * [com.google.guava:guava](http://search.maven.org/#artifactdetails%7Ccom.google.guava%7Cguava%7C18.0%7Cbundle)
  * [com.google.guava:guava-gwt](http://search.maven.org/#artifactdetails%7Ccom.google.guava%7Cguava-gwt%7C18.0%7Cbundle) (for GWT users)

Learn more about how to UseGuavaInYourBuild.

Please see the [18.0 release notes](Release18.md) for more details about this release.

## Learn about Guava ##

  * Our users' guide, [GuavaExplained](GuavaExplained.md)
  * Browse [API docs for the most recent release](http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/index.html)
  * Browse [API diffs for the most recent release](http://docs.guava-libraries.googlecode.com/git-history/release/jdiff/changes.html)
  * [Presentation slides focusing on base, primitives, and io](http://guava-libraries.googlecode.com/files/Guava_for_Netflix_.pdf)
  * [Presentation slides focusing on cache](http://guava-libraries.googlecode.com/files/JavaCachingwithGuava.pdf)
  * [Presentation slides focusing on util.concurrent](http://guava-libraries.googlecode.com/files/guava-concurrent-slides.pdf)
  * [A nice collection](http://www.tfnico.com/presentations/google-guava) of other helpful links

## How to communicate with us (and each other) ##

To stay informed and get "tip of the week"-style posts, follow [+Google Guava](https://plus.google.com/118010414872916542489) on Google+.

To just stay informed, period, about important news that every Guava user needs to know, subscribe to [guava-announce](http://groups.google.com/group/guava-announce). It is very low-traffic.

To report a defect or request a feature, click the Issues tab above and enter a new issue.

To get help on a specific question or problem, post a question to [Stack Overflow](http://stackoverflow.com/questions/ask?tags=java+guava) with the tag "guava". We monitor these questions using [this RSS feed](http://stackoverflow.com/feeds/tag?tagnames=guava&sort=newest).)

Keep an eye on project updates using any of these [Project Feeds](http://code.google.com/p/guava-libraries/feeds). (The "Updates" feed is an aggregation of all the others.) You can view our code changes as they happen, and comment on them using the code-review tool.

We have a read-only [mailing list](http://groups.google.com/group/guava-issues) consisting of email notifications of issue tracker activity, which can be easier to follow in a threaded manner than the project feed.

For general discussion that doesn't fit neatly into any of these categories, join our discussion group [guava-discuss](http://groups.google.com/group/guava-discuss).

## Important Warnings ##

Guava contains a strictly compatible **superset** of the old, deprecated [Google Collections Library](http://google-collections.googlecode.com). **You should not use that library anymore.**

APIs marked with the @Beta annotation at the class or method level
are subject to change. They can be modified in any way, or even
removed, in any major release. If your code is a library itself (i.e. it is
used on the CLASSPATH of users outside your own control), you should
not use beta APIs, unless you repackage them (e.g. using [ProGuard](UsingProGuardWithGuava.md)). Here is a current [list of all the beta APIs](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/annotations/class-use/Beta.html).

Serialized forms of ALL objects are subject to change. Do not persist these and assume they can be read by a future version of the library.

Deprecated non-beta APIs will be removed eighteen months after the release in which they are first deprecated. You must fix your usages before this time. If you don't, any manner of breakage might result (you are not guaranteed a compilation error).