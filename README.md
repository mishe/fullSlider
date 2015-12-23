# 移动端相册功能组件

这是一个类似微信端的大图浏览模式的组件，内部有识别微信的判断，在微信内，会采用微信自身的wx.previewImage()函数。

功能内部还加入了图片的预加载处理。

## 调用方式：
```javascript
$.fullSlider({
            defaultSlide: parent.find('img').index(obj) + 1,
            getImgList: function () { //获取图片列表
                var arr = [];
                parent.find('img').each(function () {
                    var src = $(this).attr('src').split('?')[0];
                    if (src) arr.push(src.replace('_s', ''));
                });
                return arr;
            }
        });
```

### 参数说明

```javascript
class: 'full_slider',//样式名称，对应于想修改样式的尝试者的扩展
loadingPic: '/st/images/loading.gif', //图片预加载的loading图
getImgList: '.full_slider img', //字符串表示可以直接使用的jquery对象，
autoPlay: false, //自动播放，默认false
defaultSlide: 1  //自动定位到 第几张，默认为1
```

### getImgList 这个做了一些拓展支持，现有支持字符串是，数组和函数；

#### 来看下源码：
```javascript
            function _getImgList () {
                var imgList = '<ul class="' + options.class + '">',
                    pages = '<ul class="' + options.class + '_pages">',
                    tempArray = [],
                    getImgList = options.getImgList;
                //zepto 的实例不能通过 getImgList intanceof $ 获得true;
                if (typeof(getImgList) === 'string' || getImgList.selector) {
                    $(getImgList).each(function () {
                        tempArray.push(src);
                    });
                    imgsrc = tempArray;
                } else if (Object.prototype.toString.call(getImgList) == '[object Array]') {
                    imgsrc = options.getImgList;
                } else if (typeof(getImgList) === 'function') {
                    imgsrc = options.getImgList();
                    if (imgsrc[0].src) {
                        $(imgsrc).each(function () {
                            tempArray.push(src);
                        });
                        imgsrc = tempArray;
                    }
                } else {
                    throw new Error('获取目标图片错误');
                    return false;
                }
                length = imgsrc.length;
                for (var i = 0; i < length; i++) {
                    imgList += '<li><img src="' + options.loadingPic + '" style="width:12px;height:12px"></li>';
                    pages += '<li></li>';
                }
                return imgList + '</ul>' + pages + '</ul>';
            }
```


1. 首先检测是否是字符串，并是zepto或者jquery的选择器，那么直接采用对应的selector的元素

2.  如果是数组，那么直接赋给内部的图片数据

3. 如果是函数，那么会这行这个函数，并检测返回的结果；这个支持2中结果集

(1) DOM元素数组
(2) 图片数组
