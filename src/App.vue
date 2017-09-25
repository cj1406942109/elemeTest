<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <v-header :seller="seller"></v-header>
    <v-tab></v-tab>
    <transition>
      <router-view :seller="seller"></router-view>
    </transition>
    <v-footer></v-footer>
  </div>
</template>

<script>
import vHeader from '@/components/header/header.vue'
import vTab from '@/components/tab/tab.vue'
import vFooter from '@/components/footer/footer.vue'

const STATUS_OK = 0;
export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  components: {
    vHeader, vTab, vFooter
  },
  created () {
      this.$http.get('/api/seller').then(response => {
          var responseData = response.body;
          if (responseData.errno === STATUS_OK) {
                this.seller = responseData.data;
          }
      }, response => {
          console.log(response);
      })
  }
}
</script>

<style>
#app {
  font-family: 'PingFang SC', 'STHeitiSC-Light', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  /* color: #2c3e50; */
  /* margin-top: 60px; */
}
.clearfix {
  display: inline-block;  
}
.clearfix::after{
  content: '.';
  height: 0;
  line-height: 0;
  clear: both;
  visibility: hidden;
}
</style>
