# Introduction to Bash Scripting

[Source: Codecademy](https://www.codecademy.com/courses/learn-the-command-line)

Bash (or shell) scripting is a great way to automate repetitive tasks and can save you a ton of time as a developer. Bash scripts execute within a Bash shell interpreter terminal. Any command you can run in your terminal can be run in a Bash script. When you have a command or set of commands that you will be using frequently, consider writing a Bash script to perform it.

There are some conventions to follow to ensure that your computer is able to find and execute your Bash scripts. The beginning of your script file should start with `#!/bin/bash`on its own line. This tells the computer which type of interpreter to use for the script. When saving the script file, it is good practice to place commonly used scripts in the `~/bin/`directory.

The script files also need to have the "execute" permission to allow them to be run. To add this permission to a file with filename: `script.sh` use:

```
chmod +x script.sh
```

Your terminal runs a file every time it is opened to load its configuration. On Linux style shells, this is `~/.bashrc` and on OSX, this is `~/.bash_profile`. To ensure that scripts in `~/bin/` are available, you must add this directory to your `PATH` within your configuration file:

- `PATH=~/bin:$PATH`

Now any scripts in the `~/bin` directory can be run from anywhere by typing the filename.

# Variables

Within bash scripts (or the terminal for that matter), variables are declared by simply setting the variable name equal to another value. For example, to set the variable `greeting` to "Hello", you would use the following syntax:

```
greeting="Hello"
```

Note that there is no space between the variable name, the equals sign, or "Hello".

To access the value of a variable, we use the variable name prepended with a dollar sign (`$`). In the previous example, if we want to print the variable value to the screen, we use the following syntax:

```
echo $greeting
```

# Conditionals

When bash scripting, you can use conditionals to control which set of commands within the script run. Use `if` to start the conditional, followed by the condition in square brackets (`[ ]`). `then` begins the code that will run if the condition is met. `else` begins the code that will run if the condition is not met. Lastly, the conditional is closed with a backwards `if`, `fi`.

A complete conditional in a bash script uses the following syntax:

```
if [ $index -lt 5 ]
then
  echo $index
else
  echo 5
fi
```

Bash scripts use a specific list of operators for comparison. Here we used `-lt` which is "less than". The result of this conditional is that if `$index` is less than 5, it will print to the screen. If it is 5 or greater, "5" will be printed to the screen.

Here is the list of comparison operators for numbers you can use within bash scripts:

- Equal: `-eq`
- Not equal: `-ne`
- Less than or equal: `-le`
- Less than: `-lt`
- Greater than or equal: `-ge`
- Greater than: `-gt`
- Is null: `-z`

When comparing strings, it is best practice to put the variable into quotes (`"`). This prevents errors if the variable is null or contains spaces. The common operators for comparing strings are:

- Equal: `==`
- Not equal: `!=`

For example, to compare if the variables `foo`and `bar` contain the same string:

```
if [ "$foo" == "$bar"]
```

# Loops

There are 3 different ways to loop within a bash script: `for`, `while` and `until`.

A for loop is used to iterate through a list and execute an action at each step. For example, if we had a list of words stored in a variable `paragraph`, we could use the following syntax to print each one:

```
for word in $paragraph
do
  echo $word
done
```

Note that `word` is being "defined" at the top of the for loop so there is no `$` prepended. Remember that we prepend the `$` when accessing the value of the variable. So, when accessing the variable within the `do` block, we use `$word` as usual.

Within bash scripting `until` and `while` are very similar. `while` loops keep looping while the provided condition is true whereas `until`loops loop until the condition is true. Conditions are established the same way as they are within an `if` block, between square brackets. If we want to print the `index` variable as long as it is less than 5, we would use the following `while` loop:

```
while [ $index -lt 5 ]
do
  echo $index
  index=$((index + 1))
done
```

Note that arithmetic in bash scripting uses the `$((...))` syntax and within the brackets the variable name is not prepended with a `$`.

The same loop could also be written as an `until` loop as follows:

```
until [ $index -eq 5 ]
do
  echo $index
  index=$((index + 1))
done
```

# Inputs

To make bash scripts more useful, we need to be able to access data external to the bash script file itself. The first way to do this is by prompting the user for input. For this, we use the `read` syntax. To ask the user for input and save it to the `number` variable, we would use the following code:

```
echo "Guess a number"
read number
echo "You guessed $number"
```

Another way to access external data is to have the user add input arguments when they run your script. These arguments are entered after the script name and are separated by spaces. For example:

```
saycolors red green blue
```

Within the script, these are accessed using `$1`, `$2`, etc, where `$1` is the first argument (here, "red") and so on. Note that these are 1 indexed.

If your script needs to accept an indefinite number of input arguments, you can iterate over them using the `"$@"` syntax. For our `saycolors` example, we could print each color using:

```
for color in "$@"
do
  echo color
done
```

Lastly, we can access external files to our script. You can assign a set of files to a variable name using standard bash pattern matching using regular expressions. For example, to get all files in a directory, you can use the `*` character:

```
files = /some/directory/*
```

You can then iterate through each file and do something. Here, lets just print the full path and filename:

```
for file in $files
do
  echo $file
done
```

# Aliases

You can set up aliases for your bash scripts within your `.bashrc` in Linux or `.bash_profile` in MacOS file to allow calling your scripts without the full filename. For example, if we have our `saycolors.sh` script, we can alias it to the word `saycolors` using the following syntax:

```
alias saycolors='./saycolors.sh'
```

You can even add standard input arguments to your alias. For example, if we always want "green" to be included as the first input to `saycolors`, we could modify our alias to:

```
alias saycolors='./saycolors.sh "green"'
```

# Review

Take a minute to review what you've learned about bash scripting.

- Any command that can be run in the terminal can be run in a bash script.
- Variables are assigned using an equals sign with no space (`greeting="hello"`).
- Variables are accessed using a dollar sign (`echo $greeting`).
- Conditionals use `if`, `then`, `else`, `fi`syntax.
- Three types of loops can be used: `for`, `while`, and `until`.
- Bash scripts use a unique set of comparison operators:
  - Equal: `-eq`
  - Not equal: `-ne`
  - Less than or equal: `-le`
  - Less than: `-lt`
  - Greater than or equal: `-ge`
  - Greater than: `-gt`
  - Is null: `-z`
- Input arguments can be passed to a bash script after the script name, separated by spaces (myScript.sh "hello" "how are you").
- Input can be requested from the script user with the `read` keyword.
- Aliases can be created in the `.bashrc` or `.bash_profile` using the `alias` keyword.

# Running Commands

We can run any CLI commands inside the bash script. Example: `ls -al`

In order to store the output of running a command, we can assign them to variables like this:

```
VAR_NAME=$(command)
```
Example: `listAll=$(ls -a)`

# Project: Build a Build Script

One common use of bash scripts is for releasing a "build" of your source code. Sometimes your private source code may contain developer resources or private information that you don't want to release in the published version.

In this project, you'll create a release script to copy certain files from a **source** directory into a **build** directory.

1. Take a look at the **build** and **source** folders. The objective of our script is to files from **source** to **build**, with a couple of exceptions and modifications.

```
$ ls                                      
build  script.sh  source  
```

Get started on the script by adding a header to **script.sh**, identifying the type of script.

```bash
#!/bin/bash
```

2. Let's welcome the user to the script. Feel free to use your own style here. Make sure to save your script. Test your script in the terminal using `./script.sh`.

```bash
#!/bin/bash
echo "Welcome User"
```

```
$ ./script.sh
Welcome User
```

3. Since we are creating a new build, let's verify with the user that they have updated **changelog.md** with the current release version.

The first line of the file contains a version number with markdown formatting like so:

```
## 1.1.1
```

Read the first line of this file into a variable `firstline`. You can use the linux command [head](http://www.linfo.org/head.html) for this purpose.

```
$ ls source/
bar.js        foo1.html  secretinfo.md
buzz.css      foo2.html
changelog.md  foo3.html
```

```bash
#!/bin/bash
echo "Welcome User"
release=$(head -n1 ./source/changelog.md)
echo $release
```

```
$ ./script.sh
Welcome User
## 11.2.3
```

4. We want just the version number without the markdown formatting. The command [read](http://linuxcommand.org/lc3_man_pages/readh.html) can be used to split a string into an array using the `-a` argument.

Split the string `firstline` into the array `splitfirstline`.

The syntax for splitting a string `foo`into an array `bar` is:

```bash
read -a bar <<< $foo
```

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
echo $splitfirstline
```

```
$ ./script.sh
Welcome User
##
```

5. Now we are ready to set the value of the version of the script. It is located in index `1` of the array `splitfirstline`.

The syntax for accessing the value at `index` of an array `foo` is:

```
${foo[index]}
```

Save the version to a variable, `version`.

Print a statement to the terminal notifying the user of the version they are building.

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
```

```
$ ./script.sh
Welcome User
11.2.3
```

6. Let's give the user a chance to exit the script if they need to make a change to the version.

Ask the user to enter "1" (for yes) to continue and "0" (for no) to exit.

Assign their response to the variable `versioncontinue`.

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
echo "Enter 1 to continue, 0 to exit"
read versioncontinue
```

```
$ ./script.sh
Welcome User
11.2.3
Enter 1 to continue, 0 to exit
1
```

7. Add a conditional. If the user said "1" to the continue question, we will execute the rest of our script. For now, respond "OK".

If the user did not, tell them "Please come back when you are ready".

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
echo "Enter 1 to continue, 0 to exit"
read versioncontinue
if [ $versioncontinue -eq 1 ]
then
	echo 'OK'
else
	echo 'Please come back when you are ready'
fi
```

```
$ ./script.sh
Welcome User
11.2.3
Enter 1 to continue, 0 to exit
0
Please come back when you are ready
```

8. Now we want to copy every file from **source** to **build** except for **secretinfo.md**.

Within the positive conditional (where we told the user "OK"), start by iterating over all the files in the **source ** directory and printing their names to the terminal.

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
echo "Enter 1 to continue, 0 to exit"
read versioncontinue
if [ $versioncontinue -eq 1 ]
then
	files=./source/*
	for file in $files
  do
  	echo $file
  done
else
	echo 'Please come back when you are ready'
fi
```

```
$ ./script.sh
Welcome User
11.2.3
Enter 1 to continue, 0 to exit
1
./source/bar.js
./source/buzz.css
./source/changelog.md
./source/foo1.html
./source/foo2.html
./source/foo3.html
./source/secretinfo.md
```

9. Check if the `filename` is "source/secretinfo.md". If it is, inform the user that it is not being copied.

Otherwise, inform the user that it is being copied.

Make sure to use spaces in your string conditional.

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
echo "Enter 1 to continue, 0 to exit"
read versioncontinue
if [ $versioncontinue -eq 1 ]
then
	files=source/*
	for file in $files
  do
  	if [ $file == "source/secretinfo.md" ]
    then
    	echo "$file Not copied"
    else
    	echo "$file being copied"
    fi
  done
else
	echo 'Please come back when you are ready'
fi
```

```
$ ./script.sh
Welcome User
11.2.3
Enter 1 to continue, 0 to exit
1
source/bar.js being copied
source/buzz.css being copied
source/changelog.md being copied
source/foo1.html being copied
source/foo2.html being copied
source/foo3.html being copied
source/secretinfo.md Not copied
```

10. Now we can actually copy the files. After informing the user the file is being copied, copy the file into the `build` directory.

You can use the terminal to make sure the right files have been copied:

```
ls build/
```

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
echo "Enter 1 to continue, 0 to exit"
read versioncontinue
if [ $versioncontinue -eq 1 ]
then
	files=source/*
	for file in $files
  do
  	if [ $file == "source/secretinfo.md" ]
    then
    	echo "$file Not copied"
    else
    	echo "$file being copied"
      cp $file build/
    fi
  done
else
	echo 'Please come back when you are ready'
fi
```

```
$ ./script.sh
Welcome User
11.2.3
Enter 1 to continue, 0 to exit
1
source/bar.js being copied
source/buzz.css being copied
source/changelog.md being copied
source/foo1.html being copied
source/foo2.html being copied
source/foo3.html being copied
source/secretinfo.md Not copied
$ ls build/
bar.js    changelog.md  foo2.html
buzz.css  foo1.html     foo3.html
```

11. The final thing we want to do is list the files in the **build** directory for the user.

Outside of the loop over the filenames in the directory, use the script to change the directory to the build directory. So that we don't forget, also add the command to change back to the directory with the script.

12. Add code to notify the user what files are currently in the build directory.

Be sure to reference the version in your message.

```bash
#!/bin/bash
echo "Welcome User"
firstline=$(head -n1 ./source/changelog.md)
read -a splitfirstline <<< $firstline
version=${splitfirstline[1]}
echo $version
echo "Enter 1 to continue, 0 to exit"
read versioncontinue
if [ $versioncontinue -eq 1 ]
then
	files=source/*
	for file in $files
  do
  	if [ $file == "source/secretinfo.md" ]
    then
    	echo "$file Not copied"
    else
    	echo "$file being copied"
      cp $file build/
    fi
  done
  cd build/
  echo "Build version $version contains:"
	ls
  cd ..
else
	echo 'Please come back when you are ready'
fi
```

```
$ ./script.sh
Welcome User
11.2.3
Enter 1 to continue, 0 to exit
1
source/bar.js being copied
source/buzz.css being copied
source/changelog.md being copied
source/foo1.html being copied
source/foo2.html being copied
source/foo3.html being copied
source/secretinfo.md Not copied
Build version 11.2.3 contains:
bar.js    changelog.md  foo2.html
buzz.css  foo1.html     foo3.html
```

13. You now have a build script for this repository. Feel free to play around with making it more robust. Some ideas:

- Copy **secretinfo.md** but replace "42" with "XX".
- Zip the resulting **build** directory.
- Give the script more character with emojis.
- If you are familiar with `git`, commit the changes in the build directory.

