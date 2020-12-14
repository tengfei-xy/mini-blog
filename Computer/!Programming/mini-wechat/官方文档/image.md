# image

[链接](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)

**mode 的合法值**

| 值          | 说明                                                         | 最低版本                                                     |
| :---------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| scaleToFill | 缩放模式，不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素 |                                                              |
| aspectFit   | 缩放模式，保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。 |                                                              |
| aspectFill  | 缩放模式，保持纵横比缩放图片，只保证图片的短边能完全显示出来。也就是说，图片通常只在水平或垂直方向是完整的，另一个方向将会发生截取。 |                                                              |
| widthFix    | 缩放模式，宽度不变，高度自动变化，保持原图宽高比不变         |                                                              |
| heightFix   | 缩放模式，高度不变，宽度自动变化，保持原图宽高比不变         | [2.10.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |