<template>
  <div class="cartcontrol">
      <div class="cart-decrease" v-show="food.count>0" @tap="decreaseCart">
      </div>
      <div class="cart-count" v-show="food.count>0">
          {{food.count}}
      </div>
      <div class="cart-add" @tap="addCart">

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
      console.log(this.food);
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
    }
    .cartcontrol .cart-decrease {
        display: inline-block;
        padding: 6px;
        font-size: 24px;
        line-height: 24px;
        background-image: url('./decrease.svg');
        background-position: 50% 50%;
        background-size: 16px 16px;
        background-repeat: no-repeat;
    }
    .cartcontrol .cart-count {
        display: inline-block;
        font-size: 10px;
        width: 20px;
        vertical-align: top;
        line-height: 12px;
        text-align: center;
        color:  rgb(147, 153, 159 )
    }
    .cartcontrol .cart-add {
        display: inline-block;
        padding: 6px;
        font-size: 24px;
        line-height: 24px;
        background-image: url('./add.svg');
        background-position: 50% 50%;
        background-size: 16px 16px;
        background-repeat: no-repeat;
    }
</style>

