# _Permissionless Digital Stage_

_Permissionless Digital Stage_ is a platform which allows anyone connected to your network to transiently stream (audio-visual) AV digital content, without requiring permission.

## Specific description

Specifically, _Permissionless Digital Stage_ is a server-based media streaming software platform.

Anyone who has access to the server can:

- stream AV content _to_ the _Permissionless Digital Stage_

- stream AV content _from_ the _Permissionless Digital Stage_ for viewing

### Special feature

The platform will also "shrink" AV digital content into "lighter" formats.

This has the following advantages for viewers:

- __Faster load time__ - the stream starts straight away, because less data is required (up to 900x less)

- __Lower bandwidth requirements__ - making it possible to watch on slow connections (e.g. 2G, 3G, 3.5G)

- __Lower power requirements__ - making it possible to watch on weaker, older devices (e.g. old smartphones)

This generally makes it much easier for streamers to quickly reach larger audiences.

## Installation and Testing

### What you will need

To operate a _Permissionless Digital Stage_, you will need:

- A Linux server with three ports open to the internet (22, 1935 and 8935)
  - 1 CPU and 2GB RAM is enough to start with.

- Some basic command line skills
  - SSH, screen, other general commands

### Test suite

The test suite for this platform is to run the following command. __This command includes some parameters__:

`ffplay http://{server-ip-address):8935/stream/{key}/P144p30fps16x9.m3u8`

This command contains the following parameters:

- `{server-ip-address)` is the IP address of the server, which you must provide
- `{your-key}` a string of text, without spaces, that you must create

If platform __is working__, you __will__ see this test-card image, and the number will be incrementing every second:

![Screenshot from 2019-12-31 17-18-11](https://user-images.githubusercontent.com/59374467/71627376-608aa380-2bf2-11ea-9ae3-dfb2e87a45b0.png)

If platform __is not working__, you __will not__ see the test card.

### Build the platform

#### Platform architecture

##### Logical architecture

The following diagram shows the platform's logical architecture.

![Screenshot from 2019-12-31 19-39-16](https://user-images.githubusercontent.com/59374467/71630796-502ff400-2c05-11ea-8ffc-d3fff4e7bc2f.png)

#### Build instructions

- SSH to your server

- Download and unzip Livepeer:
 
 ```
 wget https://github.com/livepeer/go-livepeer/releases/download/v0.5.1/livepeer-linux-amd64.tar.gz
 tar -xzf livepeer-linux-amd64.tar.gz
 ```
 
- Run `screen -DR orchestrator`
  - Install `screen` with `sudo apt install screen`

- Run the following command in the `orchestrator` screen:

```
./livepeer-linux-amd64/livepeer
   -orchestrator 
   -cliAddr 127.0.0.1:7936  
   -httpAddr 127.0.0.1:8936 
   -serviceAddr 127.0.0.1:8936 
   -orchSecret secret 
   -v 99
```

- `ctrl-A-D` to exit the `orchestrator` screen

- Run `screen -DR transcoder`

- Run the following command in the `transcoder` screen:

```
./livepeer-linux-amd64/livepeer
   -transcoder 
   -cliAddr 127.0.0.1:7937  
   -httpAddr 127.0.0.1:8937 
   -orchAddr 127.0.0.1:8936 
   -orchSecret secret 
   -v 99
```

- `ctrl-A-D` to exit the `transcoder` screen

- Run `screen -DR broadcaster`

- Run the following command in the `broadcaster` screen:

```
./livepeer-linux-amd64/livepeer
   -broadcaster
   -cliAddr 127.0.0.1:7935 
   -rtmpAddr 127.0.0.1:1935 
   -httpAddr 0.0.0.0:8935 
   -orchAddr 127.0.0.1:8936 
   -transcodingOptions P144p30fps16x9 
   -v 99
```

- `ctrl-A-D` to exit the `broadcaster` screen

- Run `screen -DR publisher`

- Run the following command in the `publisher` screen:

```
ffmpeg
   -re 
   -f lavfi 
   -i testsrc=size=426x240:rate=30,format=yuv420p 
   -i sine
   -threads 1
   -c:v libx264
   -b:v 10000k
   -preset ultrafast
   -x264-params keyint=30
   -c:a aac
   -f flv rtmp://localhost:1935/{your-key}
```

Note: `{your-key}` must be a string of text, without spaces.

#### Test Again

If you now re-run your test suite, you should see that the platform is working.

![Screenshot from 2019-12-31 17-18-11](https://user-images.githubusercontent.com/59374467/71627376-608aa380-2bf2-11ea-9ae3-dfb2e87a45b0.png)

# Future Roadmap

This section explains some areas where this platform could be upgraded.

## Outsource your Transcoding and pay with Ethereum

If you do not have specialist equipment for Transcoding, you may evolve your configuration to outsource the transcoding to a third party transcoder. In this section, we will explain how.

## Configuring for IPV6

All contributions welcome!

## Whitelisting IP Addresses

All contributions welcome!

## Distribute transcoding to host with GPU

All contributions welcome!

## Testing with OBS

All contributions welcome

## Mobile App

- MVP: 1) tap launcher to turn on channel, 2. tap screen to turn off channel

- Extra - integrate with payment channels for paying for content

# About _Permissionless Digital Stage_

_Permissionless Digital Stage_ is a [Video DAC](https://github.com/videodac) project, part of [Livepeer](https://github.com/livepeer) project on [Ethereum](https://github.com/ethereum).
