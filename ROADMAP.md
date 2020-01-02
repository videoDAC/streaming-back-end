# Future Roadmap

This section explains some areas where this platform could be upgraded.

## Product Ideas

### Single Channel Burner Wallet Web App

Rough sketch of user journey:

- User loads e.g. http://criticaltv.videodac.eth.link
- This loads a page from IPFS containing:
  - a Video Player
  - a Burner Wallet
- User presses play
- If Burner balance > 0, play video, show balance, and pay to criticaltv.videodac.eth
- If Burner balance = 0, do not play video, do not pay to anyone.

### Mobile App

- A MVP app for this would be:
  - 1 app per channel - e.g. Critical TV Mobile App with launcher icon.
  - User taps launcher icon, this turns `on` the "TV", starts paying Critical TV
  - User taps screen, this turns `off` the "TV", stop paying.
  - For first time usage, just show ETH burner address, 0 balance.
  - Maybe also show on screen balance when "TV" is `on`.
  
This is further discussed in [this issue](https://github.com/criticaltv/infinite-permissionless-digital-stage/issues/2).

### Critical TV Box (or dAppNode)

A beautifully designed device, to live under or behind a TV screen. This could perhaps use a Raspberry Pi as its board. Perhaps it can attach to the wall. Also, it could integrate into something like a Google Chromecast HDMI dongle, perhaps controlled by smartphone.

#### User Journey

- User plugs box in to:
  - the big screen via HDMI.
  - a network cable connection with internet access.
  - a USB power supply.
- User turns box on, playback of live streaming video __starts__
- User turns box off, playback of live streaming video __stops__

### Wallet Integration

## Infrastructure Level Upgrades

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
