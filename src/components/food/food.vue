<template>
  <transition name="move">
    <scroll class="food" v-if="showFlag" ref="scroll" :probeType="3" :data="[this.selectType,this.onlyContent]">
      <div class="food-content">
          <div class="image-header">
              <img :src="food.image" alt="食物图片" class="food-img">
              <div class="back" @click="hide">
                 <i class="icon-arrow_lift">返回</i>
              </div> 
          </div>
          <div class="content">
              <h1 class="title">{{food.name}}</h1>
              <div class="detail">
                  <span class="sell-count">月售{{food.sellCount}}份</span>
                  <span class="rating">好评率{{food.rating}}%</span>
              </div>
              <price :food="food"></price>
              <div class="cartcontrol-wrapper">
                 <cartcontrol :food="food" ref='cartcontrol' v-on:addCart='addCart()'></cartcontrol>   
              </div>
              <transition name="fade">
                <div class="buy" v-show="!food.count" @click="addFirst">加入购物车</div>
              </transition>
          </div>
          <split v-show="food.info"></split>
          <div class="info" v-show="food.info">
            <header class="title">商品信息</header>
            <p class="text">{{food.info}}</p>
          </div>
          <split></split>
          <div class="rating">
            <header class="title">商品评价</header>
            <ratingselect :desc='desc' :ratings='food.ratings' v-on:selectALL="show" v-on:selectPositive="show" v-on:selectNegative="show" v-on:toggleContent="toggle"></ratingselect>
            <div class="rating-wrapper">
              <ul v-if='food.ratings && food.ratings.length'>
                <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" :key="rating" class="rating-item border-1px">
                  <div class="user">
                    <span class="name">{{rating.username}}</span>
                    <img class="avatar" width="12" height="12" :src='rating.avatar' alt="头像">
                  </div>
                  <div class="time">{{rating.rateTime | formatDate}}</div>
                  <p class="text">
                    <span :class="{'icon-thumb_up': rating.rateType === 0,'icon-thumb_down': rating.rateType === 1}"></span>{{rating.text}}
                  </p>
                </li>
              </ul>
              <div class="no-rating" v-if='!food.ratings||!food.ratings.length'>暂无评价</div>
            </div>
          </div>
      </div>
    </scroll>
  </transition>
</template>

<script>
import price from '../price/price'
import scroll from '../BScroll/better-scroll.vue'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
import {formatDate} from '../../common/js/date'

// const POSITIVE = 0
// const NEGATIVE = 1
const ALL = 2
export default {
  name: 'food',
  props: {
    food: {
      type: Object
    }
  },
  data: function () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  methods: {
    show (type = ALL) {
      this.showFlag = true
      this.selectType = type
    },
    toggle () {
      this.onlyContent = !this.onlyContent
    },
    hide () {
      this.showFlag = false
    },
    addFirst () {
      if (!this.$refs.scroll.isConstructed()) {
        return
      }
      this.$refs.cartcontrol.addCart()
    },
    addCart () {
      this.$emit('addCart', event.target)
    },
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  components: {
    price,
    scroll,
    cartcontrol,
    split,
    ratingselect
  }
}
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin'
    .move-enter-active,.move-leave-active
      transition all .2s linear
    .move-enter,.move-leave-to
      transform translate3d(100%,0,0)
    .food
      position fixed
      left 0
      top 0
      bottom 48px
      z-index 25
      width 100%
      background #fff
      overflow hidden
      .image-header
        position relative
        width 100%
        height 0
        padding-top 100%
        .food-img
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
            display block
            padding 10px
            font-size 18px
            font-weight 700
            color #787e83
      .content
        padding 18px
        position relative
        // border-1px(rgba(7,17,27,0.1))
        .title
          line-height 14px
          margin-bottom 8px
          font-size 14px
          font-weight 700
          color rgb(7,17,27)
        .detail
          margin-bottom 18px
          line-height 10px
          height 10px
          font-size 0
          .sell-count,.rating
            font-size 10px
            color rgb(147,153,159)
          .sell-count
            margin-right 12px
        .cartcontrol-wrapper
          position absolute
          right 12px
          bottom 12px
        .fade-leave-to,.fade-enter
          opacity 0
        .fade-leave-active,.fade-enter-active
          transition all .3s
        .fade-leave
          opacity 1
        .buy
          position absolute
          right 18px
          bottom 18px
          z-index 10
          height 24px
          line-height 12px
          padding 6px 12px
          box-sizing border-box
          border-radius 12px
          font-size 10px
          color #fff
          background rgb(0,160,220)
      .info
        padding 18px
        border-1px(rgba(7,17,27,0.1))
        .title
          line-height 14px
          margin-bottom 6px 
          font-size 14px
          color rgb(7,17,27)
        .text
          line-height 24px
          padding 0 8px
          font-size 12px
          color: rgb(77,85,93)
      .rating
        padding-top 18px
        .title
          line-height 14px
          margin-left 6px 
          font-size 14px
          color rgb(7,17,27)
        .rating-wrapper
          padding 0 18px
          .rating-item
            position relative
            padding 16px 0
            border-1px(rgba(7,17,27,0.1))
            .user
              position absolute
              right 0
              top 16px
              line-height 12px
              font-size 0
              .name
                display inline-block
                vertical-align top
                font-size 10px
                color rgb(147,153,159)
                margin-right 6px
              .avatar
                border-radius 50%
            .time
              margin-bottom 6px
              line-height 12px
              font-size 10px
              color rgb(147,153,159)
            .text
              line-height 16px
              font-size 12px
              color rgb(7,17,27)
              .icon-thumb_up,.icon-thumb_down
                margin-right 4px
                line-height 16px
                font-size 12px
              .icon-thumb_up
                color rgb(0,160,220)
              .icon-thumb_down
                color rgb(147,153,159)
        .no-rating
          padding 16px 0
          font-size 12px
          color rgb(147,153,159)
</style>