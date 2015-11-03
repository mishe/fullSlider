# fullSlider
移动端全屏图片浏览组件

调用方式：
<pre>
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
</pre>


#参数说明
<pre>
class: 'full_slider',//样式名称
loadingPic: '/st/images/loading.gif', //图片预加载的loading图
getImgList: '.full_slider img', //字符串表示可以直接使用的jquery对象，可以是函数，返回的是相应需要放大的图片数组,
autoPlay: false, //自动播放，默认false
defaultSlide: 1  //自动定位到 第几张，默认为1
</pre>
