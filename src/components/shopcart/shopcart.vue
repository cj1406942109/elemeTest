<template>
  <div class="shopcart">
      <div class="content">
          <div class="content-left">
              <div class="logo-wrapper">
                  <div class="logo" :class="{'highlight':totalCount>0,'normal':totalCount<=0}">
                      <span class="icon-shopcart"></span>
                  </div>
                  <div class="count" v-show="totalCount>0">
                      {{totalCount}}
                  </div>
              </div>
              <div class="price" :class="{'highlight':totalPrice>0}">
                  ￥{{totalPrice}}元
              </div>
              <div class="desc">
                  另需配送费￥{{deliveryPrice}}元
              </div>
          </div>
          <div class="content-right">
              <div class="pay" :class="payClass">
                  {{payDesc}}
              </div>
          </div>
      </div>
  </div>
</template>

<script>
    export default {
        props: {
            selectFoods: {
                type: Array,
                default () {
                    return [
                        {
                            price: 13,
                            count: 2
                        }
                    ];
                }
            },
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            }
         },
         computed: {
             totalPrice () {
                 let total = 0;
                 this.selectFoods.forEach(food => {
                    total += food.price * food.count;
                 });
                 return total;
             },
             totalCount () {
                 let count = 0;
                 this.selectFoods.forEach(food => {
                     count += food.count;
                 })
                 return count;
             },
             payDesc () {
                 if (this.totalPrice === 0) {
                     return `￥${this.minPrice}元起送`;
                 } else if (this.totalPrice < this.minPrice) {
                     let diff = this.minPrice - this.totalPrice;
                     return `还差￥${diff}元起送`;
                 } else {
                     return '去结算';
                 }
             },
             payClass () {
                 if (this.totalPrice < this.minPrice) {
                     return 'not-enough';
                 } else {
                     return 'enough';
                 }
             }
         }
    }
</script>

<style scoped>
    .shopcart {
        position: fixed;
        left: 0;
        bottom: 0;
        z-index: 50;
        width: 100%;
        height: 48px;
        background-color: #000;
    }
    .shopcart .content {
        display: flex;
        background-color: #141d27;
        font-size: 0;
        color: rgba(255, 255, 255, 0.4);
    }
    .shopcart .content .content-left {
        flex: 1;
    }
    .shopcart .content .content-left .logo-wrapper{
        display: inline-block;
        position: relative;
        top: -10px;
        margin: 0 12px;
        padding: 6px;
        width: 56px;
        height: 56px;
        box-sizing: border-box;
        vertical-align: top;
        border-radius: 509%;
        background-color: #141d27;
    }
    .shopcart .content .content-left .logo-wrapper .logo{
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-repeat: no-repeat;
        background-position: 50% 50%;
        background-size: 70% 70%;
    }
    .shopcart .content .content-left .logo-wrapper .normal{
        background-image: url('./shopcart.svg');
        background-color: #2b343c;
    }
    .shopcart .content .content-left .logo-wrapper .highlight{
        background-image: url('./shopcart_white.svg');
        background-color: #00A0DC;
    }
    .shopcart .content .content-left .price{
        display: inline-block;
        vertical-align: top;
        line-height: 24px;
        margin-top: 12px;
        font-size: 16px;
        font-weight: 700;
        box-sizing: border-box;
        padding-right: 12px;
        border-right: 1px solid rgba(255, 255, 255, 0.1);
    }
    .shopcart .content .content-left .highlight{
        color: #fff;
    }
    .shopcart .content .content-left .desc{
        display: inline-block;
        vertical-align: top;
        line-height: 24px;
        margin: 12px 0 0 12px;
        font-size: 10px;
    }
    .shopcart .content .content-right {
        flex: 0 0 105px;
        width: 105px;
    }
    .shopcart .content .content-right .pay{
        height: 48px;
        line-height: 48px;
        text-align: center;
        font-size: 12px;
        font-weight: 700;
    }
    .shopcart .content .content-right .pay.not-enough{
        background-color: #2b333b;
    }
    .shopcart .content .content-right .pay.enough{
        background-color: #00b43c;
        color: #fff;
    }
    .shopcart .content .content-left .logo-wrapper .count {
        position: absolute;
        top: 0;
        right: 0;
        width: 24px;
        height: 16px;
        line-height: 16px;
        text-align: center;
        border-radius: 16px;
        font-size: 9px;
        font-weight: 700;
        background-color: rgb(240, 20, 20);
        color: #fff;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
    }
</style>
