imapsync
========

This is a fork of the latest imapsync code under GPL, included in the Ubuntu 10.04 repos

NAME
----

imapsync - IMAP synchronisation, sync, copy or migration tool.
Synchronise mailboxes between two imap servers. Good at IMAP migration.
More than 32 different IMAP server softwares supported with success.

INSTALL
-------

imapsync works fine under any Unix OS with perl.
imapsync works fine under Windows (2000, XP) and ActiveState's 5.8 Perl

Get imapsync at https://github.com/patkar/imapsync
Clone the git repo or download a tarball from the download section.

You'll find a compressed tarball called imapsync-x.xx.tar.gz
where x.xx is the version number. Untar the tarball where
you want (on Unix):

    tar xzvf  imapsync-x.xx.tar.gz

SYNOPSIS
--------

      imapsync [options]

    To get a description of each option just run imapsync like this:

      imapsync --help
      imapsync

    The option list:

      imapsync [--host1 server1]  [--port1 <num>]
               [--user1 <string>] [--passfile1 <string>]
               [--host2 server2]  [--port2 <num>]
               [--user2 <string>] [--passfile2 <string>]
               [--ssl1] [--ssl2]
               [--authmech1 <string>] [--authmech2 <string>] 
               [--noauthmd5]
               [--folder <string> --folder <string> ...]
               [--folderrec <string> --folderrec <string> ...]
               [--include <regex>] [--exclude <regex>]
               [--prefix2 <string>] [--prefix1 <string>] 
               [--regextrans2 <regex> --regextrans2 <regex> ...]
               [--sep1 <char>]
               [--sep2 <char>]
               [--justfolders] [--justfoldersizes] [--justconnect] [--justbanner]
               [--syncinternaldates]
               [--idatefromheader]
               [--buffersize  <int>]
               [--syncacls]
               [--regexmess <regex>] [--regexmess <regex>]
               [--maxsize <int>]
               [--maxage <int>]
               [--minage <int>]
               [--skipheader <regex>]
               [--useheader <string>] [--useheader <string>]
               [--skipsize] [--allowsizemismatch]
               [--delete] [--delete2]
               [--expunge] [--expunge1] [--expunge2] [--uidexpunge2]
               [--subscribed] [--subscribe]
               [--nofoldersizes]
               [--dry]
               [--debug] [--debugimap]
               [--timeout <int>] [--fast]
               [--split1] [--split2] 
               [--reconnectretry1 <int>] [--reconnectretry2 <int>]
               [--version] [--help]
  
DESCRIPTION
-----------

The command imapsync is a tool allowing incremental and recursive imap
transfer from one mailbox to another.

By default all folders are transferred, recursively.

We sometimes need to transfer mailboxes from one imap server to another.
This is called migration.

imapsync is a good tool because it reduces the amount of data
transferred by not transferring a given message if it is already on both
sides. Same headers, same message size and the transfer is done only
once. All flags are preserved, unread will stay unread, read will stay
read, deleted will stay deleted. You can stop the transfer at any time
and restart it later, imapsync works well with bad connections. imapsync
is CPU hungry so nice and renice commands can be a good help. imapsync
can be memory hungry too, especially with large messages.

You can decide to delete the messages from the source mailbox after a
successful transfer (it is a good feature when migrating). In that case,
use the ```--delete --expunge1``` options.

You can also just synchronize a mailbox A from another mailbox B in case
you just want to keep a "live" copy of B in A.

OPTIONS
-------

To get a description of each option just invoke:

    imapsync --help

HISTORY
-------

The original imapsync code comes from Gilles LAMIRAL and was under the GPL
until this version. Then he changed it to away from the GPL, because he want
to test a new business model. Good idea and with a price about 5 EUR/USD
everyone would buy it, but the real price 42 EUR (ca. 50 USD) is a unbounded
cheek. This feels like microsoft! So I took the latest GPL licensed code from
the Ubuntu 10.04 package repos and forked it on github.

EXAMPLE
-------

While working on imapsync parameters please run imapsync in dry mode (no
modification induced) with the ```--dry``` option. Nothing bad can be done
this way.

To synchronize the imap account ```buddy``` on host ```imap.src.fr``` to the
imap account ```max``` on host ```imap.dest.fr``` (the passwords are located in
two files ```/etc/secret1``` for ```buddy```, ```/etc/secret2``` for ```max```):

    imapsync --host1 imap.src.fr  --user1 buddy --passfile1 /etc/secret1 \
              --host2 imap.dest.fr --user2 max   --passfile2 /etc/secret2

Then, you will have max's mailbox updated from buddy's mailbox.

