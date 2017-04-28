# Distribution of Packages

We have a few “forks” of GIT packaging repositories in the ARPA2 project
at GitHub, to help us with distribution / release stuff. Our immediate
concern for now is to release in these two-and-a-half streams:

  * everything should end up in Docker repositories, covering a variety
    of Linux distros
  * DEPRECATED: everything should end up in Nix (covering a variety of Linux
    distros; Nix provides building and packaging; as it turned out, this
    was impossible and we could only service NixOS in practice)
  * some things in MXE (a sort of ports tree for cross-compilation to
    Windows; packaging will be done through MakeNSIS)
  * everything that uses CMake should create packages through
    CPack for selected target platforms (although we haven't
    really defined what those are, and it depends on running
    CPack on the target platform).

The two repositories for this are

  * https://github.com/arpa2/nixpkgs
  * https://github.com/arpa2/mxe
  * (the CPack stuff lives in each individual repo)

Why forking the original repos? Because we may want to have another release
cycle, and perhaps take a stab ahead, for instance by taking out some of
the dependencies in a binary format for the time being (notably, no
Windows). We can do this in our own distro without asking the main
distribution to swallow the same. When things smoothen out, we can
prepare a pull request.

To facilitate several of these things on their way to a PR to co-exist,
We’ve created branches for each porting project. Please push your changes
in there, until you are satisfied that the work is good enough to send a
PR to the original tree. As far as we're concerned dependencies (such as
TLS Pool dependency on GnuTLS) can all go into the same branch (in this
case, for TLS Pool) to retain some overview of the branches and their PR
status.

Just ask if you need other branches (or create them) or
if you need access and we didn’t prepare that for you yet.

Cheers,
-Rick

P.S. I am aware that Nix recipes could be mapped to MXE recipes, at
least in theory, but I don’t know of any tool doing so.  DEPRECATED.

P.P.S. I’m interested if a cross-building + cross-packaging environment
to deliver ready-to-install packages for Mac OS X. Building on Mac OS X
is not desirable, because it implies hardware dependencies and manual
build steps; the same holds for anything based on "download X-code, then
rip llvm out of xxx/bin..." type manual processing. This is why Mac OS X
is not currently an ARPA2 target.

