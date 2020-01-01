This section contains information relevant to a Publisher of Live AV Content.

It explains how to use open and freely available software to stream content to a __Permissionless Digital Stage__.

## Using encoder (e.g. OBS Studio) instead of -publisher

[OBS Studio](https://obsproject.com/) is free and open-source software capable of publishing RTMP content.

This can be used instead of the `-publisher` to stream richer content.

### How

- Download and install OBS Studio

- Set up a Custom Server with your `{server-ip-address}` and your `{your key}` (required)

![Screenshot from 2019-12-31 20-25-55](https://user-images.githubusercontent.com/59374467/71631999-507fbd80-2c0c-11ea-8929-c9ef147cbaac.png)

- Set the keyframe interval to `1` (required)

![Screenshot from 2019-12-31 20-28-30](https://user-images.githubusercontent.com/59374467/71632001-507fbd80-2c0c-11ea-8b6f-e128af95b0ab.png)

- Set the frame size to 426x240 (240p) (recommended)

![Screenshot from 2019-12-31 20-28-20](https://user-images.githubusercontent.com/59374467/71632000-507fbd80-2c0c-11ea-9699-a0686d9bc5a8.png)

- Add some content to your OBS

  - Click the + to add a source of content
  
  ![image](https://user-images.githubusercontent.com/59374467/71632396-a6556500-2c0e-11ea-8f28-f0de09d79905.png)

  - Select "Test (Freetype 2)"
  
  ![image](https://user-images.githubusercontent.com/59374467/71632261-de0fdd00-2c0d-11ea-9247-7e0dee287b92.png)

  - Type "HELLO WORLD"
  
  ![image](https://user-images.githubusercontent.com/59374467/71632229-b7ea3d00-2c0d-11ea-8b08-4147e53ab443.png)

  - Click "Start Streaming"
  
  ![image](https://user-images.githubusercontent.com/59374467/71632290-0992c780-2c0e-11ea-8ae9-b04e9a17e8cf.png)

Your stream will be available at:

`http://{server-ip-address}:8935/stream/{your key}`

This is best viewed on mobile, but also can be viewed via VLC on a laptop.

If you use ffmpeg, you can run `ffmpeg http://{server-ip-address}:8935/stream/{your key}` in Terminal.
