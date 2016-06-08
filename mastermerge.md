# How to Merge with Master for ARPA2 projects

Currently, we are putting our work out in two forms: Nix and MXE.  We
use Nix for POSIX platforms and MXE for crossover building to Windows.

# Branches

Subprojects usually work in their own branch of these repositories, and
can add it to `master` when done.  But when is something done?

# Merge branch to master in Nix

First, make sure the branch is merged with the current `master`, and
ensure that the only difference with `master` that remains is the work
of that branch.

Then, check the commits in the branch are complete:

  #. Clone the branch to a fresh repository, preferrably on a "fresh" system
  #. Clean the repository and build it

# Merge branch to master in MXE

First, make sure the branch is merged with the current `master`, and
ensure that the only difference with `master` that remains is the work
of that branch.

Then, check the commits in the branch are complete:

  #. Clone the branch to a fresh repository
  #. Perform a `make clean` -- this is expensive, but the caution is worth it
  #. Build the package added or modified in this branch
  #. Iterate over all packages added, doing `make clean` for each, except perhaps when the only things built so far are dependencies of the new-to-build package
  #. When a package was modified that has dependencies, then build all those dependent packages as well

When this all succeeds, we have a suitable candidate for merging into `master`.
