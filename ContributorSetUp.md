# Setting Up Your Clone #

_TODO: pictures_

_TODO: add current as-accurate-as-possible Eclipse formatter download_

Eventually, you'll be dealing with three clones of the Guava repository:

  1. The original repository, at `https://code.google.com/p/guava-libraries/`
  1. Your online repository, for the Guava team to pull from.
  1. Your local repository, on your machine to develop on.

If you want to host your online repository on Google Code, then at the bottom of [this page](http://code.google.com/p/guava-libraries/source/checkout), create a clone.  This is on Google Code, and it's easiest for the Guava team to pull from.

Assuming you're using Eclipse, follow these instructions to get a Guava clone working with both Git and Maven:
  1. Make sure you have the EGit and m2eclipse plugins installed in Eclipse.
  1. Open the "Git Repositories" perspective.  On the "Git Repositories" tab, click the "Clone a Git Repository" button.
  1. Paste `https://code.google.com/p/guava-libraries/` into the URI, and give your `googlecode.com` username and [password](https://code.google.com/hosting/settings).
  1. Check out all branches of the source.

You should now have a local clone of Guava on your machine, albeit it's not a Java or Maven project.  We now want it to get built.

  1. Run `mvn install antrun:run enforcer:enforce`, if those plugins are not already installed.
  1. In Eclipse, use "File/Import", and choose "Maven/Existing Maven Projects".
  1. Designate the root directory of your repository -- typically `~/git/guava-libraries`.
  1. Select the root `pom.xml`, labeled `guava-parent`.  If you encounter Maven problems, choose "Resolve All Later."

You're almost done!  You should now have several projects in Eclipse:

  * `guava-parent`, the "root" project, with which you'll do all your interacting with Git: commits, pushes, pulls, rebases, checkouts, etc.
  * `guava`: the main source of Guava.
  * `guava-gwt`: the GWT-compatible supersource that helps make Guava GWT-compatible.
  * `guava-testlib`: the test suite builders that assemble customized, exhaustive test suites for collection implementations.
  * `guava-tests`: the tests for Guava.

Right-click on `guava-parent`, and go to `Team/Share Project`.  Select "Git," and choose "Use repository in parent folder of project," which will tell Eclipse to use the preexisting repository that already has all the Guava history and the connection to the remote master branch.

Now you have the Guava source checked out into Eclipse, and working with Maven!  Yay!

# Build and Test #

_TODO: fill this in_

`mvn clean install`

Pass `-Dmaven.test.skip=true` to skip tests.

# Code Reviews #

The Guava team uses http://codereview.appspot.com to do code reviews for external submissions.  (It's inspired by the Google-internal code review system.)

First, you'll need to get your Google account set up at http://codereview.appspot.com.  Next, follow the instructions [here](http://code.google.com/p/rietveld/wiki/CodeReviewHelp).  Once you've downloaded [upload.py](http://codereview.appspot.com/static/upload.py), the usual command line to upload a change is
```
upload.py --rev master -r <reviewer email> --send_mail
```
from your root Guava directory -- typically `~/git/guava-libraries/`.

Once you get an LGTM from your reviewer, send your reviewer a link to your change, typically in the form of a Guava clone and the branch with your change.  (This is typically the Google Code clone, but it can also be hosted on e.g. GitHub.)  Your reviewer will make sure the change is imported internally and then mirrored back out.

The first time you submit code to Guava, you may need to e-sign a SLA.  It's quite painless.  Ask your reviewer for details once you receive an LGTM.