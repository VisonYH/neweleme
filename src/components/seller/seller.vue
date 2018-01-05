<template>
    <div class="seller" v-el:sellerwrapper>
        <div class="seller-wrapper">
            <div class="seller-header">
                <div class="seller-name">{{seller.name}}</div>
                <div class="desc">
                    <star :score="seller.score" :size="starSize" class="star"></star>
                        <span class="ratingCount">({{seller.ratingCount}})</span>
                        <span class="sellCount">月售{{seller.sellCount}}单</span>
                </div>
                <div class="detail-data">
                    <div class="minprice item">起送价<br><p class="text">{{seller.minPrice}}</p>元</div>
                    <div class="deliveryPrice item">商家配送<br><p class="text">{{seller.deliveryPrice}}</p>元</div>
                    <div class="deliveryTime item">平均配送时间<br><p class="text">{{seller.deliveryTime}}</p>分钟</div>
                </div>
            </div>
            <div class="bulletin">
                <div class="title">
                    <h1>公告与活动</h1>
                    <p class="text">{{seller.bulletin}}</p>
                </div>
                <ul class="supports" v-if="seller.supports">
                    <li class="item" v-for="supportItem in seller.supports" :key="supportItem">
                        <span class="icon" :class="classMap[supportItem.type]"></span>
                        <span class="description">{{supportItem.description}}</span>
                    </li>
                </ul>
            </div>
            <div class="seller-pics">
                <h1>商家实景</h1>
                <div class="pics" v-el:picswrapper>
                    <div class="picswrapper" v-el:picList>
                        <img :src="pic" alt="商家实景图片" v-for="pic in seller.pics" :key="pic" class="imgitem">
                    </div>
                </div>
            </div>
            <div class="info">
                <h1>商家信息</h1>
                <ul class="content">
                    <li class="item" v-for="item in seller.infos" :key="item">{{item}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
import star from '../star/star';
import BScroll from 'better-scroll';
const ERR_OR = 0;

export default {
    data() {
        return {
            seller: {},
            starSize: 36
        };
    },
    components: {
        star
    },
    watch: {
        'seller'() {
            this._initScroll();
            this._initPicsScroll();
        }
    },
    methods: {
        _initScroll() {
            this.$nextTick(() => {
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$els.sellerwrapper, {click: true});
                } else {
                    this.scroll.refresh();
                }
            });
        },
        _initPicsScroll() {
            this.$nextTick(() => {
                let picWidth = 120;
                let marginRight = 6;
                let width = (picWidth + marginRight) * this.seller.pics.length - marginRight;
                document.getElementsByClassName('picswrapper')[0].style.width = width + 'px';
              //  this.$els.picList.style.width = width + 'px';
                this.$nextTick(() => {
                    if (!this.picsScroll) {
                    this.picsScroll = new BScroll(this.$els.picswrapper, {
                        scrollX: true,
                        eventPassthrough: 'vertical'
                    });
                    } else {
                        this.picsScroll.refresh();
                    }
                });
            });
        }
    },
    created() {
            this.$http.get('/api/seller').then(response => {
                response = response.body;
                if (response.errno === ERR_OR) {
                    this.seller = response.data;
                }
            });
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin";
.seller
    position absolute
    top 174px
    left 0
    bottom 0
    width 100%
    background #f3f5f7
    overflow hidden
    .seller-header
        position relative
        padding 18px
        background #ffffff
        .seller-name
            font-size 14px
            font-weight 700
            color rgb(7,17,27)
            line-height 14px
        .desc
            width 100%
            font-size 0
            padding-bottom 18px
            border-1px(rgba(7,17,27,0.1))
            .star
                display inline-block
                vertical-align top
                margin-top 8px
                height 18px
            .ratingCount
                display inline-block
                vertical-align top
                margin 8px 12px 0 8px
                font-size 10px
                color rgb(77,85,93)
                line-height 18px
            .sellCount
                display inline-block
                font-size 10px
                line-height 18px
                vertical-align top
                color rgb(77,85,93)
                margin-top 8px
        .detail-data
            display flex
            font-size 10px
            color rgb(147,153,159)
            text-align center
            .item
                flex 1
                margin-top  18px
                border-right 1px solid rgba(7,17,27,0.1)
                &:last-child
                    border none
                .text
                    display inline-block
                    margin-top 4px
                    font-size 24px
                    line-height 24px
                    color rgb(7,17,27)
    .bulletin
        margin-top 16px
        background #ffffff
        .title
            padding 18px 18px 0 18px
            h1
               font-size 14px
               color rgb(7,17,27)
               line-height 14px
            .text
                font-size 12px
                color rgb(240,20,0)
                line-height 24px
                padding 8px 12px 16px 12px
                border-1px(rgba(7,17,27,0.1))
        .supports
            padding 0 18px
            .item
                padding 16px 12px
                border-1px(rgba(7,17,27,0.1))
                font-size 0
                .icon
                    display inline-block
                    vertical-align top
                    width 16px
                    height 16px
                    background-repeat no-repeat
                    background-size 16px 16px
                    &.decrease
                        bg-image('decrease_4')
                    &.invoice
                        bg-image('invoice_4')
                    &.special
                        bg-image('special_4')
                    &.discount
                        bg-image('discount_4')
                    &.guarantee
                        bg-image('guarantee_4')
                .description
                    font-size 12px
                    line-height 16px
                    margin-left 6px
                    color rgb(7,17,27)
    .seller-pics
        padding 18px
        background #ffffff
        h1
            font-size 14px
            color rgb(7,17,27)
            line-height 14px
        .pics
            margin-top 12px
            width 100%
            overflow hidden
            white-space nowrap
            font-size 0
            .imgitem
                display inline-block
                width 120px
                height 90px
                margin-right 6px
                &:last-child
                    margin-right 0
    .info
        margin-top 18px
        padding 18px 18px 0
        background #ffffff
        h1
            padding-bottom 12px
            font-size 14px
            color rgb(7,17,27)
            line-height 14px
            border-1px(rgba(7,17,27,0.1))
        .content
            font-size 12px
            color rgb(7,17,27)
            line-height 16px           
            .item
                padding 16px 12px
                border-1px(rgba(7,17,27,0.1))

</style>