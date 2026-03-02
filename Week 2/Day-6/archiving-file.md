## Archived the file & moved it to /home directory so dev could access it 
### Raw Code :
---
    #3) With great power comes great responsibility.

[sudo] password for natasha: 
tar: Removing leading `/' from member names
/data/mark/
/data/mark/nautilus2.txt
/data/mark/nautilus3.txt
/data/mark/nautilus1.txt
[natasha@ststor01 ~]$ sudo tar -xzvf mark.tar.gz -c /home
tar: You may not specify more than one '-Acdtrux', '--delete' or  '--test-label' option
Try 'tar --help' or 'tar --usage' for more information.
 Informative output:

      --checkpoint[=NUMBER]  display progress messages every NUMBERth record
                             (default 10)
      --checkpoint-action=ACTION   execute ACTION on each checkpoint
      --full-time            print file time to its full resolution
      --index-file=FILE      send verbose output to FILE
  -l, --check-links          print a message if not all links are dumped
      --no-quote-chars=STRING   disable quoting for characters from STRING
      --quote-chars=STRING   additionally quote characters from STRING
      --quoting-style=STYLE  set name quoting style; see below for valid STYLE
                             values
  -R, --block-number         show block number within archive with each message
                            
      --show-defaults        show tar defaults
      --show-omitted-dirs    when listing or extracting, list each directory
                             that does not match search criteria
      --show-snapshot-field-ranges
                             show valid ranges for snapshot-file fields
      --show-transformed-names, --show-stored-names
                             show file or archive names after transformation
      --totals[=SIGNAL]      print total bytes after processing the archive;
                             with an argument - print total bytes when this
                             SIGNAL is delivered; Allowed signals are: SIGHUP,
                             SIGQUIT, SIGINT, SIGUSR1 and SIGUSR2; the names
                             without SIG prefix are also accepted
      --utc                  print file modification times in UTC
  -v, --verbose              verbosely list files processed
      --warning=KEYWORD      warning control
  -w, --interactive, --confirmation
                             ask for confirmation for every action

 Compatibility options:

  -o                         when creating, same as --old-archive; when
                             extracting, same as --no-same-owner

 Other options:

  -?, --help                 give this help list
      --restrict             disable use of some potentially harmful options
      --usage                give a short usage message
      --version              print program version

Mandatory or optional arguments to long options are also mandatory or optional
for any corresponding short options.

The backup suffix is '~', unless set with --suffix or SIMPLE_BACKUP_SUFFIX.
The version control may be set with --backup or VERSION_CONTROL, values are:

  none, off       never make backups
  t, numbered     make numbered backups
  nil, existing   numbered if numbered backups exist, simple otherwise
  never, simple   always make simple backups

Valid arguments for the --quoting-style option are:

  literal
  shell
  shell-always
  shell-escape
  shell-escape-always
  c
  c-maybe
  escape
  locale
  clocale

*This* tar defaults to:
--format=gnu -f- -b20 --quoting-style=escape --rmt-command=/etc/rmt
--rsh-command=/usr/bin/ssh
[natasha@ststor01 ~]$ sudo tar -xzvf mark.tar.gz /home
              
tar: /home: Not found in archive
tar: Exiting with failure status due to previous errors
[natasha@ststor01 ~]$ cd /home

[natasha@ststor01 home]$ sudo tar -xvf mark.tar.gz

tar: mark.tar.gz: Cannot open: No such file or directory
tar: Error is not recoverable: exiting now

[natasha@ststor01 home]$ sudo mv ~/mark.tar.gz /home/

[sudo] password for natasha: 
[natasha@ststor01 home]$ ls -lf /home/mark.tar.gz

/home/mark.tar.gz

[natasha@ststor01 home]$ 
---
