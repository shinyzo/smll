<template>
  <div class="food-detail" v-show="showFlag" transition="move">
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image">
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false
      };
    },
    methods: {
      show() {
        this.showFlag = true;
      }
    },
    computed: {
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .food-detail
    position fixed
    top: 0
    bottom: 48px
    width: 100%
    height: 100%
    background: #ffffff
    &.move-transition
      transition :all 0.3s linear
      opacity :1
      transform :translate3d(0,0,0)
    &.move-enter,&.move-leave
      opacity :0
      transform :translate3d(100%,0,0)

</style>

