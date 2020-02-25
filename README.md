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
Unzip File | ```tar zxvf filename.tar.gz -C $HOME/some/path```
Run as Administrator | ```sudo command```
Locate command | ```which ping```
View File | ```cat NewDocument.txt```
Search Files | ```find ./your-path -name '*.txt'```

## Pipelines

Operation | Pipeline example
--- | ---
Page view | ```cat NewDocument \| less```
Search in content | `cat NewDocument \| grep something`
Count lines | ```cat NewDocument \| wc -l```

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

## Internet

Operation | Command Example
--- | ---
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

### Go to Prison

```bash
sudo rm -rf /*
```
