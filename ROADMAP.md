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

### Critical TV Box (or dAppNode)

A beautifully designed device, to sit under a TV screen. This could perhaps use a Raspberry Pi as it's board. 

#### User Journey

- User plugs in to
  - the big screen via HDMI.
  - a network cable connection with internet access.
  - the power supply.
- User turns box on, screen displays 0 balance and Burner Wallet Address + QR code
  - Burner Wallet has non-zero balance, streaming of video __starts__, streaming of payment to publisher __starts__
  - Burner Wallet has zero balance, streaming of payment __stops__, streaming of video __stops__
- User turns box off, streaming of payment __stops__, streaming of video __stops__

## Infrastructure Level Upgrades

### Outsource your Transcoding to Livepeer

Livepeer is an protocol providing pay-as-you-go Open-Source Video Infrastructure Services using Ethereum for payment clearing.

Livepeer's protocol and cryptocurrency govern a transcoding marketplace where broadcasters can buy and sell transcoding services from GPU Miners in exchange for Ethereum crypto currency.

This section will explain how to configure your Permissionless Digital Stage to use these services.

### Deploying on a Raspberry Pi

The binaries from Livepeer releases are compiled for a x86 chipset.

We would need to compile some binaries for the RPi, but otherwise everything else should be the same.

### Configuring for IPv6

Enable this to work over IPv6, so that individuals have better control over their privacy and security.

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
