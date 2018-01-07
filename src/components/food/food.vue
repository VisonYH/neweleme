<template>
    <div v-el:foodwrapper class="food" v-show="showFlag" transition="move"> 
        <div class="food-detail" >
            <div class="head">
                <div class="head-image">
                    <img :src="food.image" alt="食物大图" class="image">
                </div>
                <div class="back" @click="hideFlag"><i class="icon-arrow_lift"></i></div>
                <div class="text">
                    <h1 class="title">{{food.name}}</h1>
                    <div class="extra">
                        <span>月售{{food.sellCount}}份</span>
                        <span>好评率{{food.rating}}%</span>
                    </div>
                    <div class="price">
                            <span>￥{{food.price}}</span>
                            <span v-show="food.oldPrice" class="oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                    <div class="addcart" @click.stop.prevent="addcart($event)"  v-show="!food.count || food.count === 0" transition="fade">加入购物车</div>
                    <cartcontrol class="cart" v-show="food.count > 0" :food="food"></cartcontrol>
                </div>    
            </div>
            <div class="desc" v-show="food.info">
                <h1 class="title">商品介绍</h1>    
                <p class="info">{{food.info}}</p>
            </div>
            <div class="food-ratings">
                <h1 class="title">商品评价</h1>
                <ratingselect :ratings="food.ratings" v-ref:ratingselect @toggle="toggle" @selecttype="typeSelect"></ratingselect>
                
                <div class="rating-content">
                    <ul v-show="food.ratings && food.ratings.length">
                        <li class="content border-1px" v-for="rating in food.ratings" :key="rating" v-show="needShow(rating)">
                            <div class="time">{{getTime(rating.rateTime)}}</div>
                            <div class="user">
                                <span class="username">{{rating.username}}</span>
                                <span class="avatar"><img :src="rating.avatar" alt="用户头像" width="16px" height="16px"></span>
                            </div>
                            <div class="comment">
                                <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                                <span class="text">{{rating.text}}</span>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>
<script type="ecmascript-6">
import BScroll from 'better-scroll';
import Vue from "Vue";
import ratingselect from "../ratingselect/ratingselect";
import cartcontrol from '../cartcontrol/cartcontrol';
const ALL = 2;
export default {
  data() {
      return {
          showFlag: false,
          onlyContent: true,
          ratingselect: 2,
          selectshow: true
      };
  },
  components: {
      cartcontrol,
      ratingselect
  },
  props: {
      food: Object
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
      typeSelect(type) {
        this.ratingselect = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      toggle() {
          this.onlyContent = !this.onlyContent;
          this.$nextTick(() => {
            this.scroll.refresh();
          });
      },
      addcart(event) {
        if (!event._constructed) {
            return;
        }
        this.$dispatch('cart.add', event.target);
        Vue.set(this.food, 'count', 1);
      },
      show() {
          this.showFlag = true;
          this.$nextTick(() => {
            if (!this.scroll) {
                this.scroll = new BScroll(this.$els.foodwrapper, {
                    click: true
                });
            } else {
                this.scroll.refresh();
            }
         });
      },
      hideFlag() {
          this.showFlag = false;
      },
      getTime(timeStamp) {
            let date = new Date(timeStamp);
            let Y = date.getFullYear() + '-';
            let M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
            let D = date.getDate() + ' ';
            let h = date.getHours() + ':';
            let m = date.getMinutes();
            return Y+M+D+h+m;
      },
     
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin";
.food
    position fixed
    top 0
    left 0
    bottom 48px
    width 100%
    z-index 40
    background-color #f3f5f7 
    &.move-transition
        transition all 0.2s linear
        transform translate3d(0,0,0)
    &.move-enter,&.move-leave
        transform translate3d(100%,0,0) 
    .head
        .head-image
            position relative
            width 100%
            height 0
            padding-top 100%
            .image
                position absolute
                top 0
                left 0
                width 100%
                height 100%
        .back
            position absolute
            top 10px
            left 0
            .icon-arrow_lift
                padding 10px
                font-size 15px
                color #fff
        .text
            position relative
            background-color #fff
            padding 18px
            .title
                font-size 14px
                font-weight 700
                color rgb(7,17,27)
                line-height 14px
            .extra
                margin 8px 0 18px
                font-size 10px
                line-height 10px
                color rgb(147,153,159)
            .price    
                font-size 14px
                font-weight 700
                line-height 24px
                color rgb(240,20,20)
                .oldPrice
                    font-size 10px
                    font-weight 200
                    line-height 24px
                    color rgb(147,153,159)
                    text-decoration line-through
            .addcart
                position absolute
                right 18px
                bottom 16px
                height 24px
                font-size 10px
                color rgb(255,255,255)
                line-height 24px
                padding 0 12px
                box-sizing border-box
                border-radius 12px
                background-color rgb(0,160,220)
                &.fade-transition
                    opacity 1
                    transition all 0.2s linear
                &.fade-enter,&.fade-leave
                    opacity 0
                    z-index -1
            .cart
                position absolute
                right 18px
                bottom 16px
    .desc
        margin 16px 0
        padding 18px
        background-color #ffffff

        .title
            margin-bottom 6px
            font-size 14px
            font-weight 700
            color rgb(7,17,27)
            line-height 14px
        .info
            font-size 12px
            font-weight 200
            color rgb(75,85,93)
            line-height 24px
    .food-ratings
        margin 16px 0
        padding 18px
        background-color #ffffff
        .title
            margin-bottom 6px
            font-size 14px
            font-weight 700
            color rgb(7,17,27)
            line-height 14px
        .rating-content
            .content
                position relative
                padding 16px 0
                border-1px(rgba(147,153,159,0.1))
                .time
                    font-size 10px
                    color rgb(147,153,159)
                    line-height 12px
                .user
                    position absolute
                    right 0
                    top 16px
                    font-size 0
                    color rgb(147,153,159)
                    line-height 12px
                    .username
                        vertical-align top
                        font-size 10px
                        margin-right 6px
                    .avatar
                        height 16px
                        width 16px
                        position relative
                        top -3px
                        img
                            border-radius 50%
                .comment
                    font-size 12px
                    line-height 24px
                    .icon-thumb_up
                        color rgb(0,160,220)
                    .icon-thumb_down
                        color rgb(147,153,159)
                    .text
                        padding: 6px 0 0 0;
                        font-size: 12px;
                        color: rgb(7,17,27);
                        line-height: 16px;
                        
</style>
