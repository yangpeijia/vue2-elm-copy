<template>
    <span class="icon" :class="[iconType, classMap[number]]"></span>
</template>

<script>
export default {
  name: 'icon',
  props: {
    Type: {
      type: Number
    },
    number: {
      type: Number
    }
  },
  created: function () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  computed: {
    iconType: function () {
      return 'icon-' + this.Type
    }
  }
}
</script>

<style lang="stylus" scoped>
   @import '../../common/stylus/mixin'
  .icon
    display inline-block
    background-repeat no-repeat
    vertical-align top
    &.icon-1
      margin 0 4px 2px 0
      width 12px
      height 12px
      background-size 12px 12px
      &.decrease
        bg-image('decrease_1')   
      &.discount
        bg-image('discount_1')
      &.guarantee
        bg-image('guarantee_1')
      &.invoice
        bg-image('invoice_1') 
      &.special
        bg-image('special_1')
    &.icon-2
      width 16px
      height 16px
      margin-right 6px
      background-size 16px 16px
      &.decrease
        bg-image('decrease_2') 
      &.discount
        bg-image('discount_2')
      &.guarantee
        bg-image('guarantee_2')
      &.invoice
        bg-image('invoice_2') 
      &.special
        bg-image('special_2')
    &.icon-3
      margin-right 2px
      width 12px
      height 12px
      background-size 12px 12px
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
    &.icon-4
      margin-right 6px
      width 16px
      height 16px
      background-size 16px 16px
      &.decrease
        bg-image('decrease_4')   
      &.discount
        bg-image('discount_4')
      &.guarantee
        bg-image('guarantee_4')
      &.invoice
        bg-image('invoice_4') 
      &.special
        bg-image('special_4')
</style>