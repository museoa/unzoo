Extracted from
http://archives.math.utk.edu/software/multi-platform/gap/util/unzoo.c

--------------------------------------------------------------------------------

A  unzoo.c                     Tools                        Martin Schoenert

H  @(#)$Id: unzoo.c,v 4.4 2000/05/29 08:56:57 sal Exp $

Y  This file is in the Public Domain.

 SYNTAX

 'unzoo'
 'unzoo [-l] [-v] <archive>[.zoo] [<file>..]'
 'unzoo -x [-abnpo] <archive>[.zoo] [<file>..]'

 DESCRIPTION

 'unzoo' is  a zoo  archive extractor.   A zoo archive   is  a  file  that
 contains several files, called its members, usually in compressed form to
 save space.  'unzoo' can list all or  selected members or  extract all or
 selected members, i.e.,  uncompress them and write   them  to files.   It
 cannot add new members or  delete  members.  For this   you need the  zoo
 archiver, called 'zoo', written by Rahul Dhesi.

 If you call 'unzoo'  with no arguments, it will  first print a summary of
 the commands and  then prompt for  command lines interactively, until you
 enter an empty line.  This is useful  on systems  that do not support the
 notion of command line arguments such as the Macintosh.

 If you call  'unzoo' with the  '-l' option,  it lists the  members in the
 archive <archive>.   For each member 'unzoo'   prints  the size  that the
 extracted file  would  have, the  compression factor,  the  size that the
 member occupies in the archive (not  counting  the  space needed to store
 the attributes such as the path name of the file), the date and time when
 the files were last modified, and finally  the path name itself.  Finally
 'unzoo' prints a grand total for the  file sizes, the compression factor,
 and the member sizes.

 The '-v' suboption causes 'unzoo' to append to each path name,  separated
 by a ';', the generation number of the member,  where higher numbers mean
 later generations.  Members for which generations are disabled are listed
 with  ';0'.  Also 'unzoo'   will print the  comments associated  with the
 archive itself or the members, preceeded by the string '# '.

 If you call 'unzoo' with the '-x' option,  it extracts the  members  from
 the archive <archive>.  Members are  stored with a  full path name in the
 archive and if the operating system supports this, they will be extracted
 into   appropriate subdirectories,   which will   be  created on  demand.
 The members are usually  extracted as binary files,  with no translation.
 However, if a member has a  comment that starts with the string '!TEXT!',
 it is  extracted as a  text file, i.e.,  it will be  translated from  the
 universal text file format (with <lf> as line separator as under UNIX) to
 the local text file format (e.g., with <cr>/<lf> as separator under DOS).
 If the archive  itself has a  comment that starts with  '!TEXT!' then all
 members will be extracted as text files, even those that have no comment.
 For each member the name is printed followed by  '-- extracted as binary'
 or '-- extracted as text' when the member has been completely extracted.

 The '-a' suboption causes  'unzoo' to extract all members  as text files,
 even if they have no comment starting with  '!TEXT!'.

 The '-b' suboption causes 'unzoo' to extract all members as binary files,
 even if they have a comment starting with  '!TEXT!'.

 The '-n' suboption causes 'unzoo' to suppress writing the files.  You use
 this suboption  to test the integrity  of the archive  without extracting
 the members.  For each member the name is printed followed by '-- tested'
 if the member is intact or by '-- error, CRC failed' if it is not.

 The '-p' suboption causes 'unzoo' to print the files to stdout instead of
 writing them to files.

 The '-o'  suboption causes 'unzoo'   to overwrite existing  files without
 asking  you for confirmation.   The  default is  to ask for  confirmation
 '<file> exists, overwrite it? (Yes/No/All/Ren)'.   To this you can answer
 with 'y' to overwrite the  file, 'n' to skip  extraction of the file, 'a'
 to overwrite this and all following files, or 'r' to enter a new name for
 the file.  'unzoo' will never overwrite existing read-only files.

 The '-j <prefix>' suboption causes 'unzoo' to prepend the string <prefix>
 to  all path names for  the members  before  they  are extracted.  So for
 example if an archive contains absolute  path names under  UNIX,  '-j ./'
 can be used to convert them to relative pathnames.   This option  is also
 useful  on  the Macintosh where   you start 'unzoo' by clicking,  because
 then the current directory will be the one where 'unzoo' is,  not the one
 where the  archive is.   Note  that the  directory  <prefix> must  exist,
 'unzoo' will not create it on demand.

 If no  <files>  argument is given all members  are  listed or  extracted.
 If  one or  more <files>  arguments are given,  only members whose  names
 match at least one of  the  <files> patterns  are  listed  or  extracted.
 <files> can  contain the wildcard   '?', which  matches any character  in
 names, and '*', which  matches any number  of characters  in names.  When
 you pass the <files> arguments on the command  line you will usually have
 to quote them to keep the shell from trying to expand them.

 Usually 'unzoo' will  only list or extract the  latest generation of each
 member.  But if you append ';<nr>' to a path  name pattern the generation
 with the number <nr> is listed or extracted.  <nr> itself can contain the
 wildcard characters '?' and '*', so appending ';*' to a path name pattern
 causes all generations to be listed or extracted.


 COMPATIBILITY

 'unzoo'  is based heavily on the 'booz' archive extractor by Rahul Dhesi.
 I basically stuffed everything in one file (so  no 'Makefile' is needed),
 cleaned it up (so that it is now more portable and  a little bit faster),
 and added the  support for  long file names,  directories,  and comments.

 'unzoo' differs in some details from  'booz' and the zoo archiver  'zoo'.

 'unzoo' can  only list  and extract members   from archives, like 'booz'.
 'zoo' can also add members, delete members, etc.

 'unzoo' can extract members as text files, converting from universal text
 format to the local text format,  if the '-a' option is given or the '-b'
 option is not given and the  member has a comment starting with '!TEXT!'.
 So in the absence of the '-a' option and comments starting with '!TEXT!',
 'unzoo' behaves like  'zoo' and 'booz',  which always extract as  binary.
 But  'unzoo' can  correctly extract  text files from  archives that  were
 created under UNIX (or other systems using the universal text format) and
 extended with '!TEXT!' comments on systems such as DOS, VMS, Macintosh.

 'unzoo' can handle  long names, which it converts  in  a system dependent
 manner to local  names, like  'zoo'  (this may not   be available on  all
 systems).  'booz' always uses the short DOS format names.

 'unzoo' extracts  members  into  subdirectories, which  it  automatically
 creates, like 'zoo' (this  may not be available on  all systems).  'booz'
 always extracts all members into the current directory.

 'unzoo'  can handle comments and generations in the  archive, like 'zoo'.
 'booz' ignores all comments and generations.

 'unzoo' cannot handle  members compressed with  the old method, only with
 the new  high method or  not compressed  at all.   'zoo' and  'booz' also
 handle members compress with the old method.  This shall be fixed soon.

 'unzoo' can handle archives in  binary format under  VMS, i.e., it is not
 necessary to convert  them to stream linefeed  format  with 'bilf' first.
 'zoo' and 'booz' require this conversion.

 'unzoo' is somewhat faster than 'zoo' and 'booz'.

 'unzoo' should be much easier to port than both 'zoo' and 'booz'.

 COMPILATION

 Under  UNIX  with the  standard  C compiler,  compile  'unzoo' as follows
     cc  -o unzoo  -DSYS_IS_UNIX   -O  unzoo.c
 If your UNIX has the 'mkdir' system call,  you may add  '-DSYS_HAS_MKDIR'
 for a slightly faster executable.   BSD has it,  else try  'man 2 mkdir'.

 Under  DOS  with the  DJGPP  GNU C compiler,  compile  'unzoo' as follows
     gcc  -o unzoo.out  -DSYS_IS_DOS_DJGPP  -O2  unzoo.c
     copy /b \djgpp\bin\go32.exe+unzoo.out unzoo.exe

 Under TOS with the GNU compiler and unixmode, compile  'unzoo' as follows
     gcc  -o unzoo.ttp  -DSYS_IS_TOS_GCC  -O2  unzoo.c

 Under OS/2 2 with the emx development system, compile  'unzoo' as follows
     gcc  -o unzoo.exe  -DSYS_IS_OS2_EMX  -Zomf -Zsys  -O2  unzoo.c
 To create an executable that runs under OS/2 and DOS,  but which requires
 the emx runtime, compile without the '-Zomf' and '-Zsys' options.

 On a VAX running VMS with the DEC C compiler, compile  'unzoo' as follows
     cc   unzoo/define=SYS_IS_VMS
     link unzoo
 Then perform the following global symbolic assignment
     unzoo :== $<dev>:[<dir>]unzoo.exe
 where  <dir> is the    name of the   directory  where you  have installed
 'unzoo' and  <dev> is the device on which this directory is,  for example
     unzoo :== $dia1:[progs.archivers]unzoo
 You may want to put this symbolic assignment into your  'login.com' file.

 On a  Macintosh  with  the  MPW C  compiler,  compile  'unzoo' as follows
     C    -model far  -d SYS_IS_MAC_MPW  -opt on  unzoo.c
     Link -model far -d -c '????' -t APPL unzoo.c.o -o unzoo   <continued>
         "{CLibraries}"StdClib.o "{Libraries}"SIOW.o           <continued>
         "{Libraries}"Runtime.o  "{Libraries}"Interface.o
     Rez  -a "{RIncludes}"SIOW.r  -o unzoo
 Afterwards choose the  'Get Info' command in the  finder 'File' menu  and
 increase the  amount of memory  'unzoo' gets upon startup to  256 KBytes.
 To  create a MPW  tool instead of a  standalone, link with creator 'MPS '
 instead of '????', with type 'MPST' instead  of 'APPL' and with 'Stubs.o'
 instead of 'SIOW.o'.  The  'Rez' command  is  not required for the  tool.
 Alternatively choose the 'Create Build Commands...'  command from the MPW
 'Build' menu to create a  makefile.  Edit it  and add '-d SYS_IS_MAC_MPW'
 to the  compile command.  Choose the  'Build...' command from the 'Build'
 menu to build 'unzoo'.

 On  other systems with a C compiler,  try to  compile  'unzoo' as follows
     cc  -o unzoo -DSYS_IS_GENERIC  -O  unzoo.c

 PORTING

 If this  does not work,  you must supply new   definitions for the macros
 'OPEN_READ_ARCH',   'OPEN_READ_TEXT' and  'OPEN_WRIT_TEXT'.  If you  want
 'unzoo' to keep long file  names, you must   supply a definition for  the
 macro 'CONV_NAME'.  If  you want 'unzoo'  to extract into subdirectories,
 you   must supply a  definition for  the macro 'CONV_DIRE'.   If you want
 'unzoo' to automatically create directories, you must supply a definition
 for the macro 'MAKE_DIR'.  If you want  'unzoo' to set the permissions of
 extracted  members to those  recorded in the archive,  you must  supply a
 definition for the macro 'SETF_PERM'.  Finally if you want 'unzoo' to set
 the times of the extracted members to  the times recorded in the archive,
 you must supply a definition for the  macro 'SETF_TIME'.  Everything else
 should be system independent.

 ACKNOWLEDGMENTS

 Rahul Dhesi  wrote the  'zoo' archiver and the  'booz' archive extractor.
 Haruhiko Okumura  wrote the  LZH code (originally for his 'ar' archiver).
 David Schwaderer provided the CRC-16 calculation in PC Tech Journal 4/85.
 Jeff Damens  wrote the name match code in 'booz' (originally for Kermit).
 Harald Boegeholz  ported 'unzoo' to OS/2 with the emx development system.
 Dave Bayer ported 'unzoo' to the Macintosh,  including Macbinary support.

 HISTORY
H  $Log: unzoo.c,v $
H  Revision 4.4  2000/05/29 08:56:57  sal
H  Remove all the \ continuation lines -- who needs the hassle.	SL
H
H  Revision 4.3  1999/10/27 08:51:11  sal
H  Fix date problem on alphas (I hope)	SL
H
H  Revision 4.2  1999/05/26 09:27:03  gap
H  burkhard: use fseek to access file comments; Mac version: several minor fixes
H
H  Revision 1.5  1994/01/21  13:32:32  mschoene
H  added Mac support from Dave Bayer
H
H  Revision 1.4  1994/01/20  20:45:46  mschoene
H  cleaned up determination of write mode
H
H  Revision 1.3  1993/12/02  12:43:12  mschoene
H  added OS/2 support from Harald Boegeholz
H
H  Revision 1.2  1993/12/02  12:33:39  mschoene
H  fixed several typos, renamed MS-DOS to DOS
H
H  Revision 1.1  1993/11/09  07:17:50  mschoene
H  Initial revision
H
