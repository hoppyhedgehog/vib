# vib
VI [B]etter. VIB is a Wrapper for the VI editor that backs up the files being edited to a directory (depending on the configurable size of the file) and provides better revision control and file recovery.

# vib -h
###################################################################
[VI] [B]ackup utility (VIB)
by ben@datastorageguy.com v6.0
###################################################################
OVERVIEW:
###################################################################
This tool is designed to create backup files of any
file edited using the VI (VIM) editor. It allows you to
backup the files in a default subdirectory of
/opt/vib, or the option to create backups in the
'current' working directory (not advised)

By default, VIB adds the user name to the file name and
it also allows you to add the directory name to the
file name that is saved if you back it up to a specified
destination directory.

###################################################################
TO RESET BACK TO THE DEFAULT WHERE:
###################################################################
Issue:
	# vi -r

	This re-sets 'alias vi=/usr/bin/vim'

###################################################################
EXAMPLES
###################################################################


EXAMPLE I
echo ###################################################################
echo
echo This section illustrates using VIB with the default
echo backup directory of /opt/vib
echo
echo In other words you keep all backups in one location
echo
echo ###################################################################
Set the following:

BACKUP_DIRECTORY=/opt/vib
USEDIR=0
#---------------------------
#pwd
/home/ovnuser
#
ls -al /opt/vib
total 8
drwxr-xr-x    2 root     root         4096 Jul 23 15:05 .
drwxrwxrwt    3 root     root         4096 Jul 23 15:05 ..
#
# ls -al testfile
-rw-r--r--    1 root     root           43 Jul 23 14:11 testfile
#
# cat testfile
here is some stuff
#
# vi testfile
..
<adding 'here is some more stuff'>
:wq!
Commit Changes [n] y
Changes Saved ...
#
# cat testfile
here is some stuff
here is some more stuff
#
# ls -al /opt/vib
total 44
drwxr-xr-x    2 ovnuser trc          4096 Jul 23 14:12 .
drwxrwxrwx    8 root     root         4096 Jul 23 14:13 ..
-rw-r--r--    1 root     root           43 Jul 23 14:12 testfile.bpatridg.1
#
#
# cat /var/tmp/viblog/testfile.bpatridg.1
here is some stuff
#
#
# vi testfile
..
<edit file adding 'adding even more stuff'>
:wq!
Commit Changes [n] y
Changes Saved ...
#
# cat testfile
here is some stuff
here is some more stuff
adding even more stuff
#
# ls -al /var/tmp/viblog
total 16
drwxr-xr-x    2 ovnuser trc          4096 Jul 23 14:18 .
drwxrwxrwx    8 root     root         4096 Jul 23 14:17 ..
-rw-r--r--    1 root     root           43 Jul 23 14:17 testfile.bpatridg.1
-rw-r--r--    1 root     root           19 Jul 23 14:17 testfile.bpatridg.2
#
# cat /var/tmp/viblog/testfile.bpatridg.2
here is some stuff
here is some more stuff
#



###################################################################
EXAMPLE III
###################################################################

 This section describes saving all files in a designated
 backup location, while including the path to the files
 in the backup file name.

###################################################################


Set the following:

BACKUP_DIRECTORY=/var/tmp/viblogs
USEDIR=1
#---------------------------
#pwd
/home/ovnuser
#
ls -al /var/tmp/viblogs

drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
#

#
# ls -al /home/ovnuser/testfile
-rw-r--r--    1 root     root           43 Jul 23 14:11 testfile
#
# cat testfile
here is some stuff
here is some more stuff
#
# vi testfile
..
<adding 'adding a bunch more stuff again'>
:wq!
Commit Changes [y] y
Changes Saved ...
#
# cat testfile
here is some stuff
here is some more stuff
adding a bunch more stuff again

#
# ls -al /var/tmp/viblog
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
-rw-r--r--    1 root     root           78 Jul 23 16:21 testfile.ovnuser.testfile.1

# vi testfile
..
<adding 'here I go again'>
:wq!
Commit Changes [y] y
Changes Saved ...
#
# cat testfile
here is some stuff
here is some more stuff
adding a bunch more stuff again

#
# ls -al /var/tmp/viblog
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
-rw-r--r--    1 root     root           78 Jul 23 16:21 testfile.ovnuser.testfile.1

# vi testfile
..
<adding 'here I go again'>
:wq!
Commit Changes [y] y
Changes Saved ...

# cat testfile
here is some stuff
here is some more stuff
adding a bunch more stuff again
here I go again

#
# ls -al /var/tmp/viblog
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
-rw-r--r--    1 root     root           78 Jul 23 16:21 testfile.ovnuser.testfile.1
-rw-r--r--    1 root     root           78 Jul 23 16:23 testfile.ovnuser.testfile.2


#
#
# cat testfile.ovnuser.testfile.1
here is some stuff
here is some more stuff
#
#
#
# cat testfile.ovnuser.testfile.2
here is some stuff
here is some more stuff
adding a bunch more stuff again
#
