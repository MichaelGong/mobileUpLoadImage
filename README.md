# mobileUpLoadImage
移动端图片上传并处理

目前我所知道的移动端上传图片的方式大致有几种：
* 直接上传图片（由于移动端照出来的图片会比较大，这种方式上传的图片没有经过压缩，上传时间比较长且有可能失败）
* 通过URL.createObjectURL的方式读取图片并处理后上传base64格式的数据（兼容性）
* 通过FileReader的方式读取并处理后上传base64格式的数据（兼容性）

> URL.createObjectURL兼容性：Android4.0以上、IOS6.1以上

> FileReader兼容性：Android3.0以上、IOS6.1以上

所以两种方式都有兼容性问题。