SECURITY
--------

You can use ```--password1``` instead of ```--passfile1``` to give the password but
it is dangerous because any user on your host can see the password by
using the ```ps auxwwww``` command. Using a variable (like ```$PASSWORD1```) is
also dangerous because of the ```ps auxwwwwe``` command. So, saving the
password in a well protected file (600 or rw-------) is the best
solution.

imapsync is not totally protected against sniffers on the network since
passwords may be transferred in plain text if CRAM-MD5 is not supported
by your imap servers. Use ```--ssl1``` and ```--ssl2``` to enable encryption on
host1 and host2.

You may authenticate as one user (typically an admin user), but be
authorized as someone else, which means you don't need to know every
user's personal password. Specify ```--authuser1 "adminuser"``` to enable this
on host1. In this case, ```--authmech1 PLAIN``` will be used by default since
it is the only way to go for now. So don't use ```--authmech1 SOMETHING```
with ```--authuser1 "adminuser"```, it will not work. Same behavior with the
```--authuser2``` option.

EXIT STATUS
-----------

imapsync will exit with a 0 status (return code) if everything went
good. Otherwise, it exits with a non-zero status.

So if you have an unreliable internet connection, you can use this loop
in a Bourne shell:

    while ! imapsync ...; do 
        echo imapsync not complete
    done

LICENSE
-------

imapsync is free, gratis and open source software cover by the GNU
General Public License. See the GPL file included in the distribution or
the web site http://www.gnu.org/licenses/licenses.html

AUTHOR
------

FORKER:
Patrik Karisch <patrik.karisch@abimus.com>

OLD AUTHOR:
Gilles LAMIRAL <lamiral@linux-france.org>

Feedback good or bad is always welcome.

BUGS and BUG REPORT
-------------------

No known serious bug.

Report any bugs or feature requests to the github issues tool.
https://github.com/patkar/imapsync/issues

Help us to help you: follow the following guidelines.

Make a good title, not just "imapsync" or "problem", a good title is
made of keywords summary, not too long (one visible line).

Help us to help you: in your report, please include:

- imapsync version.
- output given with ```--debug --debugimap``` near the failure point.
  Isolate a message in a folder 'BUG' and use ```--folder 'BUG'```
- imap server software on both side and their version number.
- imapsync with all the options you use,  the full command line
  you use (except the passwords of course). 
- ```IMAPClient.pm``` version.
- operating system running imapsync.
- operating systems on both sides and the third side in case
  you run imapsync on a foreign host from the both.
- virtual software context (vmware, xen etc.)

Most of those values can be found as a copy/paste at the begining of the output.

IMAP SERVERS
------------

Failure stories reported with the following 4 imap servers:

- MailEnable 1.54 (Proprietary) http://www.mailenable.com/
- DBMail 0.9, 2.0.7 (GPL). But DBMail 1.2.1 works.
  Patient and confident testers are welcome.
- dkimap4 2.39
- Imail 7.04 (maybe).

Success stories reported with the following 35 imap servers (software
names are in alphabetic order):

- Archiveopteryx 2.03, 2.04, 2.09, 2.10 [dest], 3.0.0 [dest]
  (OSL 3.0) http://www.archiveopteryx.org/
