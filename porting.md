# How to ensure posix/windows compatibility 
## cmake ##
* Use python scripts instead of shell scripts
## C ##

### Functionality only available on POSIX ###
* POSIX signals
* fork()
* AF_UNIX sockets, alternative: windows named pipes
* manipulate file descriptors using fcntl, use socket options when possible
* setsid()
