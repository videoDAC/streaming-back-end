# Product Roadmap

This section explains some areas where the _Infinite Digital Stage_ could be upgraded.

It divides upgrades into the following categories:

- Viewer Product - the tools and services available to someone **viewing content** on a Stage.
- Publisher Product - the tools and services available to someone **publishing content** to a Stage.
- Stage Operator Product - the tools and services available to someone **operating** a Stage.

## Viewer Product

### Stage's Content Index

Create an index of all content currently streaming on stage.

e.g. a `.m3u8` file publised over HTTP 8935. Call it `index.m3u8`.

Below is an example of what `index.m3u8` might look like:

```
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=16000000,RESOLUTION=3840x2160
hello_world/source.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=400000,RESOLUTION=256x144
hello_world/P144p30fps16x9.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=16000000,RESOLUTION=7680x4320
criticaltv/source.m3u8
#EXT-X-STREAM-INF:PROGRAM-ID=0,BANDWIDTH=400000,RESOLUTION=256x144
criticaltv/P144p30fps16x9.m3u8
```

It might also show a single manifest per stream, e.g. `hello_world.m3u8` and `criticaltv.m3u8`.

### Stage Viewer Mobile App

- A MVP app for this would be:
  - 1 app per channel - e.g. Critical TV Mobile App with launcher icon.
  - User taps launcher icon, this turns `on` the "TV", starts paying Critical TV
  - User taps screen, this turns `off` the "TV", stop paying.
  - For first time usage, just show ETH burner address, 0 balance.
  - Maybe also show on screen balance when "TV" is `on`.
  
This is further discussed in [this issue](https://github.com/criticaltv/infinite-digital-stage/issues/2).

### StageCast Box (or on a dAppNode)

A beautifully designed device, to live under or behind a TV screen. This could perhaps use a Raspberry Pi as its board. Perhaps it can attach to the wall. Also, it could integrate into something like a Google Chromecast HDMI dongle, perhaps controlled by smartphone.

#### User Journey

- User plugs StageCast Box in to:
  - the big screen via HDMI.
  - a network cable connection with internet access.
  - a USB power supply.
- User turns Stagecast Box __on__, playback of live streaming video __starts__
- User turns Stagecast Box __off__, playback of live streaming video __stops__

### Wallet Integration

If the playback of content can be linked to an Ethereum wallet, this can result in peer-to-peer payment direct from viewer to publisher in real time.

#### Single Channel Burner Wallet Web App

Rough sketch of user journey:

- User loads e.g. http://criticaltv.videodac.eth.link
- This loads a page from IPFS containing:
  - a Video Player
  - a Burner Wallet
- User presses play
- If Burner balance > 0, play video, show balance, and pay to criticaltv.videodac.eth
- If Burner balance = 0, do not play video, do not pay to anyone.

### Stream Layering

This feature would allow a viewer to request to watch 2 streams layered on top of each other.

So, for example, `publisher 0` publishes a stream of 500x500 black content

![image](https://user-images.githubusercontent.com/59374467/71674822-98bded80-2d7c-11ea-9818-25970c4c5e81.png)

Then `publisher 1` publishes HELLO in red text on a transparent background 500x500:

![image](https://user-images.githubusercontent.com/59374467/71674945-f6523a00-2d7c-11ea-991f-a871bae2736c.png)

Then, `viewer 0` requests to watch `publisher 1`'s streaming content layered on top of `publisher 0`'s streaming content:

![hello_layered](https://user-images.githubusercontent.com/59374467/71675146-74164580-2d7d-11ea-85b9-d5847ba85ed8.png)

Such a feature would allow publishers to layer their content on top of each other, potentially paving the way towards a permissionless Audio and Visual collaborative creative space on an infinite permissionless digital stage - musicians, artists, photographers, cameramen, smartphone-owners, mixing their own AV streams with other people's streams...

Then if we can get money flowing into this model, divided up based on who contributed what, then we have a whole new way of funding global collaborative digital art... like a DAO++.

## Publisher Product

### Rebadged OBS Studio

Contribute functionality to [OBS Studio](https://obsproject.com/) which makes it really easy for publishers to stream to any available _Infinite Permissionless Digital Stage_.

Some potential features of an MVP:

- Publisher installs OBS with this feature enabled
- Publisher enters their `{your-key}`
- Publisher enters their `{server-ip-address}`
- Sets keyframe interval to 2s
- Publisher clicks "Start steaming"
- Application streams the canvas 
- Viewer can watch at http://{server-ip-address}:8935/stream/{your-key}/source.m3u8

Some potential enhancements:

- Query a dynamic onchain registry of rtmp endpoints for {server-address}, to provide publisher with list of stages they can publish on
- Perhaps even embed an _Infinite Permissionless Digital Stage_ inside OBS, and start streaming to it when you open OBS.

## Stage Operator Product

This section contains upgrade ideas for helping a Stage Operator to operate their stage.

### Stage Registry

Create a permissionless decentralised registry of IP Addresses of Stages.

This would allow any stage operator to publish the address of their stage onchain, allowing anyone to use it.

For example, a stage operator might publish `10.133.65.17` as the IP address of the server which hosts the stage.

Publishers and Viewers would then be able to query this registry to share content.

### Run as a single process

For ease of deployment, it would be very convenient to be able to run the entire _Infinite Permissionless Digital Stage_ as a single process, e.g. by running:

```
./livepeer
    -httpAddr 0.0.0.0:8935
    -rtmpAddr 0.0.0.0:1935
    -publishingOptions P1080p30fps16x9
    -transcodingOptions P144p30fps16x9 
```

### Deploying on a Raspberry Pi

The binaries from Livepeer releases are compiled for a x86 chipset.

We would need to compile some binaries for the RPi, but otherwise everything else should be the same.

### Outsource your Transcoding to Livepeer

Livepeer is an protocol providing pay-as-you-go Open-Source Video Infrastructure Services using Ethereum for payment clearing.

Livepeer's protocol and cryptocurrency govern a transcoding marketplace where broadcasters can buy and sell transcoding services from GPU Miners in exchange for Ethereum crypto currency.

This section will explain how to configure your Permissionless Digital Stage to use these services.

### Configuring for IPv6

Enable this to work over IPv6, so that individuals have better control over their privacy and security.

### ENS Integration

Be able to reference streams via their ENS name.

### Containerising

Various different suggestions here to replace the use of `screen`.

`screen` was originally chosen for ease of entry - to help anyone get started, without needing to know about more complex solutions.

Research needs to go into the following potential options:

- Docker
- SupervisorD

### Access Control

Currently, any client which has access to the server can stream into it or out of it.

This topic is to investigate how to set up MVP access control.

### Distribute transcoding to host with GPU

If you have GPUs available, you can configure your transcoder process to use these.