- BincImap 1.2.3 (GPL) (http://www.bincimap.org/)
- CommuniGatePro server (Redhat 8.0) (Solaris)
- Courier IMAP 1.5.1, 2.2.0, 2.1.1, 2.2.1, 3.0.8, 3.0.3, 4.1.1 (GPL) 
  (http://www.courier-mta.org/)
- Critical Path (7.0.020)
- Cyrus IMAP 1.5, 1.6, 2.1, 2.1.15, 2.1.16, 2.1.18 
  2.2.1, 2.2.2-BETA, 2.2.10, 2.2.12, 
  v2.2.3-Invoca-RPM-2.2.3-8,
  2.3-alpha (OSI Approved),
  v2.2.12-Invoca-RPM-2.2.12-3.RHEL4.1,
  2.2.13,
  v2.3.1-Invoca-RPM-2.3.1-2.7.fc5,
  v2.3.7,
  (http://asg.web.cmu.edu/cyrus/)
- David Tobit V8 (proprietary Message system).
- DBMail 1.2.1, 2.0.4, 2.0.9, 2.2rc1 (GPL) (http://www.dbmail.org/).
  2.0.7 seems buggy.
- Deerfield VisNetic MailServer 5.8.6 [from]
- Dovecot 0.99.10.4, 0.99.14, 0.99.14-8.fc4, 1.0-0.beta2.7, 
  1.0.0 [dest/source] (LGPL) (http://www.dovecot.org/)
- Domino (Notes) 4.61[from], 6.5, 5.0.6, 5.0.7, 7.0.2, 6.0.2CF1, 7.0.1 [from]
- Eudora WorldMail v2
- GMX IMAP4 StreamProxy.
- Groupwise IMAP (Novell) 6.x and 7.0. Buggy so see the FAQ.
- iPlanet Messaging server 4.15, 5.1, 5.2
- IMail 7.15 (Ipswitch/Win2003), 8.12
- MDaemon 7.0.1, 8.0.2, 8.1, 9.5.4 (Windows server 2003 R2 platform)
- Mercury 4.1 (Windows server 2000 platform)
- Microsoft Exchange Server 5.5, 6.0.6249.0[from], 6.0.6487.0[from], 6.5.7638.1 [dest]
- Netscape Mail Server 3.6 (Wintel !)
- Netscape Messaging Server 4.15 Patch 7
- OpenMail IMAP server B.07.00.k0 (Samsung Contact ?)
- OpenWave
- Qualcomm Worldmail (NT)
- Rockliffe Mailsite 5.3.11, 4.5.6
- Samsung Contact IMAP server 8.5.0
- Scalix v10.1, 10.0.1.3, 11.0.0.431
- SmarterMail
- SunONE Messaging server 5.2, 6.0 (SUN JES - Java Enterprise System)
- Sun Java(tm) System Messaging Server 6.2-2.05,  6.2-7.05
- Surgemail 3.6f5-5
- UW-imap servers (imap-2000b) rijkkramer IMAP4rev1 2000.287
  (RedHat uses UW like 2003.338rh), v12.264 Solaris 5.7 (OSI Approved) 
  (http://www.washington.edu/imap/)
- UW - QMail v2.1
- Imap part of TCP/IP suite of VMS 7.3.2
- Zimbra-IMAP 3.0.1 GA 160, 3.1.0 Build 279, 4.0.5, 4.5.2, 4.5.6, 5.5.

Please report to the author any success or bad story with imapsync and
do not forget to mention the IMAP server software names and version on
both sides. This will help future users. To help the author maintaining
this section report the two lines at the begining of the output if they
are useful to know the softwares. Example:

    From software:* OK louloutte Cyrus IMAP4 v1.5.19 server ready
    To   software:* OK Courier-IMAP ready

You can use option ```--justconnect``` to get those lines. Example:

    imapsync --host1 imap.troc.org --host2 imap.trac.org --justconnect

HUGE MIGRATION
--------------

Pay special attention to options ```--subscribed``` ```--subscribe``` ```--delete```
```--delete2``` ```--expunge``` ```--expunge1``` ```--expunge2``` ```--uidexpunge2``` ```--maxage```
```--minage``` ```--maxsize``` ```--useheader``` ```--fast```

If you have many mailboxes to migrate think about a little shell
program. Write a file called ```users.csv``` (for example) containing users and
passwords. The separator used in this example is ';'

The ```users.csv``` file contains:

    user0001;password0001;user0002;password0002
    user0011;password0011;user0012;password0012 ...

And the shell program is just:

    { while IFS=';' read  u1 p1 u2 p2; do 
           imapsync --user1 "$u1" --password1 "$p1" --user2 "$u2" --password2 "$p2" ...
    done ; } < users.csv

Welcome in shell programming !

Hacking
-------

Feel free to hack imapsync as the GPL Licence permits it.

SIMILAR SOFTWARES
-----------------

- imap_tools     : http://www.athensfbc.com/imap_tools
- offlineimap    : http://software.complete.org/offlineimap
- mailsync       : http://mailsync.sourceforge.net/
- imapxfer       : http://www.washington.edu/imap/
  part of the imap-utils from UW.
- mailutil       : replace imapxfer in 
  part of the imap-utils from UW.
  http://www.gsp.com/cgi-bin/man.cgi?topic=mailutil
- imaprepl       : http://www.bl0rg.net/software/
  http://freshmeat.net/projects/imap-repl/
- imap_migrate   : http://freshmeat.net/projects/imapmigration/
- imapcopy       : http://home.arcor.de/armin.diehl/imapcopy/imapcopy.html
- migrationtool  : http://sourceforge.net/projects/migrationtool/
- imapmigrate    : http://sourceforge.net/projects/cyrus-utils/
- wonko_imapsync : http://wonko.com/article/554
  see also tools/wonko_ruby_imapsync
- pop2imap       : http://www.linux-france.org/prj/pop2imap/

Feedback (good or bad) will always be welcome.
