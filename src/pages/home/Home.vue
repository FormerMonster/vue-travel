<template>
    <div>
       <home-header></home-header>
       <home-swiper :list="SwiperList"></home-swiper>
       <home-icons :list ="iconList"></home-icons>
       <home-recommend :list="recommendList"></home-recommend>
       <home-weekend :list="weekendList"></home-weekend>
    </div>

</template>

<script >
import HomeHeader from './components/Header'
import HomeSwiper from './components/Swiper'
import HomeIcons from './components/Icons'
import HomeRecommend from './components/Recommend'
import HomeWeekend from './components/Weekend'
import axios from 'axios'
export default {
  name: 'Home',
  components: {
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  data () {
    return {
      SwiperList: [],
      recommendList: [],
      iconList: [],
      weekendList:[]
    }
  },
  methods: {
    getHomeInfo () {
      axios.get('/api/index.json')
      .then (this.getHomeInfoSuccess)
    },
    getHomeInfoSuccess (res) {

      res = res.data
      console.log(res)
      if (res.ret && res.data) {
        const data = res.data
        this.SwiperList = data.swiperList
        this.iconList = data.iconList
        this.recommendList = data.recommendList
        this.weekendList = data.weekendList

        
      }
    }
  },
  mounted () {
    this.getHomeInfo()
  }
  
}
</script>

<style>
 touch-action: pan-y
</style>
