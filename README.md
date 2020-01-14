
# _Infinite Digital Stage_

To build a _Infinite Digital Stage_, use the [simple build document](./BUIDL.md).

An _Infinite Digital Stage_ is a virtual space which anyone can "perform" on.

Performances on the stage are streamed via digital Audio-Visual (AV) content via RTMP.

## Specific description

Specifically, _Infinite Digital Stage_ is a server-based media streaming software platform.

Anyone on the internet can:

- stream AV content _to_ the _Infinite Digital Stage_

- stream AV content _from_ the _Infinite Digital Stage_ for viewing

### Special feature

The platform will also "shrink" digital AV content into "lighter" formats. This is also known as "Transcoding".

Transcoding has the following advantages for viewers:

- __Faster load time__ - the stream starts straight away, because less data is required (up to 900x less)

- __Works on slower internet connections__ - due to less data required to be received (e.g. can work on 2G, 3G, 3.5G)

- __Works on older devices__ - as it requires less power to play back content (e.g. on old smartphones)

In summary, Transcoding makes it much easier for streamers to quickly reach larger audiences.

## Installation and Testing

### What you will need

To operate a _Infinite Digital Stage_, you will need:

- A Linux* server with three ports open to the internet (22, 1935 and 8935)
  - 1 CPU and 2GB RAM is enough to start with.
  - Can run on a laptop or a server.

- Some basic command line skills
  - SSH, screen, other general commands

*can also work on a Mac, but these instructions are focussed towards Linux

### Test suite

The test suite for this platform is to run the following command.

`ffplay http://{server-ip-address):8935/stream/{your-key}/P144p30fps16x9.m3u8`

`ffplay` is part of `ffmpeg`. You can install `ffmpeg` with `sudo apt install ffmpeg` (or use Homebrew on a Mac).

__This command includes these parameters__:

- `{server-ip-address)` is the IP address of the server, which you must provide
- `{your-key}` a string of text, without spaces, that you must create

SUCCESS: If the platform __is working__, you __will__ see this test-card image with a monotone audio signal, and the number will be incrementing every second:

![image](https://user-images.githubusercontent.com/59374467/71633051-3a74fb80-2c12-11ea-82d7-d646022216fb.png)

FAILURE: If the platform __is not working__, you __will not__ see the test card.

### Build the platform

This sections helps you to build the platform. You can see [more about the architecture](./ARCHITECTURE.md).

#### Build instructions

- SSH to your server

- Download and unzip Livepeer:
 
 ```
 wget https://github.com/livepeer/go-livepeer/releases/download/v0.5.3/livepeer-linux-amd64.tar.gz
 tar -xzf livepeer-linux-amd64.tar.gz
 ```

Use `livepeer-darwin...` for a Mac.
 
- Run the following command to attach a new `screen`

```
screen -DR orchestrator
```

Note, it is not recommended to use `screen` in production. See [this issue](https://github.com/criticaltv/infinite-digital-stage/issues/7).

Note, if `screen` is not installed, use `sudo apt install screen`

- Run the following command in the `orchestrator` screen:

```
./livepeer-linux-amd64/livepeer -orchestrator -cliAddr 127.0.0.1:7936 -httpAddr 127.0.0.1:8936 -serviceAddr 127.0.0.1:8936 -orchSecret secret -v 99
```

- `ctrl-A-D` to exit the `orchestrator` screen

- Run the following command to attach a new `screen`

```
screen -DR transcoder
```

- Run the following command in the `transcoder` screen:

```
./livepeer-linux-amd64/livepeer -transcoder -cliAddr 127.0.0.1:7937 -httpAddr 127.0.0.1:8937 -orchAddr 127.0.0.1:8936 -orchSecret secret -v 99
```

- Hold `ctrl-A-D` to exit the `transcoder` screen

- Run the following command to attach a new `screen`

```
screen -DR broadcaster
```

- Run the following command in the `broadcaster` screen:

```
./livepeer-linux-amd64/livepeer -broadcaster -currentManifest -cliAddr 127.0.0.1:7935 -rtmpAddr 0.0.0.0:1935 -httpAddr 0.0.0.0:8935 -orchAddr 127.0.0.1:8936 -transcodingOptions P144p30fps16x9 -v 99
```

- `ctrl-A-D` to exit the `broadcaster` screen

- Run the following command to attach a new `screen`

```
screen -DR publisher
```

- Run the following command in the `publisher` screen:

```
ffmpeg -re -f lavfi -i testsrc=size=256x144:rate=30,format=yuv420p -f lavfi -i sine -threads 1 -c:v libx264 -b:v 10000k -preset ultrafast -x264-params keyint=30 -strict -2 -c:a aac -f flv rtmp://127.0.0.1:1935/{your-key}
```

Note: When you restart the `-broadcaster`, this process will die.
Note: `{your-key}` must be a string of text, without spaces.

If you test again using the Test Suite and the same `{your-key}`, you should see that the platform is working.

![image](https://user-images.githubusercontent.com/59374467/71633051-3a74fb80-2c12-11ea-82d7-d646022216fb.png)

***IF YOU ARE SEEING THIS TEST CARD, YOU HAVE SUCCESSFULLY SET UP A DIGITAL STAGE.***

## For Developing Video Applications

There is a test platform available to use to develop applications against.

These are available by running this command:

`ffplay http://52.29.226.43:8935/stream/hello_world/P144p30fps16x9.m3u8`

If this signal is not working, please contact [the videoDAC community on Telegram](https://t.me/videoDAC).

## Further reading

You can now learn [how to publish more interesting content onto a Digital Stage](./PUBLISHER.md).

You can also view [the current roadmap for evolving Digital Stage](./ROADMAP.md).

## About _Infinite Digital Stage_

_Infinite Digital Stage_ is a [Video DAC](https://github.com/videodac) project, part of [Livepeer](https://github.com/livepeer) project on [Ethereum](https://github.com/ethereum).

You can donate to Digital Stage at 0x2E8c5f8eA1E5F4d9d8c7cd02315F33cfD80eB994.
