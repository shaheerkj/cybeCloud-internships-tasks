# What is systemd?

Very first process that runs on the system when we turn our pc on (PID 1). It manages all services in the background.

Systemd is a newer init system as opposed to the older one (init). First we look at units:

### Unit:
Units in systemd are resources that its able to manage. These include services, timers, mounts, automounts etc. 

For now, we are focusing on one type of unit --> **service**. But its important to know that there are many more.

Systemd looks at unit files to see how it should interact with processes. Since we are talking about service files, the Unit files for services are stored in the following directories:

- `/etc/systemd/system` -> (most common that we work with). 
- `/run/systemd/system` -> will store runtime systemd units.
- `/lib/systemd/system` -> when you install a package that comes with a service file, it goes here.

These directories are stored in priorities, so the service files in the directory mentioned over the lower one has more priority. If you want your configurations to take into effect immediately and be prioritized, you would prefer to store it in the `/etc/systemd/system` directory.

A typical service unit file looks like this:
```bash
[Unit]
Description=My Custom Service
After=network.target

[Service]
ExecStart=/usr/bin/myapp
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Explanation:
- `[Unit]` → metadata and dependencies
- `[Service]` → how the service behaves
- `[Install]` → when/how the service should be started