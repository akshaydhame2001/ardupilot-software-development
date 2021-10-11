# Automated forwarding services

1. Install screen and nano packages from terminal.

`sudo apt-get install screen nano`
2. To forward telemetry stream using MAVProxy:

`mavproxy.py --master=X --out=Y` where X is source (your vehicle) and Y is the destination.
3. Run `sudo nano ~/startup.sh`.
4. Add the following:

```
#!/bin/bash
screen -L -Logfile proxy.log -S proxy -d -m bash -c "mavproxy.py --master 127.0.0.1:14550 --out 127.0.0.1:10000 --out 127.0.0.1:20000 --daemon"
```
5. If you are Raspberry Pi, add the following:

```
#!/bin/sh
export LOCALAPPDATA="LOCALAPPDATA"
screen -L -Logfile proxy.log -S proxy -d -m bash -c "mavproxy.py --master 127.0.0.1:14550 --out 127.0.0.1:10000 --out 127.0.0.1:20000 --daemon"
```
6. To explain above command:
   1. `-L` log the screen session.
   2. `-Logfile file_name.ext` specify the log file name with extension.
   3. `-S screen_name` specify the session name. It makes easier to resume session like `screen -r screen_name`.
   4. `-d` used to start the screen session as detached.
   5. `-m executable_name -c "commands"` used to run an executable with arguments.

7. Save the script by pressing Ctrl+X, y and ENTER.
8. Give permissions to the script.

`sudo chmod +x ~/startup.sh`
8. Run the following and copy the path:

`echo $(pwd)"/startup.sh"`
10. To run a command at start up:
    1. Run `sudo nano /etc/rc.local`.
    2. Paste your copied text before `exit 0` like the following:

`sh /home/m/startup.sh &` 

where `/home/m/startup.sh` is my copied full path for example.

11. Save the script by pressing Ctrl+X, y and ENTER.

12. Enable the service using `sudo systemctl enable rc-local.service`.
13. Start the service using `sudo systemctl start rc-local.service` or `reboot`.
14. Monitor the service using `sudo systemctl status rc-local.service`.

[Source](https://ardupilot.org/mavproxy/docs/getting_started/forwarding.html)