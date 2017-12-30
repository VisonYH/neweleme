<template>
  <div class="shopcart">
      <div class="content">
          <div class="content-left" @click="toggleList()">
              <div class="logo-wrapper">
                  <div class="logo" :class="{'highlight': totalPrice > 0}">
                      <i class="icon-shopping_cart"></i>
                  </div>
                  <div class="num" v-show="totalCount">{{totalCount}}</div>
                  </div>
              <div class="price" :class="{'highlight': totalPrice > 0}">￥{{totalPrice}}</div>
              <div class="desc">另需配送费{{deliverprice}}元</div>
          </div>
          <div class="content-right">
              <div class="pay" :class="payclass">{{priceStyle}}</div>
          </div>
      </div>
      <div class="ball-container">
        <div class="ball" v-show="ball.show" v-for="ball in balls" :key="ball" transition="drop">
              <div class="inner inner-hook"></div>
        </div>
      </div>
      <div class="shopcart-list" v-show="showShoplist()" transition="slide">
          <div class="list-header">
              <h1 class="title">购物车</h1>
              <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" v-el:listcontent>
              <ul>
                  <li class="food" v-for="food in selectedfoods" :key="food">
                      <span class="name">{{food.name}}</span>
                      <div class="price">
                          <span>￥{{food.price*food.count}}</span>
                      </div>
                      <div class="cartcontrol-wrapper">
                          <cartcontrol :food="food" ></cartcontrol>
                      </div>
                  </li>
              </ul>
          </div>
      </div>
      <div class="list-mask" @click="hidelist()" v-show="showShoplist()" transition="fade"></div>
  </div>
