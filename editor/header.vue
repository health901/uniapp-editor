<template>
    <view class="head">
        <view class="btn grey" @tap="cancel">取消</view>
        <view class="btn" @tap="save">保存</view>
    </view>
</template>

<script>
    export default {
        methods:{
            cancel:function(){
                this.$emit('cancel')
            },
            save:function(){
                this.$emit('save')
            }
        }
    }
</script>

<style lang="scss" scoped>
    .head {
        width: 100%;
        height: 60upx;
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        background-color: #fff;

        .btn {
            line-height: 60upx;
            font-size: 30upx;
            text-align: right;
            padding: 0 10upx;
            color: #007AFF;

            &.grey {
                color: #333333;
            }
        }
    }
</style>
