<template>
  <div class="ratings">
      <div class="ratings-content">
          <div class="overview">
              <div class="overview-left">
                  <h1 class="score">{{seller.score}}</h1>
                  <div class="title">综合评分</div>
                  <div class="rank">高于周边商家{{seller.rankRate}}%</div>
              </div>
              <div class="overview-right">
                  <div class="score-wrapper">
                      <span class="title">服务态度</span>
                      <star :size="36" :score="seller.serviceScore"></star>
                      <span class="score">{{seller.serviceScore}}</span>
                  </div>
                  <div class="score-wrapper">
                      <span class="title">商品评分</span>
                      <star :size="36" :score="seller.foodScore"></star>
                      <span class="score">{{seller.foodScore}}</span>
                  </div>
                  <div class="delivery-wrapper">
                      <span class="title">送达时间</span>
                      <span class="delivery">{{seller.deliveryTime}}分钟</span>
                  </div>
              </div>
          </div>
          <split></split>
          <ratingselect v-on:ratingtype:select="selectRatingType" v-on:content:toggle="toggleContent" :select-type="selectType" :only-content="onlyContent" :ratings="ratings" :desc="desc"></ratingselect>
          <div class="rating-wrapper">
              <ul>
                  <li v-show="needShow(rating.rateType, rating.text)" v-for="rating in ratings" :key="rating.id" class="rating-item">
                      <div class="avatar">
                          <img :src="rating.avatar" width="28" height="28 ">
                      </div>
                      <div class="content">
                          <h1 class="name">{{rating.username}}</h1>
                          <div class="star-wrapper">
                              <star :size="24" :score="rating.score"></star>
                              <span class="delivery" v-show="rating.deliveryTime"></span>
                          </div>
                          <p class="text">{{rating.text}}</p>
                          <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                              <span class="icon-thumb_up"></span>
                              <span v-for="item in rating.recommend" :key="item.id" class="recommend-item">{{item}}
                              </span>
                          </div>
                          <div class="time">
                              {{rating.rateTime | formDate}}
                          </div>
                      </div>
                  </li>
              </ul>
          </div>
      </div>
  </div>
</template>

<script>
    import BScroll from 'better-scroll';
    import star from '@/components/star/star.vue';
    import split from '@/components/split/split.vue';
    import ratingselect from '@/components/ratingselect/ratingselect.vue';
    import {formatDate} from '@/assets/js/date';
    
     const ALL = 2;
     const STATUS_OK = 0;

    export default{
        data () {
            return {
                selectType: ALL,
                onlyContent: false,
                desc: {
                    all: '全部',
                    positive: '推荐',
                    negative: '吐槽'
                },
                ratings: []
            };
        },
        created () {
            this.$http.get('/api/ratings').then(response => {
                var responseData = response.body;
                if (responseData.errno === STATUS_OK) {
                     this.ratings = responseData.data;
                     this.$nextTick(() => {
                        if (!this.contentScroll) {
                            this.contentScroll = new BScroll('.ratings', {
                                click: true,
                                tap: true
                            });
                        }
                     })
                }
            }, response => {
                console.log(response);
            })
        },
        props: {
            seller: {
                type: Object
            }
        },
        methods: {
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
            star, ratingselect, split
        }
    }
</script>

<style scoped>
    .ratings {
        position: absolute;
        top: 174px;
        bottom: 0;
        width: 100%;
        overflow: hidden;
    }
    .overview {
        display: flex;
        padding: 18px 0;
    }
    .overview .overview-left {
        padding: 6px 0;
        flex: 0 0 137px;
        width: 137px;
        border-right: 1px solid rgba(7, 17, 27, 0.1);
        text-align: center;
    }
    .overview-left .score {
        margin: 0;
        line-height: 28px;
        font-size: 24px;
        margin-bottom: 6px;
        color: rgb(255, 153, 0);
    }
    .overview-left .title {
        margin-bottom: 8px;
        line-height: 12px;
        font-size: 12px;
        color: rgb(7, 17, 27);
    }
    .overview-left .rank {
        line-height: 10px;
        padding-bottom: 6px;
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .overview .overview-right {
        flex: 1;
        padding: 6px 0 6px 24px;
    }
    .score-wrapper {
        margin-bottom: 8px;
        font-size: 0;
    }
    .score-wrapper .title {
        display: inline-block;
        vertical-align: top;
        line-height: 18px;
        font-size: 12px;
        color: rgb(7, 17, 27);
    }
    .score-wrapper .star {
        vertical-align: top;
        display: inline-block;
        margin: 0 12px;
    }
    .score-wrapper .score {        
        vertical-align: top;
        display: inline-block;
        line-height: 18px;
        font-size: 12px;
        color: rgb(255, 153, 0); 
    }    
    .delivery-wrapper {
        font-size: 0;
    }
    .delivery-wrapper .title {        
        line-height: 18px;
        font-size: 12px;
        color: rgb(7, 17, 27);
    }
    .delivery-wrapper .delivery {
        line-height: 18px;
        font-size: 12px;
        color: rgb(147, 153, 159);
        margin-left: 12px;
    }

    @media screen and (max-width: 320px) {
        .overview {
            display: flex;
            padding: 18px 0;
        }
        .overview .overview-left {
            flex: 0 0 120px;
            width: 120px;
        }
        .overview .overview-right {
            padding: 6px 0 6px 4px; 
        }
    }
    .rating-wrapper {
        padding:  0 18px;
    }
    .rating-item {
        display: flex;
        padding: 18px 0;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }
    .rating-item .avatar {
        flex: 0 0 28px;
        width: 28px;
        border-radius: 50%; 
        margin-right: 12px;
    }
    .rating-item .avatar img{
        border-radius: 50%; 
    }
    .rating-item .content {
        position: relative;
        flex: 1;
    }
    .rating-item .content .name {
        line-height: 12px;
        margin-bottom: 4px;
        font-size: 10px;
        color: rgb(7, 17, 27);
    }
    .rating-item .content .star-wrapper {
        margin-bottom: 6px;
        font-size: 0;
        
    }
    .rating-item .content .star-wrapper .star{
         display: inline-block;
         margin-right: 6px;
         vertical-align: top;
    }
    .rating-item .content .star-wrapper .delivery{
        display: inline-block;
        vertical-align: top;
        color: rgb(147, 153, 159);
    }
    .rating-item .content .text {
        line-height: 18px;
        font-size: 12px;
        margin-bottom: 8px;
    }
    .rating-item .content .recommend {
        line-height: 16px;
        font-size: 0;
    }
    .rating-item .content .recommend .icon-thumb_up {
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
    .rating-item .content .recommend .recommend-item{
        font-size: 9px;
        padding: 0 6px;
        margin: 0 8px 4px 0;
        color: rgb(147, 153, 159);
        border: 1px solid rgba(7, 17, 27, 0.1);
        border-radius: 1px; 
    }
    .time {
        position: absolute;
        top: 0;
        right: 0;
        line-height: 12px;
        font-size: 12px;
    }
</style>
