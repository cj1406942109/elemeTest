<template>
    <div class="v-header">
        <!-- {{seller}} -->
        <!-- 内容区 -->
        <div class="content-wrapper">
            <div class="avatar">
                <img :src="seller.avatar">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}} / {{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="iconClassMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="support-count">
                <span class="count">{{seller.supports.length}}个</span>
                <span class="more" @click="showDetail()"></span>
            </div>
        </div>
        <!-- 公告区 -->
        <div class="bulletin-wrapper">
            <span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
            <span class="more" @click="showDetail()"></span>
        </div>
        <div class="background">
            <img :src="seller.avatar">    
        </div>
        <transition name="fade">
            <div v-show="detailShow" class="detail" transition="fade">
                <div class="detail-wrapper clearfix">
                    <div class="detail-main">
                        <h1 class="name">{{seller.name}}</h1>
                        <div class="star-wrapper">
                            <star :size="48" :score="seller.score"></star>
                        </div>
                        <div class="title">
                            <div class="line"></div>
                            <div class="text">优惠信息</div>
                            <div class="line"></div>
                        </div>
                        <ul v-if="seller.supports" class="supports">
                            <li class="support-item" v-for="item in seller.supports" :key="item.id">
                                <span class="icon" :class="iconClassMap[item.type]"></span>
                                <span class="text">{{item.description}}</span>
                            </li> 
                        </ul>
                        <div class="title">
                            <div class="line"></div>
                            <div class="text">商家公告</div>
                            <div class="line"></div>
                        </div>
                        <div class="bulletin">
                            <p class="content">{{seller.bulletin}}</p>
                        </div>
                    </div>
                </div>
                <div class="detail-close" @click="hideDetail()">
                </div>
            </div>
        </transition>
    </div>    
</template>

<script>
    import star from '@/components/star/star.vue';

    export default{
        name: 'v-header',
        data () {
            return {
                iconClassMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
                detailShow: false
            }
        },
        props: {
            seller: {}
        },
        methods: {
            showDetail: function () {
                this.detailShow = true;
            },
            hideDetail: function () {
                this.detailShow = false;
            }
        },
        components: {
            star
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .v-header{
        color: #fff;
        background-color: rgba(7, 17, 27, 0.5);
        overflow: hidden;
        position: relative;
    }
    .content-wrapper{
        padding: 24px 12px 18px 24px;
        position: relative;
        font-size: 0;
    }
    .avatar {
        display: inline-block;
        vertical-align: top;
    }
    .avatar img{
        width: 64px;
        height: 64px;
        border-radius: 2px;
    }
    .content {
        display: inline-block;
        margin-left: 16px;
    }
    .content .title {
        margin: 2px 0 8px;
    }
    .content .brand {
        width: 30px;
        height: 18px;
        display: inline-block;
        vertical-align: top;
        background-image: url('./brand@2x.png');
        background-size: 30px 18px;
    }
    .content .name {
        margin-left: 6px;
        font-size: 16px;
        line-height: 18px;
        font-weight: bold;
    }
    .content .description {
        margin-bottom: 10px;
        line-height: 12px;
        font-size: 12px;
    }
    .content .icon {
        display: inline-block;
        vertical-align: top;
        width: 12px;
        height: 12px;
        margin-right: 4px;
        background-size: 12px 12px;
        background-repeat: no-repeat;
    }
    .content .icon.decrease {
        background-image: url('./decrease_1@2x.png');
    }
    .content .icon.discount {
        background-image: url('./discount_1@2x.png');
    }
    .content .icon.special{
        background-image: url('./special_1@2x.png');
    }
    .content .icon.invoice {
        background-image: url('./invoice_1@2x.png');
    }
    .content .icon.guarantee {
        background-image: url('./guarantee_1@2x.png');
    }    
    .support .text {
        line-height: 12px;
        font-size: 12px;
    }
    .support-count {
        position: absolute;
        right: 12px;
        bottom: 16px;
        padding: 0 8px;
        height: 22px;
        font-size: 10px;
        line-height: 24px;
        border-radius: 14px;
        background-color: rgba(0, 0, 0, 0.2);
        text-align: center;
    }
    .support-count .count {
        vertical-align: top;
    }
    .support-count .more {
        display: inline-block;
        width: 10px;
        vertical-align: middle;
        height: 10px;
        background-image: url('./more.svg');
        background-size: 10px 10px;
    }
    .bulletin-wrapper {
        position: relative;
        height: 28px;
        line-height: 28px;
        padding: 0 22px 0 12px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        background-color: rgba(7, 17, 27, 0.2);
    }
    .bulletin-title {
        display: inline-block;
        width: 22px;
        height: 12px;
        vertical-align: top;
        margin-top: 9px;
        background-image: url('./bulletin@2x.png'); 
        background-size: 22px 12px;
    }
    .bulletin-text {
        margin: 0 4px;
        font-size: 10px;
        line-height: 12px;
    }
    .bulletin-wrapper .more {
        position: absolute;
        bottom: 8px;
        right: 16px;
        display: inline-block;
        width: 10px;
        height: 10px;
        background-image: url('./more.svg');
        background-size: 10px 10px;
    }
    .background {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        filter: blur(10px);
    }
    .background img {
        width: 100%;
        height: 100%;
    }
    .detail {
        position: fixed;
        top: 0;
        z-index: 100;
        width: 100%;
        height: 100%;
        overflow: auto;
        background-color: rgba(7, 17, 27, 0.8);
        -webkit-backdrop-filter: blur(10px);
    }
    .fade-enter-active, .fade-leave-active {
      transition: opacity .5s
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active in below version 2.1.8 */ {
      opacity: 0
    }
    .detail-wrapper {
        min-height: 100%;
        width: 100%;
    }
    .detail-main {
        margin-top: 64px;
        padding-bottom: 64px;
    }
    .detail-close {
        position: relative;
        width: 32px;
        height: 32px;
        margin: -64px auto 0 auto;
        clear: both;
        background-image: url('./close.svg');
        background-size: 32px 32px;
    }
    .detail-main .name {
        line-height: 16px;
        text-align: center;
        font-size: 16px;
        font-weight: 700;
    }
    .star-wrapper {
        margin-top: 18px;
        padding: 2px 0;
        text-align: center;
    }
    .detail-main .title {
        display: flex;
        width: 80%;
        margin: 30px auto 24px auto;
    }
    .detail-main .title .line {
        flex: 1;
        position: relative;
        top: -6px;
        border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    }
    .detail-main .title .text {
        padding: 0 12px;
        font-size: 14px;
        font-weight: 700;
    }
    .detail-main .supports {
        width: 80%;
        margin: 0 auto;
    }
    .detail-main .supports .support-item {
        padding:  0 12px;
        margin-bottom: 12px;
        font-size: 0;
    }
    .detail-main .support-item:last-child {
        margin-bottom: 0;
    }
    .detail-main .icon {
        display: inline-block;
        width: 16px;
        height: 16px;
        vertical-align: top;
        margin-right: 6px;
        background-size: 16px 16px;
        background-repeat: no-repeat;
    }
    .detail-main .icon.decrease {
        background-image: url('./decrease_2@2x.png');
    }
    .detail-main .icon.discount {
        background-image: url('./discount_2@2x.png');
    }
    .detail-main .icon.special{
        background-image: url('./special_2@2x.png');
    }
    .detail-main .icon.invoice {
        background-image: url('./invoice_2@2x.png');
    }
    .detail-main .icon.guarantee {
        background-image: url('./guarantee_2@2x.png');
    }
    .detail-main .text {
        font-size: 12px;
        line-height: 16px;
    }
    .detail-main .bulletin {
        width: 80%;
        margin: 0 auto;
    }
     .detail-main .bulletin .content {
         margin: 0;
         padding: 0 12px;
         font-size: 12px;
         line-height: 24px;
     }
</style>

