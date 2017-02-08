ARPA2 CMake Usage
=================

>   *Short description of ARPA2 CMake best-practices.*

CMake
-----

Most ARPA2 (sub-)projects and products are being converted to building
with CMake [1]; CMake is a meta-build system, that generates files for
an actual build system that does the work (e.g. CMake generates Makefiles
or ninja configuration, and you run make or ninja to do the build itself).
In this way it is comparable to autotools, but seems to be a nicer solution
overall.

This document describes the way we use CMake and CMake modules.

[1] https://cmake.org/cmake/help/v3.2/

CMake Library
-------------

Most ARPA2 (sub-)projects have a cmake/ directory in the top-level. This
is where the CMake modules that the project uses, live. There is a central
library of CMake modules, too, in the howto repository (this one). This
is where the canonical versions of the modules live. Individual projects
are expected to copy the modules they use to their own cmake/ directory
(and possibly update occasionally as the library is updated). This is
normal for CMake modules (unless you take an approach like KDE's 
extra-cmake-modules, which is a separate package and distribution
of a library of CMake modules).

The library contains the following modules:

 - MacroEnsureOutOfSourceBuild (taken from KDE ECM, actually)
 

CMake Coding Style
------------------

In general, we follow the coding style already established in any given
CMake file. For new code, or as a style-guide, we use:

- space after a CMake command before the parenthesis, e.g.

      set (foo 1)
- use mixed-case names for packages and project names, e.g. "LillyDAP",
  instead of making them all-caps.
- for project-level variables, use all-caps.
- for (macro-)internal variables, use an underscore and all-lower, e.g. `_foo`
 
 
CMake Sample
------------

See the Quick-DER repository for a sample `CMakeLists.txt`, since that is
a fairly straightfoward one, and extensively documented.
