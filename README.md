# Bash Shell Scripting Notes
Shell Scripting Short Notes &amp; Reference

## Introduction

Any series of terminal commands can be put into a script. The shell, which is an interpreter, reads the script and executes those commands. Shell Scripting is useful to automate tasks, performing complex execution, etc.

BASH script = **B**ourne **A**gain **SH**ell script

### Executing a Shell Script:

Give execute permission to your script. Ex: `chmod +x /path/to/yourscript.sh`

And to run your script. Ex: `/path/to/yourscript.sh`

We can also use the relative path if it is known: Ex: `./yourscript.sh` (`./` is necessary if directory is not in $PATH!)

### First Line of a shell script:

When we create a shell script and wish to run it, we need to make sure it has the execute permission.

Use `chmod` command to add it, if execute permission does not exist.

Every script STARTS with a line like this:

`#!/path/to/interpreter`

The #(sharp) and !(bang/exclamation) are collectively known as the 'SHEBANG'.

The first line tells us which interpreter(Shell) is supposed to execute this script!.
Examples:
- `#!/bin/csh` = c shell, 
- `#!/bin/ksh` = k shell, 
- `#!/bin/zsh` = z shell, 
- `#!/bin/bash` = bash shell

Basically, when the script runs: It is the specified shell that is running as a process and the file that it executes is the script's path.

You don't have to use a shell as an interpreter: Ex: `#!/usr/bin/python` => Uses the python interpreter (Hence, a python script)

