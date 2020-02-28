<template>
    <view class='wrapper'>
        <editor id="editor" class="ql-container" placeholder="开始输入..." showImgSize showImgToolbar showImgResize
            @statuschange="onStatusChange" :read-only="readOnly" @ready="onEditorReady">
        </editor>
        <topbar class="header" @cancel="cancel" @save="save"></topbar>
        <view class='toolbar' @tap="format">
            <view :class="formats.bold ? 'ql-active' : ''" class="iconfont icon-zitijiacu" data-name="bold" data-label="加粗"></view>
            <view :class="formats.italic ? 'ql-active' : ''" class="iconfont icon-zitixieti" data-name="italic"
                data-label="斜体"></view>
            <view :class="formats.underline ? 'ql-active' : ''" class="iconfont icon-zitixiahuaxian" data-name="underline"
                data-label="下滑线"></view>
            <view :class="formats.strike ? 'ql-active' : ''" class="iconfont icon-zitishanchuxian" data-name="strike"
                data-label="删除线"></view>
            <view :class="(formats.align === 'left' || !formats.align) ? 'ql-active' : ''" class="iconfont icon-zuoduiqi"
                data-name="align" data-value="left" data-label="居左"></view>
            <view :class="formats.align === 'center' ? 'ql-active' : ''" class="iconfont icon-juzhongduiqi" data-name="align"
                data-value="center" data-label="居中"></view>
            <view :class="formats.align === 'right' ? 'ql-active' : ''" class="iconfont icon-youduiqi" data-name="align"
                data-value="right" data-label="居右"></view>
            <!-- <view :class="formats.align === 'justify' ? 'ql-active' : ''" class="iconfont icon-zuoyouduiqi"
                        data-name="align" data-value="justify"></view> -->
            <!-- <view :class="formats.lineHeight ? 'ql-active' : ''" class="iconfont icon-line-height" data-name="lineHeight"
					 data-value="2"></view> -->
            <!-- <view :class="formats.letterSpacing ? 'ql-active' : ''" class="iconfont icon-Character-Spacing" data-name="letterSpacing"
					 data-value="2em"></view> -->
            <!-- <view :class="formats.marginTop ? 'ql-active' : ''" class="iconfont icon-722bianjiqi_duanqianju" data-name="marginTop"
					 data-value="20px"></view> -->
            <!-- <view :class="formats.previewarginBottom ? 'ql-active' : ''" class="iconfont icon-723bianjiqi_duanhouju"
					 data-name="marginBottom" data-value="20px"></view> -->
            <view class="iconfont icon-clearedformat" @tap="removeFormat"></view>
            <picker class="iconfont" mode="selector" :range="fontSizeRange" @change="fontSize">
                <view class="icon-fontsize"></view>
            </picker>

            <view :style="fontColor!='#FFFFFF' ? 'color:'+formats.color : ''" class="iconfont icon-text_color"
                data-name="color" @tap.stop="openColor"></view>
            <view :style="bgColor ? 'color:'+formats.backgroundColor : '' " class="iconfont icon-fontbgcolor" data-name="backgroundColor"
                @tap.stop="openColor"></view>

            <!-- <view class="iconfont icon-date" @tap="insertDate"></view> -->
            <!-- <view class="iconfont icon--checklist" data-name="list" data-value="check"></view> -->
            <!-- <view :class="formats.list === 'ordered' ? 'ql-active' : ''" class="iconfont icon-youxupailie"
                        data-name="list" data-value="ordered"></view> -->
            <!-- <view :class="formats.list === 'bullet' ? 'ql-active' : ''" class="iconfont icon-wuxupailie"
                        data-name="list" data-value="bullet"></view> -->
            <!-- <view class="iconfont icon-undo" @tap="undo"></view> -->
            <!-- <view class="iconfont icon-redo" @tap="redo"></view> -->

            <!-- <view class="iconfont icon-outdent" data-name="indent" data-value="-1"></view> -->
            <!-- <view class="iconfont icon-indent" data-name="indent" data-value="+1"></view> -->
            <!-- <view class="iconfont icon-fengexian" @tap="insertDivider"></view> -->
            <view class="iconfont icon-charutupian" @tap="insertImage"></view>
            <!-- <view :class="formats.header === 1 ? 'ql-active' : ''" class="iconfont icon-format-header-1" data-name="header"
					 :data-value="1"></view> -->
            <!-- <view :class="formats.script === 'sub' ? 'ql-active' : ''" class="iconfont icon-zitixiabiao" data-name="script"
					 data-value="sub"></view>
					<view :class="formats.script === 'super' ? 'ql-active' : ''" class="iconfont icon-zitishangbiao" data-name="script"
					 data-value="super"></view> -->
            <view class="iconfont icon-shanchu" @tap="clear"></view>
            <!-- <view :class="formats.direction === 'rtl' ? 'ql-active' : ''" class="iconfont icon-direction-rtl" data-name="direction"
					 data-value="rtl"></view> -->

        </view>
        <uni-popup ref="popup" type="bottom" @transed="colorPop">
            <colorPicker :color="color" :show="showColor" @confirm="colorChanged" @cancel="colorPopClose"></colorPicker>
        </uni-popup>
    </view>
</template>

