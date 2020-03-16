This folder contains information about setting up your server to automatically run your Streaming Back-End upon startup.

## Assumptions

This guide assumes the following:

- The username for the server is `ubuntu`
- The `livepeer` binary exists inside `/home/ubuntu/livepeer-linux-amd64/`

## Setup Intructions

- First ensure that you do not already have a Streaming Back-End running on your server.
  - For example, you may have previously run it using `screen` as per the other instructions.

- Open Terminal

- Switch to the `/etc/systemd/system` folder by running
```
cd /etc/systemd/system
```

- Fetch the 4 files in this repository, into your system folder, by running the following commands:
```
sudo wget https://github.com/videoDAC/streaming-back-end/blob/master/systemd-setup/etc/systemd/system/orchestrator.service
sudo wget https://github.com/videoDAC/streaming-back-end/blob/master/systemd-setup/etc/systemd/system/transcoder.service
sudo wget https://github.com/videoDAC/streaming-back-end/blob/master/systemd-setup/etc/systemd/system/broadcaster.service
sudo wget https://github.com/videoDAC/streaming-back-end/blob/master/systemd-setup/etc/systemd/system/publisher.service
```

- Run the following commands:
```
sudo systemctl enable /etc/systemd/system/orchestrator.service
sudo systemctl enable /etc/systemd/system/transcoder.service
sudo systemctl enable /etc/systemd/system/broadcaster.service
sudo systemctl enable /etc/systemd/system/publisher.service
sudo systemctl start /etc/systemd/system/orchestrator.service
sudo systemctl start /etc/systemd/system/transcoder.service
sudo systemctl start /etc/systemd/system/broadcaster.service
sudo systemctl start /etc/systemd/system/publisher.service
```

## Validation and Testing

In order to verify whether the Streaming Back-End is working, run the following command on the stage's server:
```
curl http://localhost:8935/stream/test-signal/P144p30fps16x9.m3u8
```
This should give a response like this:
```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=4000000,RESOLUTION=640x360
hello_world/source.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=400000,RESOLUTION=256x144
hello_world/P144p30fps16x9.m3u8
```
