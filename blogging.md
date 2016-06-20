# Blogging on InternetWide.org

> *This shows how we write our blogging articles.  If you have something
> to post, you are welcome to provide this format and have us post it,
> if you were not setup with blogging access.  Do let us know if we can be
> of help!*

## References:

  * [Published article](http://internetwide.org/blog/2015/04/22/id-2-byoid.html)
  * [Markdown notation](http://daringfireball.net/projects/markdown/)
  * [Pelican](http://blog.getpelican.com)


## Source code:

    date: 2015-04-22
    tags: architecture,identity,hosting
    slug: id-2-byoid
    author: Rick van Rein
    title: Identity 2: Bring Your Own Identity
    category: architecture
    summary: 
        With the InternetWide Architecture, you can "bring your own ID", meaning that
        you get to use the same credentials everywhere, with high levels of security
        and single sign-on.  This article describes how we build up the technical
        infrastructure to realise that.
    
    This article explains the architecture to build the ideas from the
    [introduction](/blog/2015/04/21/id-1-intro.html) to this series.
    
    ## Client and Service Realms
    
    When we speak of Bring Your Own Identity, or even just BYOID, we want to express
    that you are running an identity provider under control of the client, and use it
    to access a service.  Although it would be possible for the service to reside in
    the same "secure realm" as the client, it is more general to consider them as
    (possibly) different:
    
    ![Bring Your Own Identity](/images/id-perimeter-byoid.png)
    
    The action of the client to bind to its realm is called *authentication*, or
    proving that you are permitted to use a certain identity.  This can take the
    form of a (daily) logon with the local identity provider.  Assuming that the
    identity provider is under your control, for instance due to a service
    contract that you can influence because you pay for it, you can rest assured
    about your privacy being protected by the client realm.
    
    The service usually wants to limit access to whatever "resource" it provides,
    and to that end it conducts *authorisation* to establish what rights are
    available to the identified client.
    
    Before authorisation can commence, there first is a need to establish that the
    client's identity.  This is a difficult problem, because it involves
    [realm crossover](http://realm-xover.arpa2.net),
    and in general needs to establish trust in an independently managed realm.
    The link explains the various mechanisms available to do this, and inhowfar
    they are suitable.  We believe that
    [Kerberos](http://realm-xover.arpa2.net/kerberos.html) holds the best cards:
    
      * It is a highly secure mechanism
      * It is relatively easy to expand with realm crossover
      * It is widely used in infrastructures
      * It is the most efficient mechanism due to symmetric keys
    
    ## Changes to the client identity
    
    Even when a single login gets a user through the day, that does not necessarily
    mean that the same identity should be used throughout that day.  There are a
    few reasons to change the client identity:
    
      * local client identities may not be meant for public use
      * services may be best accessed from a group identity
      * some services should only get a temporary, or remote-specific identity
    
    This means that we want to have a facility to change our identity when we
    crossover to another realm.  The general idea is one of *limited disclosure*
    of identities; or more accurately, the end user gets to setup a desired identity
    for each remote service accessed.
    
    ![Local and Remote Identity Filtering](/images/id-perimeter-xlate.png)
    
    The resulting infrastructure now boils down to an outgoing filter at the client
    side, and incoming filtering at the server side.  This looks like the most
    general facility one could have.
    
