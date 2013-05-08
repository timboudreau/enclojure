Enclojure
=========

This project is a fork of Eric Thorsen's [original Enclojure project](https://github.com/EricThorsen/enclojure).  I have got it building and working on
NetBeans 7.3.

Builds are available [on timboudreau.com](http://timboudreau.com/builds/job/Enclojure).

Big Honkin' Caveats
-------------------

This module cannot coexist with NetBeans Hibernate support, due to a conflict
over SLF4J.  So, disable hibernate before you try to use this module. This is probably
solvable, but would probably require changes in NetBeans Hibernate module to split
out SLF4J into a wrapper both could depend on.

There are other issues - the naive approach to integrating a scripting
language into an IDE is to load up user code inside the IDE's VM.  In practice
this has disasterous consequences - user code gets to wreak havoc with the IDE.
This plugin does exactly that, so caveat emptor.

The SLF4J problem in more detail:

 * Clojure really, really wants to use the system classloader (you can assign 
it, but there is no obvious entry point)
 * The Clojure Maven Plugin does not run in Maven's compile scope, so SLF4J cannot be a compile-scope dependency - so it ends up on the module classpath
 * The Hibernate plugin also includes a copy of SLF4J
 * The classloader Clojure ends up using has two copies of SLF4J on it, and for very sane reasons refuses to arbitrarily choose one or the other

There is some code that assumes other Clojure code that initializes the REPL window will have run before a project or the editor is opened;  in practice windows are
lazy-loaded, so having it open is a good idea.


