# ARPA2 Deliverables Platform

> *We develop and test, initially for Debian Stable.  This is also how
> we intend to produce deliverables, usually in Docker Images.*

While developing and testing, the platform used can be pinned to the
then-current version named Debian Stable.  Ideally however, the code
moves galantly along version upgrades, to new releases.

For Deliverables, we aim to bind more dynamically to Debian Stable,
so we can quickly rebuild the environment after an upgrade to the
platform, and inherit its benefits (such as security patches).

Deliverables should be widely usable, and will be released as
Docker Images.  The basis is setup with the *name*
Debian Stable, so it will route to whatever is current in
[the repository](https://hub.docker.com/_/debian/) for Docker.
This also happens to be the
[advised basic image](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#general-guidelines-and-recommendations)
by Docker.

## Dockerfiles

Our `Dockerfile`s should therefore look like this:
```
# Dockerfile for Nginx-served index.html
#
# From: John Doe <john.doe@example.com>


# ARPA2 base image is the moving target Debian Stable
FROM debian:stable

# Install required packages
RUN \
	apt-get update && \
	apt-get -y upgrade && \
	apt-get install -y nginx

# Add files relative to this Dockerfile
ADD index.html /var/www/index.html

# Set environment variables
ENV HOME /var/www

# Define working directory
WORK /var/www

# Define default command
CMD ["nginx"]
```

We can publish these files in the `arpa2` project, where they shall be
named `docker-XXX` where `XXX` is the project name itself.


## Shared environment

We may create an intermediate layer with an ARPA2 filesystem, based on the
Debian Stable environment, holding the software we tend te rely on.  This
may include both external work and our own libraries:

  * [TLS Pool](https://github.com/arpa2/tlspool)
  * [SteamWorks](https://github.com/arpa2/steamworks)
  * [Quick DER](https://github.com/vanrein/quick-der)
  * [LillyDAP](https://github.com/vanrein/lillydap)
  * MIT Kerberos with [KXOVER](https://github.com/arpa2/kxover)

At a later point, we may decide to setup a number of minimal environments for various use cases:

  * Identity hosting platform (IdentityHub)

      - [IdentityHub components](http://internetwide.org/images/idhub-block-diagram.png):
      - [TLS Pool](https://github.com/arpa2/tlspool)
      - [SteamWorks Crank](https://github.com/arpa2/steamworks)
      - [Quick DER](https://github.com/vanrein/quick-der)
      - [LillyDAP](https://github.com/vanrein/lillydap)
      - MIT Kerberos with [KXOVER](https://github.com/arpa2/kxover)

  * Service hosting platform (ServiceHub)

      - [TLS Pool](https://github.com/arpa2/tlspool)
      - [SteamWorks Pulley](https://github.com/arpa2/steamworks)
      - Authorisation against IdentityHub
      - [Quick DER](https://github.com/vanrein/quick-der)
      - [LillyDAP](https://github.com/vanrein/lillydap)
      - MIT Kerberos with [KXOVER](https://github.com/arpa2/kxover)

The savings currently look minute, so an ARPA2 distribution forked off
of Debian Stable seems best.  It may however be prudent to already introduce
the various names for future forks:

  * `debian:stable`
  * `arpa2:base` for RCn versions, plus `:base-unstable`, `:base-alpha`, `:base-beta`
  * `arpa2:identityhub` for RCn versions, plus `:identityhub-unstable`, `:identityhub-alpha`, `:identityhub-beta`
  * `arpa2:servicehub` for RCn versions, plus `:servicehub-unstable`, `:servicehub-alpha`, `:servicehub-beta`


## Example integration with CMake

An example for creating a Docker Image as an additional target from CMake
is given here:

  * A [custom target](https://github.com/apache/nifi-minifi-cpp/blob/master/CMakeLists.txt#L157) can allow us to `make docker` in a build directory
  * A [build script](https://github.com/apache/nifi-minifi-cpp/blob/master/docker/DockerBuild.sh) then does the actual building
      - The base image gives us a number of basic facilities to build on
      - We may use CMake to build and install anything needed as dependencies
  * A [Dockerfile](https://github.com/apache/nifi-minifi-cpp/blob/master/docker/Dockerfile) defines the general structure of the Docker Image
  * A [.dockerignore](https://github.com/apache/nifi-minifi-cpp/blob/master/docker/.dockerignore) file can be used to avoid inclusion of unwanted files

