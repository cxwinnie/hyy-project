<template lang="html">
  <div class="cartcontrol" :style="{'flex-direction': (food.count==0 ? 'row-reverse':'row')}">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
          <image :src="removeImg" style="width:100%;height:100%;"></image>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">
      {{food.count}}
    </div>
    <div class="cart-add" @click.stop.prevent="addCart($event)">
        <image :src="addImg" style="width:100%;height:100%;"></image>
    </div>
  </div>
</template>


<script>
var globalDefine = require('../globalDefine');

export default {
  props: {
    food: { // 购物车按钮的信息由父组件传入
      type: Object
    },
  },
  data(){
    return {
      addImg:globalDefine.apiUrl.resUrl + 'add_circle.png',
      removeImg:globalDefine.apiUrl.resUrl + 'remove.png'
    }
  },
  created() {
    // console.log(this.food)
  },
  methods: {
    addCart(event) {
      // if (!event._constructed) {
      //   return // 因为better-scroll在PC端不阻止默认事件，所以需要过滤
      // }
      if (!this.food.count) {
        // Vue.set(this.food, `count`, 1) // Vue.set添加一个原来不存在的属性
        this.$set(this.food, 'count', 1)
        /* $set是Vue.set的别名，添加原本没有的属性，需要用$set方法
        触发视图的响应，文档见"https://cn.vuejs.org/v2/api/#vm-set",
        $set 方法作用和 Vue.set 一样，但可以不需要 import 引入 Vue */
      } else {
        this.food.count++
      }
      this.$emit('cartAdd', event.target) // $emit派发的事件只能在当前组件监听,父组件不能监听到到
    },
    decreaseCart(event) {
      // if (!event._constructed) {
      //   return // 因为better-scroll在PC端不阻止默认事件，所以需要过滤
      // }
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>

<style lang="scss">
.cartcontrol {
    width: 100px;
    margin-right: 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .cart-decrease {
        width: 24px;
        height: 24px;
        transition: translate3d(0,0,0);
        .inner {
            display: inline-block; // 动画的元素要有宽高，inline元素不能动画
            line-height: 24px;
            font-size: 24px;
            color: rgb(0,160,220);
            transition: all 0.4s linear; // 渐变效果在原生DOM中也要加transition
        }
        &.move-enter-active,
        &.move-leave-active {
            // 进入、离开的过程
            transition: all 0.4s linear;
        }
        &.move-enter,
        &.move-leave-to {
            // 进入的初始，离开的最终态
            opacity: 0;
            transform: translate3d(24px,0,0);
            .inner {
                transform: rotate(180deg);
            }
        }
    }
    .cart-count {
        width: 24px;
        padding-top: 6px;
        line-height: 24px;
        text-align: center;
        font-size: 24px;
        color: rgb(147,153,159);
    }
    .cart-add {
        // background: #000000;
        width: 24px;
        height: 24px;
        color: rgb(0,160,220);
        img{
          width: 100%;
          height: 100%;
        }
    }

}
</style>
