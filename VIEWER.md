This section talks about the Viewer Experience.

A viewer can be in many forms:

What device do they watch on:

- Mobile Screen + Headphones / Speaker
- Laptop / Desktop Screen
- TV Screen (TFT, LED)

They may be

- Sitting watching
- Standing watching
- Moving watching

They may be watching for a long time, or for a short time.

A simple viewer application can be found here:

http://criticaltv.videodac.eth.link

The javascript for that is as below. where you must provide the `{server-ip-address}` and `{your-key}`:

```
<script src="https://cdn.jsdelivr.net/npm/hls.js@canary"></script>
<!-- Or if you want the latest stable version -->
<!-- <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script> -->

<div class="row">
  <div class="column">
    <video width=256 height=144 id="video0" controls></video>
  </div>
</div>

<script>
  var video0 = document.getElementById('video0');
  if(Hls.isSupported()) {
    var hls = new Hls();
    hls.loadSource('http://{server-ip-address}:8935/stream/{your-key}/P144p30fps16x9.m3u8');
    hls.attachMedia(video0);
    hls.on(Hls.Events.MANIFEST_PARSED,function() {
      video0.play();
  });
 }
 // hls.js is not supported on platforms that do not have Media Source Extensions (MSE) enabled.
 // When the browser has built-in HLS support (check using `canPlayType`), we can provide an HLS manifest (i.e. .m3u8 URL) directly to the video element through the `src` property.
 // This is using the built-in support of the plain video element, without using hls.js.
 // Note: it would be more normal to wait on the 'canplay' event below however on Safari (where you are most likely to find built-in HLS support) the video.src URL must be on the user-driven
 // white-list before a 'canplay' event will be emitted; the last video event that can be reliably listened-for when the URL is not on the white-list is 'loadedmetadata'.
  else if (video0.canPlayType('application/vnd.apple.mpegurl')) {
    video0.src = 'http://{server-ip-address}:8935/stream/{your-key}/P144p30fps16x9.m3u8';
    video0.addEventListener('loadedmetadata',function() {
      video0.play();
    });
  }
</script>
```
