# Linux Cheatsheet for Windows Users
For Windows folks like me who struggle to remember Linux commands

## File Explorer

Operation | Command Example
--- | ---
Show Files (List View) | ls
Show Files (Detail View) | ls -l
Show Hidden Files | ls -a
Delete File | rm filename
Delete Folder | rm foldername -r -f
Move File | mv filename /some/path
Unzip File | tar zxf filename.tar.gz -C $HOME/some/path

## Notepad

Product | Example | Note
--- | --- | ---
nano | nano config.json | use Ctrl to invoke short cuts
micro | micro config.json | use Ctrl+E for commands, quit for exit, Ctrl + A,S just work like Windows

## Task Manager

Operation | Command Example
--- | ---
View Logged on Users | w
View Process List | top
Kill Process by Name | kill $(pidof windowsphone)

## Internet

Operation | Command Example
--- | ---
Download File | wget https://some/file.zip
Ping | ping bing.com
Check DNS Records | dig bing.com 
