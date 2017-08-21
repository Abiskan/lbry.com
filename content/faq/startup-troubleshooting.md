---
title: LBRY Startup Troubleshooting
category: troubleshooting
order: 1
---

Internet connectivity and ports are key to running LBRY successfully since it is a Peer to Peer protocol that requires clients to see and speak to each other. Since individual local network configurations, firewalls, and other security settings vary greatly, LBRY may require some basic network/PC troubleshooting in order to function properly. Also since LBRY is in beta, there are certain configurations which we don't support yet but can offer workarounds (see below) or offer one-on-one [support](https://lbry.io/faq/how-to-report-bugs). 

One of the common areas to check for start-up issues is the [LBRY log file](https://lbry.io/faq/how-to-find-lbry-log-file). Except in extreme cases, a log file should be generated by the daemon startup process and will usually reveal clues on where to look. 

### Connectivity and Ports
LBRY operates on a couple different ports and if there are conflicts/firewall rules/security settings that prevent them from being utilized, the app/daemon will experience start-up issues. LBRY tries to employ port forwarding through the use of UPnP, which may be disabled on some routers. Either UPnP must be enabled or you have to manually forward the ports below (3333 TCP and 4444 UDP) in order to fully take advantage of the LBRY network. Only 1 PC on a network can have an open outside port, so if you are running multiple PCs with LBRY, they will need to be configured manually. 

- Port 3333 - LBRY daemon runs and shares data on port 3333 (TCP) by default. Often times, this port can be already in use to due to mining software or other applications/services. 

- Port 4444 - LBRY daemon utilizes port 4444 to stream and download data from the LBRY network. If this port is taken or not working properly, you generally won't have start-up issues, but instead, results in the inability to download any content. See the [streaming guide](https://lbry.io/faq/unable-to-stream) for the workaround. 

- Port 50001 - LBRY wallet connections happen over port 50001. LBRY may fail to start if this port is blocked by a firewall or network rules. 

### This is my first time running LBRY and it won't start
- Port 3333 already in use. This issue would reveal itself in the log file. You can see how to change this port [here](https://lbry.io/faq/how-to-change-port). If the port is properly forwarding correctly, you are able to successfully see port 3333 Open on this [port checker tool](https://www.canyouseeme.org). 

- Port 50001 wallet connection fails. This issue would reveal itself in the log file. Typical things to check would be firewall/security settings that may block this connection. 

- On Linux, LBRY may fail to start because of missing authentication capability. Please see [giuthub issue](https://github.com/lbryio/lbry-app/issues/386) or possible workaround below.

- On Windows, LBRY may fail to start because of non-ASCII characters in your Windows username. Check your c:\users\<username> path to see if there are any such characters. Please see [github issue](https://github.com/lbryio/lbry/issues/794) or workaround below.

### LBRY used to work previously but now it won't start
First and foremost, please ensure you are on the [latest verison](https://lbry.io/get) of LBRY. Reinstalling the latest version may alleviate some start-up issues. Before installing, either make sure no LBRY/lbrynet processes or simply reboot your computer. 

- On Windows, you may receive an error titled `Uncaught Exception - Error: Object has been destroyed...` which prevents you from starting LBRY. This usually means LBRY.exe or lbrynet-daemon.exe is already running in the background. Check the task manager, kill the process(es) and start LBRY again. Please see this [GitHub issue](https://github.com/lbryio/lbry-app/issues/353) for more details.

- On Windows, you may receive an error titled `Uncaught Exception - Error spawn...ENOENT` which usually means LBRY can't find the daemon exe file. Please see this [GitHub issue](https://github.com/lbryio/lbry-app/issues/396) for more details. The workaround is to rerun the [latest](https://lbry.io/get) LBRY installation file. 

- On older MAC installations, you may run into an issue with the daemon shutting down immediately. Please see [this GitHub issue](https://github.com/lbryio/lbry-app/issues/291) for troubleshooting. 

- Other typical startup troubleshooting would be to ensure that LBRY or lbrynet-daemon is not already running in the background. If the processes cannot be killed, a restart of your computer may be required.

### Known Startup issues and Workarounds
#### Linux Auth_token requirements
Currently, LBRY requires an authorization token to be generated using the [keytar](https://github.com/atom/node-keytar) libraries. Please ensure libsecret and keytar are installed. On some distributions, LBRY won't run unless gnome keyring is also installed/operational. See [giuthub issue](https://github.com/lbryio/lbry-app/issues/386) for more information. 

#### Windows User Path has non-ASCII characters
Currently, the LBRY app may fail to start because it does not support non-ASCII / non-English letters in the c:\Users\<username> directory where it tries to create your LBRY wallet, downloads and application data. As a workaround, you can manually set these directories in the `daemon_settings.yml` file. If this file does not exist in your [lbrynet folder](https://lbry.io/faq/lbry-directories), you can create one or you can use [this sample](https://goo.gl/opybNE).

This will setup your directories as follows or you can create/edit the file to configure your own paths (but again, don't use any folders with non-ASCII letters):
```
data_dir: c:\lbry\lbrynet
lbryum_wallet_dir: c:\lbry\lbryum
download_directory: c:\lbry\Downloads
```

After you are done inserting or editing the `daemon_settings.yml` configuration file, try running LBRY again. If you still receive this warning after completing the above steps, please [reach out to us](https://lbry.io/faq/how-to-report-bugs) for additional support. 