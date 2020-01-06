# 修改 wx-plugin/image-cropper 微信小程序裁剪，支持支付宝小程序
* 去除了裁剪动画，动画在支付宝小程序会有卡顿  
* 修改了组件属性和方法 适用于支付宝小程序
* 支付宝小程序使用了 ref来获取组件，开发工具选项勾选 `启用component2编译`
* 注意支付宝小程序里的 canvas 组件id  `支付宝<canvas id='image-cropper'>` 和微信的区别`微信<canvas canvas-id='image-cropper'>`
* 支付宝小程序旋转或者移动canvas的画布后需要还原

#### axml
```html
<image-cropper a:if="{{show}}" ref="imagecropper" id="image-cropper" limit_move="{{true}}" disable_rotate="{{true}}" width="{{width}}" height="{{height}}" imgSrc="{{src}}" bindload="cropperload" onLoadimage="loadimage" onClickcut="clickcut"></image-cropper>
```
#### js
```javascript
data:{
    src: '',
    width: 250, //宽度
    height: 250, //高度
}

imagecropper(ref) {
    // 存储自定义组件实例，方便以后调用
    this.cropper = ref;
},

imgCropp() {
    console.log('imgCropp');
    this.cropper.getImg(obj => {
        console.log(obj);
        my.previewImage({
            urls: [obj.url] // 需要预览的图片http链接列表
        });
    });
},

loadimage(e) {
    console.log('图片加载完成', e.detail);
    my.hideLoading();
    //重置图片角度、缩放、位置
    this.cropper.imgReset();
},
```
#### json
```json
{
    "usingComponents": {
    "image-cropper": "./image-cropper/image-cropper"
  },
}
```