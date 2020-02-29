# 富文本编辑器插件
uniapp 富文本编辑器插件

## 兼容性
|微信小程序|H5|APP|
|:--:|:--:|:--:|
|√|√ |x|


## 使用方式
在 `script` 中引用组件
```js
import myeditor from "@/components/editor/editor.vue"
export default {
    components: {myeditor}
}
```
在 `template` 中使用组件
```html
<myeditor class="editor" 
    @cancel="hideEditor" 
    @save="saveEditor" 
    :html="html"
    :imageUploader="uploadImg" 
    :muiltImage="true">
</myeditor>
```

## demo
https://github.com/health901/uniapp-editor-demo

## 属性说明
|属性|类型|默认值|说明|
|--|--|--|--|
|html|String||初始化富文本|
|imageUploader|Function||上传图片处理函数 接受参数 img:本地图片地址,callback:上传成功回调传入图片链接|
|muiltImage|Boolean|false|是否支持多图上传|
|compressImage|Boolean|true|图片上传是否压缩|
|previewMode|Boolean|false|预览模式,不可编辑|

## 事件说明
|事件|说明|参数|
|--|--|--|
|cancel|点击取消按钮|
|save|点击保存按钮|e={html,text,delta}|

## 依赖
|组件|链接|备注|
|---|--|--|
|Popup 弹出层<sup>[[1]](#注)</sup>|https://ext.dcloud.net.cn/plugin?id=329|uni-ui库|
|Transition动画|https://ext.dcloud.net.cn/plugin?id=1231|uni-ui库,Popup依赖|
|颜色选择器ColorPicker<sup>[[2]](#注)</sup>|https://ext.dcloud.net.cn/plugin?id=1237|字体颜色,背景色|


## 注

1. 修改:新增动画结束事件
2. 修改:添加按钮,支持预设颜色值
