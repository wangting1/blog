# webp格式的图片
### 什么是webp
- webp是谷歌推出的一种图片格式，在同等画面质量下，体积比jpg、png这些少了25%以上。
是一种支持有损压缩和无损压缩的图片文件格式，派生自图像编码格式 VP8。但是由于该技术是谷歌推出的所以存在兼容性问题，经测试在Safari、iOS Safari、iPhone下微信内置浏览器
都是不兼容的。所以现在使用的时候需要判断该浏览器是否支持。
### webp的使用
- 支持webp图片的浏览器在向服务器发送请求时，会在请求头Accept中带上image/webp，然后服务器判断
是否返回webp的文件。
优：只需在服务器端做处理简单方便。
缺：1.通过请求头检测，准确性存在问题。
2.大多图片是放在cdn服务器上的，在这个层面加逻辑判断难度较大。
- 前端检测
目前我们用的方案源码：
img.onload = function() {
    isSupportWebp = true; 
    that._firstCheckImages();
};
img.onerror = function() {
    isSupportWebp = false;
    that._firstCheckImages();
};
img.src = 'data:image/webp;base64,UklGRkoAAABXRUJQVlA4WAoAAAAQAAAAAAAAAAAAQUxQSAsAAAABBxAREYiI/gcAAABWUDggGAAAADABAJ0BKgEAAQABABwlpAADcAD+/gbQAA==';
因为这张base64图是webp格式。如果不支持会触发image.error方法，所以如果触发了onload方法，
那就说明浏览器支持webp格式。
### 其它比较有意思的点
#### 图片规格
-  WebP 使用的是 Fancy 采样算法，既然是采样算法必然有采样区块，比如：JPEG的采样区块是8*8，
对于原始图片的长宽不是8的倍数，都需要先补成8的倍数，使其能一块块的处理，所以对于8的整数倍的图片，压缩会更高效。
对于webp的压缩目前没有找到相关的结论。
#### 色彩数       
- 小于256色：适合采用Webp无损压缩
- 大于256色：适合采用WebP有损压缩，选择高压缩比(100%~75%)
- 远大于256色：适合采用WebP有损压缩，选择适中压缩比(75%以下)