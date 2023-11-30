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

m4a转mp3

```bash
 ffmpeg -i 音频.m4a  -codec:a libmp3lame -b:a 320k -ar 44100 音频.mp3
```

一键下载QQ音乐的歌曲，原理是用ffmpeg下载并转格式为mp3

```bash
ffmpeg -i https://dl.stream.qqmusic.qq.com/C400002q1JZH1nuZDA.m4a\?guid\=5143114248\&vkey\=033C49BCA79BD942642B9CB23DDF48EA323CD76AAF1675352A3D5A935FAA13AAE23FA34AABD238821D77AA421396599BA1051D32BF0E7E56\&uin\=1765552388\&fromtag\=120032 -codec:a  libmp3lame  陈绮贞-我喜欢上你时的内心活动.mp3
```
