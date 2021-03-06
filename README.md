# AVFoundation + （OpenCV + Zxing）


![](https://github.com/JDongKhan/JDScan/blob/master/demo.gif)


## 前言

二维码扫描是很多公司需要用到的一款工具，也是一个老大难问题，市面上免费的开源组件都只能覆盖到特定的场景，而实际生活、工作中场景非常复杂，有的二维码打印出来的，有点破损，也有的在电脑上受距离、光照等影响。 


## 这是一款二维码扫描的解决方案。

iOS端扫描二维码无非是苹果提供的AVFoundation系列、Zxing、Zbar。而Zbar早已不在维护，我们不打算再使用（Android现在还在使用Zbar）。
   
所以本方案是基于AVFoundation + Zxing共同实现的，当然这两种技术网上`例子一大堆，但是都是一些单独使用的Demo，并没有整合的方案，能像本项目这样能直接拿去用的就没有了。 

## 为何这样做
为何用到Zxing，事实上Zxing确实扫描能力不如AVFoundation，目前苹果的扫码器能满足大部分场景，但是不是所有的二维码都是完美的。

比如

  1、有些二维码是打印出来的颜色变成了灰色（因手里的异常二维码属于公司私有不能拿出来）。
  
  2、扫描受到光照的影响。
  
  3、斜扫二维码
  
这些异常场景普通二维码扫码器是搞不定的，而我们又搞不定二维码扫码器，那么我们能做的就是在识别图片之前将图片处理一下再交给扫码器。

由于苹果的API太过于封闭，而CIDetector只能识别二维码、人脸之类的，没有Zxing识别的多，所以这里就暂时使用到了Zxing来做接盘侠。 

当然苹果的二维码识别器我们也没浪费，AVCaptureSession可以支持多个AVCaptureOutput，AVCaptureMetadataOutput就是苹果提供的二维码扫码器，我们在使用AVCaptureMetadataOutput的同时也将使用AVCaptureVideoDataOutput，AVCaptureVideoDataOutput可以将每一帧回调出来，在这里就可以做Zxing的识别了。

这样两种扫码器就能同时工作了。

 
## OpenCV

在二维码识别之前如果有一种技术能把异常的二维码处理成高清的那就好了，此时OpenCV就上场了，由于二维码识别不在乎颜色，那么图片二值化就是我们首先想到的了，能把颜色淡的像素点处理成黑色那相信二维码识别器就能较高成功率的识别。

只单单将图片二值化并不能完全解决我们的问题，因为现实的场景是很复杂的，我们在扫码的时候会受到各种影响，比如光照，这里我们又需要先降低光照的影响，OpenCV降噪目前不是很理想，我们也只是调用了medianBlur而已，这里有待继续研究。



这里的OpenCV目前也只是做了 降噪 + 图片二值化的工作，但是实际的工作中只要不是太过苛刻也足以满足需求了。


因OpenCV相关的知识缺乏目前只能做到这里。


## 支持功能

1、手势/自动缩放

2、自动对焦

3、识别图片

4、生成二维码

5、扫描view自定义

6、样式小调整

7、灯光打开/关闭

8、识别各种类型码

## Cocoapods 使用 

``` 
pod 'JDScan'

```

注：部分代码/资源来源于https://github.com/MxABC/LBXScan

如果你是一名Android开发，https://github.com/LiuhangZhang/qrcode_android 这里是Android的解决方案，本方案一开始使用的整体二值化不是太理想，后面借鉴此项目中分块二值化效果还不错，毕竟本人OpenCV知识也是有限。

本方案也不是万能的，当然足够满足大部分企业需求，如果需要做成微信那样，可能就需要使用OCR技术了。
