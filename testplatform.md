# IdentityHub Test Environment

> *How to setup the test environment used during development.  The instructions double as a hint of how a deployment could be structured.  Please refine these instructions with problems and variations that you run into, including alternate operating system instructions!*

**TODO: TRY IT OUT**

## Install the Host OS

We are (silently) assuming Debian Stable.  That is not the stable-at-the-time-of-installation, but actually the `stable` tag in your `/etc/apt/sources.list` file.
```
debootstrap stable /dev/sdXN
```
Now boot into this `/dev/sdXN` partition, or start a VM on it.


## Install Docker for Developers

We will be building and deploying Docker Containers for the purposes of our tests.  Among the Docker Containers that are being run may well be those created by others.  You may run those locally, or connect to them remotely.

**TODO:DOCKER SETUP**

We base our deliverables on Debian Stable.
This is the advised
distribution by Docker, and it is also our development
standard.  Since we have a small pile of libraries and
tools that we use more than others, and since layers
of images can be beneficial from a sharing viewpoint,
we generally base our work on an extending layer on
top of Debian Stable.

[Read more](https://github.com/arpa2/howto/blob/master/deliverables.md)

Docker Containers can also be run on other operating systems like Mac OS X or Windows, because the base filesystem is downloaded along with it, and the runtime operates within a standard Docker environment.

It is even possible to build images on those other operating systems, but what is being packages must then be suitable for running on Debian Stable (which implies cross-builds on anything but that platform).  [We could however share a building environment to accommodate that, if so desired.]


## Install Apache Qpid

Our Docker Containers connect through AMQP 1.0, and one implementation of that choice is the Apache Qpid toolkit.  Even within this toolkit, various alternatives exist and ended up in a choice from the
[Apache Qpid Components](http://qpid.apache.org/components/index.html) offered.

  * [Proton](http://qpid.apache.org/proton/index.html)
    is a client library that can act as a full-blown
    AMQP endpoint, taking care of router tasks with
    redundancy/fallback and so on.  It is used in other
    Qpid components too.
  * [Messaging API](http://qpid.apache.org/components/messaging-api/index.html)
    is a client library that connects to a server for
    further handling of messages.  It is available in
    many programming languages and is the simplest
    way to reach an AMQP facility run elsewhere.
  * [C++ Broker](http://qpid.apache.org/components/cpp-broker/index.html)
    is a server that takes in AMQP messages, decides
    how to pass them on or deliver them in local
    queues.  It applies an ACL for authorisation and
    can authenticate over TLS or GSS-API.
    The Broker *takes ownership* of messages, meaning
    that it will store them for further forwarding.
    This is an ideal component to use at the netwerk
    perimeter.
  * [Dispatch Router](http://qpid.apache.org/components/dispatch-router/index.html)
    is a server that may decides how to forward
    messages, though it will never take ownership.
    It will simply be a middle party that greases the
    message forwarding on which it decides.  It has no
    queues and *never takes ownership* of a message.
    This is an ideal component to use as a low-cost
    interconnection bridge.
  * All components are implemented in a variety of
    languages, with preference for Java (it is an
    Apache project after all).  We do not believe that
    Java is the best choice for a hosting platform and
    will not use that in our setup.

The various IdentityHub components will not be made
aware of dynamic routing of AMQP messages, for instance
to handle a customer's wish to have a component
redirected to their premises.  This sort of work is
left to the interconnect between the components.  As a result, the Messaging API is just the right facility for the components of the IdentityHub.  **However:** There is no Messaging API for C, so we either may have to use the Qpid Proton library for C... *or* we should arrange
[glue functions](http://www.objectvalue.com/articles/LinkingCtoCplusplusV02.html) to the C++ member functions... *or* we should use a main loop in C++ and call worker functions written in C.  Either may be beneficial, because the library is
[really easy to use](http://qpid.apache.org/releases/qpid-0.32/programming/programming-book.pdf).

While routing messages between local Docker Components,
there is no strict need for taking ownership of a
message by storing them in a queue, but dynamicity
in message routing between components is highly
desirable.  This means that the Host OS for testing
is best setup with Dispatch Router, to match what may
be expect from hosting parties.

When connecting remotely, it may or may not make sense
to insert a queue to avoid repeated action in the
Dispatch Router, which will be the central bus to
interconnect all IdentityHub components.  This may
then be accomplished with the C++ Broker.  Yes, AMQP
means scalability `:-D`

To install **TODO:FROMHERE**
