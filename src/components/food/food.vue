<template>
    <transition name="move">
        <div class="food" v-show="showFlag">
            <div class="food-content">
                <div class="image-header">
                    <img :src="food.image">
                    <div class="back-icon" @click="hide"></div>
                </div>
                <div class="content">
                    <h1 class="title">{{food.name}}</h1>
                    <div class="detail">
                        <span class="sell-count">月售{{food.sellCount}}份</span>
                        <span class="rating">好评率{{food.rating}}</span>
                    </div>
                    <div class="price">
                        <span class="now">￥{{food.price}}</span>
                        <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        data () {
            return {
                showFlag: false
            };
        },
        props: {
            food: {
                type: Object
            }
        },
        methods: {
            show: function () {
                this.showFlag = true;
            },
            hide: function () {
                this.showFlag = false;
            }
        }
    }
</script>

<style scoped>
    .food {
        position: fixed;
        left: 0;
        top: 0;
        bottom: 48px;
        z-index: 30;
        width: 100%;
        background-color: #fff;
        transition: all 0.2s linear;
        transform: translate3d(0, 0, 0)
    }
    .food .image-header {
        position: relative;
        width: 100%;
        height: 0;
        padding-top: 100%;
    }
    .food .image-header img{
        position: absolute;
        top: 0;
        left: 0;  
        width: 100%;
        height: 100%;
    }
    .back-icon {
        position: absolute;
        top: 10px;
        width: 24px;
        height: 24px;
        padding-left: 20px;
        left: 0;
        background-image: url(./back.svg);
        background-position: 50% 50%;
        background-size: 24px 24px;
        background-repeat: no-repeat;
    }
    .move-enter, .move-leave-to {
        transform: translate3d(100%, 0, 0);
    }
</style>

