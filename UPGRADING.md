This document is to explain how to update your Streaming Back-End when there are new releases of Livepeer's software.

It assumes that you already have a working Streaming Back-End, running a version of Livepeer.

Let's begin:

- Connect to your server, and make sure you're in your home `~` (if not, run `cd ~`)

- Run the following commands to remove the old version of Livepeer:
```
sudo rm -rf livepeer-linux-amd64
sudo rm -rf livepeer-linux-amd64.tar.gz
```
Note: you may need to enter a `sudo` password

- Run the following command to download the latest version of Livepeer (At time of writing, this was `0.5.5`):
```
wget https://github.com/livepeer/go-livepeer/releases/download/v0.5.5/livepeer-linux-amd64.tar.gz
```
Note: you may wish to check if this is the latest version at [Livepeer's Releases page](https://github.com/livepeer/go-livepeer/releases).

- Run the following command to extract the new software
```
tar -xzf livepeer-linux-amd64.tar.gz
```

- Run this command to validate that you have the new software:
```
./livepeer-linux-amd64/livepeer -version
```

Next you must restart all processes.

- If you have [set up your server to use systemd](https://github.com/videoDAC/streaming-back-end/blob/master/systemd-setup/README.md), you can simply restart your entire server with `sudo reboot` and things should just start again

- If you are using `screen`, you must:
  - Re-enter each of the 3 `screen` processes (e.g. run `screen -DR O` to re-enter the Orchestrator `screen`)
  - Stop the running processes by typing `Ctrl-C`.
  - Restart the process by pressing the "up" arrow to reload the last command, and press Enter.
