ARPA2 Release Process
=====================

>   *A quick description of how we like to tag our versions and release them.*

Version Tagging
---------------

-   In our versioning systems, we release alpha, beta and RC versions, with
    suggested naming like this:

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    version-X.Y-alphaN
    version-X.Y-betaN
    version-X.Y-RCn
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   All the values of `N` (or `n`) count from 1 up; but `X` and `Y` count from 0
    up; this is what developers tend to do

-   Version tags are always applied to an entry from the master/default branch
    of a versioning system

-   For versions with `X` = `0`, we may choose to skip alpha en beta naming

-   Release numbers may be derived from the version tags:

    -   `version-X.Y-alphaN` would become `X.Y-alphaN`

    -   `version-X.Y-betaN` would become `X.Y-betaN`

    -   `version-X.Y-RCn` would become `X.Y-RCn` by default

    -   package builders may publish `RCn` versions as real versions by
        attaching a subversion to form `X.Y.Z`; there is no constraint on the
        form of `Z`, except that it must increase as the covered `RCn` increases

Release Process (with git)
--------------------------

1.  Annotate your code with the version strings, etcetera

2.  Commit your code

3.  Create a version tag and push it to the publicly visible repository, as in

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    git tag version-0.7-RC3
    git push origin version-0.7-RC3
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

4.  Construct a package for the source code; when using GitHUB, we can now find
    a download location under the “releases” pane; please use `.tar.gz` for
    POSIX, with the only exception of using `.zip` for downloads to Windows
    platforms

5.  Submit the package to Rick, with your PGP signature on it, or refer to a
    known-good download location; Rick will post it on the FTP server, possibly
    renaming it as instructed (applying the aforementioned mapping from tag to
    package version by default)

6.  Rick will sign the package with a detached signature in a `.sig` file using
    PGP; the corresponding PGP key is found on the server as well as in a `CERT`
    record with DNSSEC protection

    -   PGP provides end-to-end protection, which is much more secure than a
        certificate which basically states “you clicked on an email link once
        and now your identity is validated for a few years to come”

    -   PGP signatures can be made offline, thus yielding better protection of
        the signing key than would be possible with an X.509 certificate

    -   Until we see a reason for them, we forego `MD5SUM` or similarly
        weak-check files

    -   This mechanism enables us to do disaster recovery; we can always launch
        a new PGP key and store it in the `CERT` record once more

7.  When Rick is done, you should find your package back on
    `http://download.arpa2.org` — NEVER spread any `https://` links, as that may
    not continue to work in the future.  This especially applies to packaging
    descriptions!  Instead, check the signature and/or checksum as stored in the
    `.sig` as this protects the data rather than the transport connection

8.  When you create packages, you should validate the detached `.sig` before you
    configure a secure hash in the package validation section of your package
    description
