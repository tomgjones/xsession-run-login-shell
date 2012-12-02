# xsession-run-login-shell

xsession-run-login-shell tries to ensure that X sessions on Debian-like
systems run under a login shell.

This allows the user to initialise their shell using its normal
startup files.  In the case of bash users, it means that settings
(including environment variables) made via ~/.bash_profile will
be take effect within the X session.

<http://wiki.debian.org/DotFiles> has a good description of the
problem that is being solved here.  Some more discussion is at
<http://lists.debian.org/debian-user/2005/06/msg03761.html>.

An example of a user coming across the issue is
<http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=601578> (although
an unsuitable fix was proposed in that instance).

# Discussion of implementation

If the X session scripts were being run under 
bash rather than just sh, we could support a wider range of
shells by running "exec -l $SHELL ..." (as Xsession does on Fedora)
to prepend a "-" to argv[0] in the exec'd process ala login(1).  This
has very wide support among shells to mean run as a login shell.
But we're being run under sh, so we go for giving the "-l" option to
the shell.  This appears to reasonably widely supported by shells.

Another option would be to exec bash -c 'exec -l ...', which, using
the `-` argv0 prefix method, would support a wider range of shells.
This would introducing a dependency on bash, but bash is priority
required in Debian anyway.

Another way would be to use DJB's argv0(1) program, but as things
are currently packaged that would introduce a dependency on
ucspi-tcp.

## Survey of shells' support for `-l` option to mean login

bash, dash, tcsh have an `-l` option meaning it's a login shell.

ksh (on Debian) doesn't have such an option, but does support the zeroth
argument starting with `-` to mean start a login shell.

# Comparison with other OSs

## Fedora

X sessions are run under a login shell, see 
`exec -l ...` in `/etc/gdm/Xsession` (checked on Fedora 14).
Bash's `exec -l` places a dash at the start of argv0 in
whatever then gets exec'd.
