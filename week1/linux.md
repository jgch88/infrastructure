http://www.ee.surrey.ac.uk/Teaching/Unix/

# Linux
## Operating System
* kernel, shell, programs
    * kernel: allocates time / memory to programs, handles filestore / communications in response to system calls
        * e.g. rm readme.md
        * search for program `rm` in filestore
    * shell
        * interface between user and kernel, a cli
* everything in UNIX is either a file or a process
    * a process is an executing program identified by a unique PID (process identifier)
    * a file is a collection of data
        * a file could be a directory (which contains information about its contents)

## Commands
    * `ls`
        * .files are hidden files
        * use `ls -a` to see them
        * `-l` or `ll` (long listing to see permissions)
        * `-lg` (group)
    * `mkdir`
    * `cd`
    * `pwd`
    * `cp`
    * `mv`
    * `rm`
    * `rmdir`
    * `clear` cmd+K?
    * `cat` bat?
    * `less` `head` `tail` (other ways to view parts of a file)
    * `grep`
        * `grep -i linux linux.md`
        * `grep . -rnw -e 'sometext'`
            * `-v` non matching
            * `-n` line number
            * `-c` count
    * `wc`
        * `-w` words
        * `-l` lines
        * `-c` chars
    * `sort`
    * `who` (shows who is on the system with you)
    * `man grep` (manual, man pages)
    * `apropos grep` (show related commands)
    * `chmod`
    * `ps` (see your own processes, PIDs and statuses)
    * `jobs` (list running, suspended, background processes, but only for current terminal window)
    * & (run process in background, e.g. `sleep 10 &`, `cat &`)
    * `fg` (foreground)
    * `kill` (can kill by PID via ps, or %job via jobs)
    * `quota` (disk space limits)
    * `df` (disk free space)
    * `du` (disk space used)
    * `gzip` `gunzip` `zcat`
    * `file` (not very accurate, e.g. js files are java?)
    * `diff` (git diff is probably based on this)
    * `find` (find files)
        * `find . -name "te*"`
    * `history`
    * ! (the up key in terminal is probably based on this)

## cat
    * reads in a file, or standard input (e.g. keyboard if no file provided)
    * shows it on standard output (e.g. screen)
    * can read multiple files `cat linux.md linux.md`

## >
    * `cat linux.md > file1`
    * redirects (standard output) into (file)
## >>
    * `cat linux.md >> file1`
    * appends (standard output) into file

## <
    * `cat < linux.md`
    * `sort < linux.md`
    * redirects (file) into (program?)

## Operator chaining
    * `sort < linux.md > sorted.txt`
    * can't do `who > names.txt < sort`, split into `who > names.txt`, `sort < names.txt`

## Piping
    * `who | sort` instead of the above names.txt
    * `who | wc`
    * `cat list1 list2 | grep p | sort` (display all lines of list1 and list 2 containing the letter 'p', and sort the result)

## Wildcards
    * `ls *.md`, `ls linux*`, `cat l*`, `cat list1 | grep 'a*'`, (0 or more characters matching)
    * `cat lis?1` (exactly 1 match)

## Filenames, Directorynames
    * / * & % have special meanings, avoid them in names.
    * files typically have a filename and a file extension separated by a '.'

## Filesystem Security
    * `ls -al` (https://unix.stackexchange.com/questions/103114/what-do-the-fields-in-ls-al-output-mean)
        * `-rwxrw-r--` 
            * 10 character string. first char is either d for directory or - for file
            * remaining 9 are in groups of 3:
                * left group: permissions for user that owns the file
                * middle group: permissions for group of people 
                * right group: permissions for all others
        * meaning of rwx:
            * directories:
                * r - allows users to list files
                * w - allows users to delete files, move files into it
                * x - the right to access files in directory (seems on by default in most cases)
            * files:
                * r - read
                * w - write
                * x - execute (may not have x rights to run a shell script, gradle script if not marked as executable)
    * `chmod`
        * `ugoa` (user, group, other, all (all is short for u+g+o, can also leave blank))
        * `+-` (add, remove permission)
        * `rwx` (read, write, execute)
        * e.g. `chmod +x list1` (gives all permissions to execute)
        * e.g. `chmod u-rw list2`
        * e.g. `chmod u-r backupdir` (can't do ls backupdir, BUT can still do vim backupdir/test.md!)

## Processes and Jobs
    * `ps` (if you have multiple terminals open you will see everything, e.g. vim, sleep 10, etc.)
        * `ps | wc` (shows an extra line... from wc?)
    * a process may be in the foreground, background or suspended
    * & (run process in background)
        * `sleep 10 &` (try it with fg, ctrl+z)
        * sleep seems to be based on time -> if you suspend it more than 10 seconds then return it to fg it finishes immediately instead of continuing the sleep where it left off
        * `sleep 10`
            * after suspending with ctrl+z
            * can either do `fg` or `bg` to resume the suspend
    * ctrl+z (suspend a process) (SIGSTP, SIGCONT)
        * cannot be intercepted by program
        * try this with a python turtle project stuck in turtle.mainloop() or `cat` with no file
            * will make the GUI window disappear
            * return control of terminal back to user
            * `jobs` to list suspended processes
            * `fg %1` to bring suspended process [1] back to foreground
    * ctrl+c (kill the process) (SIGINT)
        * program can intercept and decide how to shut down
    * ctrl+d (EOF)
        * terminates stdin
    * aside:
        https://stackoverflow.com/questions/11886812/whats-the-difference-between-sigstop-and-sigtstp
    * `kill`
        `-9` (killed by kernel, cannot be ignored)