</template>
<script type="text/ecmascript-6">
import cartcontrol from 'components/cartcontrol/cartcontrol';
import BScroll from 'better-scroll';
export default {
  components: {
      cartcontrol
  },
  data() {
      return {
          balls: [{show: false}, {show: false}, {show: false}, {show: false}, {show: false}],
          dropballs: [],
          payclass: '',
          show: false
      };
  },
  created() {
  },
  props: {
      selectedfoods: {
          type: Array
      },
      deliverprice: {
          type: Number,
          default: 0
      },
      minprice: {
          type: Number,
          default: 0
      }
  },
  methods: {
      drop(el) {
          for (let i = 0; i < this.balls.length; i++) {
              let ball = this.balls[i];
              if (!ball.show) {
                  ball.show = true;
                  ball.el = el;
                  this.dropballs.push(ball);
                  return;
              }
          }
      },
      empty() {
          this.selectedfoods.forEach((food) => {
              food.count = 0;
          });
      },
      showShoplist() {
          if (!this.totalCount) {
              this.show = false;
              return false;
          }
          let temp = this.show;
          if (temp) {
              this.$nextTick(() => {
                if (!this.shoplistScroll) {
                    this.shoplistScroll = new BScroll(this.$els.listcontent, {click: true});
                } else {
                    this.shoplistScroll.refresh();
                }
              });
          }
          return temp;
      },
      toggleList() {
          if (!this.totalCount) {
             return false;
          }
          this.show = !this.show;
          return this.show;
      },
      hidelist() {
          this.show = false;
      }
  },
  computed: {
      totalPrice() {
          let totalprice = 0;
          this.selectedfoods.forEach((food) => {
              if (food.count > 0) {
                  totalprice += food.count * food.price;
              }
          });
          return totalprice;
      },
      totalCount() {
          let totalcount = 0;
          this.selectedfoods.forEach((food) => {
              if (food.count > 0) {
                  totalcount += food.count;
              }
          });
          return totalcount;
      },
      priceStyle() {
          if (this.totalPrice >= this.minprice) {
              this.payclass = 'enough';
              return '去结算';
          } else if (this.totalPrice > 0) {
              this.payclass = 'not-enough';
              return '还差￥' + (this.minprice - this.totalPrice) + '元起送';
          } else {
              this.payclass = 'not-enough';
              return '￥' + this.minprice + '起送';
          }
      }
  },
  transitions: {
      drop: {
          beforeEnter(el) {
              let count = this.balls.length;
              while (count--) {
                  let ball = this.balls[count];
                  if (ball.show) {
                      let rect = ball.el.getBoundingClientRect();
                      let x = rect.left - 32;
                      let y = -(window.innerHeight - rect.top - 22);
                      el.style.display = '';
                      el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
                      el.style.transform = `translate3d(0, ${y}px, 0)`;
                      let inner = el.getElementsByClassName('inner-hook')[0];
                      inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
                      inner.style.transform = `translate3d(${x}px, 0, 0)`;
                  }
              }
          },
          enter(el) {
              /* eslint-disable no-unused-vars */
              let rf = el.offsetHeight;  // 触发浏览器重绘
              this.$nextTick(() => {
                    el.style.webkitTransform = 'translate3d(0, 0, 0)';
                    el.style.transform = 'translate3d(0, 0, 0)';
                    let inner = el.getElementsByClassName('inner-hook')[0];
                    inner.style.webkitTransform = 'translate3d(0, 0, 0)';
                    inner.style.transform = 'translate3d(0, 0, 0)';
              });
          },
          afterEnter(el) {
              let ball = this.dropballs.shift();
              if (ball) {
                  ball.show = false;
                  el.style.display = 'none';
              }
          }
      }
  }
};
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';
.shopcart
    z-index 50
    .content
        display flex
        color rgba(255,255,255,0.4)
        background #141d27 
        .content-left
            flex 1
            .logo-wrapper
                position relative 
                top -10px
                display inline-block
                width 56px
                height 56px
                margin 0 12px
                padding 6px
                border-radius: 50%
                box-sizing border-box
                background-color: #141d27;           
                .logo
                    width 100%
                    height 100%
                    border-radius: 50%;
                    font-size 24px                                       
                    background-color #2b343c
                    text-align center
                    &.highlight
                        background #00a0dc
                        .icon-shopping_cart
                            color #ffffff
                    .icon-shopping_cart
                        font-size 24px
                        line-height 44px
                        color rgba(255,255,255,0.4)
                .num
                    position absolute
                    top 0
                    right 0
                    width 24px
                    font-size 9px
                    font-weight 700
                    color rgb(255,255,255)
                    line-height 16px
                    background-color rgb(240,20,20)
                    border-radius 6px
                    text-align center
                    box-shadow 0 2px 4px 0 rgba(0,0,0,0.4)
            .price
                display inline-block
                font-size 16px
                line-height 24px
                font-weight 700
                margin-top 12px
                vertical-align top
                padding-right 12px
                border-right 1px solid rgba(255,255,255,0.1)
                color rgba(255,255,255,0.4)
                &.highlight
                    color #ffffff
                    font-weight 700
            .desc
                display inline-block
                font-size 10px
                line-height 24px
                margin-top 12px
                vertical-align top
                padding-left 12px
        .content-right
            flex 0 0 105px
            width: 105px
            .pay
                height 48px
                font-size 12px
                line-height 48px
                text-align center
                background-color #2b333b
                &.enough
                    background-color #00b43c
                    color #fff
    .ball-container
        .ball
            position: fixed;
            left: 32px;
            bottom: 22px;
            z-index 200
            transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
            .inner
                width: 16px
                height: 16px
                border-radius: 50%
                background: rgb(0, 160, 220)
                transition: all 0.4s linear
    .shopcart-list
        position absolute
        top 0
        left 0
        width 100%
        z-index: -1
        transition all 0.4s linear
        
        &.slide-transition
            transform translate3d(0,-100%,0)
        &.slide-enter, &.slide-leave
            transform: translate3d(0, 0, 0)
                
        .list-header
            height 40px
            line-height 40px
            padding 0 18px
            background-color #f3f5f7
            color rgb(7,17,27)
            .title
                float left
            .empty
                float right
        .list-content
            background-color #fff
            padding 0 18px
            max-height: 217px;
            overflow: hidden;
            .food
                position relative
                height 48px
                line-height 48px
                border-1px(rgba(7,17,27,0.1))
                color rgb(7,17,27)
                .name
                    color rgb(7,17,27)
                .price
                    position absolute
                    right 90px
                    top 0
                    line-height 48px
                    font-size 14px
                    font-weight 700
                    color #f01414
                .cartcontrol-wrapper
                    position absolute
                    right 0
                    top 6px
                    height 36px
                    font-size 14px
    .list-mask
        position fixed
        top 0
        left 0
        width 100%
        height 100%
        z-index -20
        transition all 0.4s linear
        &.fade-transition
            opacity 1
            background rgba(7,17,27,0.6)
        &.fade-enter,&.fade-leave
            opacity 0
            background: rgba(7, 17, 27, 0)
        
</style>
