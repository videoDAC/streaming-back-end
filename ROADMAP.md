# Future Roadmap

This section explains some areas where this platform could be upgraded.

## Outsource your Transcoding to Livepeer

Livepeer is an protocol providing pay-as-you-go Open-Source Video Infrastructure Services using Ethereum for payment clearing.

Livepeer's protocol and cryptocurrency govern a transcoding marketplace where broadcasters can buy and sell transcoding services from GPU Miners in exchange for Ethereum crypto currency.

This section will explain how to configure your Permissionless Digital Stage to use these services.

## Deploying on a Raspberry Pi

The binaries from Livepeer releases are compiled for a x86 chipset.

We would need to compile some binaries for the RPi, but otherwise everything else should be the same.

## Configuring for IPV6

All contributions welcome!

## Containerising

Various different suggestions here to replace the use of `screen`.

`screen` was originally chosen for ease of entry - to help anyone get started, without needing to know about more complex solutions.

Research needs to go into the following potential options:

- Docker
- SupervisorD

## Access Control

Currently, any client which has access to the server can stream into it or out of it.

This topic is to investigate how to set up MVP access control.

## Distribute transcoding to host with GPU

If you have GPUs available, you can configure your transcoder process to use these.

## Mobile App

- A MVP app for this would be:
  - 1 app per channel - e.g. Critical TV Mobile App with launcher icon.
  - User taps launcher icon, this turns `on` the "TV", starts paying Critical TV
  - User taps screen, this turns `off` the "TV", stop paying.
  - For first time usage, just show ETH burner address, 0 balance.
  - Maybe also show on screen balance when "TV" is `on`.
