<template>
    <div class="ratings" v-el:ratings>
         <div>
             <div class="rating-header">
                <div class="header-left">
                    <div class="totalScore">{{seller.score}}</div>
                    <div class="text">综合评分</div>
                    <div class="desc">高于周边商家69.2%</div>
                </div>
                <div class="header-right">
                    <div class="item"><span class="text">服务态度</span><star :size="36" :score="seller.serviceScore"></star><span class="score">{{seller.serviceScore}}</span></div>
                    <div class="item"><span class="text">商品评分</span><star :size="36" :score="seller.foodScore"></star><span class="score">{{seller.foodScore}}</span></div>
                    <div class="item"><span class="text">送达时间</span><span class="time">{{seller.deliveryTime}}分钟</span></div>
                </div>
             </div>
             <ratingselect :ratings='ratings' class="ratingselect" @toggle="toggle" @selecttype="typeSelect"></ratingselect>
             <div class="rating-content">
                 <ul>
                     <li class="item" v-for="rating in ratings" :key="rating" v-show="needShow(rating)">
                         <div class="image-wrapper">
                             <img :src="rating.avatar" alt="用户头像" class="avatar" width="28" height="28">
                         </div>
                         <div class="content">
                             <div class="user">{{rating.username}}</div>
                             <star :size="24" :score="rating.score" class="star"></star>
                             <span class="time">{{rating.deliveryTime}}钟送达</span>
                             <p class="comment">{{rating.text}}</p>
                             <div class="bottom">
                                 <i class="goodorbad" :class="{'icon-thumb_down': rating.rateType === 1, 'icon-thumb_up': rating.rateType === 0}"></i>
                                 <ul class="recommend">
                                     <li class="recommend-item" v-for="item in rating.recommend" :key="item">{{item}}</li>
                                 </ul>
                             </div>
                         </div>
                     </li>
                 </ul>
             </div>
         </div>
    </div>
</template>

<script type="text/ecmascript-6">
    import BScroll from 'better-scroll';
    import star from '../star/star.vue';
    import ratingselect from '../ratingselect/ratingselect.vue';
    const ERR_OR = 0;
    const ALL = 2;
    export default {
        data() {
            return {
                ratings: [],
                onlyContent: true,
                ratingselect: 2
            };
        },
        props: {
            seller: {}
        },
        components: {
            star,
            ratingselect
        },
        created() {
            this.$http.get('/api/ratings').then(response => {
                response = response.body;
                if (response.errno === ERR_OR) {
                    this.ratings = response.data;
                    this.$nextTick(() => {
                        this.scroll = new BScroll(this.$els.ratings, {click: true});
                    });
                }
            });
        },
        methods: {
            needShow(rating) {
                if (this.onlyContent && !rating.text) {
                    return false;
                }
                if (this.ratingselect === ALL) {
                    return true;
                } else {
                    return rating.rateType === this.ratingselect;
                }
            },
            toggle() {
                this.onlyContent = !this.onlyContent;
                this.$nextTick(() => {
                    this.scroll.refresh();
                });
            },
            typeSelect(type) {
                this.ratingselect = type;
                this.$nextTick(() => {
                this.scroll.refresh();
                });
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin';
.ratings
    position absolute
    top 174px
    left 0
    bottom 0
    overflow hidden
    width 100%
    background #f3f5f7
    .rating-header
        display flex
        background #ffffff
        .header-left
            flex: 0 0 137px
            width 137px
            margin 18px 0
            text-align center
            border-right 1px solid rgba(7,17,27,0.1)
            @media only screen and (max-width: 320px)
                flex 0 0 120px
                width 120px
            .totalScore
                font-size 24px
                color rgb(255,153,0)
                line-height 28px
            .text
                font-size 12px
                color rgb(7,17,27)
                line-height 12px
                margin-top 6px
            .desc
                margin 8px 0 6px
                font-size 10px
                color rgba(7,17,27,0.6)
        .header-right
            flex: 1
            margin 18px 24px
            .item
                margin 0 0 8px
                font-size 0
                &:last-child
                    margin-bottom 0
                .text
                    display inline-block
                    margin-right 12px
                    vertical-align top
                    font-size 12px
                    color rgb(7,17,27)
                    line-height 18px
                .star
                    display inline-block
                    vertical-align top
                .score
                    display inline-block
                    vertical-align top
                    font-size 12px
                    color rgb(255,153,0)
                    line-height 18px
                .time
                    font-size 12px
                    color rgb(147,153,159)
                    line-height 18px
    .ratingselect
        background #ffffff
        margin-top 18px
        padding 0 18px
        border-1px(rgba(7,17,27,0.1))
    .rating-content
        background #ffffff
        padding 0 18px
        .item
            display flex
            padding 18px 0
            border-1px(rgba(7,17,27,0.1))
            font-size 0
            .image-wrapper
                flex 0 0 28px
                img
                    width 28px
                    height 28px
                    border-radius 50%
            .content
                flex 1
                margin-left 12px
                vertical-align top
                font-size 10px
                .user
                    font-size 10px
                    color rgb(7,17,27)
                    line-height 12px
                .star
                    display inline-block
                    margin-top 4px
                .time
                    margin-left 6px
                    font-size 10px
                    color rgb(147,153,159)
                    line-height 12px
                .comment
                    margin-top 6px
                    font-size 12px
                    color rgb(7,17,27)
                    line-height 18px
                .bottom
                    display flex
                    margin-top 8px
                    .goodorbad
                        flex 0 0 12px
                        font-size 12px
                        line-height 16px
                        &.icon-thumb_up
                            color rgb(0,160,220)
                        &.icon-thumb_down
                            color #93999f
                    .recommend
                        flex 1
                        .recommend-item
                            display inline-block
                            margin-left 8px
                            margin-bottom 4px
                            font-size 9px
                            color rgb(147,153,159)
                            line-height 16px
                            border 1px solid rgba(7,17,27,0.1)
                            border-radius 2px
</style>