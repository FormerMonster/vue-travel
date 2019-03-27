<template>
  <div>
    <detail-banner 
     :gallaryImgs="gallaryImgs" 
     :bannerImg="bannerImg" 
     :sightName="sightName"
    >
      
    </detail-banner>

    <detail-header></detail-header>
    <div class="content">
       <detail-list :list="list"></detail-list>
    </div>
  </div>
  
</template>

<script>
import DetailBanner from './components/Banner'
import DetailHeader from './components/Header'
import DetailList from './components/List'
import axios from 'axios'
export default {
  name: 'Detail',
  components: {
    DetailBanner,
    DetailHeader,
    DetailList
  },
  data () {
    return {
      sightName:'',
      bannerImg: '',
      gallaryImgs: [],
      list: []
    }
  },
  methods: {
    getDetailInfo () {
      axios.get('/api/detail.json', {
        params: {
          id: this.$route.params.id
        }
      }).then(this.handleGetDataSucc)
    },
    handleGetDataSucc (res) {
      const dict = res.data
      if (dict.ret && dict.data) {
          this.list = dict.data.categoryList
          this.sightName = dict.data.sightName
          this.bannerImg = dict.data.bannerImg
          this.gallaryImgs = dict.data.gallaryImgs
      }
    }
  },
  mounted () {
    this.getDetailInfo()
  }
}
</script>

<style lang = "stylus" scoped="">
  .content 
    height: 50rem
</style>
