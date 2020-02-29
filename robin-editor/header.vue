<template>
    <view class="head">
        <view class="btn grey left" @tap="cancel" v-if="labelCancel">{{labelCancel}}</view>
        <view class="btn right" @tap="save" v-if="labelConfirm">{{labelConfirm}}</view>
    </view>
</template>

<script>
    export default {
        props:{
           labelCancel:{
               type:String,
               default:"取消"
           },
           labelConfirm:{
               type:String,
               default:"确定"
           }
        },
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
        justify-content: space-between;
        background-color: #fff;

        .btn {
            display: block;
            line-height: 60upx;
            font-size: 30upx;
            text-align: right;
            padding: 0 10upx;
            color: #007AFF;
            &.left{
              float: left;  
            }
            &.right{
                float: right;
            }
            &.grey {
                color: #333333;
            }
        }
    }
</style>
