<template>
    <div class="star" :class="starSize">
        <span class="star-item" v-for="itemType in itemTypes" :class="itemType" track-by="$index" :key="itemType"></span>
    </div>
</template>

<script type="text/ecmascript6">
const LENGTH = 5;
const CLS_ON = "on";
const CLS_OFF = "off";
const CLS_HALF = "half";
export default {
  props: {
    score: Number,
    size: Number
  },
  computed: {
    starSize() {
      return "star" + this.size;
    },
    itemTypes() {
      let result = [];
      let score = Math.floor(this.score * 2) / 2;
      let integer = Math.floor(score);
      let hasDecimal = score % 1 !== 0;
      for(let i = 0; i < integer; i++) {
          result.push(CLS_ON);
      }
      if(hasDecimal) {
          result.push(CLS_HALF);
      }
      while(result.length < LENGTH) {
          result.push(CLS_OFF);
      }
      return result;
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin";
.star 
    font-size: 0;
    .star-item
        display: inline-block;
        background-repeat: no-repeat;
    &.star24 
        .star-item 
            width: 10px;
            height: 10px;
            background-size: 10px 10px;
            margin-right: 3px;
            &:last-child 
                margin-right: 0;
            &.on 
                bg-image('star24_on');
            &.off 
                bg-image('star24_off');
            &.half 
                bg-image('star24_half');
    &.star36 
        .star-item 
            width: 15px;
            height: 15px;
            background-size: 15px 15px;
            margin-right: 8px;
            &.on 
                bg-image('star36_on');
            &.off 
                bg-image('star36_off');
            &.half 
                bg-image('star36_half');
    &.star48 
        .star-item 
            width: 20px;
            height: 20px;
            background-size: 20px 20px;
            margin-right: 22px;
            &.on
                bg-image('star48_on');
            &.off 
                bg-image('star48_off');
            &.half 
                bg-image('star48_half');
</style>