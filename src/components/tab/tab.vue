<template>
    <div class="tab">
        <div class="tab-head border-1px">
            <div class="tab-item">
                <a v-link="{path:'/goods'}">商品</a>
            </div>
            <div class="tab-item">
                <a v-link="{path:'/ratings'}">评论</a>
            </div>
            <div class="tab-item">
                <a v-link="{path:'/seller'}">商家</a>
            </div>
        </div>
        <router-view :seller='seller'></router-view>
    </div>
</template>

<script type="text/ecmascript-6">
    export default {
        props: {
            seller: {
                type: Object
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';
    .tab
        .tab-head
            display flex
            width 100%
            height 40px
            line-height 40px
            border-1px(rgba(7,17,27,0.1))
            .tab-item
                flex 1
                text-align center
                & > a
                    display block
                    font-size 14px
                    color rgb(77,85,93)
                    &.active
                        color rgb(240,20,20)
                    

</style>