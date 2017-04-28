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


