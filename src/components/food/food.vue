<template>
   <div class="food-detail" v-el:food-wrapper>
       <div class="head">
           <img :src="food.image" alt="食物大图" class="image">
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
               <div class="addcart">加入购物车</div>
           </div>    
       </div>
       <div class="desc">
           <h1 class="title">商品介绍</h1>    
           <p class="info">{{food.info}}</p>
       </div>
       <div class="ratings">
           <h1 class="title">商品评价</h1>
           <div class="classification">
               <span class="item all">全部<i></i></span>
               <span class="item good">推荐<i></i></span>
               <span class="item bad">吐槽<i></i></span>
           </div>
           <div class="onlyhascontent"><i></i>只看有内容的评价</div>
           <div class="rating-content">
               <ul>
                   <li class="content" v-for="rating in food.ratings" :key="rating">
                       <div class="time">{{rating.rateTime}}</div>
                       <div class="user">
                           <span class="avatar"></span>
                           <span class="username">{{rating.username}}</span>
                       </div>
                       <div class="comment">{{rating.text}}</div>
                   </li>
               </ul>
           </div>
       </div>
   </div>

</template>
<script type="ecmascript-6">
import BScroll from 'better-scroll';
export default {
  props: {
      food: Object
  },
  created() {
      
      this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$els.foodWrapper, {
              click: true
            });
            console.log('test');
          } else {
            this.scroll.refresh();
          }
      });
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.food-detail
    background-color #f3f5f7
    overflow hidden
    .head
        width 100%
        .image
            width 100%
            height 100%
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

        
</style>
