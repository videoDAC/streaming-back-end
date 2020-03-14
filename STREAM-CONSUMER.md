This section talks about the consumer of the stream from a Streaming Back-End.

# Minimum Viable User Interface

The simplest interface for consuming content from a Streaming Back-End is a video player in a webpage.

For example, embedding a javascript `hls` audio / video player in a webpage.

## Example Deployment

An example of a simple interface is at http://criticaltv.videodac.eth.link

- The html for this page is stored in IPFS.
- The resource naming is done in ENS, with DNS resolution via [EthDNS and EthLink](http://eth.link/).
- The `hls` video content available in this player is being served from a [Streaming Back-End](./README.md).

## Deploying

Here is a tutorial on [Hosting a Decentralized Static Website using ENS and IPFS](https://diode.io/diode/4-steps-to-hosting-a-decentralized-static-website-using-ens-and-ipfs-19350/).

You can use the Code below to deploy as the "static website".

## Code

### Single-Screen / Single-Channel / Single-Stream

The base html page for this is [here](./hlsjs-single-player.html).

Note: You must replace the IP Address (`52.29.226.43`) with the IP address of your own Streaming Back-End.
Note: you may wish to include the script served by `cdn.jsdelivr.net` inside the script, to reduce external dependencies.

### Multi-Screen

It is also possible to embed more than one stream in the same html page, as demonstrated in this graphic from ETH Berlin 2018:

![image](https://user-images.githubusercontent.com/2212651/76328622-87abd280-6311-11ea-90be-621464876dc4.png)

A base html page for this is [here](./hlsjs-multi-player.html).

# Pay-to-play App Template (Android)

videoDAC has developed a template for creating a Pay-to-play streaming app, and publishing tpo Google Play, so that a consumer can watch on their Android smartphone, and pay per minute to play the stream.

More instructions on that are in [the android repository in the videoDAC project](../../../android).

# Who is the consumer?

A consumer of streaming content can be in many forms:

What device do they watch on:

- Mobile Screen + Headphones / Speaker
- Laptop / Desktop Screen
- TV Screen (TFT, LED)

They may be

- Sitting watching
- Standing watching
- Moving watching
- Only listening, not watching

They may be watching for a long time, or for a short time.
