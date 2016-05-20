# bashcpio
An incomplete cpio extractor in bash (plus dd, mkdir -p, and dirname)


The goal of this project is to implement enough of cpio extraction that
when combined with the rpm2cpio.sh bash script floating around, when combined 
with only bash and a very few external, hopefully universal shell utils, one
could successfully extract binary rpms as seen in centos releases.

It may have other uses, but this is all I'll be testing on and really it's
probably better just to get either a cpio binary, or perhaps a libarchive 
linked tar or something.

Expected usage is:

    rpm2cpio.sh foo.rpm | bashcpio [ignored options] 

and have the files in foo.rpm extracted to the current working dir.

This means among other things, that I'm only aiming for rpm 3.0 packages,
implying gzip compression, and supporting only the 'New Ascii' header
format (with or without "CRC") as defined here:

https://people.freebsd.org/~kientzle/libarchive/man/cpio.5.txt




Notes:  In the long run, I could probably do without dirname, but something
that handles chunks of a binary file (dd) and mkdir (-p) dont seem to be 
implementable with only bash builtins.

"real" end goal here?  add a centos target to "crouton" for chromebooks,
of which this is a tiny, but necessary piece.


A few notes for the curious:

*   Bash, and awk (including chromebook mawk), and sed, all dont handle
binary nulls very well if at all.  Thus, I have to pick the file apart
with dd to get the ascii headers, and then dd directly to files for the
binary contents of the cpio archives.

*   There is an rpm2cpio package for redhat/centos that just uses the 
binary library that the rpm commands use, but they also ship a .sh 
shell script that does about 80% or more, in a less robust way.  This is 
what I'll be using

*   This is terribly insecure - a malicious archive could write over anything
that the user you run this as has write access to. Hm - chroot to this dir?

*   Perms will be maintained, but not ownership?  since I'm going to be building
single user chroots... hm well, tbd

