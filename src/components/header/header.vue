<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar"><img width="64" height="64" :src="seller.avatar" alt="商家店铺头像"></div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">{{seller.description}}/{{seller.deliveryTime}}钟送达</div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="this.classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div class="support-count" v-if="seller.supports">
                <span class="count">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper">
            <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background">
            <img :src="seller.avatar" alt="" width="100%" height="100%">
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
export default {
    props: {
        seller: {
            type: Object
        }
    },
    created() {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/mixin";
    .header
        position relative
        color #ffffff
        background rgba(7,17,27,0.5)
        .content-wrapper
            padding 24px 12px 18px 24px
            font-size 0
            position relative
            .avatar
                display inline-block
                vertical-align top
                img 
                    border-radius 2px
            .content
                display inline-block
                vertical-align top
                margin-left 16px
                font-size 14px
                .title
                    margin 2px 0 8px 0
                    font-size 0
                    .brand
                        display inline-block
                        vertical-align top
                        width 30px
                        height 18px
                        bg-image('brand')
                        background-repeat no-repeat
                        background-size 30px 18px
                    .name
                        font-size 16px
                        color rgb(255,255,255)
                        margin-left 6px
                        font-weight bold
                        line-height 18px
                .description
                    font-size 12px
                    color rgb(255,255,255)
                    font-weight 200
                    line-height 12px
                .support
                    margin-top 10px
                    font-size 0
                    .icon
                        display inline-block
                        vertical-align top
                        width 12px
                        height 12px
                        background-size 12px 12px
                        background-repeat no-repeat
                        &.decrease
                            bg-image('decrease_1')
                        &.discount
                            bg-image('discount_1')
                        &.guarantee
                            bg-image('guarantee_1')
                        &.invoice
                            bg-image('invoice_1')
                        &.special
                            bg-image('special_1')
                    .text
                        margin-left 4px
                        font-size 10px
                        color rgb(255,255,255)
                        font-weight 200
                        line-height 12px
            .support-count
                position absolute
                right 12px
                bottom 12px
                padding 7px 8px
                font-size 10px
                color rgb(255,255,255)
                background rgba(0,0,0,0.2)
                border-radius 16px
                line-height 12px


        .bulletin-wrapper
            position relative
            padding 0 22px 0 12px
            line-height 28px
            background-color rgba(7,17,27,0.2)
            white-space nowrap
            overflow hidden
            text-overflow ellipsis
            
            .bulletin-title
                display inline-block
                vertical-align top
                margin-top 7px
                width 22px
                height 12px
                background-size 22px 12px
                background-repeat no-repeat
                bg-image('bulletin')
            .bulletin-text
                
                vertical-align top
                margin-left 4px
                font-size 10px
                color rgb(255,255,255)
                font-weight 200
            .icon-keyboard_arrow_right
                position absolute
                top 8px
                right 12px
                margin-left 4px
                font-size 10px
                color rgb(255,255,255)
        .background
            position absolute
            top 0
            left 0
            width 100%
            height 100%
            z-index -1
            filter blur(10px)




                        

</style>