Guava is a fairly sizable JAR file, and your app may be using only a small portion of it.  If you'd like an easy way to include just the parts of Guava you really need in your own application, we recommend you look into [ProGuard](http://proguard.sourceforge.net/).

Here's a very simple quick-start guide.

First, [download ProGuard](http://sourceforge.net/projects/proguard/files/) and copy `proguard.jar` into your `lib` directory.

Unfortunately, you'll also need to grab [jsr305.jar](http://mvnrepository.com/artifact/com.google.code.findbugs/jsr305), which is not included in the Guava zip distribution because you don't need it under normal circumstances.

Create a file called `myapplication.pro` containing:

```
-injars path/to/myapplication.jar
-injars lib/guava-r07.jar
-libraryjars lib/jsr305.jar
-outjars myapplication-dist.jar

-dontoptimize
-dontobfuscate
-dontwarn sun.misc.Unsafe
-dontwarn com.google.common.collect.MinMaxPriorityQueue

-keepclasseswithmembers public class * {
    public static void main(java.lang.String[]);
}
```

Then use

```
java -jar lib/proguard.jar -libraryjars $JAVA_HOME/jre/lib/rt.jar @myapplication.pro
```

Of course, you may have other entry points or any number of other considerations, so by all means peruse the ProGuard manual to learn more.

Other note: if "myapplication" is actually a _library_, which will be used by other users outside your control (who might also use Guava or use other libraries that do), this procedure is probably not a good idea.  It's much safer if your users who are building applications bring in the Guava dependency separately.  You may want to communicate to them what is the minimum version of Guava you require.  **Unlike most users, you should avoid any Guava APIs marked @Beta**.  An alternative to all of this is to use ProGuard's repackaging feature to create your own private version of Guava that will serve your library alone, but this is clearly not the best solution for application performance.

We would like to hear about your experiences using ProGuard with Guava so we can improve this page.