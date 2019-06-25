ARPA2 CMake Usage
=================

>   *Short description of ARPA2 CMake best-practices.*

CMake
-----

Most ARPA2 (sub-)projects and products are use [CMake][CMake] for configuration and
building; CMake is a meta-build system, that generates files for
an actual build system that does the work (e.g. CMake generates Makefiles
or ninja configuration, and you run make or ninja to do the build itself).
In this way it is comparable to autotools, but seems to be a nicer solution
overall.

This document describes the way we use CMake and CMake modules.


CMake Library
-------------

Most ARPA2 (sub-)projects have a `cmake/` directory in the top-level. This
is where the CMake modules that the project uses, live. There is a central
library of CMake modules, too, in the [*arpa2cm* repository][arpa2cm]. That
is where the canonical versions of the modules live. Individual projects
are expected to use the *arpa2cm* CMake modules, and write their own modules
(in their own `cmake/` directory) in exceptional cases.


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
- long lists (or function calls) put the closing parenthesis on a line
  by itself, indented to match the start of the call.


CMake Sample
------------

See the Quick-DER repository for a sample `CMakeLists.txt`, since that is
a fairly straightfoward one, and extensively documented.


[CMake]: https://cmake.org/cmake/help/v3.2/
[arpa2cm]: https://github.com/arpa2/arpa2cm/
