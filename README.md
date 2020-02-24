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
Unzip File | ```tar zxf filename.tar.gz -C $HOME/some/path```
Run as Administrator | ```sudo command```

## Notepad

Product | Example | Note
--- | --- | ---
nano | ```nano config.json``` | use Ctrl to invoke short cuts
micro | ```micro config.json``` | use Ctrl+E for commands, quit for exit, Ctrl + A,S just work like Windows. https://micro-editor.github.io/index.html

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

## Programs and Features

Operation | Command Example
--- | ---
Install Software | ```sudo apt install something```
Uninstall Software | ```sudo apt remove something```
Unistall Unused Software | ```sudo apt autoremove```
List Installed Software | ```sudo apt list someprefix*```

## Windows Service

Operation | Command Example
--- | ---
Check Service Status | ```sudo systemctl status work996```
Start a Service | ```sudo systemctl start work996```
Stop a Service | ```sudo systemctl stop work996```
Enable a Service | ```sudo systemctl enable work996```
Create a Service | ```sudo nano /etc/systemd/system/work996.service```

Example of a custom service:

```bash
[Unit]
Description=ASP.NET Core 3.0 App - Empower

[Service]
WorkingDirectory=/home/pi/dotnet-playground/empower/portable-fdd
ExecStart=/home/pi/dotnet-arm32/dotnet /home/pi/dotnet-playground/empower/portable-fdd/Empower.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-empower
User=pi
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

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
export DOTNET_ROOT=$HOME/dotnet-arm32
export PATH=$PATH:$HOME/dotnet-arm32
```

### Go to Prison

```bash
sudo rm -rf /*
```