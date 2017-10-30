<template>
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span class="block positive" @click="selectRating(2,$event)" :class="{'active':selectType===2}">{{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span class="block positive" @click="selectRating(0,$event)" :class="{'active':selectType===0}">{{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span class="block negative" @click="selectRating(1,$event)" :class="{'active':selectType===1}">{{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="switch">
      <span class="icon-check_circle" @click="toogleContent($event)" :class="{'on':onlyContent}"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      ratings: {
        type: Array,
        default() {
          return [];
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
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
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        });
      },
      negatives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        });
      }
    },
    methods: {
      selectRating(type, event) {
        if (!event._constructed) {
          return;
        }
        this.selectType = type;
        this.$dispatch('ratingtype.select', type);
      },
      toogleContent(event) {
        if (!event._constructed) {
          return;
        }
        this.onlyContent = !this.onlyContent;
        this.$dispatch('content.toogle', this.onlyContent);
      }
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .ratingselect
    .rating-type
      padding: 18px 0;
      margin: 0 18px
      border-1px(rgba(7, 17, 27, 0.1))
      font-size: 0
      .block
        display: inline-block
        padding: 8px 12px
        margin-right: 8px
        line-height: 16px
        border-radius: 2px
        font-size: 12px
        color: rgb(77, 85, 93)
        &.active
          color: #ffffff
        .count
          margin-left: 2px
          font-size: 8px
        &.positive
          background: rgba(0, 160, 220, 0.2)
          &.active
            background: rgb(0, 160, 220)
        &.negative
          background: rgba(77, 85, 93, 0.2)
          &.active
            background: rgb(77, 85, 93)

    .switch
      padding: 12px 18px
      font-size: 0
      border-bottom: 1px solid rgba(7, 17, 27, 0.1)
      color: rbg(147, 153, 159)
      line-height: 24px
      .icon-check_circle
        margin-right: 4px
        font-size: 24px
        display: inline-block
        vertical-align: top
        &.on
          color: #00c850
      .text
        display: inline-block
        vertical-align: top
        font-size: 12px


</style>
