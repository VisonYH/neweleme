<template>
    <div class="ratingselect">
        <div class="classification">
            <span class="item all" :class="{'active': ALL_active}" @click.stop.prevent="select($event,2)">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
            <span class="item positive" :class="{'active': POSITIVE_active}" @click.stop.prevent="select($event,0)">{{desc.positive}}<span class="count">{{positives}}</span></span>
            <span class="item negative" :class="{'neg-active': NEGATIVE_active}" @click.stop.prevent="select($event,1)">{{desc.negative}}<span class="count">{{negatives}}</span></span>
        </div>
        <div class="onlyhascontent" @click="toggleContent" >
            <i class="icon-check_circle"  :class="{'active': active}"></i><span>只看有内容的评价</span>
        </div>
    </div>
</template>
<script type="ecmascript-6">
const POSITIVE = 0;
const ALL = 2 ;
const NEGATIVE = 1;
export default {
  data() {
      return {
        active: true,
        positiveCount: 0,
        negativeCount: 0,
        ALL_active: true,
        POSITIVE_active: false,
        NEGATIVE_active: false
      }
  },
  props: {
      ratings: {
          type: Array,
          default() {
              return [];
          }
      },
      desc: {
          type: Object,
          default() {
              return {
                  all: '全部',
                  positive: '满意',
                  negative: '不满意'
              };
          }
      }
  },
  computed: {
      positives() {
        this.positiveCount = 0;
        this.ratings.forEach((rating) => {
          if (rating.rateType === POSITIVE) {
              this.positiveCount++;
          }
        });
        return this.positiveCount;
      },
      negatives() {
        this.negativeCount = 0;
        this.ratings.forEach((rating) => {
          if (rating.rateType === NEGATIVE) {
              this.negativeCount++;
          }
        });
        return this.negativeCount;
      }
  },
  methods: {
      toggleContent(event) {
          if (!event._constructed) {
            return;
          }
          this.active = !this.active; 
          this.$emit('toggle');
      },
      select(event,type) {
          if (!event._constructed) {
              return;
          }
          this.$emit('selecttype', type);
          if (type === 2) {
              this.ALL_active = true;
              this.POSITIVE_active = false;
              this.NEGATIVE_active = false;
          } else if (type === 1) {
              this.ALL_active = false;
              this.POSITIVE_active = false;
              this.NEGATIVE_active = true;
          } else {
              this.ALL_active = false;
              this.POSITIVE_active = true;
              this.NEGATIVE_active =false;
          }
      }
  }
};
</script>
<style rel="stylesheet/stylus" lang="stylus">
@import '../../common/stylus/mixin.styl';
.ratingselect
    .classification
        padding 18px 0 18px 0
        font-size 0
        border-1px(rgba(7,17,27,0.1))
        .item
            display inline-block
            margin-right 8px
            padding 8px 12px
            font-size 12px
            line-height 12px
            color rgb(255,255,255)
            .count
                font-size 8px
                margin-left 3px
            &.all
                background rgba(0,160,220,0.2)
                color rgb(77,85,93)
            &.positive
                background rgba(0,160,220,0.2)
                color rgb(77,85,93)
            &.negative
                color rgb(77,85,93)
                background rgba(77,85,93,0.2)
            &.active
                background rgb(0,160,220)
                color #ffffff
            &.neg-active
                background rgb(77,85,93)
                color #ffffff
    .onlyhascontent
        padding 18px 0
        border-1px(rgba(7,17,27,.1))
        font-size 12px
        line-height 24px
        color rgb(147,153,159)
        .icon-check_circle
            display inline-block
            margin-right 4px
            font-size 24px
            color rgb(147,153,159)
            line-height 24px
            vertical-align top
            &.active
                color #00c850
</style>
