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

# Comparison with other OSs

## Fedora

X sessions are run under a login shell, see 
`exec -l ...` in `/etc/gdm/Xsession` (checked on Fedora 14).
