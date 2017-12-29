<template>
    <div class="goods">
        <div class="menu-wrapper" v-el:menu-wrapper>
            <ul>
                <li v-for="(index,item) in goods" :key="item" class="menu-item" :class="{current: getCurrentIndex === index}" @click="selectMenu(index,$event)">                
                    <span class="text border-1px">
                        <span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>
                        <span class="name">{{item.name}}</span>
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" v-el:foods-wrapper>
            <ul>
                <li v-for="item in goods" class="foods-list" :key="item">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li v-for="food in item.foods" :key="food" class="food-item">
                          <div class="icon">
                              <img :src="food.icon" alt="食品图片">
                          </div>
                          <div class="content">
                              <h2 class="name">{{food.name}}</h2>
                              <p class="desc" v-show="food.description">{{food.description}}</p>
                              <div class="extra">
                                  <span>月售{{food.sellCount}}份</span>
                                  <span>好评率{{food.rating}}%</span>
                              </div>
                              <div class="price">
                                  <span>￥{{food.price}}</span>
                                  <span v-show="food.oldPrice" class="oldPrice">￥{{food.oldPrice}}</span>
                              </div>
                              <div class="cartcontrol-wrapper">
                                  <cartcontrol :food="food"></cartcontrol>
                              </div>
                          </div>

                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart v-ref:shopcart :selectedfoods="selectedFoods" :deliverprice="seller.deliveryPrice" :minprice="seller.minPrice"></shopcart>
    </div>
</template>

<script type="text/ecmascript-6">
    import shopcart from 'components/shopcart/shopcart.vue';
    import BScroll from 'better-scroll';
    import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
    const ERR_OR = 0;
    export default {
        data() {
            return {
                goods: [],
                scrollY: 0,
                HeightArray: []
            };
        },
        components: {
            cartcontrol,
            shopcart
        },
        props: {
            seller: {
                type: Object
            }
        },
        created() {
            this.$http.get('/api/goods').then(response => {
                response = response.body;
                if (response.errno === ERR_OR) {
                    this.goods = response.data;
                    this.$nextTick(() => {
                        this._initScroll();
                        this.goodsListHeight();
                    });
                }
            });
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
        },
        methods: {
            _initScroll() {
                this.menuScroll = new BScroll(this.$els.menuWrapper, {click: true});
                this.foodsScroll = new BScroll(this.$els.foodsWrapper, {click: true, probeType: 3});
                this.foodsScroll.on('scroll', (pos) => {
                    if (pos.y <= 0) {
                        this.scrollY = Math.abs(Math.floor(pos.y));
                    }
                });
            },
            _drop(target) {
                 // 体验优化,异步执行下落动画
                this.$nextTick(() => {
                    this.$refs.shopcart.drop(target);
                });
            },
            goodsListHeight() {
                let foodsList = document.getElementsByClassName('foods-list');
                let height = 0;
                this.HeightArray.push(height);
                for (let i = 0; i < foodsList.length; i++) {
                    height += foodsList[i].clientHeight;
                    this.HeightArray.push(height);
                }
                return 0;
            },
            followScroll(index) {
                let menu = document.getElementsByClassName('menu-item');
                let el = menu[index];
                this.menuScroll.scrollToElement(el, 300);
            },
            selectMenu(index, event) {
                if (!event._constructed) {
                    return;
                }
                let el = document.getElementsByClassName('foods-list')[index];
                this.foodsScroll.scrollToElement(el, 300);
            }
        },
        computed: {
            getCurrentIndex() {
                for (let i = 0; i < this.HeightArray.length; i++) {
                    if (!this.HeightArray[i + 1] || (this.scrollY >= this.HeightArray[i] && this.scrollY < this.HeightArray[i + 1])) {
                        this.followScroll(i);
                        return i;
                    }
                }
                return 0;
            },
            selectedFoods() {
                let foods = [];
                this.goods.forEach((good) => {
                    good.foods.forEach((food) => {
                        if (food.count > 0) {
                            foods.push(food);
                        }
                    });
                });
                return foods;
            }
        },
        events: {
            'cart.add'(target) {
                this._drop(target);
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin";
.goods
    position absolute
    display flex
    overflow hidden
    top 174px
    left 0
    bottom 46px
    width 100%

    .menu-wrapper
        flex 0 0 80px
        width 80px
        background #f3f5f7
        .menu-item
            display table
            width 56px
            padding 0 12px
            height 54px     
            font-size 0
            &.current
                font-weight 700                
                z-index: 10
                margin-top: -1px
                background: #fff
                .text
                    border-none()
            .text
                display table-cell
                vertical-align middle
                font-size 0
                line-height 12px
                border-1px(rgba(7,17,27,0.1))
                .name
                    font-size 12px
                    color rgb(7,17,27)
                .icon
                    display inline-block
                    vertical-align top
                    height 12px
                    width 12px
                    margin-right 2px
                    background-size 12px 12px
                    background-repeat no-repeat
                    &.decrease
                        bg-image('decrease_3')
                    &.discount
                        bg-image('discount_3')
                    &.guarantee
                        bg-image('guarantee_3')
                    &.invoice
                        bg-image('invoice_3')
                    &.special
                        bg-image('special_3')
            


    .foods-wrapper
        flex 1
        .foods-list
            .title
                height 26px
                font-size 12px
                color rgb(147,153,159)
                line-height 26px
                border-left 2px solid #d9dde1
                padding-left 14px
                background #f3f5f7
            .food-item
                display flex
                border-1px(rgba(7,17,27,0.1))
                padding 18px
                .icon
                    flex 0 0 70px
                    height 70px
                    width 70px
                    margin-right 10px
                    img 
                        width 100%
                .content
                    flex 1
                    font-size 10px
                    color rgb(147,153,159)
                    line-height 10px
                    position relative
                    .name
                        font-size 14px
                        color rgb(7,17,27)
                        line-height 14px
                        margin 2px 0 8px
                    .desc
                        margin-bottom 8px
                        line-height 14px
                    .extra
                        span:first-child
                            margin-right 12px
                    .price
                        font-size 14px
                        color rgb(240,20,20)
                        font-weight 700
                        line-height 24px
                        .oldPrice
                            font-size 10px
                            color rgb(147,153,159)
                            text-decoration line-through
                    .cartcontrol-wrapper
                        position absolute
                        right 0
                        bottom -8px
    .shopcart
        position fixed
        bottom 0
        left 0
        height 48px
        width 100%







</style>