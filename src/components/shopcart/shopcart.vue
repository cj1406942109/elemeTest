<template>
  <div>
        <div class="shopcart">
        <div class="content" @click="toggleList">
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
                <div class="pay" :class="payClass" @click.stop.prevent="pay">
                    {{payDesc}}
                </div>
            </div>
        </div>
        <!-- <div class="ball-container">          
            <transition-group name="drop" tag="div">
                <div v-for="ball in balls" v-bind:key="ball.toString()" class="ball" v-show="ball.show">            
                </div>
                <div class="inner"></div>
            </transition-group>
        </div> -->
        <transition name="fold">
                <div class="shopcart-list" v-show="listShow">                
                    <div class="list-header">
                        <h1 class="title">购物车</h1>
                        <span class="empty" @click="empty">清空</span>
                    </div>
                    <div class="list-content">
                        <ul>
                            <li class="food" v-for="food in selectFoods" :key="food.id">
                                <span class="name">{{food.name}}</span>
                                <div class="price">
                                    <span>￥{{food.price*food.count}}元</span>
                                </div>
                                <div  class="cartcontrol-wrapper">
                                    <cartcontrol :food="food"></cartcontrol>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
        </transition>
        </div>
        <transition name="fade">      
            <div class="list-mask" v-show="listShow" @click="hideList">
            </div>
        </transition>
  </div>
</template>

<script>
    import BScroll from 'better-scroll';
    import cartcontrol from '@/components/cartcontrol/cartcontrol.vue';
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
         data () {
             return {
                 balls: [
                     {show: false},
                     {show: false},
                     {show: false},
                     {show: false},
                     {show: false}
                 ],
                 fold: true
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
             },
             listShow () {
                 if (!this.totalCount) {
                     this.fold = true;
                     return false;
                 }

                 let show = !this.fold;
                 if (show) {
                     this.$nextTick(() => {
                         if (!this.foldScroll) {
                            this.foldScroll = new BScroll('.list-content', {
                                click: true,
                                tap: true
                            });
                         } else {
                             this.foldScroll.refresh();
                         }
                     })
                 }
                 return show;
             }
         },
         methods: {
             drop: function (el) {
                 console.log(el);
             },
             toggleList: function () {
                 if (!this.totalCount) {
                     return;
                 }
                 this.fold = !this.fold;
             },
             empty: function () {
                 this.selectFoods.forEach(food => {
                     food.count = 0;
                 })
             },
             hideList: function () {
                 this.fold = true;
             },
             pay: function () {
                 if (this.totalPrice < this.minPrice) {
                     return;
                 }
                 window.alert(`支付${this.totalPrice}元`);
             }
         },
         components: {
             cartcontrol
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
    .ball-container {

    }
    .ball-container .ball {
        position: fixed;
        left: 32px;
        bottom: 22px;
        z-index: 200;        
    }
    .drop-enter-active, .drop-leave-active {
        opacity: 0.5
    }
    .drop-enter, .drop-leave-to {
        opacity: 0
    }
    .ball-container .inner {
        width: 16px;
        height: 16px;
        border-radius: 50%;
        background-color: #00A0DC;
        transition: all 0.4s;
    }
    .shopcart-list {
        position: absolute;
        top: 0;
        left: 0;
        z-index: -1;
        width: 100%;
        transition: all 0.5s;
        transform: translate3d(0, -100%, 0);
    }
    .fold-enter-active, .fold-leave-active {
        /* transform: translate3d(0, -100%, 0); */
    }
    .fold-enter, .fold-leave-to {
        transform: translate3d(0, 0, 0);
    }
    .shopcart-list .list-header {
        height: 40px;
        line-height: 40px;
        padding: 0 18px;
        background-color: #f3f5f7;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }
    .shopcart-list .title {
        float: left;
        margin: 0;
        font-size: 14px;
        color: rgb(7, 17, 27);
    }
    .shopcart-list .empty {
        float: right;
        font-size: 12px;
        color: rgb(0, 160, 220);
    }
    .shopcart-list .list-content {
        padding: 0 18px;
        max-height: 217px;
        width: 100%;
        background-color: #fff;
        overflow: hidden;
    }
    .list-content .food {
        position: relative;
        padding:  12px 0;
        box-sizing: border-box;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }
    .list-content .food:last-child {
        border-bottom: none;
    }
    .list-content .food .name {
        line-height: 24px;
        font-size: 14px;
        color: rgb(7, 17, 27);
    }
    .list-content .food .price {
        position: absolute;
        right: 120px;
        bottom: 12px;
        line-height: 24px;
        font-size: 14px;
        font-weight: 700;
        color: rgb(240, 20, 20);
    }
    .cartcontrol-wrapper {
        position: absolute;
        right: 35px;
        bottom: 12px;
    }
    .list-mask {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 40;
        -webkit-backdrop-filter: blur(10px);
        transition: all 0.5s;
        opacity: 1;
        background-color: rgba(7, 17, 27, 0.6);
    }
    .fade-enter-active, .fade-leave-active {
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
        background-color: rgba(7, 17, 27, 0);
    }
</style>
