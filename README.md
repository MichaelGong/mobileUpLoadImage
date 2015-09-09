# mobileUpLoadImage
移动端图片上传并处理

目前我所知道的移动端上传图片的方式大致有几种：
* 直接上传图片（由于移动端照出来的图片会比较大，这种方式上传的图片没有经过压缩，上传时间比较长且有可能失败）
* 直接通过某些兼容移动端的插件进行上传
* 通过URL.createObjectURL的方式读取图片并处理后上传base64格式的数据（兼容性）
* 通过FileReader的方式读取并处理后上传base64格式的数据（兼容性）
前两种方式不用说，想试验的可以自行百度，主要说下后面两种方式：
> URL.createObjectURL兼容性：Android4.0以上、IOS6.1以上

> FileReader兼容性：Android3.0以上、IOS6.1以上

所以两种方式都有兼容性问题。

这两种方式操作起来其实都很方便

首先你需要在页面上创建一个input标签，
```html
<input type="file" capture="camera" id="imgInput" />
```
如果是通过FileReader，你的代码大致应该这样：

```js
var imgInput = document.getElementById('imgInput');
function getImageByFileReader(){
	if(typeof FileReader == 'undefined'){  
		alert('你的浏览器不支持FileReader!');
		return;
	}
	var file = null;
	imgInput.onchange = function(){
		if(this.files.length<=0){
			return;
		}
		file = this.files[0];
		//判断是否是图片
		if(!/image\/\w+/.test(file.type)){  
			alert("请上传图片！");  
			return false;  
		}  
		//创建FileReader对象
		var reader = new FileReader();
		reader.readAsDataURL(file);
		reader.onload = function(){
			console.log(this.result);
			//this.result是一个base64格式的字符串，你可以操作这个base64的字符串
		}
	}
}
getImageByFileReader();
```

如果是通过URL.createObjectURL的形式：

```js
function getImageByURL(){
	var URL = URL || webkitURL;
	if(typeof URL == 'undefined'){
		lert('你的浏览器不支持URL对象');
		return;
	}
	var file = null;
	imgInput.onchange = function(){
		file = this.files[0];
		var blob = URL.createObjectURL(file);
		//blob是一个blob字符串，将这个字符串放到img的src中就可以正常的展示图片
	}
}
getImageByURL();
```
两种方式都不难。

[https://github.com/think2011/localResizeIMG4](https://github.com/think2011/localResizeIMG4)这里有个插件，可以利用这个插件来达到上面我们所说的目的，用法很简单，而且这个插件对各种平台的canvas中bug做了优化处理，很方便。

图片本身有一些基础的属性，这些属性被统称为exif信息，exif信息包含了比如图片拍摄时间、焦距、曝光度、拍摄方向等等诸多信息，由于拍摄时拍照者可能会将拍照设备旋转各种方向，所以我们可以利用exif中的拍摄方向的属性来调整图片在我们页面中的展示方向，[http://code.ciaoca.com/javascript/exif-js/](http://code.ciaoca.com/javascript/exif-js/)这个插件可以获取图片的exif信息。


