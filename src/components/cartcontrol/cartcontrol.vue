<template>
  <div class="cartcontrol">
      <transition name="move">
          <div class="cart-decrease" v-show="food.count>0" @tap.stop.prevent="decreaseCart"></div>
      </transition>
      <transition name="rotate">
          <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      </transition>
      <div class="cart-add" @tap.stop.prevent="addCart">
      </div>
  </div>
</template>

<script>
import Vue from 'vue';

export default {
  props: {
      food: {
          type: Object
      }
  },
  created () {
    //   console.log(this.food);
  },
  methods: {
      addCart: function () {
          if (!this.food.count) {
              Vue.set(this.food, 'count', 1);
          } else {
              this.food.count ++;
          }
      },
      decreaseCart: function () {
          this.food.count --;
      }
  }
}
</script>

<style scoped>
    .cartcontrol {
        font-size: 0;
        height: 24px;
    }
    .cartcontrol .cart-decrease {
        display: inline-block;
        /* padding: 6px; */
        width: 24px;
        height: 24px;
        font-size: 24px;
        line-height: 24px;
        background-image: url('./decrease.svg');
        background-position: 50% 50%;
        background-size: 24px 24px;
        background-repeat: no-repeat;
        transition: all .3s;
    }
    .cartcontrol .cart-count {
        display: inline-block;
        font-size: 14px;
        width: 20px;
        vertical-align: top;
        line-height: 24px;
        text-align: center;
        color:  rgb(147, 153, 159 );
        transition: all .3s;
    }
    .cartcontrol .cart-add {
        display: inline-block;
        /* padding: 6px; */
        width: 24px;
        height: 24px;
        font-size: 24px;
        line-height: 24px;
        background-image: url('./add.svg');
        background-position: 50% 50%;
        background-size: 24px 24px;
        background-repeat: no-repeat;
    }

    .move-enter-active, .move-leave-active {
        transform: translate3d(0, 0, 0) rotateZ(0);
        opacity: 1;
    }
    .move-enter, .move-leave-to {
        opacity: 0;
        transform: translate3d(24px, 0, 0) rotateZ(360deg);
    }
    .rotate-enter-active, .rotate-leave-active {
        transform: translate3d(0, 0, 0) rotateZ(0);
        opacity: 1;
    }
    .rotate-enter, .rotate-leave-to {
        opacity: 0;
        transform: translate3d(12px, 0, 0) rotateZ(360deg);
    }
</style>

