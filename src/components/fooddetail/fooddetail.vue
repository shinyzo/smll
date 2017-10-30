<template>
  <div class="food-detail" v-show="showFlag" transition="move" v-el:food>
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image">
        <div class="back" @click="hideDetail">
          <i class="icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <h1 class="title">{{food.name}}</h1>
        <div class="desc">
          <span class="sell-count">月售{{food.sellCount}}份</span>
          <span class="rating">好评{{food.rating}}%</span>
        </div>
        <div class="price">
          <span class="now">¥{{food.price}}</span>
          <span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontrol-wrapper">
          <v-cartcontrol :food="food"></v-cartcontrol>
        </div>
        <div class="buy" transition="fade" @click.stop.prevent="addFirstCart" v-show="!food.count || food.count===0">
          加入购物车
        </div>

      </div>
      <v-split></v-split>
      <div class="info">
        <h1 class="title">商品介绍</h1>
        <p class="text">{{food.info}}</p>
      </div>
      <v-split></v-split>
      <div class="rating">
        <h1 class="title">商品评价</h1>
        <v-ratingselect :desc="desc" :select-type="selectType" :only-content="onlyContent"
                        :ratings="food.ratings"></v-ratingselect>
        <div class="rating-wrapper">
          <ul v-show="food.ratings && food.ratings.length">
            <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
              <div class="user">
                <span class="name">{{rating.username}}</span>
                <img class="avatar" :src="rating.avatar" width="12" height="12"/>
              </div>
              <div class="time">{{rating.rateTime | formateDate}}</div>
              <p class="text">
                <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                {{rating.text}}
              </p>
            </li>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
              暂无评价
            </div>
          </ul>
        </div>

      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue';
  import BScroll from 'better-scroll';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  import Split from 'components/split/split';
  import ratingselect from 'components/ratingselect/ratingselect';

  import {formateDate} from '../../common/js/date';

  //  const POSITIVE = 0;
  //  const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      show() {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$els.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
          ;
        });
      },
      hideDetail() {
        this.showFlag = false;
      },
      addFirstCart(event) {
        if (!event._constructed) {
          return;
        }
        ;
        this.$dispatch('cart.add', event.target);
        Vue.set(this.food, 'count', 1);
      },
      needShow(type, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return this.selectType === type;
        }
      }
    },
    filters: {
      formateDate(time) {
        let date = new Date(time);
        return formateDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    computed: {},
    events: {
      'content.toogle'(onlyContent) {
        this.onlyContent = onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      'ratingtype.select'(type) {
        this.selectType = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      }
    },
    components: {
      'v-cartcontrol': cartcontrol,
      'v-split': Split,
      'v-ratingselect': ratingselect
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .food-detail
    position fixed
    top: 0
    bottom: 48px
    width: 100%
    height: 100%
    background: #ffffff
    &.move-transition
      transition: all 0.3s linear
      opacity: 1
      transform: translate3d(0, 0, 0)
    &.move-enter, &.move-leave
      opacity: 0
      transform: translate3d(100%, 0, 0)
    .image-header
      position: relative
      width: 100%
      height: 0
      padding-top: 100%
      img
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
      .back
        position: absolute
        top: 10px
        left: 0
        .icon-arrow_lift
          display: block
          padding: 10px 0 0 12px
          font-size: 12px
          color: #ffffff
    .content
      position: relative
      padding: 18px
      .title
        line-height: 14px
        margin-bottom: 8px
        font-size: 14px
        font-weight: 700
        color: rgb(7, 17, 27)
      .desc
        margin-bottom: 18px
        height: 10px
        line-height: 10px
        font-size: 0
        .sell-count, .rating
          font-size: 10px
          height: 10px
          color: rgb(147, 153, 159)
        .sell-count
          margin-right: 10px
      .price
        font-weight: 700
        line-height: 24px
        .now
          margin-right: 8px
          font-size: 14px
          color: rgb(240, 20, 20)
        .old
          text-decoration: line-through
          font-size: 10px
          color: rgb(147, 153, 159)

      .cartcontrol-wrapper
        position: absolute
        right: 12px
        bottom: 12px
      .buy
        position: absolute
        right: 18px
        bottom: 18px
        font-size: 10px
        color: #ffffff
        background: rgb(0, 160, 220)
        padding: 0 12px
        border-radius: 12px
        box-sizing: border-box
        height: 24px
        line-height: 24px
        z-index: 10
        &.fade-transition
          transition: all 0.4s
          opacity: 1
        &.fade-enter, &.fade-leave
          opacity: 0
    .info
      padding: 18px
      .title
        margin-bottom: 6px
        height: 14px
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)
      .text
        padding: 0 12px
        line-height: 24px
        font-size: 12px
        color: rgb(77, 85, 93)
    .rating
      padding-top: 18px
      .title
        margin-left: 18px
        height: 14px
        line-height: 14px
        font-size: 14px
        color: rgb(7, 17, 27)

      .rating-wrapper
        padding: 0 18px
        .rating-item
          position: relative
          padding: 16px 0
          border-1px(rgba(7, 17, 27, 0.1))
          .user
            position: absolute
            right: 0
            top: 16px
            line-height: 12px
            font-size: 0
            .name
              display: inline-block
              vertical-align: top
              margin-right: 6px
              font-size: 10px
              color: rgb(147, 153, 159)
            .avatar
              border-radius: 50%
          .time
            margin-bottom: 6px
            line-height: 12px
            font-size: 10px
            color: rgb(147, 153, 159)
          .text
            line-height: 16px
            font-size: 12px
            color: rbg(7, 17, 27)
            .icon-thumb_up, .icon-thumb_down
              line-height: 24px
              margin-right: 4px
              font-size: 12px
            .icon-thumb_up
              color: rgb(0, 160, 220)
            .icon-thumb_down
              color: rgb(147, 153, 159)
        .no-rating
          padding: 12px 0
          font-size: 12px
          color: rgb(147, 153, 159)
</style>

