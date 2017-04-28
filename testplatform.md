# APRA2 Test Platforms

> *As developers, we should aim for the same testing platform so we can
> easily use each other's work.  This is a guideline rather than something
> forceful.*

Our aim is to use Nix on a current Debian Stable as our initial platform
for development and testing.

  * Use the [stable Debian](https://www.debian.org/releases/stable/) and
    be sure to setup `/etc/apt/sources.list` with `stable` as your target,
    not `jessie` or anything; this means you can follow the change to a
    new platform automatically.
  * DEPRECATED: Use our [Nix Packages stream](https://github.com/arpa2/nixpkgs),
    possibly specialised to your current environment.
  * For daemons, create a [Docker project](deliverables.html) as deliverable
    once the code becomes useful for sharing.  This enables others to test
    your work.

When reporting problems among ourselves, we should always strive to have
these sources up to date, and be able to reproduce the problem there.

