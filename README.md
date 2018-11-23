# webrtc-streaming
WebRTC Live Video Stream Broadcasting One-To-Many and Watching with RTMP

![media-streaming.png](images/media-streaming.png)

## Streaming live to YouTube with Janus
#### 1. Janus
* janus.plugin.videoroom.cfg
```
[1234]
description = Demo Room
secret = adminpwd
publishers = 1
bitrate = 1024000
fir_freq = 1
;audiocodec = opus
videocodec = h264
record = false
;rec_dir = /path/to/recordings-folder
```
* rtp_forward request
```javascript
// Input this in Google Chrome debug console.
var rtpforward = { "request" : "rtp_forward", "room" : 1234, "publisher_id" : <myid>, "host" : "127.0.0.1", "audio_port" : 6002, "video_port" : 6004, "data_port" : 6000, "secret" : "adminpwd" };
sfutest.send({"message": rtpforward});
```

#### 2. YouTube
* YouTube live streaming: https://support.google.com/youtube/answer/2474026?hl=ko
* Get RTMP URL and stream name

#### 3. FFmpeg
* RTP to RTMP
```sh
# ffmpeg version 2.8.15
ffmpeg -analyzeduration 300M -probesize 300M -i janus.sdp -c:v copy -c:a aac -strict -2 -preset ultrafast -tune zerolatency -f flv <RTMP URL>/<stream name>

# ffmpeg version 3.4.4
ffmpeg -max_delay 5000 -reorder_queue_size 16384 -protocol_whitelist file,crypto,udp,rtp -re -i janus.sdp -vcodec copy -acodec aac -f flv <RTMP URL>/<stream name>
```

