# Guava Release 08: Release Notes #

Release 08 was released on January 27, 2011.  (See ReleaseHistory.)

[Download it](http://guava-libraries.googlecode.com/files/guava-r08.zip)

Javadocs are included, but more complete javadocs (using the "-linksource" and "-use" javadoc options) are always available on-line:

[Full API Documentation](http://guava-libraries.googlecode.com/svn/tags/release08/javadoc/index.html)

## Using Guava in your project ##

Here are some code/config snippets for how to UseGuavaInYourBuild.

This release is identified in the Maven "central" repository used by maven, ivy, gradle, ant (with maven ant tasks) as [com.google.guava:guava:r08](http://mavencentral.sonatype.com/#artifactdetails%7Ccom.google.guava%7Cguava%7Cr08).  (Note: the GWT artifact is not yet released to the maven central repository.  Stay tuned.)

## API Changes ##

[Full JDiff Report](http://guava-libraries.googlecode.com/svn/tags/release08/javadoc/jdiff/changes.html) of changes since release 07

To build a combined report of the API changes between release 08 and any older release, check out our source tree and run `svn/tags/release08/javadoc/jdiff/jdiff.sh` with the previous release number as argument (example: `jdiff.sh 05`).

## Issues resolved ##

[Issues fixed](http://code.google.com/p/guava-libraries/issues/list?can=1&q=milestone%3DRelease08+status%3DFixed&sort=id+-owner&colspec=ID+Type+Status+Milestone+Summary&nobtn=Update)