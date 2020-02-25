# Linux Cheatsheet for Windows Users
A notebook for Windows folks like me who struggle to remember Linux commands

## File Explorer

Operation | Command Example
--- | ---
Show Files (List View) | ```ls```
Show Files (Detail View) | ```ls -l```
Show Hidden Files | ```ls -a```
Delete File | ```rm filename```
Delete Folder | ```rm foldername -r -f```
Move File | ```mv filename /some/path```
Unzip File | ```tar zxvf ./file.tar.gz```
Zip File | ```tar czvf file.tar.gz ./folder```
Run as Administrator | ```sudo command```
Locate command | ```which ping```
View File | ```cat NewDocument.txt```
Search Files | ```find . -name '*.txt'```(Search *.txt in current folder)
View Folder size | ```du -d 3 -h``` (3 is depth)

## Pipelines

Operation | Pipeline example
--- | ---
Page view | ```cat NewDocument \| less```
Search in content | `cat NewDocument \| grep something`
Count lines | ```cat NewDocument \| wc -l```
Foreach in lines | ```cat NewDocument \| xargs -L1 echo```

## Hardware

Operation | Pipeline example
--- | ---
Copy by bytes | ```dd if=filesource of=target```

## Notepad

Product | Example | Note
--- | --- | ---
nano | ```nano config.json``` | use Ctrl to invoke short cuts
micro | ```micro config.json``` | use Ctrl+E for commands, Ctrl + Q for exit, Ctrl + A,S just work like Windows. https://micro-editor.github.io/index.html

## Task Manager

Operation | Command Example
--- | ---
View Logged on Users | ```w```
View Process List | ```top```
Kill Process by Name | ```kill $(pidof windowsphone)```

## Network

Operation | Command Example
--- | ---
View my IP Address | ```ifconfig```
Download File | ```wget https://some/file.zip```
Ping | ```ping bing.com```
Check DNS Records | ```dig bing.com```

### HTTP

Operation | Command Example
--- | ---
GET Url | ```curl -v https://www.baidu.com```
POST Url | ```curl -v -d 'abc' https://www.baidu.com```
POST JSON | ```curl -v -d '{ }' -H "Content-Type: application/json" https://www.baidu.com```
OPTIONS Url | ```curl -v -X OPTIONS https://www.baidu.com```

## Programs and Features

Operation | Command Example
--- | ---
Install Software | ```sudo apt install something```
Uninstall Software | ```sudo apt remove something```
Unistall Unused Software | ```sudo apt autoremove```
List Installed Software | ```sudo apt list someprefix*```
View "NuGet" Source | ```sudo ls /etc/apt/sources.list.d```
Remove "NuGet" Source | ```sudo rm -i /etc/apt/sources.list.d/PPA_Name.list```

## Windows Service

Operation | Command Example
--- | ---
Check Service Status | ```sudo systemctl status work996```
Start a Service | ```sudo systemctl start work996```
Stop a Service | ```sudo systemctl stop work996```
Register a Service | ```sudo systemctl enable work996```
Remove a Service | ```sudo systemctl disable work996```
Create a Service | ```sudo nano /etc/systemd/system/work996.service```

Example of a custom service:

```bash
[Unit]
Description=Work 996

[Service]
WorkingDirectory=/home/pi/some/dir
ExecStart=/home/pi/some/dir/work996
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=work-996
User=pi
Environment=ENV_VAR=SomeValue
Environment=ANOTHER_ENV_VAR=AnotherValue

[Install]
WantedBy=multi-user.target
```

## Windows Update

Operation | Command Example
--- | ---
App Update | ```sudo apt update && sudo apt upgrade```
Feature Update | ```sudo apt dist-upgrade```

## Tricks

### Auto run commands on start up

```bash
sudo nano .profile
```

Add commands you want to execute, e.g:

```bash
# set .NET Core SDK and Runtime path
export DOTNET_ROOT=$HOME/dotnet
export PATH=$PATH:$HOME/dotnet
```

### Examples

#### Count total lines of my code

```bash
find . -name "*.cs" | xargs -L1 cat | wc -l
```

#### Pull all repositories:

```bash
find . -maxdepth 2 -mindepth 2 -type d \( ! -name . \) -exec bash -c "cd '{}' && pwd && git pull" \;
```

#### Test disk speed

```bash
dd if=/dev/zero of=/tmp/tmp count=1 bs=1G
```

### Go to Prison

```bash
sudo rm -rf /*
```

