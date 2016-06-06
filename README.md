# bashcpio
An incomplete cpio extractor in bash.

# external dependencies:  dd, mkdir, dirname, chown, chmod, touch, ln
 (could probably get rid of dirname, but the rest have no pure-bash equivalent)

The goal of this project is to implement enough of cpio extraction that 
when combined with the rpm2cpio.sh bash script floating around could 
successfully extract binary centos rpms.

It may have other uses, but this is all I'll be testing on and really it's
probably better just to get either a cpio binary, or perhaps a libarchive 
linked tar or something.

Expected usage is:

    rpm2cpio.sh foo.rpm | bashcpio [ignored options] 

and have the files in foo.rpm extracted to the current working dir.

    or
    bashcpio foo.cpio

This means among other things, that I'm only aiming for rpm 3.0 packages,
implying gzip compression, and supporting only the 'New Ascii' header
format (with or without "CRC") as defined here:

https://people.freebsd.org/~kientzle/libarchive/man/cpio.5.txt

Notes:  Something that handles chunks of a binary file (dd) and mkdir (-p) dont seem to be 
implementable with only bash builtins. Bash eats Nulls in assignment. Similarly, modifying 
file attributes has no real equivalent, though I might swing some perms with umask manipulation.

"real" end goal here?  add a centos target to "crouton" for chromebooks,
of which this is a tiny, but necessary piece.


A few notes for the curious:

*   Bash, and awk (including chromebook mawk), and sed, all dont handle
binary nulls very well if at all.  Thus, I have to pick the file apart
with dd to get the ascii headers, and then dd directly to files for the
binary contents of the cpio archives. The headers have embedded nulls, and
of course the contents are potentially anything including nulls.

*   There is an rpm2cpio package for redhat/centos that just uses the 
binary library that the rpm commands use, but they also ship a .sh 
shell script that does about 80% or more, in a less robust way.  This is 
what I'll be using.  Included a neutered, "works on chromeos" copy in this
package.

*   This is terribly insecure - a malicious archive could write over anything
that the user you run this as has write access to. Hm - chroot to this dir?

*   More on insecurity - a non-malicious archive presented by an attacker with 
local access and good timing might get a file overwritten or perms-changed. 
You should probably unpack in an area that only you have write-access to 

* You can get a copy of rpm2cpio.sh (GPL'd) here: https://github.com/rpm-software-management/rpm/tree/master/scripts

*    TODO:  add gpl license info for my modified rpm2cpio.sh
*    TODO:  device files, fifos, and uh... 