<script>
    import colorPicker from '@/components/colorPicker.vue'
    import uniPopup from "@/components/uni-popup/uni-popup.vue"
    import topbar from '@/components/editor/header.vue'
    export default {
        components: {
            colorPicker,
            uniPopup,
            topbar
        },
        props: {
            html: {
                type: String
            },
            imageUploader: {
                type: Function
            },
            muiltImage: {
                type: Boolean,
                default: false
            },
            compressImage: {
                type: Boolean,
                default: false
            }
        },
        data() {
            return {
                readOnly: false,
                formats: {},
                fontColor: "#000000",
                bgColor: "",
                color: "",
                colorPickerName: "",
                showColor: false,
                fontSizeRange: [10, 12, 14, 16, 18, 24, 32]
            }
        },
        methods: {
            openColor(e) {
                var name = e.currentTarget.dataset.name
                var color = this.formats[name]
                this.colorPickerName = name
                if (name == 'backgroundColor' && !color) {
                    color = "#FFFFFF"
                }
                if (name == 'color' && !color) {
                    color = "#000000"
                }
                this.color = color
                this.$refs.popup.open({
                    type: "bottom"
                })
            },
            colorPop(e) {
                this.showColor = e.show
            },
            colorPopClose() {
                this.$refs.popup.close()
            },
            colorChanged(e) {
                let label = ''
                switch (this.colorPickerName) {
                    case 'backgroundColor':
                        if (e.color == '#FFFFFF') {
                            e.color = ''
                        }
                        this.bgColor = e.color
                        label = '背景色'
                        break
                    case 'color':
                        this.fontColor = e.color
                        label = '颜色'
                        break
                }
                this.colorPopClose()
                this._format(this.colorPickerName, e.color, label + e.color)
            },
            readOnlyChange() {
                this.readOnly = !this.readOnly
            },
            onEditorReady() {
                uni.createSelectorQuery().in(this).select('#editor').context((res) => {
                    this.editorCtx = res.context
                    if (this.html) {
                        this.editorCtx.setContents({
                            html: this.html
                        })
                    }
                }).exec()
            },
            undo() {
                this.editorCtx.undo()
                this.toast("撤销")
            },
            redo() {
                this.editorCtx.redo()
                this.toast("重做")
            },
            format(e) {
                let {
                    name,
                    value,
                    label
                } = e.target.dataset
                if (!name) return
                this._format(name, value, label)
            },
            _format(name, value, label) {
                this.editorCtx.format(name, value)
                this.toast(label)
            },
            toast(label) {
                uni.showToast({
                    duration: 600,
                    icon: 'none',
                    title: label
                })
            },
            onStatusChange(e) {
                const formats = e.detail
                this.formats = formats
            },
            insertDivider() {
                this.editorCtx.insertDivider({
                    success: function() {
                        this.toast("插入分割线")
                    }
                })
            },
            clear() {
                this.editorCtx.clear({
                    success: (res) => {
                        this.toast("清空")
                    }
                })
            },
            removeFormat() {
                this.editorCtx.removeFormat()
                this.toast("清除格式")
            },
            insertDate() {
                const date = new Date()
                const formatDate = `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()}`
                this.editorCtx.insertText({
                    text: formatDate
                })
                this.toast("插入日期")
            },
            insertImage() {
                let params = {}
                params.count = this.muiltImage ? 9 : 1
                params.sizeType = this.compressImage ? ['compressed'] : ['original']
                uni.chooseImage({
                    ...params,
                    success: (res) => {
                        res.tempFilePaths.map(path => {
                            this.imageUploader(path, (url) => {
                                this.editorCtx.insertImage({
                                    src: url,
                                    alt: '图像'
                                })
                            })
                        })
                    }
                })
            },
            fontSize(e) {
                const index = e.detail.value
                const fz = this.fontSizeRange[index] + 'px'
                this._format('fontSize', fz, '字体大小:' + fz)
            },
            cancel() {
                this.$emit('cancel')
            },
            save() {
                this.editorCtx.getContents({
                    success: res => {
                        this.$emit('save', res)
                    }
                })

            }
        }
    }
</script>

<style lang="scss" scoped>
    @import "./editor-icon.css";

    .wrapper {
        padding: 5px;

        .header {
            width: 100%;
            position: fixed;
            left: 0;
            /* #ifndef H5 */
            top: 0;
            /* #endif */
            /* #ifdef H5 */
            top: 44px;
            /* #endif */

        }
    }

    .toolbar {
        position: fixed;
        width: 100%;
        left: 0;
        bottom: 0;
        box-sizing: border-box;
        font-family: 'Helvetica Neue', 'Helvetica', 'Arial', sans-serif;
        background-color: #fff;
        border-top: 1px solid #ccc;
        line-height: 45upx;
        display: flex;
        justify-content: space-evenly;

        .iconfont {
            padding: 12upx 0;
            width: 50upx;
            text-align: center;
            font-size: 35upx;
        }
    }


    .ql-container {
        box-sizing: border-box;
        padding: 24upx 30upx;
        width: 100%;
        min-height: 30vh;
        height: auto;
        background: #fff;
        margin-top: 80upx;
        font-size: 32upx;
        line-height: 1.5;
        border: 1px solid #333333;
    }

    .ql-active {
        color: #06c;
    }
</style>
