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
                        <span class="rating">好评率{{food.rating}}%</span>
                    </div>
                    <div class="price">
                        <span class="now">￥{{food.price}}</span>
                        <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                    <div class="cartcontrol-wrapper" v-show="food.count&&food.count!==0">
                        <cartcontrol :food="food"></cartcontrol>
                    </div>
                    <transition name="fade">
                        <div @tap.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
                    </transition>
                </div>
                <split v-show="food.info"></split>
                <div class="info" v-show="food.info">
                    <h1 class="title">商品介绍</h1>
                    <p class="text">{{food.info}}</p>
                </div>
                <split></split>
                <div class="rating">
                    <h1 class="title">商品评价</h1>
                    <ratingselect v-on:ratingtype:select="selectRatingType" v-on:content:toggle="toggleContent" :select-type="selectType" :only-content="onlyContent" :ratings="food.ratings" :desc="desc"></ratingselect>
                    <div class="rating-wrapper">
                        <ul v-show="food.ratings && food.ratings.length">
                            <li v-show="needShow(rating.rateType, rating.text)" v-for="rating in food.ratings" :key="rating.id" class="rating-item">
                                <div class="user">
                                    <span class="name">{{rating .username}}</span>
                                    <img  class="avatar" :src="rating.avatar" width="12" height="12">
                                </div>
                                <div class="time">{{rating.rateTime | formDate}}</div>
                                <p class="text">
                                    <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
                                </p>
                            </li>
                        </ul>
                        <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
                            暂无评价
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    import Vue from 'vue';
    import BScroll from 'better-scroll';
    import cartcontrol from '@/components/cartcontrol/cartcontrol.vue';
    import split from '@/components/split/split.vue';
    import ratingselect from '@/components/ratingselect/ratingselect.vue';
    import {formatDate} from '@/assets/js/date';
    // const POSITIVE = 0;
    // const NEGATIVE = 1;
    const ALL = 2;

    export default {
        data () {
            return {
                showFlag: false,
                selectType: ALL,
                onlyContent: false,
                desc: {
                    all: '全部',
                    positive: '推荐',
                    negative: '吐槽'
                }
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

                this.$nextTick(() => {
                    if (!this.contentScroll) {
                        this.contentScroll = new BScroll('.food', {
                            click: true,
                            tap: true
                        });
                    }
                })
            },
            hide: function () {
                this.showFlag = false;
            },
            addFirst: function () {
                Vue.set(this.food, 'count', 1);
            },
            needShow (type, text) {
                if (this.onlyContent && !text) {
                    return false;
                }
                if (this.selectType === ALL) {
                    return true;
                } else {
                    return type === this.selectType;
                }
            },
            selectRatingType (type) {
                this.selectType = type;
                this.$nextTick(() => {
                    this.contentScroll.refresh();
                });
            },
            toggleContent (onlyContent) {
                this.onlyContent = onlyContent;
                this.$nextTick(() => {
                    this.contentScroll.refresh();
                });
            }
        },
        filters: {
            formDate (time) {
                let date = new Date(time);
                return formatDate(date, 'yyyy-MM-dd hh:mm');
            }
        },
        components: {
            cartcontrol, split, ratingselect
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
    .food .content {
        padding: 18px;
        position: relative;
    }
    .food .content .title {
        margin: 0; 
        line-height: 14px;
        font-size: 14px;
        margin-bottom: 8px;
        font-weight: 700;
        color: rgb(7, 17, 27);
    }
    .food .content .detail {
        margin-bottom: 18px;
        line-height: 10px;
        height: 10px;
        font-size: 0;        
    }
    .food .detail .sell-count {
        font-size: 10px;
        color: rgb(147, 153, 159);
        margin-right: 12px;
    }
    .food .detail .rating {
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .food .content .price {
        font-weight: 700;
        line-height: 24px;
    }
    .food .content .price .now{
        margin-right: 8px;
        font-size: 14px;
        color: rgb(240, 20, 20);
    }
    .food .content .price .old{
        margin-right: 8px;
        text-decoration: line-through;
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .cartcontrol-wrapper {
        position: absolute;
        right: 16px;
        bottom: 20px;
    }
    .buy {
        position: absolute;
        right: 18px;
        bottom: 18px;
        z-index: 10;
        height: 24px;
        line-height: 24px;
        padding: 0 12px;
        box-sizing: border-box;
        font-size: 10px;
        border-radius: 12px;
        color: #fff;
        background-color: rgb(0, 160, 220);
        transition: all 0.2s;
        opacity: 1;
    }
    .fade-enter, .fade-leave-to {
        transform: rotateX(180deg);
        opacity: 0;
    }
    .food-content .info {
        padding: 18px;
    }
    .food-content .info .title {
        margin: 0 0 6px 0;
        line-height: 14px;
        font-size: 14px;
        color: rgb(7, 17, 27);
    }
    .food-content .info .text {
        line-height: 24px;
        font-size: 12px;
        padding: 0 8px;
        color: rgb(77, 85, 93);
    }
    .rating {
        padding: 0;
    }
    .rating .title {
        line-height: 14px;
        padding: 0 18px;
        margin: 0;
        font-size: 14px;
        color: rgb(7, 17, 27);
    }
    .rating-wrapper {
        padding:  0 18px;
    }
    .rating-wrapper .rating-item {
        position: relative;
        padding:  16px 0;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }
    .rating-wrapper .user {
        position: absolute;
        right: 0;
        top: 16px;
        font-size: 0;
        line-height: 12px;        
    }
    .rating-wrapper .user .name {
        font-size: 10px;
        display: inline-block;
        vertical-align: top;
        margin-right: 6px;
        color: rgb(147, 153, 159);
    }
    .rating-wrapper .user .avatar {
        border-radius: 50%;
    }
    .rating-wrapper .time {
        line-height: 12px;
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .rating-wrapper .text {
        line-height: 16px;
        font-size: 12px;        
        color: rgb(7, 17, 27);
    }
    .rating-wrapper .text .icon-thumb_up {
        display: inline-block;
        vertical-align: middle;
        width: 24px;
        height: 24px;
        background-image: url(./thumb_up.svg);
        background-size: 20px 20px;
        background-position: -2px -2px;
        background-repeat: no-repeat;
        margin-right: 4px;
    }
    .rating-wrapper .text .icon-thumb_down {
        display: inline-block;
        vertical-align: middle;
        width: 24px;
        height: 24px;
        background-image: url(./thumb_down.svg);
        background-size: 20px 20px;
        background-position: -2px -2px;
        background-repeat: no-repeat;
        margin-right: 4px;
    }
    .no-rating {
        padding: 16px 0;
        font-size: 12px;
        color: rgb(147, 153, 159);
    }
</style>

