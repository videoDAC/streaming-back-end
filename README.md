# Welcome to _Streaming Back-End_

This document provides a recipe for creating your own Streaming Back-End.

[**Shortcut**: instructions for most basic setup](https://github.com/criticaltv/go-livepeer/blob/patch-1/doc/install.md#option-1-download-pre-built-executables-from-livepeer).

[**Shortcut**: skip to instructions for building from source](https://github.com/criticaltv/go-livepeer/blob/patch-1/doc/install.md#option-2-build-from-source)

Or, to learn a little more first, read on :)

## Introduction

A _Streaming Back-End_ is all you need to stream Audio / Video (A/V) content.

It is optimised for streaming live content (e.g. from camera / microphone), and can also handle streaming pre-produced A/V content from disk.

**Disclaimer**: by design, the **Streaming Back-End will not record** content being streamed. If you are streaming live content, you must record at source in order to guarantee it will be recorded.

If you are a seasoned devOps professional, you may wish to use [these instructions](./BUIDL.md) to build a _Streaming Back-End_. Also, you can used these [systemd setup instructions](./systemd-setup/README.md), and these [Streaming Back-End upgrade instructions](./UPGRADING.md).

## What is a Streaming Back-End?

_Streaming Back-End_ is an open-source, free-to-use, server-based digital Audio / Video (A/V) content streaming platform.

Anyone on the internet can:

- publish A/V content _to_ the _Streaming Back-End_
  - via `RTMP`
- consume A/V content _from_ the _Streaming Back-End_
  - via `HLS` over `HTTP`.

### Accessibility

A Streaming Back-End will maximise the accessibility of streaming content. It does this by "shrinking" (= transcoding) A/V content into "lighter" formats. These "lighter" formats have the following advantages for consumers of A/V content:

- __Faster load time__ - the stream starts straight away, because less data is required (up to 900x less)
- __Works on slower internet connections__ - due to less data required to be received (e.g. can work on 2G, 3G, 3.5G)
- __Works on older devices__ - as it requires less power to play back content (e.g. on old smartphones)

## Installation and Testing

### What you will need

To operate a _Streaming Back-End_, you will need:

- A Linux* server
  - `1 CPU` and `2GB RAM` is enough to start with. Can be on `localhost` or VPS.
  - Ports `22`, `1935` and `8935` accepting inbound `TCP` connections from the internet
  - Install `FFmpeg`, `screen` and `curl` with `sudo apt install ffmpeg screen curl`

- Some basic command line skills
  - SSH, screen, other general commands

If you don't have a Linux server, here is [some advice about getting a server](./GETSERVER.md).

*can also work on a Mac, but these instructions are focussed towards Linux

### Build the platform

This sections helps you to build the platform.

![image](https://user-images.githubusercontent.com/59374467/78892333-d40c3e80-7a86-11ea-8823-5b90dc9055e1.png)

#### Build instructions

- SSH to your server

- Download and unzip Livepeer:
```
wget https://github.com/livepeer/go-livepeer/releases/download/v0.5.6/livepeer-linux-amd64.tar.gz
tar -xzf livepeer-linux-amd64.tar.gz
```
Note, if you are trying this on `MacOS`, use `livepeer-darwin-amd64.tar.gz`.
 
- Run the following command to attach a new `screen`:
```
screen -DR O
```
(`O` is for "Orchestrator")
 
- Run the following command in the `O` screen:
```
./livepeer-linux-amd64/livepeer -orchestrator -cliAddr 127.0.0.1:7936 -httpAddr 127.0.0.1:8936 -serviceAddr 127.0.0.1:8936 -orchSecret secret -v 99
```

- `Ctrl-A-D` on your keyboard to exit the `O` screen
  - Hold down `Ctrl` then hold down `A` then hold down `D`, then release.

- Run the following command to attach a new `screen`
```
screen -DR T
```
(`T` is for "Transcoder")
 
- Run the following command in the `T` screen:
```
./livepeer-linux-amd64/livepeer -transcoder -cliAddr 127.0.0.1:7937 -httpAddr 127.0.0.1:8937 -orchAddr 127.0.0.1:8936 -orchSecret secret -v 99
```

- Hold `ctrl-A-D` to exit the `T` screen

- Run the following command to attach a new `screen`
```
screen -DR B
```
(`B` is for "Broadcaster")

- Run the following command in the `B` screen:
```
./livepeer-linux-amd64/livepeer -broadcaster -currentManifest -cliAddr 127.0.0.1:7935 -rtmpAddr 0.0.0.0:1935 -httpAddr 0.0.0.0:8935 -orchAddr 127.0.0.1:8936 -transcodingOptions P144p30fps16x9 -v 99
```
Note: you can remove access for publishing remotely by instead running the above command with `-rtmpAddr 127.0.0.1:1935` instead of `-rtmpAddr 0.0.0.0:1935`.

- `ctrl-A-D` to exit the `B` screen

- Run the following command to attach a new `screen`
```
screen -DR P
```
(`P` is for "Publisher")

- Run the following command in the `P` screen:
```
ffmpeg -re -f lavfi -i testsrc=size=640x480:rate=30,format=yuv420p -f lavfi -i sine -threads 1 -c:v libx264 -b:v 10000k -preset ultrafast -x264-params keyint=30 -strict -2 -c:a aac -f flv rtmp://127.0.0.1:1935/hello_world
```
Note: if the process in the `B` `screen` is stopped, this process will also stop and will need to be restarted.
Note: you can change `hello_world` to your own choice of stream ID.
Note: you can re-enter any screen by running e.g. `screen -DR O` to re-enter the Orchestrator screen.

### Test suite

The test suite for this platform is to run the following command on the server.

`curl http://127.0.0.1:8935/stream/hello_world/P144p30fps16x9.m3u8`
Note: If you are using your own choice of stream ID, you must replace `hello_world` with your stream ID.

**Test pass**: if you see something like this, it will mean that your Streaming Back-End is running:
```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-MEDIA-SEQUENCE:119802
#EXT-X-TARGETDURATION:2
#EXTINF:1.980,
/stream/hello_world/P144p30fps16x9/119805.ts
#EXTINF:1.980,
/stream/hello_world/P144p30fps16x9/119806.ts
#EXTINF:1.980,
/stream/hello_world/P144p30fps16x9/119807.ts
```

**Test fail**: if you **do not** see something like that...

## Further reading

You can now learn how to [upgrade your Streaming Back-End to the latest version of Livepeer](./UPGRADING.md).

You can also learn how to [set your Streaming Back-End to run on server startup](./systemd-setup/README.md)

You can learn more about [the architecture of a Streaming Back-End](./ARCHITECTURE.md).

You can learn [how to publish more interesting content onto a Streaming Back-End](./PUBLISHER.md).

You can contribute to [the current roadmap for evolving Streaming Back-End](./ROADMAP.md).

## videoDAC Test Signal

### Important Note

If you open this URL in a Desktop browser, it _may_ prompt you to download a `.m3u8` file if your browser does not support native `.hls` streams.

In this case, use a different option to play the test signal, from the options below.

### Test Signal

There is a test signal maintained by [the videoDAC community on Telegram](https://t.me/videoDAC) at the following URL:

`http://52.29.226.43:8935/stream/hello_world.m3u8`

[http://52.29.226.43:8935/stream/hello_world.m3u8](http://52.29.226.43:8935/stream/hello_world.m3u8)

This `URL` is a stream of `hls` video segments (`.hs` files), packaged in a `.m3u8` wrapper served over `http` over `IPv4`.

This video stream can be viewed at [http://criticaltv.videodac.eth.link](http://criticaltv.videodac.eth.link), which contains a `hls.js` javascript player, served as `html` from `IPFS`, using `ENS` namespace.

The URL can also be played back on any device with network access, using `ffplay http://52.29.226.43:8935/stream/hello_world.m3u8`, in [VLC Player](https://www.videolan.org/vlc/index.html) on Mobile and Desktop, or in `Chromium-based` and `Firefox-based` browsers on Mobile (alas not on Desktop, at least, not yet).

## About _Streaming Back-End_

A Streaming Back-End is the backbone of Your Own Streaming Platform:

![Screenshot from 2020-03-15 17-33-55](https://user-images.githubusercontent.com/2212651/76701081-e1cae000-66e3-11ea-977e-1510955c5e9e.png)

_Streaming Back-End_ is a [Video DAC](https://github.com/videodac) project, part of [Livepeer](https://github.com/livepeer) project on [Ethereum](https://github.com/ethereum).

You can donate to Streaming Back-End at [0xdac817294c0c87ca4fa1895ef4b972eade99f2fd](https://etherscan.io/address/0xdac817294c0c87ca4fa1895ef4b972eade99f2fd).
