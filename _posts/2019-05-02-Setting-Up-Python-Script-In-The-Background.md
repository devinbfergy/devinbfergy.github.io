---
layout: post
title: Setting Up Python Script in the Background
---

## Goal
I found that many times in my internship that I needed to setup a script to either continually run in the background like a service or run it in the background while I worked. This blog post will be a summary of a couple of different ways I found to run a python script/program in the background. 

## Systemd Service
First modify with your favorite text editor a file located in /lib/systemd/system with the name of your service.
`sudo nano /lib/systemd/system/script.service`

The Contents of that script.service file will be the following.
```
[Unit]
Description=My Script Service
After=multi-user.target
[Service]
Type=idle
ExecStart=/usr/bin/python /location/of/script.py > /var/log/script.log 2>&1
[Install]
WantedBy=multi-user.target
```

Modify the permissions for that file.
`sudo chmod 644 /lib/systemd/system/script.service`

Reload the service.
`sudo systemctl daemon-reload`

Enable that service.
`sudo systemctl enable script.service`

Perform a reboot to make sure it is enabled correctly
`sudo reboot`

This will allow you to check the status of the script.
`sudo systemctl status script.service`


## NoHup Script
Nohup runs a command immune to hangups, with output to a non-tty
This way you can lose connection to the server and the command is still running.
`sudo nohup python3 script.py &`

You can grep for the name of script and find pid to kill the running process.
`ps ax | grep script.py`


## Screen

## Tmux

## Combining Screen and Systemd Service
I first learned how to do this from setting up a terraria server following this [guide](https://www.linode.com/docs/game-servers/host-a-terraria-server-on-your-linode/) from linode.

First create a user for the python script
`sudo useradd -r -m -d /pythonscriptdirectory pythonScriptUser`
/pythonscriptdirectory is the full path to the directory for the python script

Then add the following into a file under /etc/systemd/system/python-script.service
```
[Unit]
Description=server daemon for python script

[Service]
Type=forking
User=pythonScriptUser
KillMode=none
ExecStart=/usr/bin/screen -dmS pythonScriptUser /bin/python3 /pythonscriptdirectory/pythonscript.py
ExecStop=/usr/local/bin/pythonscriptadminstration exit

[Install]
WantedBy=multi-user.target
```

Type=forking This will allow the script to have multithreading/mulitprocesses