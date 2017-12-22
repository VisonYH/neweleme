<template>
    <div class="bac" >
        <div class="detail-wrapper clearfix">
            <div class="detail-main">
                <h1 class="name">{{seller.name}}</h1>
                <star :score="seller.score" :size="starSize" class="star"></star>
                <div class="discount detail-item">
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <div class="content">
                        <div class="supports" v-for="supportItem in seller.supports" track-by="$index" :key="supportItem">
                            <span class="icon" :class="classMap[supportItem.type]"></span>
                            <span class="description">{{supportItem.description}}</span>
                        </div>
                    </div>
                </div>
                <div class="bulletin detail-item">
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <p class="content">{{seller.bulletin}}</p>
                </div>
            </div>
        </div>
        <div class="detail-close" @click="DetailClose"><i class="icon-close"></i></div>
    </div>
</template>

<script type="text/ecmascript6">
    import star from "components/star/star";
    export default {
        data() {
            return{
                starSize: 48
            };
        },
        created() {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        },
        components: {
            star
        },
        props: {
            seller: {
                type: Object
            },
            detailshow: {
                type: Object
            }
        },
        methods: {
            DetailClose() {
                this.detailshow.show = false;
            }
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin";
    .bac
        .detail-wrapper
            min-height 100%
            width 100%
            .detail-main
                margin-top 64px
                padding 0 36px 64px
                h1
                    text-align center
                    font-size 16px
                    font-weight 700
                    color rgb(255,255,255)
                    line-height 16px
                .star
                    margin 16px 0 0
                    text-align center
                    line-height 24px
                .detail-item
                    margin-top 28px                   
                    .title
                        position relative
                        display flex
                        .line
                            position relative
                            top -6px
                            flex 1      
                            border-bottom 1px solid rgba(255,255,255,0.2)
                        .text
                            padding 0 12px
                            font-size 14px
                    .content
                        padding 24px 12px 0 12px
                        font-size 12px
                        line-height 24px
                        .supports
                            font-size 0
                            margin-bottom 12px
                            .icon
                                display inline-block
                                vertical-align top
                                width 16px
                                height 16px
                                background-repeat no-repeat
                                background-size 16px 16px
                                &.decrease
                                    bg-image('decrease_2')
                                &.invoice
                                    bg-image('invoice_2')
                                &.special
                                    bg-image('special_2')
                                &.discount
                                    bg-image('discount_2')
                                &.guarantee
                                    bg-image('guarantee_2')
                            .description
                                font-size 12px
                                line-height 16px
                                margin-left 6px
                                color rgb(255,255,255)
        .detail-close
            position relative
            margin -48px auto 0 auto
            clear both
            width 32px
            height 32px
            .icon-close
                font-size 32px
                color rgba(255,255,255,0.5)
            
            


</style>