Note: If we don't specify the interpreter, the current shell executes the script but this is tricky.
If the script contains commands not understood by the current shell, it could cause errors (Don't do this!)

### Printing/Displaying:

`echo` command = It prints the supplied argument/string. Every echo statement prints on a NEW LINE.

Ex: `echo "Hello World!"` => Output: `Hello World!`

### Comments:

Every line other than the first line (#! /bin/..) that STARTS with a '#'(pound/sharp/hash) marks a comment, example:

```
 #!/bin/bash

 # Let's print something	=> A comment

 echo "Hello There"

 # End of printing		=> A comment
```

- If '#' STARTS on the line, the WHOLE LINE is IGNORED.
- If '#' appears in the MIDDLE of a line, Anything to the RIGHT of a '#' is IGNORED.

### Variables:

Storage locations that have a name. They are 'name-value' pairs.

Syntax: `VARIABLE_NAME="Value"`

**NOTE: NO SPACES before or after the '='**

- The variable names are CASE-SENSITIVE!.
- And, by "convention", they are usually in UPPERCASE.

### Variable Names:

Variable names can contain: 
- LETTERS (a-z, A-Z), 
- DIGITS (0-9), & 
- UNDERSCORES (_) [ONLY!]

VARIABLE NAMES **CANNOT** START WITH A DIGIT!

### Using Variables:

Precede the variable name with a '$' sign. Ex:
```
MY_SHELL="bash"
echo "This script uses the $MY_SHELL shell"
```
Output: This script uses the bash shell.

**Alternatively: (Optional)**

Enclose the variable in curly brackets '{}' and preced it with a '$', Ex:
```
MY_SHELL="bash"
echo "I am ${MY_SHELL}ing shell"
```
Output: I am bashing on my keyboard

Hint: It's a best practice to use the ${VARIABLE} syntax if there is text or characters that directly precede or follow the variable. 

If a specified variable does NOT exist, Nothing is printed in its place in the echo statement.

### Storing Commands in variables:

`VAR_NAME=$(command)`	

(or)	

```
VAR_NAME=`command`
```

(Command enclosed with tilde sign: Older Scripts) 

### Tests:

Syntax: `[ condition-to-test-for ]`

Returns true if test passes, else false. Ex: 

`[ -e /etc/passwd]` = tests if the '/etc/passwd' file exists.

#### File Operator Tests:

General Test Syntax: `[ -flag fileOrDirPath]`
Flags:
- `-d` = True if file is a directory
- `-e` = True if file exists
- `-f` = True if file exists and is a regular file
- `-s` = True if file exists and is NOT empty
- `-r` = True if file is readable by you
- `-w` = True if file is writable by you
- `-x` = True if file is executable by you

#### String Operator Tests:

##### General Test Syntax: `[ -flag STRING]`

Flags:
- `-z` = True if STRING is empty
- `-n` = True if STRING is NOT empty

Equality Tests:
- `STRING1 = STRING2` = True if strings are equal
- `STRING1 != STRING2` = True if strings are NOT equal

NOTE: For testing Variable string in the conditions, enclose it in quote(""). Ex: 

`"$MY_SHELL" = "bash"`

#### Arithmetic Operator Tests:

General Test Syntax: `[ arg1 -flag arg2]`

Flags:
- `-eq` = True if arg1 equals to arg2
- `-ne` = True if arg1 is NOT equal to arg2
- `-lt` = True if arg1 is LESS THAN arg2
- `-le` = True if arg1 is LESS THAN OR EQUAL TO arg2
- `-gt` = True if arg1 is GREATER THAN arg2
- `-ge` = True if arg1 GREATER THAN OR EQUAL TO arg2


(`man test` => Help on tests.)

### The `if` Condition: (Uses Tests in its conditional)

```
if [ condition-is-true ]
then
	command 1
 	command 2
 	...
	command N
fi
```

### The `if-else` Condition: (Uses Tests in its conditional)

```
if [ condition-is-true ]
then
	commands 1 ... N
else
	commands 1 ... N
fi
```

### The `if-elif-else` Condition: (Uses Tests in its conditional)

```
if [ condition-is-true ]
then
	commands 1 ... N
elif [ condition-is-true ]
then
	commands 1 ... N
else
	commands 1 ... N
fi
```

### The `for` loop:

```
for VARIABLE_NAME in ITEM_1 ... ITEM_N
do
	command 1
	command 2
	...
	command N
done
```

First item in the block is assigned to variable and commands are executed, next item in in the block is assigned to variable and commands are executed again, and so on ...

VARIABLE_NAME need NOT be declared earlier.

**Ex 1:**

```
for COLOR in red green blue
do
    echo "COLOR: $COLOR"
done
```

Output:
```
COLOR: red
COLOR: green
COLOR: blue
```

**We can also store the items in a variable, separated by spaces!**

**Ex 2:**

```
COLORS="red green blue"
for COLOR in $COLORS
do
    echo "COLOR: $COLOR"
done
```

Output:
```
COLOR: red
COLOR: green
COLOR: blue
```

Note:: Bash scripting also contains `while` loops.

### Positional Parameters:

Syntax (On the CLI): `$ script.sh parameter1 parameter2 parameter3`

- `$0` = "script.sh" (The script itself)
- `$1` = "parameter1"
- `$2` = "parameter2"
- `$3` = "parameter3"

We can Assign Positional Parameters to Variables. Ex: 

`USER=$1`

#### The `$@`:

`$@` contains all the parameters starting from Parameter 1 to the last parameter. It can be used to loop over parameters. Ex:
```
for USER in $@
do
	echo "Archiving user : $USER"
	...
	...
done
```

### Accepting User Input (STDIN):

`read` command.

Syntax: `read -p "PROMPT" VARIABLE`

Note: The input could also come from pipelined(|) or redirected output/input(<, >) if STDIN is changed to those.

Ex:
```
read -p "Enter a UserName: " USER
echo "ARCHIVING $USER"
```

### Exit Status of Commands:

Every command returns an exit status. Range of the status is from `0 - 255`. It is used for Error checking. 

- `0` 			= Success
- Other than `0` 	= Error condition

**Use `man` on the command to find out what exit status means what (for the command).**

#### The `$?`: (Return code/exit status)

`$?` contains the return code(exit status) of the 'previously executed' command.

Ex:
```
ls /not/here
echo "$?"
```

Output: `2` is echoed, which is the return command.

Ex:
```
HOST="google.com"
ping -c 1 $HOST		=> -c tells command to send only 1 packet to test connection
if [ "$?" -eq "0" ]
then
	echo "$HOST reachable."
else
	echo "$HOST unreachable."
fi
```

#### Storing return code/exit status in a variable:

Ex: 
```
ping -c 1 "google.com"
RETURN_CODE=$?		=> Now, we can use the variable RETURN_CODE anywhere in the script.
```

### Chaining Commands:

- `&&` => AND => Executes commands one after the other UNTIL one of them FAILS. (in that case -> short-circuiting)

(It executes commands as long as they are returning exit statuses `0`. STOPS as soons as a command does NOT return 0)

Syntax: `cmd1 && cmd2 && ...`

- `||` => OR => Executes commands one after the other UNTIL one of them SUCCEEDS. (in that case -> short-circuiting)

(It executes commands as long as they are returning exit statuses NOT `0`. STOPS as soons as a command returns 0)

Syntax: `cmd1 || cmd2 || ...`

- `;` => Semicolon => Executes commands ONE AFTER ANOTHER without checking the exit statuses/return codes.

(It it same as/equivalent to executing each of the commands on a separate line)

Syntax: `cmd1 ; cmd2 ; ...`

### Exit Statuses of Shell scripts:

Shell scripts too can have exit statuses.

`exit` command needs to be used. 

- `exit` => Exits the script with exit status equal to that of the previously executed command within the script.
- `exit X` => Exits with exit status X. X is a number between 0 & 255. (0 = Success, !0 = Error)

(No exit status => This also equals the exit status of the previously executed command within the script)

### Functions:

Reduces script length, **"DRY"(Dont Repeat Yourself) - concept of functions**.

A function:
- Is a block of reusable code.
- Must be defined before use.
- Has parameter support.
- It can return an "exit status/return code".

**Method 1:**
```
function function-name() {
	# code goes here
}
```

**Method 2:**
```
function-name() {
	# code goes here
}
```

#### Calling a function:

`function-name` 

We do **NOT** use parentheses () in the function like in other programming languages. Functions need to be defined before they are used. (In Top Down order of parsing) (That is, Function Definition must have been scanned(Top->Down parsing) before the call to the function)

**Functions can call other functions**

#### Parameters to functions:

`function-name parameter1 parameter2 ...`

- `$1`, `$2`, ... = Parameter1, Parameter2, ...
- `$@` = Array of all the parameters

NOTE: `$0` REFERS TO THE SHELL SCRIPT ITSELF (NOT THE FUNCTION!)

Ex:
```
function hello() {
	echo "Hello, $1"
}
hello Jason
```

Output: `"Hello, Jason"`

#### Variable Scope:

- ALL Variables are GLOBAL by Default!
- Variables have to be DEFINED before Use.

Therefore, All variables defined 'before' a 'function call' can be accessed within it. Those that are defined 'after' the 'function call' **cannot** be accessed.

- Accessing a variable that has NOT been defined before the function call : Nothing/null/No Value for Variable.
- Accessing a variable that has been defined before the function call : Correct value.

#### Local Variables:

Variables that can be accessed only within the function that it is declared. Use the keyword `local`.

Syntax: `local LOCAL_VAR=someValue`

Only functions can have local variables! (Try to keep variables local inside a function)

#### Exit Status/Return Code of functions:

- Explicity: `return <RETURN_CODE>`

Note: For the WHOLE SCRIPT, we used the `exit <RETURN-CODE>` command. For functions it is `return` keyword.

- Implicitly: The exit status of the last command executed within the function.

Exit status range: `0 to 255`

- `0` = Success, 
- All other values = errors of some kind,

- `$?` = gets the exit status of last executed command(after execution of cmd)/function(after the call)/script(terminal))

Tip: write function to backup files, returning 0 exit status if successful.

NOTE:
- `basename fileOrDirPath` => returns just the filename/Directory name after stripping off the path to the file.
- `dirname fileOrDirPath` => Getting the directory of the file/directory.

### Shell Script CheckList: 

HOW to write your scripts (PATTERN!)

1. Does your script start with a shebang?
2. Does your script include a comment describing the purpose of the script?
3. Are the global variables declared at the top of your script, following the initial comment(s)?
4. Have you grouped all of your functions together following the global variables?
5. Do your functions use local variables?
6. Does the main body of your shell script follow the functions?
7. Does your script exit with an explicit exit status?
8. At the various exit points, are exit statuses explicitly used?

**EXAMPLE**
```
#!/bin/bash
#
# <Replace with the description and/or purpose of this shell script.>

GLOBAL_VAR1="one"
GLOBAL_VAR2="two"

function function_one() {
	local LOCAL_VAR1="one"
	# <Replace with function code.>
}

# Main body of the shell script starts here.
#
# <Replace with the main commands of your shell script.>

# Exit with an explicit exit status. Ex: exit 0
```

### WILDCARDS:                       

**(Read in detail in: 'Command Line Basics Course/pdf/cheatsheet')**

Wildcards are Character or Strings used for pattern matching. Also referred to as **'Globbing'**: Commonly used to match file or directory paths. Wilcards can be used with **MOST** commands.

- * - matches 0 or more characters (Ex: *.txt, a*, a*.txt, *a*.txt)
- ? - matches exactly 1 character (Ex: ?.txt, a?, a?.txt, ?a.txt, a???.txt)
- [] - a character class(Mathces any of the characters inside bracket, exactly one match.) (Ex: [aeiou], ca[nt]* => can cant candy ... etc)
- [!] - matches any of the characters NOT included inside the bracket. Exactly one match. (Ex: [!aeiou]* => First character should not be a vowel)
- [x-y] - creating a range of values to match. Exactly one match. (Ex: [a-g]* => start with any letter between a and g, [3-6]* => start with any number between 3 and 6)

- [[:alpha:]] => Matches upper and lower case letters. Exactly One match.
- [[:alnum:]] => Matches upper and lower case letters or any decimal digits (0-9). Exactly One match.
- [[:digit:]] => Matches decimal digits (0-9). Exactly One match.
- [[:lower:]] => Matches lower case letters. Exactly One match.
- [[:space:]] => Matches White Space. Exactly One match.
- [[:upper:]] => Matches upper case letters. Exactly One match.

- \ - Match a wildcard character (Escape) (Ex: *\? => matches anything followed by a question mark [Ex: 'done?'])

TIP:: good practice to **not** include wildcards as filenames/directory names.

Examples:
- `ls *.txt` => list all files ending with '.txt'
- `ls a*` => list all files starting with 'a'
- `ls *aA` => list all files containing 'aA'
- `ls ?` => list all files containing one character
- `ls ??` => list all files containing two characters
- `ls ?.txt` => list all files containing one character and ending with '.txt'
- `ls fil?` => list all files such as 'file', 'filk', 'fild' ... (starting with 'fil' followed by any one character)
- `ls [a-d]*` => list all files starting with any lowercase letter in between 'a' and 'd'
- `ls *mp[[:digit:]]` => lists all files ending with 'mp' and a digit(0-9). [Ex: ending with 'mp3', 'mp4', ...]

#### Using Wildcards in Shell Scripts:

Wildcards are great for working with groups of files of directories.

We can use it just like in regualar commands. Ex1:
```
#!/bin/bash
cd /var/www
cp *.html /var/www-just-html
```

Ex2:
```
for FILE in /var/www/*.html
do
	echo "Copying $FILE"
	cp $FILE /var/www-just-html
done
```

(NOTE: In the above example, the wildcard matches in the for loop expand as a List/Array that can be Iterated.)

### Switch-Case Statements:

(Similar to `Switch-Case`) => Testing for multiple values. Use in-place of many 'if-elif-elif-elif...' scenarios!

Syntax:
```
case "$VAR" in
	pattern_1)
		#commands go here
		;;
	pattern_N)
		#commands go here
		;;
esac
```

Patterns are case-sensitive!. Once a pattern has been matched, the commands of a pattern are executed until `;;` is reached! `;;` is like a break statement.

Examples. Ex1:
```
case "$1" in
	start)
		/usr/sbin/sshd			# Executes the script at the specified path!
		;;
	stop)
		kill $(cat /var/run/sshd.pid)
		;;
esac
```

Note: Wildcards can be used as patterns: Ex: `*)` matches anything.

The pipe(|) maybe used as an 'OR' in the patterns. Ex2:
```
case "$1" in
	start|START)
		/usr/sbin/sshd
		;;
	stop|STOP)
		kill $(cat /var/run/sshd.pid)
		;;
	*)
		echo "Usage: $0 start|stop" ; exit 1
		;;
esac
```

The above example matches either upper or lower case 'start', else either upper or lower case 'stop', else it
matches anything that did not match one of the first two cases.

We can use other wildcards in the patterns as well. Ex3:
```
case "$1" in
	[yY]|[yY][eE][sS])
		echo "You answered yes."
		;;
	[nN]|[nN][oO])
		echo "You answered no."
		;;
	*)
		echo "Invalid Answer."
		;;
esac
```
Above matches y, yes (case-insensitive) or n, no(case-insensitive).

NOTE: All the rules of wildcards are valid for patterns of case statements.

Case Statements:
- In place of if statements,
- Wildcards maybe used for patterns.
- Multiple patterns can be matched with the help of a pipe(|) which acts as an 'OR' in the pattern.

### Logging:

`Syslog` => Uses standard facilities and severities to categorize messages.

- `Facilities`: `kern`, `user`, `mail`, `daemon`, `auth`, `local0`, `local7`, etc.
- `Severities`: `emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`.

Ex: if your script is using mail, you could use the 'mail' facility for logging.

#### Log files locations are CONFIGURABLE:

1. '/var/log/messages' 

(or)

2. '/var/log/syslog'

(Location depends on system)

#### Logging with `logger`:

`logger` is a command line utility - used for logging 'syslog' messages. By default, it creates `user.notice` messages.

1. Basic logging message: `logger "Message"`

Example output: 'Aug 2 01:02:44 linuxsvr: Message'.

2. Logging message with facility and severity: Syntax: `logger -p facility.severity "Message"` 

Ex: `logger -p local0.info "Message"`

3. Tagging the log message: Use the `-t` option followed by tagName

Usually you want to use the script's name as tag in order to easily identify the source of the log message. `logger -t myscript -p local0.info "Message"`

Example output: 'Aug 2 01:02:44 linuxsvr myscript: Message'

4. Include PID(Process ID in the log message): use `-i` option: `logger -i -t myscript "Message"`

Ex output: 'Aug 2 01:02:44 linuxsvr myscript[12986]: Message'

5. Additionally display log message on screen (apart from already logging it to the log file): use `-s` option

Ex: `logger -s -p local0.info "Message"`

NOTE: Different facilities and severities could cause the system logger to route the log messages to a different locaton/log file.

**NOTE:**

`$RANDOM` generates a random number. Ex:

`echo "$RANDOM"` => 29133

Using `shift`: `shift` shifts the command line arguments to the left (The script name is deleted).

The original param1 becomes $0, original param2 becomes $1, and so on ...

### `while` loops:

Loop control (Alternative to `for`)

Syntax:
```
while [ CONDITION_IS_TRUE ]
do
	command 1
	command 2
	...
	command N
done
```

While the command in the command keeps returning 0(success) exit status, the while loop keeps looping/executing. Usually commands inside the while loop change the condition for the next iteration's check.

#### Infinite While loops:

Condition is always true, keeps looping forever (Use `CTRL-C` to exit the script - when executed in the terminal). You may need to use the `kill` command to kill the process - when run as an application outside terminal.

You may want to run some background processes infinitely: Ex: Running a daemon process in the background:
```
while true
do
	command 1 ... N
	sleep 1
done
```

The `sleep` command: Used to 'PAUSE' the execution of the code for a given number of seconds (as argument).

Examples of while loops: Ex1:
```
INDEX=1
while [ $INDEX -lt 6 ]
do
	echo "Creating project-${INDEX}"
	mkdir /usr/local/project-${INDEX}
	((INDEX++))				# Arithmetic operations are enclosed within '((...))'
done
```

Ex2:
```
while [ "$CORRECT" != "y" ]
do
	read -p "Enter your name: " NAME
	read -p "Is $NAME correct?: " CORRECT
done
```

Ex3:
```
while ping -c 1 app1 > /dev/null	# ping must succeed & redirect o/p to /dev/null as we don't want to see msg
do
	echo "app1 still up ... "
	sleep 5				# Pause execution for 5 seconds
done
echo "app1 is down! ... "
```

#### Reading a file LINE-BY-LINE:

- Not possible in `for` loop since it reads word by word.

While loop example:
```
LINE_NUM=1
while read LINE
do
	echo "${LINE_NUM}: ${LINE}"
	((LINE_NUM++))
done < /etc/fstab
```

- The /etc/fstab is taken as input to the whole while loop.
- `read LINE` reads the current line of the file
- Since it is a condition of the while, while stops when lines of the files are over

Note: input to read command need not only be lines from a file, we can use pipelining to the while to read from commands, etc.

Note: read command supports splitting of data that it reads into multiple variables!

#### Exiting a loop before it's normal end:

- Use `break` statement.
```
while [ CONDITION_IS_TRUE ]
do
	...
	break
	...
done
```

#### Skipping iteration and executing next iteration on a loop:

- Use `continue` statement.
```
while [ CONDITION_IS_TRUE ]
do
	...
	continue
	...
done
```

### Arithmetic Operations:

We can execute arithmetic operations within double parentheses'((...))'. Ex: 

`((INDEX++))` = increments INDEX value by 1 (++ operator). Useful within loops.


### Debugging Shell Scripts:

'bug' => Means error

- Examine inner workings of your script
- Get to the Source/Root of the problem
- Fix Bugs(errors)

#### `-x` option:

Debug Whole script:

- `#!/bin/bash -x` = prints commands and their arguments as they execute. That is: Values of variables, values of wildcard matches. 

Ex: `VAR1="HI"` (Call an x-trace)

- `PS4` controls what is shown before a line while using '-x' command (default is '+') (LATER..)

- Debug from command line: 
	- `set -x` = Start Debugging
	- `set +x` = Stop Debugging

Ex: 
```
$ set -x
$ ./scriptName.sh
<output>
$set +x
```

- Debug only a portion of the code:
Ex:
```
#!/bin/bash
...
set -x
echo $VAR_NAME
set +x
...
```

Note: For every command that is debugged, a '+' sign appears to it's left. The outputs of commands (Ex: output of echo command) DON'T have a '+' next to them.

Note: Using ONLY THE `-x` flag executes subsequent lines of code (commands) even if a previous command was erroneous!

#### Exit on Error:

- `-e` flag. (Exit immediately if a command exists with a non-zero status.)

Syntax: `#!/bin/bash -e`

Ex: It can be combined with the trace (-x) option.
- `#!/bin/bash -e -x`
- `#!/bin/bash -ex`
- `#!/bin/bash -xe`
- `#!/bin/bash -x -e`

#### Print Shell Input Lines (As they are being read):

- `-v` flag. (Can also be combined with -x and -e.)

Prints input lines before (without) any substitutions and expansions are performed. Therefore, All the lines of the shell script are printed as they are, and the outputs of printing commands(like echo) are also displayed.

- `#!/bin/bash -vx` = Useful, because we can see trace (substituted input) lines as well as shell script lines!

More Info: `help set` or `help set | less`

### Manual Debugging:

You can create your own debugging code. Ex: Use a sepcial variable like DEBUG. (DEBUG=true, DEBUG=false)

- Boolean `true`  :   exit status 0 (success)
- Boolean `false` :   exit status non-zero (failure)

Ex 1:
```
#!/bin/bash
DEBUG=true
$DEBUG && echo "debug mode on!"
```

```
#!/bin/bash
DEBUG=false
$DEBUG || echo "debug mode off!"
```

Ex 2: (When you want to echo lines in debug mode)
```
#!/bin/bash
DEBUG="echo"
$DEBUG ls
```

Prints ls output to screen since $DEBUG is nothing but 'echo'

Ex 3:
```
#!/bin/bash
function debug() {
	echo "Executing $@"
	$@
}
debug ls
```

### Syntax Highlighting:

Syntax errors are common. Use a text editor and enable syntax highlighting to identify syntax errors. (Ex: vim, emacs) Helps us catch syntax errors.

#### PS4:

Controls what is displayed before a line while using the `-x` option (during debugging). Default value is '+'

Bash Variables:
- BASH_SOURCE,  (name of the script)
- LINENO,       (line number of the script)
- FUNCNAME 		(function name)
- etc ... 

We can explicitly set the PS4 variable.

Ex:
```
#!/bin/bash -x
...
PS4='+ $BASH_SOURCE : $LINENO : ${FUNCNAME[0] : '
...
```

Example Output: '+ ./test.sh : 3 : debug() : TEST_VAR=test'

### File Types (DOS/WINDOWS vs UNIX/LINUX):

Control character is used to represent end of line in both DOS and Unix text files.

Control Character:
1. Unix/Linux: Line Feed
2. DOS: Carriage return & a Line Feed (2 characters)

- `cat -v script.sh` = View the file along with the carriage returns (^M)

When opening Linux/Unix text files on a DOS/Windows system: We may see one long line without new lines.

And, in the opposite: We may see additional characters on Unix/Linux (`^M`). => Might run into errors while executing the files.

Therefore, need to remove the carriage returns in order to run the file. The shebang `#!` is very important.

#### Knowing what type of file: (To which shell does the script belong to?)

`file script.sh` 

Example Output: 
- `script.sh: Bourne-Again shell script, ASCII text executable` => UNIX script,
- `script.sh: Bourne-Again shell script, ASCII text executable, with CRLF line terminators` => DOS script,

#### Converting DOS text scripts to UNIX scripts:

- `dos2unix script.sh` => Removes incompatible characters(ex: DOS carriage returns) in DOS to match with UNIX text scripts. (So that we can run it on UNIX/LINUX)

Confirm the removal of incompatible characters by running `file script.sh` again to see what type of shell the script runs in. (Should be one of the unix shells that you are using.)

#### Converting UNIX text scripts to DOS scripts:

- `unix2dos script.sh` => Does the opposite of dos2unix. (So that we can run it on DOS/WINDOWS)

How does all this happen? => When editing file in one OS and operating and using it another, Copying from one OS and pasting in another (via net, etc), Copying from web browsers into the system ... many ways!]


*************
- USE SHELL SCRIPTS TO AUTOMATE TASKS - REPETITIVE WORK
- SHELL SCRIPTS CAN BE SHORTCUTS - DON'T HAVE TO REMEMBER EVERYTHING (LIKE EVERY COMMAND)
- HAPPY SCRIPTING!
*************

**THE END**


















