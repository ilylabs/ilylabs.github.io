---
layout: single
title: "Run a python app (or script) as a systemd service"
category: [posts]
tags:  python ubuntu telegram
date: 2020-05-08 09:42:00
---

This post shows how you could run a python script on a Raspberry Pi as a `systemd` service that is running Debian. This method will also work with a laptop or computer running Ubuntu or Debian.
Running as a script a `systemd` service means that the script will automatically run when the machine boots and it will be restarted even if it crashes for any reason. Basically, it will be running forever.
I used the excellent [dietpi](https://dietpi.com/) that truly brings lightweight justice to you Raspberry Pi.
{: style="text-align: justify;"}

### Test method: Telegram Bot

We are going to use a very basic [Telegram](https://telegram.org/) bot in order to test that our script will:
1. Automatically start when the machine boots
2. Automatically restarts when it crashes/exits for whichever reason

If the bot is alive, then it means that our method works. Of course, we will also be able to check the status of the service through `systemd`, but just to be sure ... This bot is going to send us a message through Telegram once its online. If you are not interested in the bot, you should still be able to use this for some other python script.
{: style="text-align: justify;"}

1. Create a ~/Temp folder on your Raspberry Pi through SSH
2. Create a virtual environment in ~/Temp (you might need to install `sudo apt-get install python3-venv`): 
```
dietpi@solidsnake:~/Temp$ python3 -m venv .env
```
3. Load the virtual environment: `source .env/bin/activate`

4. Let's also update pip, and install a package we need here:
```
(.env) dietpi@solidsnake:~/Temp$ pip install --upgrade pip
(.env) dietpi@solidsnake:~/Temp$ pip install pytelegrambotAPI
```

5. Create a `bot.py` file: 
```
(.env) dietpi@solidsnake:~/Temp$ nano bot.py
```

Let's give our bot:
* A help message

```python
# -*- coding: utf-8 -*-
import telebot

# initialize the bot connection
bot = telebot.TeleBot("PUT_YOUR_BOT_TOKEN_HERE")

# this function will send Help! if you send it the string
# `help` or `aide` 
@bot.message_handler(commands=['help', 'aide'])
def send_welcome(message):
    text = """
Help !
"""
    bot.reply_to(message, text)

# this is how the bot is constantly listening to you messages
bot.polling()
```
In order to create the bot on the Telegram network and get your `PUT_YOUR_BOT_TOKEN_HERE`, you wlll need to interact with the [botfather](https://t.me/botfather) ([here is a basic tutorial](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-connect-telegram?view=azure-bot-service-4.0) in case you get stuck with this step, stop after section `Copy the access token`). Send a message to your bot (which is currently an empty shell) using its username once you create it.
{: style="text-align: justify;"}

6. Test your script: `python bot.py`

If everything worked fine, you should be able to send `/help` to your bot, and your bot will answer you.

Let's now make this script into a systemd service.


## On Dietpi

**Important**

All the paths in your scripts have to be absolute paths, there can be no relative path in your scripts. If there are relative paths that you must keep, you will have to change your current working directory by retrieving 
{: style="text-align: justify;"}

1. Modify the python script, add first line: `#!/home/dietpi/Temp/.env/bin/python3` which is the path to the python in the virtual env
2. `chmod +x bot.py`  to make it executable, it will execute with the python you specified in the previous step, which the python you installed in your vritual environment. You can try this by directly running `./bot.py` without any python before it, it should run the script.
3. Type the following command in your terminal to add a `systemd` service: 
```
sudo systemctl edit --force --full dummy.service
```
You will then automatically be taken to a terminal text editor. Paste the following content  :

```
[Unit]
Description=Dummy Service
Wants=network.target
After=network.target

[Service]
ExecStartPre=/bin/sleep 10
ExecStart=/home/dietpi/Temp/bot.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Save and exit using `Ctrl+X` then `Y`, followed by `Enter`, if your default terminal editor is `nano`, otherwise, good luck.

* Note that ExecStart is the path to the Python file directly if you made it an executable using the right virtual environment. If you did not, then you have to specify a python binary to execute it.

* We have to add an ExecStartPre delay otherwise the service keeps trying to start before internet is even available and we get this error:

```
● dummy.service
   Loaded: loaded (/etc/systemd/system/dummy.service; enabled; vendor preset: enabled)
   Active: failed (Result: exit-code) since Tue 2019-10-29 00:26:18 GMT; 5min ago
  Process: 442 ExecStart=/home/dietpi/bot/bot.py (code=exited, status=1/FAILURE)
 Main PID: 442 (code=exited, status=1/FAILURE)

Oct 29 00:26:18 solidsnake bot.py[442]:     timeout=(connect_timeout, read_timeout), proxies=proxy)
Oct 29 00:26:18 solidsnake bot.py[442]:   File "/home/dietpi/bot/.env/local/lib/python2.7/site-packages/requests/sessions.py", line 465, in request
Oct 29 00:26:18 solidsnake bot.py[442]:     resp = self.send(prep, **send_kwargs)
Oct 29 00:26:18 solidsnake bot.py[442]:   File "/home/dietpi/bot/.env/local/lib/python2.7/site-packages/requests/sessions.py", line 573, in send
Oct 29 00:26:18 solidsnake bot.py[442]:     r = adapter.send(request, **kwargs)
Oct 29 00:26:18 solidsnake bot.py[442]:   File "/home/dietpi/bot/.env/local/lib/python2.7/site-packages/requests/adapters.py", line 415, in send
Oct 29 00:26:18 solidsnake bot.py[442]:     raise ConnectionError(err, request=request)
Oct 29 00:26:18 solidsnake bot.py[442]: requests.exceptions.ConnectionError: ('Connection aborted.', gaierror(-3, 'Temporary failure in name resolution'))
Oct 29 00:26:18 solidsnake systemd[1]: dummy.service: Main process exited, code=exited, status=1/FAILURE
Oct 29 00:26:18 solidsnake systemd[1]: dummy.service: Failed with result 'exit-code'.
```
* We also add a Restart flag in order to get systemd to always restart the script if it were to ever fail

Use `Ctrl X + Y` to save and exit when you finished editing. 

4. Enable the service: `sudo systemctl enable dummy.service`
5. Reboot, wait for 30 seconds
6. Try to contact your bot with `/help`, All good !
7. SSH into your RPi
8. Check your service status: `sudo systemctl status dummy.service`
9. Manipulate your service:
```
sudo systemctl stop dummy.service          #To stop running service 
sudo systemctl start dummy.service         #To start running service 
sudo systemctl restart dummy.service       #To restart running service 
```
10. Let's validate that it will really restart on crash. Let's add a function to our bot that simply kills the script. By killing, I mean that we are going to create an error in order to get the script to crash. When we are working on a telegram bot script, each function is kind of loaded separately, we are going to create an error in a new function and use it to check if the bot truely restarts, add this:

```python
#!/home/dietpi/Temp/.env/bin/python3
# -*- coding: utf-8 -*-
import telebot

# initialize the bot connection
bot = telebot.TeleBot("TELEGRAM_BOT_TOKEN")

# this function will send Help! if you send it the string
# `help` or `aide` 
@bot.message_handler(commands=['help', 'aide'])
def send_welcome(message):
    text = """
Help !
"""
    bot.reply_to(message, text)

# never do this (again)
@bot.message_handler(commands=["kill"])
def get_kill(message):
    print(a)

bot.polling()
```
If you try ths script (in your Virtual Environment not as a service), you will see that the script will return the `/help` command, but it will simply crash if you try to run the `/kill command` which tries to print a variable `a` that was never defined. Because python sees each telegram bot function as a separate function, it does not check that all variables exist before, as a variable can be defined with an incoming Telegram message.
{: style="text-align: justify;"}

11. Deploy (copy/paste) this new script on the Raberry pi, then reboot the RPi, so it properly loads as the new systemd service
12. Wait for 30 seconds, then contact your bot using `/help` to check that it is online
13. Use the kill command, the bot should die, wait for a bit more than 10 seconds, as we have a 10 second timer set prior to starting the script
14. Try `/help` again, IT WORKS!

In order to delete the service:

`sudo systemctl disable dummy.service`

then reboot.

🎇🔥 And that's it ! 🔥🎇 

You should now be able to run any python script `forever`.


