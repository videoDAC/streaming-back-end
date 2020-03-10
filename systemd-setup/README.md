This folder contains information about setting up your server to automatically start Infinite Digital Stage, using systemd.

## Assumptions

This guide assumes the following:

- The username for the server is `ubuntu`
- The `livepeer` binary exists inside `/home/ubuntu/livepeer-linux-amd64/`

## Setup Intructions

- Copy the 4 files at `./etc/systemd/system` to `/etc/systemd/system/`
  - [orchestrator.service](./etc/systemd/system/orchestrator.service)
  - [transcoder.service](./etc/systemd/system/transcoder.service
  - [broadcaster.service](./etc/systemd/system/broadcaster.service)
  - [publisher.service](./etc/systemd/system/publisher.service)

- Ensure you do not have these services running, for example in a `screen` as per the other instructions.

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

In order to verify whether the infinite digital stage is working, run the following command on the stage's server:
```
curl http://localhost:8935/stream/test-signal/P144p30fps16x9.m3u8
```
