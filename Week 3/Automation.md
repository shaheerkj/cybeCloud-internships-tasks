# Automation

Through automation we can schedule tasks to run at specified intervals to reduce manual overhead. We can automate tasks using two services in linux:
- systemd
- cronjobs

## 1. Systemd:

[Systemd introduction](./Extra.md). We will be using Timers in systemd to schedule tasks. Compared to cronjobs, systemd timers have additional features. But this advantage comes at the cost of it being complex. Cronjobs are often faster to implement.

For our automation task, I made a basic job that appends the result of (myIP + CurrentTime) to a file present in my desktop.

For this, I first made a `ip-report.service` file:

```bash
[Unit]
Description=Shows IPv4 Address
After=network-online.target # Works after the network is online
Wants=network-online.target

[Service]
ExecStart=/bin/sh -c "(curl -4 ifconfig.me ; printf ' ' ; date) >> /home/shaheer/Desktop/okay.txt"
# The command that will run is specified here

[Install]
WantedBy=default.target
```

Then, we make a `ip-report.timer` file. This timer is the Unit file that helps us to schedule the job:

```bash
[Unit]
Description=Timer

[Timer]
OnBootSec=15m # This timer will wait at least 15 minutes after boot time.
OnCalendar=*-*-* *:*:00 #Run every minute
Persistent=true # If a job is missed, it will run at the next available cycle.

[Install]
WantedBy=timers.target
```

Now we need to make sure the task are run:

```bash
sudo systemctl daemon-reload
sudo systemctl start ip-report.timer

# If we want it to start at boot tim
sudo systemctl enable ip-report.timer

# We do not need to start the ip-report.service file because timer is the one that handles it automation.
```

## Cronjob:
Cron is a background service (daemon) that executes tasks automatically. This is based on the jobs specified in the Crontab file. Each user has a different crontab and it can be displayed with `crontab -l`

If we want to introduce tasks that we want to be run by cronjob, we can do so with `crontab -e`.

For this task, we will be using an example scenario where we are running a webserver on our machine and want a backup of it stored safely at a specified time each day

First we enter the cron jobs for the user `www-data` as it is an apache webserver and we want to run the jobs as that user.

-> `sudo crontab -u www-data -e` (need sudo privileges to run cronjobs as another user)

```bash
#at the end of the crontab file we put our job
0 3 * * * /usr/local/bin/backup_script.sh
```

Normally we would place a script and tell the cronjob to run it at specific intervals. In this case, we are running the backup script every day at 3 AM.

Another example, where we backups the contents of `/home/` directory to `/tmp/backups/`

```bash
0 * * * * tar -zcf /tmp/backups/home-$(date +\%Y\%m\%d-\%H\%M).tar.gz /home/
```


## Which should I use?

Both are legitimate ways to work with automation and use whichever fits your needs best. But some important points are:

- The task won't perform any better when launched when compared to the other
- Systemd timers require systemd to be present on the distro.
- If project is time-sensitive, go with cronjobs.



