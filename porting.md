# How to ensure posix/windows compatibility 
## cmake ##
* Use python scripts instead of shell scripts
* prefix python scripts with the python executable
## C ##

### Functionality only available on POSIX ###
* POSIX signals
* fork(), alternative: exec family of functions
* AF_UNIX sockets, alternative: windows named pipes
* manipulate file descriptors using fcntl, use socket options when possible
* setsid()