--------

# More details

These are called shell operators and yes, there are more of them. I will give a brief overview of the most common among the two major classes, [control operators](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_113) and [redirection operators](https://www.gnu.org/software/bash/manual/bashref.html#Redirections), and how they work with respect to the bash shell. 

## A. Control operators

> In the shell command language, a token that performs a control function.  
> It is one of the following symbols:

>     &   &&   (   )   ;   ;;   <newline>   |   ||

And `|&` in bash.

A `!` is **not** a control operator but a [Reserved Word](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_04). It becomes a logical NOT [negation operator] inside [Arithmetic Expressions](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap01.html#tag_17_01_02) and inside test constructs (while still requiring an space delimiter).

### A.1 List terminators

* `;` : Will run one command after another has finished, irrespective of the outcome of the first.

        command1 ; command2

 First `command1` is run, in the foreground, and once it has finished, `command2` will be run.

 A newline that isn't in a string literal or after certain keywords is *not* equivalent to the semicolon operator. A list of `;` delimited simple commands is still a *list* - as in the shell's parser must still continue to read in the simple commands that follow a `;` delimited simple command before executing, whereas a newline can delimit an entire command list - or list of lists. The difference is subtle, but complicated: given the shell has no previous imperative for reading in data following a newline, the newline marks a point where the shell can begin to evaluate the simple commands it has already read in, whereas a `;` semi-colon does not.

* `&` : This will run a command in the background, allowing you to continue working in the same shell.
 
         command1 & command2
     
 Here, `command1` is launched in the background and `command2` starts running in the foreground immediately, without waiting for `command1` to exit.

 A newline after `command1` is optional.

### A.2 Logical operators

* `&&` : Used to build AND lists, it allows you to run one command only if another exited successfully.
 
         command1 && command2
     
 Here, `command2` will run after `command1` has finished and _only_ if `command1` was successful (if its exit code was 0). Both commands are run in the foreground.

 This command can also be written

        if command1
        then command2
        else false
        fi

 or simply `if command1; then command2; fi` if the return status is ignored.
 
* `||` : Used to build OR lists, it allows you to run one command only if another exited unsuccessfully.
 
         command1 || command2
     
 Here, `command2` will only run if `command1` failed (if it returned an exit status other than 0). Both commands are run in the foreground.

 This command can also be written

        if command1
        then true
        else command2
        fi

 or in a shorter way `if ! command1; then command2; fi`.

 Note that `&&` and `||` are left-associative; see https://unix.stackexchange.com/questions/88850/precedence-of-the-shell-logical-operators for more information.

* `!`: This is a reserved word which acts as the “not” operator (but must have a delimiter), used to negate the return status of a command — return 0 if the command returns a nonzero status, return 1 if it returns the status 0. Also a logical NOT for the `test` utility.

        ! command1

        [ ! a = a ]

 And a true NOT operator inside Arithmetic Expressions:

        $ echo $((!0)) $((!23))
        1 0

### A.3 Pipe operator
 
* `|` : The pipe operator, it passes the output of one command as input to another. A command built from the pipe operator is called a [pipeline](http://en.wikipedia.org/wiki/Pipeline_(Unix)).
 
         command1 | command2
     
  Any output printed by `command1` is passed as input to `command2`. 
  
* `|&` : This is a shorthand for `2>&1 |` in bash and zsh. It passes both standard output and standard error of one command as input to another.

        command1 |& command2

### A.4 Other list punctuation

`;;` is used solely to mark the end of a [case statement](https://www.gnu.org/software/bash/manual/bashref.html#Conditional-Constructs). Ksh, bash and zsh also support `;&` to fall through to the next case and `;;&` (not in ATT ksh) to go on and test subsequent cases.

`(` and `)` are used to [group commands](https://www.gnu.org/software/bash/manual/bashref.html#Command-Grouping) and launch them in a subshell. `{` and `}` also group commands, but do not launch them in a subshell. See [this answer](https://stackoverflow.com/questions/6270440/simple-logical-operators-in-bash/6270803#6270803) for a discussion of the various types of parentheses, brackets and braces in shell syntax.


## B. Redirection Operators

[Redirection Operator](https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/basedefs/V1_chap03.html#tag_03_318)

> In the shell command language, a token that performs a redirection function. It is one of the following symbols:

>     <     >     >|     <<     >>     <&     >&     <<-     <>



These allow you to control the input and output of your commands. They can appear anywhere within a simple command or may follow a command. Redirections are processed in the order they appear, from left to right. 

* `<` : Gives input to a command.
 
        command < file.txt
     
 The above will execute `command` on the contents of `file.txt`.

* `<>` : same as above, but the file is open in _read+write_ mode instead of _read-only_:

        command <> file.txt

 If the file doesn't exist, it will be created.

 That operator is rarely used because commands generally only _read_ from their stdin, though [it can come handy in a number of specific situations](https://unix.stackexchange.com/a/164449).

* `>` : Directs the output of a command into a file.

        command > out.txt
    
 The above will save the output of `command` as `out.txt`. If the file exists, its contents will be overwritten and if it does not exist it will be created.  
 
 This operator is also often used to choose whether something should be printed to [standard error](http://en.wikipedia.org/wiki/Standard_streams#Standard_error_.28stderr.29) or [standard output](http://en.wikipedia.org/wiki/Standard_streams#Standard_output_.28stdout.29):
 
        command >out.txt 2>error.txt
     
 In the example above, `>` will redirect standard output and `2>` redirects standard error. Output can also be redirected using `1>` but, since this is the default, the `1` is usually omitted and it's written simply as `>`.

 So, to run `command` on `file.txt` and save its output in `out.txt` and any error messages in `error.txt` you would run:
     
        command < file.txt > out.txt 2> error.txt
      
* `>|` : Does the same as `>`, but will overwrite the target, even if the shell has been configured to refuse overwriting (with `set -C` or `set -o noclobber`).
         
        command >| out.txt
     
 If `out.txt` exists, the output of `command` will replace its content. If it does not exist it will be created.  

* `>>` : Does the same as `>`, except that if the target file exists, the new data are appended.
         
        command >> out.txt
     
 If `out.txt` exists, the output of `command` will be appended to it, after whatever is already in it. If it does not exist it will be created.  

* `&>`, `>&`, `>>&` and `&>>` : (non-standard). Redirect both standard error and standard output, replacing or appending, respectively.

        command &> out.txt
    
 Both standard error and standard output of `command` will be saved in `out.txt`, overwriting its contents or creating it if it doesn't exist.
 
        command &>> out.txt
     
 As above, except that if `out.txt` exists, the output and error of `command` will be appended to it.

 The `&>` variant originates in `bash`, while the `>&` variant comes from csh (decades earlier). They both conflict with other POSIX shell operators and should not be used in portable `sh` scripts.
 
* `<<` : A here document. It is often used to print multi-line strings.
 
         command << WORD
             Text
         WORD
         
  Here, `command` will take everything until it finds the next occurrence of `WORD`, `Text` in the example above, as input .  While `WORD` is often `EoF` or variations thereof, it can be any alphanumeric (and not only) string you like. When `WORD` is quoted, the text in the here document is treated literally and no expansions are performed (on variables for example). If it is unquoted, variables will be expanded. For more details, see the [bash manual](https://www.gnu.org/software/bash/manual/bashref.html#Here-Documents).

  If you want to pipe the output of `command << WORD ... WORD` directly into another command or commands, you have to put the pipe on the same line as `<< WORD`, you can't put it after the terminating WORD or on the line following.  For example:

         command << WORD | command2 | command3...
             Text
         WORD


  
* `<<<` : Here strings, similar to here documents, but intended for a single line. These exist only in the Unix port or rc (where it originated), zsh, some implementations of ksh, yash and bash.

        command <<< WORD
    
 Whatever is given as `WORD` is expanded and its value is passed as input to `command`. This is often used to pass the content of variables as input to a command. For example:
 
         $ foo="bar"
         $ sed 's/a/A/' <<< "$foo"
         bAr
         # as a short-cut for the standard:
         $ printf '%s\n' "$foo" | sed 's/a/A/'
         bAr
         # or
         sed 's/a/A/' << EOF
         $foo
         EOF

A few other operators (`>&-`, `x>&y` `x<&y`) can be used to close or duplicate file descriptors. For details on them, please see the relevant section of your shell's manual ([here](https://www.gnu.org/software/bash/manual/bashref.html#Moving-File-Descriptors) for instance for bash).

That only covers the most common operators of Bourne-like shells. Some shells have a few additional redirection operators of their own.

Ksh, bash and zsh also have constructs `<(…)`, `>(…)` and `=(…)` (that latter one in `zsh` only). These are not redirections, but [process substitution](http://www.gnu.org/software/bash/manual/bash.html#Process-Substitution).

