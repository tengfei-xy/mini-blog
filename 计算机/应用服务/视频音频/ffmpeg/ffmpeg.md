# ffmpeg

解析音频文件

```bash
ffmpeg -i audio.m4s -c:a copy audio.mp4
```

解析视频文件

```bash
ffmpeg -i video.m4s -c:v copy video.mp4
```

合并音频文件和视频文件

```bash
ffmpeg -i video.mp4 -i audio.mp4 -c:v copy -c:a copy output.mp4
```

调试模式

```bash
ffmpeg -v debug
```
