chksys
======
A lightweight tool for recording and verifying permissions and SHA1 checksums of important system files.

Install Me
----------
Non-one-liners need not apply.
`git clone https://github.com/dpow/chksys.git && cd chksys && \`
`chmod go-rwx chksys && chmod u+x chksys && cp chksys /usr/local/bin`

Use Me
------
chksys looks in a set of pre-defined system directories and records the current 
permissions and computes checksums for all the files found in those directories, 
then spits them out into their respective log files. When asked, it compares the 
list of files and permissions on record to ones it amasses at runtime, and reports
any changes found.

Record current system state: `chksys record`

Verify current system state: `chksys verify`

Back up the current log files: `chksys backup`

Empty the current log files: `chksys dump`

Need some help? `chksys help`

If you're looking for a program with more sophistication, you are probably looking
for something like Tripwire or Cfengine. This here is a barebones framework for the
po' man.

Tested on
---------
FreeBSD-9.0, Cygwin_NT-5.1. NOT tested on OSX or Linux distros yet. 

LICENSE
-------
My very own oh-so-special Beerware License. Hope you find this program useful!

TO DO
-----
o  Add automatic recognition of SHA1 command by OS, so users don't have to 
   change it themselves.
   
o  Condense permissions & checksum logs into one file, not two. 

o  Add sophistication. Feel free to send me pull requests if you have something
   you'd like to see added.
   
o  Optimize the code. It's not efficient at the moment.

o  Needs a cooler one-liner for installation!

Enjoy!

-Dylan