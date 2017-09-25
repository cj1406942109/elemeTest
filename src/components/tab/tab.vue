<template>
    <div class="v-tab">
        <div class="tab-item"><router-link to="/goods">商品</router-link></div>
        <div class="tab-item"><router-link to="/ratings">评论</router-link></div>
        <div class="tab-item"><router-link to="/seller">商家</router-link></div>
    </div>    
</template>

<script>
    export default{
        name: 'v-tab',
        data () {
            return {
                msg: 'Welcome to Your Vue.js App, 我是Tab栏'
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .v-tab {
        display: flex;
        display: -webkit-flex;
        text-align: center;
        width: 100%;
        height: 40px;
        line-height: 40px;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1) ;
    }
    .v-tab .tab-item {
        flex: 1;
    }
    .tab-item a {
        display: block;
        font-family: Microsoft YaHei;
        font-size: 14px;
        color: rgb(77, 85, 93);
        cursor: pointer;
    }
    .tab-item .router-link-active {
        color: rgb(240, 20, 20);
    }
</style>