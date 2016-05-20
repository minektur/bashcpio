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
