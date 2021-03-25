
# VI [B]ackup

> VIB is a BASH SHELL SCRIPT Wrapper for the VI editor 
> that backs up the files being edited to a directory (depending on 
> the configurable size of the file) and provides better revision 
> control and file recovery.

*By ben@datastorageguy.com v6.0*

## USAGE (to view full readme)
```
# vib -h
```


## OVERVIEW:

This tool is designed to create backup files of any
file edited using the VI (VIM) editor. It allows you to
backup the files in a default subdirectory of
/opt/vib, or the option to create backups in the
current working directory (not advised)

By default, VIB adds the user name to the file name and
it also allows you to add the directory name to the
file name that is saved if you back it up to a specified
destination directory.

# USAGE 

The script usage options are:
```
# vib -?
-------------------------------------------------------------------
[V][I] [B]ackup Wrapper
by ben@datastorageguy.com

usage:
  # vi [-h|-e|-d|-l|-n|-r|-t|-v]  <file>
	-h Display detailed help/usage
	-e Enable VIB
	-d Disable VIB
	-l Enable add Last Modification Date (Default is Enabled)
	-n Disable all Last Modification prepending
	-r Uninstall-Reset the alias to default
	-t FORCE Enable add Last Modification Date to <file>
	   *note: Default for any scripts
	-v Verbose (debug) output
```


# INSTALLATION

Installing VIB involves:
- manually copying the file to /usr/local/bin 
*(or your preferred directory)*
- Using the 'vib' command to edit files, OR 
	- Enable vib via the '-e' option to alias vi and vim command

## Example setting Alias to vi
```
# vib -e
-------------------------------------------------------------------
ENABLED VIB
REMOVED FILE: /var/tmp/.novib
-------------------------------------------------------------------
```

How we see the alias
```
# alias |grep vib
alias vi='/usr/local/bin/vib'
```



By default VIB appends the date/timestamp to the top of the file for BASH/KSH/PERL/SH Scripts
Example. Viewing file teststuff.sh and adding 'echo World' to the end of the file:
```
#vi teststuff.sh
#!/bin/bash
###################################################################
# Script to test stuff
#################################################
echo "Hello"
echo "World"
#
```

Re-editing the file and now wee see the last modification time!
```
#!/bin/bash
################################################
#LAST MODIFIED: 2021-03-24 20:32:02
#################################################
echo "Hello"
echo "World"
```

## BACKUP FILE EXAMPLES


### EXAMPLE I

This section illustrates using VIB with the default
backup directory of /opt/vib

In other words you keep all backups in one location
 
 
Set the following VIB options to
- Configure the VIB Backup Directory
- Set the USEDIR option to 0 so that files are kept in BACKUP_DIRECTORY


```
BACKUP_DIRECTORY=/opt/vib
USEDIR=0
```

Now in this example we show that there are no files in /opt/vib
```
#
ls -al /opt/vib
total 8
drwxr-xr-x    2 root     root         4096 Jul 23 15:05 .
drwxrwxrwt    3 root     root         4096 Jul 23 15:05 ..
#
```

Now we list a testfile in the current user HOME directory
And verify there is some content in the file

```
# pwd
/home/ovnuser
# ls -al testfile
-rw-r--r--    1 root     root           43 Jul 23 14:11 testfile
#
# cat testfile
here is some stuff
#
```

We edit the file now using vib and add some stuff to it.
> Note we see there is a confirmation to save the changes

```
# vib testfile
..
<adding 'here is some more stuff'>
:wq!
Commit Changes [n] y
Changes Saved ...
```
Now we see that there is data in the new 'testfile'
```
# cat testfile
here is some stuff
here is some more stuff
#
```

Looking in the VIB BACKUP_DIRECTORY now shows we see a backup copy of the file
```
# ls -al /opt/vib
total 44
drwxr-xr-x    2 ovnuser wheel          4096 Jul 23 14:12 .
drwxrwxrwx    8 root     root         4096 Jul 23 14:13 ..
-rw-r--r--    1 root     root           43 Jul 23 14:12 testfile.bpatridg.1
```

Looking at the content of the file we see that there is the ORIGINAL file data.
```
#
# cat /var/tmp/viblog/testfile.bpatridg.1
here is some stuff
```

Editing the file one more time and adding additional content
```
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
```

Now we see there is another backup copy of the file in the BACKUP_DIRECTORY
```
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
```



## EXAMPLE II

This section describes saving all files in a designated
backup location while including the **PATH TO THE FILE**
in the backup file name.

Meaning, the "root" filesystem directory  (/) is the BACKUP_DIRECTORY

*For Example*
> The system 'user' directory is /home/ovnuser
> When backing up the file it will write to $BACKUP_DIRECTORY/home/ovnuser




Below we set:
- The BACKUP_DIRECTORY
- The USEDIR option to 1 which sets the BACKUP_DIRECTORY as the "root" directory


```
BACKUP_DIRECTORY=/var/tmp/viblogs
USEDIR=1
```

Now we have cleared the BACKUP_DIRECTORY (for this example) and we verify it is empty
```
ls -al /var/tmp/viblogs

drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
#
```

Next we list a file in the current use directory, and then view content in that file
```
#
# ls -al /home/ovnuser/testfile
-rw-r--r--    1 root     root           43 Jul 23 14:11 testfile
#
# cat testfile
here is some stuff
here is some more stuff
#
```

Next we edit the testfile and add some temporary content
```
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
```

We see that there is a backup of the file edited contains *2 subdirectories to the file!*
```
#
# cd /var/tmp/viblog
# ls -Al
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 home
#
# cd home
# ls -Al
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ovnuser
#
# ls -Al
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
-rw-r--r--    1 root     root           78 Jul 23 16:21 testfile.ovnuser.testfile.1
#
# cat testfile
here is some stuff
here is some more stuff
```

Now we edit this file and add some more data

```
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
```

Now we see we have another file

```
#
# cd /var/tmp/viblog
# ls -Al
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 home
#
# cd home
# ls -Al
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ovnuser
#
# ls -Al
drwxr-xr-x    2 root     root         4096 Jul 23 16:24 .
drwxrwxrwt    4 root     root         4096 Jul 23 15:11 ..
-rw-r--r--    1 root     root           78 Jul 23 16:21 testfile.ovnuser.testfile.1
-rw-r--r--    1 root     root           78 Jul 23 16:33 testfile.ovnuser.testfile.2
#
#
# cat testfile
here is some stuff
here is some more stuff
adding a bunch more stuff again
here I go again
```



## TO RESET BACK TO THE DEFAULT WHERE:

Issue:
```
# vi -r
```
*This re-sets 'alias vi=/usr/bin/vim'*

results:
```
#################################################################
Reset vi back using:
	 # alias vi=/usr/bin/vim'
#################################################################
#
# alias |grep vi
#
```



