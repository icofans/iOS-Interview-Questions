# 图像处理
## 1.图像的压缩方式
- 压缩图片质量
	
	一般情况下使用UIImageJPEGRepresentation或UIImagePNGRepresentation方法实现。

- 压缩图片尺寸
	
	一般通过指定压缩的大小对图像进行重绘

## 2.如何计算图片加载内存中所占的大小

图片内存大小的计算公式 宽度 * 高度 * bytesPerPixel/8。

bytesPerPixel : 每个像素所占的字节数。

RGBA颜色空间下 每个颜色分量由32位组成

所以一般图片的计算公式是 wxhx4 
