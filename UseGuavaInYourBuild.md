Developers may use several different kinds of build system to compile/package their code.  Guava can be used with any of them.  For any code snippet below, please substitute the version given with the version of Guava you wish to use.

# Maven #

[Apache Maven](http://maven.apache.org) is a system for building projects and source code into deployment binaries, executing tests, and other software assembly lifecycle actions.  It supports managed dependencies from central and private repositories.  Guava is deployed to the common maven repositories, so to use it, you need only add the following snippet to the `<dependencies />` section of the project's pom.xml file.

```
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>12.0</version>
</dependency>
```

JSR-305 note: If you are using JSR-305 annotations (Guava's only dependency) in your own code, you must declare that dependency directly.

Scala note: The Scala compiler, unlike the Java compiler, <a href='https://issues.scala-lang.org/browse/SI-5420'>requires that annotations used by a library be available when compiling against that library</a>.  If you are compiling with Scala, you must <a href='http://stackoverflow.com/a/10013226/28465'>declare a dependency on JSR-305</a>.

# Gradle #

[Gradle](http://www.gradle.org/) is a build system that uses a domain specific language to construct instructions on how to build, test, assemble, deploy software from source.  It supports managed dependencies and can make use of the maven repository for software artifacts.  In order to use guava from Gradle, one must ensure the maven standard repositories are available like so:

```
repositories {
  mavenCentral()
}
```

Then one can add the guava dependency in the dependencies section like so:

```
dependencies {
  compile group: 'com.google.guava', name: 'guava', version: '12.0'
}
```

JSR-305 note: If you are using JSR-305 annotations (Guava's only dependency) in your own code, you must declare that dependency directly.

Scala note: The Scala compiler, unlike the Java compiler, <a href='https://issues.scala-lang.org/browse/SI-5420'>requires that annotations used by a library be available when compiling against that library</a>.  If you are compiling with Scala, you must <a href='http://stackoverflow.com/a/10013226/28465'>declare a dependency on JSR-305</a>.

# Ivy #

[Ivy](http://ant.apache.org/ivy/) is a dependency management system for builds.  Ivy users should add the following line to the dependencies section of the ivy.xml file for any module that depends on Guava:

```
<dependency org="com.google.guava" name="guava" rev="12.0" />
```

and make sure that the "public" resolver is used.

JSR-305 note: If you are using JSR-305 annotations (Guava's only dependency) in your own code, you must declare that dependency directly.

Scala note: The Scala compiler, unlike the Java compiler, <a href='https://issues.scala-lang.org/browse/SI-5420'>requires that annotations used by a library be available when compiling against that library</a>.  If you are compiling with Scala, you must <a href='http://stackoverflow.com/a/10013226/28465'>declare a dependency on JSR-305</a>.

# Buildr #

With [Buildr](http://buildr.apache.org), include this:

```
compile.with 'com.google.guava:guava:jar:10.0.1'
```

JSR-305 note: If you are using JSR-305 annotations (Guava's only dependency) in your own code, you must declare that dependency directly.

Scala note: The Scala compiler, unlike the Java compiler, <a href='https://issues.scala-lang.org/browse/SI-5420'>requires that annotations used by a library be available when compiling against that library</a>.  If you are compiling with Scala, you must <a href='http://stackoverflow.com/a/10013226/28465'>declare a dependency on JSR-305</a>.

# Manual Dependencies #

You can also just manually download JARs for the classes, sources and javadocs. See the appropriate release page, e.g. [Release10](Release10.md).

# What about GWT? #

In all the examples above, leave "com.google.guava" as it is but replace the other occurrence of "guava" with "guava-gwt".