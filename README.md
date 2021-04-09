# An Introduction to Bash
*A compact introduction to using Bash.*

The command line is a text interface which takes in commands for the computer's operating system to run. It can be used to work through files and folders on the computer, run programs, and write powerful scripts to automate tasks. Bash (or shell) scripting is a great way to execute frequently used sets of commands.

The commands here are for Unix-based systems, but most of them will also work in Windows PowerShell.

This guide is far from comprehensive and does not aim to be. Although not necessary, it will be helpful to be familiar in at least one programming language, especially for the [Bash Scripting](#bash-scripting) section.

Throughout, note that file or directory arguments for commands can also be replaced with absolute or relative paths instead.

## Table of Contents

- [Code Standards](#code-standards)
  * [Indentation](#indentation)
  * [Comments](#comments)
    + [Inline Comments](#inline-comments)
    + [Block Comments](#block-comments)
  * [Quotes](#quotes)
  * [Names](#names)
- [File System Navigation](#file-system-navigation)
- [File System Manipulation](#file-system-manipulation)
- [Redirection](#redirection)
- [Other Useful Commands](#other-useful-commands)
- [Environment](#environment)
- [Bash Scripting](#bash-scripting)

## Code Standards

All written code should follow a style guide to ensure that standards are kept consistent across any codebase and make code easier to read. Badly written code is difficult to scale, optimise, and debug. Such is the importance of high code standards that this guide will discuss it as a separate section before any code is seen. Any new terminology or concepts mentioned in this section will be properly covered later in the guide.

We follow the standards in the [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html). In particular, this section will act as a reference for language-specific best practices for indentation, comments, quotes, and names, since these can vary across different programming languages.

### Indentation

The standard is to use two spaces for indented code.

### Comments

Comments begin with `#`.

#### Inline Comments

Inline comments should be used sparingly and not state the obvious. They should be separated by at least two spaces from the code and use commanding language rather than descriptive. For example, the following is preferred:

```Bash
x=$((x + 7))  # Compensate for border
```

As a comparison, the following should be avoided:

```Bash
x=$((x + 7))  # Compensates for border
```

#### Block Comments

Block comments should be written in complete sentences, be indented with the code which it is commenting, and come *before* the code which it is commenting. Placing the comment after is merciless on a confused reader. Paragraphs are separated by a single line containing a single `#`, and multi-sentence comments should have two spaces after a sentence-ending period (except after the final sentence).

```Bash
# This script is called with an integer argument.  It does not check for valid input.
#
# It's quite a nice way to access external data.
script.sh 7
```

### Quotes

Strings can be wrapped in either double quotes `" "` or single quotes `' '`. Double quotes will honour [string interpolation](https://en.wikipedia.org/wiki/String_interpolation), whereas single quotes will not, i.e. they contain raw string literals.

See the [Single Quotes](http://www.gnu.org/software/bash/manual/html_node/Single-Quotes.html) and [Double Quotes](http://www.gnu.org/software/bash/manual/html_node/Double-Quotes.html) sections of the Bash Reference Manual for more details.

### Names

Constants and environment variable names should be written in uppercase, with any word separation done using an underscore `_`. All other names should be written in *snake_case*.

## File System Navigation

Here are some common navigation commands:

```Bash
ls  # List the files and folders inside the current directory
ls -a  # Flag option to list all contents, including hidden contents
ls -l  # Flag option to list all contents in long format
ls -t  # Flag option to order files and directories by modified time
ls -alt  # Combine flag options
```

```Bash
pwd  # Print the working directory
```

```Bash
cd directory/  # Change directory
cd ..  # Move up one directory
```

## File System Manipulation

Commands can be used to copy, move and remove files and directories.

```Bash
mkdir directory_name  # Make a directory in the current directory
```

```Bash
touch file_name.ext  # Create a new file in the current directory
```

```Bash
cp file file_copy  # Destructively copy file to file_copy
cp file1 file2 destination_directory/  # Copy file1 and file2
cp * destination_directory/  # Use wildcard to copy all files
cp m*.txt destination_directory/  # Use wildcard to copy certain files
```

```Bash
mv file1 file2  # Rename file1 to file2 (destroy file2 if it exists)
mv file1 file2 destination_directory/  # Move file1 and file2
# Typical wildcard usage can also be utilised to move, similar to cp.
```

```Bash
rm file  # Permanently remove file
rm -r directory/  # Use recursion to remove directory completely
```

## Redirection

Operators can be used to redirect the input and output (I/O) of a command to and from other files and programs, or even to chain commands together in a pipeline.

The three I/O connections are:
- Standard input (*stdin*): the source for a command, such user keyboard input into the terminal. Note that this is not the same as the arguments accepted by a command.
- Standard output (*stdout*): the output of a command.
- Standard error (*stderr*): an error message outputted by a failed command.

The following commands will be useful for redirection:

```Bash
echo "Hello"  # Output to the terminal the string used as the input
```

```Bash
cat file  # Output to the terminal the contents of file
cat file1 file2  # Output the concatenation of file1 and file2
```

The `>` operator redirects the stdout of the left to a file on the right, which gets overwritten.

```Bash
echo "Hello" > hello.txt  # File hello.txt contains: Hello
cat hello.txt > greet.txt  # File greet.txt contains: Hello
```

The `>>` operator redirects the stdout of the left to a file on the right, which gets appended to.

```Bash
echo "Goodbye" >> greet.txt  # File greet.txt contains: Hello Goodbye

cat greet.txt >> hello.txt  # File hello.txt contains: Hello Hello Goodbye
```

The `<` operator redirects the file on the right as the stdin of the command on the left. This is useful in situations where a program requires user input but testing by the usual stdin is tedious. Instead, we can use a file containing the test input as the stdin through the `<` operator. Moreover, it allows for the testing of all of the input at once.

```Bash
program < test_input.txt
```

The `|` operator "pipes" the stdout of the left to a command on the right, which takes it as the stdin. Multiple | operators can be chained together.

```Bash
cat hello.txt | wc | cat > count.txt  # The file count.txt contains: 1 3 20
# Note that without piping, the wc command will output the word count
# and the filename.
```

```Bash
cat hello.txt | wc > count.txt  # Do the above, but in an alternate way
```

Two common commands to pipe together are `sort`, which takes a file as its stdin and orders it alphabetically as its stdout; and `uniq`, which takes a file as its stdin and filters out adjacent, duplicate lines as its stdout.

```Bash
sort file.txt | uniq > unique_file.txt
```

## Other Useful Commands

The `grep` (global regular expression print) command searches files for lines that match a [regular expression](https://github.com/cnguyen-uk/An-Introduction-to-Regex) pattern and returns the results.

```Bash
grep -i regex file.txt  # Enable case insensitivity, via -i flag option
```

The command can also be used on a directory to return files which contain regular expression matches.

```Bash
grep -R regex directory/  # Return filenames with lines, via -R flag option
grep -Rl regex directory/  # Return only filenames, via -Rl flag option
```

The `sed` (stream editor) command modifies stdin based on a regular expression, then returns this as output. It is similar to "find and replace" but is more powerful since regular expressions can be utilised.

```Bash
sed s/regex/replace/g file.txt  # Substitute regex with replace, globally
```

The `less` command is similar to the `cat` command but is better suited for handling larger files. It displays files in the terminal one page at a time. It can also be configured within a bash profile (discussed later on) through the `LESS` environment variable. Setting it equal to the `"-N"` option will add line numbers to the file.

```Bash
less largefile.txt
```

The `history` command returns history information on commands previously used.

```Bash
history
```

The `clear` command clears the terminal window.

```Bash
clear
```

The `man` command returns a manual for a command.

```Bash
man command
```

## Environment

Each terminal session is loaded with settings and preferences that form the command line environment. It is possible to configure the environment to support custom commands, programs, and variables to be shared across sessions.

One way to modify the environment is through a command line text editor, such as nano (or notepad on Windows).

```Bash
nano file.txt  # Open nano, and open or create file.txt
```

The bash profile, a file which stores environment settings, is called `~/.bash_profile`. The contents of this will be loaded at the start of every session. Modifications typically follow the following lines of command:

```Bash
nano ~/.bash_profile  # Open the bash profile for modification
source ~/.bash_profile  # Make changes available for this session
```

Within the bash profile, we can make various modifications, which we discuss below, all of which are implicitly done in the `~/.bash_profile` file itself (and not the terminal session).

The `alias` command allows for the creation of aliases for commands. We must use the following syntax (including no spaces), otherwise an error will be thrown:

```Bash
alias pd="pwd"  # Enable pd to be used to print working directory
alias ll="ls -la"  # Enable ll to be used as a shortcut for ls -la
```

The `export` command allows for the configuration of environment variables.

```Bash
export USER="John Smith"
```

If we want to check the value of a variable, we can echo the variable, prefixed with `$`. 

```Bash
echo $USER  # Output to the terminal: John Smith
```

The following are some other commonly configured environment variables:

```Bash
export PS1=">>"  # Change the default command prompt to >>
export HOME="path"  # The home directory path can be changed, if necessary

# This environment variable contains all directories containing command
# line scripts.  In advanced cases we can edit this if we add our
# own scripts.
export PATH="path"
```

The `env` (environment) command returns a list of all of the environment variables for the current user.

```Bash
env  # This list will be quite long, but we can also use grep
env | grep PATH  # Use grep to return the value of the PATH variable
```

## Bash Scripting

Any command that can be run in the terminal can be run in a bash script. As with any programming language, there are conventions to follow:

- Bash scripts have `.sh` as their file extension.
- The beginning of each script file should start with `#!/bin/bash` on its own line. This tells the computer to use bash as the interpreter.
- Script files should be placed in the `~/bin/` directory, as a matter of good practice. (We may need to add this directory to the `PATH` variable by adding `PATH=~/bin:$PATH` to the bash profile configuration file.)

We also need to give script files the "execute" permission.

```Bash
chmod +x script.sh
```

To declare a variable, we use the following syntax:

```Bash
variable="value"
```

To access the value of a variable, we wrap the variable in curly braces `{ }`, and prefix this with the `$` operator.

```Bash
echo ${variable}  # Output to the terminal: value
```

Conditionals use the following syntax (ensure that the spacing for square brackets is also as shown):

```Bash
if [ condition1 ]
then
    statement1
elif [ condition2 ]
then
    statement2
else
    statement3
fi
```

For numbers, Bash has the usual comparison operators: `-eq`, `-ne`, `-le`, `-lt`, `-ge`, `-gt`, `-z`.
For strings, the comparison operators are: `==`, `!=`. The logical operators are: `&&`, `||`, `!`.

```Bash
if [ "${word1}" == "${word2}" ]  # Check if the two strings are equal
```

Note that arithmetic in bash scripting uses the following syntax:

```Bash
variable=$((variable + 7))
```

There are three different loops: `for`, `while` and `until`.

```Bash
for word in ${paragraph}
do
    statement
done
```

```Bash
while [ condition ]
do
    statement
done
```

```Bash
until [ condition ]
do
    statement
done
```

We can allow scripts to access external data. There are three main ways to do this.

The first is by using the `read` command, which prompts the user for input.

```Bash
read number  # Save the user's input to the variable number
read -p "Enter your age: " number  # Flag option to specify prompt text
```

The second is by calling the script with input arguments.

```Bash
# The following arguments can be referenced positionally in the script
# by using $1, $2, $3.  For any number of input arguments to be
# allowed, use $@.
script.sh arg1 arg2 arg3
```

The third way is by accessing external files.

```Bash
files=/directory_path/*  # Access all files in directory_path
```

For convenience, we can use our bash profile to create an alias for calling bash scripts.

```Bash
alias short='./longname.sh'  # Enable short to call the script
alias short='./longname.sh "arg"'  # Call the script with a first input
```
