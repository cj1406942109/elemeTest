<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
          <li v-for="(good,index) in goods" :key="good.id" class="menu-item" :class="{'current':currentIndex===index}" @tap="selectMenu(index)">
              <span class="text">
                <span v-show="good.type>0" class="icon" :class="iconClassMap[good.type]"></span>{{good.name}}
              </span>
          </li>
      </ul>
    </div>
    <div class="foods-wrapper">
        <ul>
            <li v-for="good in goods" :key="good.id" class="food-list food-list-hook">
                <h1 class="title">{{good.name}}</h1>
                <ul>
                    <li v-for="food in good.foods" :key="food.id" class="food-item">
                        <div class="icon"><img :src="food.icon"></div>
                        <div class="content">
                            <h2 class="name">{{food.name}}</h2>
                            <p class="desc">{{food.description}}</p>
                            <div class="extra">
                                <span class="count">月售{{food.sellCount}}份</span>
                                <span>好评率{{food.rating}}%</span>
                            </div>
                            <div class="price">
                                <span class="now">￥{{food.price}}</span>
                                <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                        </div>
                    </li>
                </ul>
            </li>
        </ul>
    </div>
    <shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script>
    import BScroll from 'better-scroll';
    import shopcart from '@/components/shopcart/shopcart.vue';
    import cartcontrol from '@/components/cartcontrol/cartcontrol.vue';

    const STATUS_OK = 0;
    export default{
        'name': 'goods',
        props: {
            seller: {
            }
        },
        data () {
            return {
                goods: [],
                iconClassMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
                listHeight: [],
                scrollY: 0
            };
        },
        computed: {
            currentIndex () {
                for (let i = 0; i < this.listHeight.length; i++) {
                    let height1 = this.listHeight[i];
                    let height2 = this.listHeight[i + 1];
                    if (!height2 || this.scrollY >= height1 && this.scrollY < height2) {
                        return i;
                    }
                }
            }
        },
        created () {
            this.$http.get('/api/goods').then(response => {
                var responseData = response.body;
                if (responseData.errno === STATUS_OK) {
                     this.goods = responseData.data;
                     this.$nextTick(() => {
                         this._initScroll();
                         this._calculateHeight();
                     });
                }
            }, response => {
                console.log(response);
            })
        },
        methods: {
            _initScroll: function () {
                this.menuScroll = new BScroll('.menu-wrapper', {
                    click: true,
                    tap: true
                });
                this.foodsScroll = new BScroll('.foods-wrapper', {
                    probeType: 3,
                    click: true,
                    tap: true
                });
                this.foodsScroll.on('scroll', (pos) => {
                    this.scrollY = Math.abs(Math.round(pos.y));
                })
            },
            _calculateHeight: function () {
                let foodList = this.$el.getElementsByClassName('food-list-hook');
                let height = 0;
                this.listHeight.push(height);
                for (let i = 0; i < foodList.length; i++) {
                    let item = foodList[i];
                    height += item.clientHeight;
                    this.listHeight.push(height);
                }
            },
            selectMenu: function (index) {
                let foodList = this.$el.getElementsByClassName('food-list-hook');
                let el = foodList[index];
                this.foodsScroll.scrollToElement(el, 300);
                console.log(index);
            }
        },
        components: {
            shopcart, cartcontrol
        }
    }
</script>

<style scoped>
    .goods {
        position: absolute;
        display: flex;
        overflow: hidden;
        top: 174px;
        bottom: 46px;
        width: 100%;
    }
    .menu-wrapper {
        flex: 0 0 80px;
        width: 80px;
        background-color: #f3f5f7;
    }
    .menu-wrapper .menu-item {
        display: table;
        height: 54px;
        width: 56px;
        line-height: 14px;
        font-size: 12px;
        padding: 0 12px;
    }
    .menu-wrapper .menu-item.current {
        position: relative;
        z-index: 10;
        margin-top: -1px;
        background-color: #fff;
        font-weight: 700;
    }
    .menu-wrapper .menu-item.current .text {
        border-bottom:none;
    }
    .menu-wrapper .menu-item .icon {
        display: inline-block;
        vertical-align: top;
        width: 12px;
        height: 12px;
        margin-right: 4px;
        background-size: 12px 12px;
        background-repeat: no-repeat;
    }
    .menu-item .icon.decrease {
        background-image: url('./decrease_3@2x.png');
    }
    .menu-item .icon.discount {
        background-image: url('./discount_3@2x.png');
    }
    .menu-item .icon.special{
        background-image: url('./special_3@2x.png');
    }
    .menu-item .icon.invoice {
        background-image: url('./invoice_3@2x.png');
    }
    .menu-item .icon.guarantee {
        background-image: url('./guarantee_3@2x.png');
    }
    .menu-item .text {
        display: table-cell;
        width: 56px;
        vertical-align: middle;
        font-size: 12px;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }
    .menu-item:last-child .text {
        border-bottom: none;
    }
    
    .foods-wrapper {
        flex: 1;
    }
    .foods-wrapper .title {
        padding-left: 14px;
        height: 26px;
        line-height: 26px;
        border-left: 2px solid #d9dde1;
        font-size: 12px;
        color: rgb(147, 153, 159);
        background-color: #f3f5f7;
    }
    .foods-wrapper .food-item {
        display: flex;
        margin: 18px;
        padding-bottom: 18px;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }
    .foods-wrapper .food-item:last-child {
        border-bottom: none;
        margin-bottom: 0;
    }
    .food-item .icon {
        flex: 0 0 57px;
        margin-right: 10px;
    }
    .food-item .icon img {
        width: 57px;
    }
    .food-item .content {
        position: relative;
        flex: 1;
    }
    .food-item .content .name {
        margin: 2px 0 8px 0;
        height: 14px;
        line-height: 14px;
        font-size: 14px;
        color: rgb(7, 17, 27);
    }
    .food-item .content .desc {
        margin-bottom: 8px;
        line-height: 12px;
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .food-item .content .extra {
        line-height: 10px;
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .food-item .content .extra .count{
        margin-right: 4px;
    }
    .food-item .content .price {
        font-weight: 700;
        line-height: 24px;
    }
    .food-item .content .price .now{
        margin-right: 8px;
        font-size: 14px;
        color: rgb(240, 20, 20);
    }
    .food-item .content .price .old{
        margin-right: 8px;
        text-decoration: line-through;
        font-size: 10px;
        color: rgb(147, 153, 159);
    }
    .foods-wrapper .cartcontrol-wrapper {
        position: absolute;
        right: 0;
        bottom: 12px;
    }
</style>

