<template>
  <div>
    <city-header ></city-header>
    <city-search :cities ="cities"></city-search>
    <city-list :hot="hotCities" :cities ="cities" :letter="letter"></city-list>
    <city-alphabet :cities ="cities" @change = "letterChange"></city-alphabet>
  </div>
  
</template>

<script>
import axios from 'axios'
import CityHeader from './components/Header'
import CitySearch from './components/Search'
import CityList from './components/List'
import CityAlphabet from './components/Alphabet'
export default {
  name: 'City',
  components: {
    CityHeader,
    CitySearch,
    CityList,
    CityAlphabet
  },
  data () {
   return {
    cities: {},
    hotCities: [],
    letter: ''
   }
  },
  methods: {
    getCityInfo () {
      axios.get('/api/city.json')
      .then (this.getCityInfoSuccess)
    },
    getCityInfoSuccess (res) {
      res = res.data
      console.log(res.data)
      if (res.ret && res.data) {
        const dict = res.data
        this.cities = dict.cities
        this.hotCities = dict.hotCities
      }
    },
    letterChange (letter) {
      this.letter = letter
    }
  },
  mounted () {
    this.getCityInfo()
  }
}
</script>

<style lang="stylus" scoped>
  
</style>
