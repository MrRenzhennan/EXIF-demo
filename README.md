# EXIF exif.js

用于从图像文件中读取EXIF元数据的JavaScript库。  

您可以在浏览器中的图像上使用它，从图像或文件输入元素。  
检索EXIF和IPTC元数据。这个包也可以在AMD或CommonJS环境中使用。  

注意:EXIF标准仅适用于.jpg和.tiff图像。  
这个包中的EXIF逻辑基于EXIF标准v2.2 (JEITA CP-3451，包含在这个repo中)。  

## Install
```node
npm install exif-js --save 
```
## Usage
该包添加了一个全局EXIF变量(或AMD或CommonJS等效变量)  
首先调用EXIF.getData函数。 您将图像作为参数传递给它：  
1. 来自`<img src =“image.jpg”>`的图像  
2. 或者用户在页面上`<input type =“file”>`元素中选择的图像。  
  
  
作为第二个参数，您可以指定回调函数。 在回调函数中，您应该使用它来访问具有上述元数据的图像，然后您可以根据需要使用它们。 该图像现在有一个额外的exifdata属性，它是一个带有EXIF元数据的Javascript对象。 您可以访问它的属性以获取图像标题，拍摄照片的日期或方向等数据。  

你可以用EXIF.getTag获得所有的tages。 或者使用EXIF.getTag获取单个标记，您可以在其中将标记指定为第二个参数。 要使用的标记名称列在exif.js中的EXIF.Tags中。  
  
  **重要说明**：请注意，在调用getData或任何其他函数之前，必须等待图像完全加载。 否则它会默默地失败。 您可以通过在window.onLoad函数上运行exif-extraction逻辑来实现此等待。 或者在图像上自己的onLoad功能。 对于jQuery用户，请注意，您不能（可靠地）使用jQuery的ready事件。 因为它在加载图像之前触发。 您可以使用$（window）.load（）而不是$（document.ready（）（请注意`exif-js不依赖于jQuery或任何其他外部库）。  
  
```js
window.onload=getExif;

function getExif() {
    var img1 = document.getElementById("img1");
    EXIF.getData(img1, function() {
        var make = EXIF.getTag(this, "Make");
        var model = EXIF.getTag(this, "Model");
        var makeAndModel = document.getElementById("makeAndModel");
        makeAndModel.innerHTML = `${make} ${model}`;
    });

    var img2 = document.getElementById("img2");
    EXIF.getData(img2, function() {
        var allMetaData = EXIF.getAllTags(this);
        var allMetaDataSpan = document.getElementById("allMetaDataSpan");
        allMetaDataSpan.innerHTML = JSON.stringify(allMetaData, null, "\t");
    });
}
``` 
```html
<img src="image1.jpg" id="img1" />
<pre>Make and model: <span id="makeAndModel"></span></pre>
<br/>
<img src="image2.jpg" id="img2" />
<pre id="allMetaDataSpan"></pre>
<br/>
```  

注意，还有一些替代标记，比如EXIF.TiffTags。有关完整定义和使用，请参阅源代码。您还可以通过使用EXIF.pretty获得图像中所有EXIF信息的字符串
