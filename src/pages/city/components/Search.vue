<template>
  <div>
    <div class="search">
      <input v-model="keyword" type="text" class="search-input" placeholder="请输入城市或拼音">
    </div>
    <div class="search-content" ref ="search" v-show ="keyword">
      <ul>
        <li 
          class="search-item border-bottom" 
          v-for="item of list" 
          :key = "item.id" 
          @click ="handleClick(item.name)"
        >
         {{item.name}}
       </li>
        <li class="search-item border-bottom" v-show="hasNoData">
          没有找到匹配数据
        </li>
      </ul>
    </div>

  </div>

</template>

<script>

import Bscroll from 'better-scroll'
import { mapMutations} from 'vuex'
export default {
  name: 'CityHeader',
  props: {
    cities: Object,
  },
  data () {
    return {
      keyword: '',
      list: [],
      timer: null
    }
  },
  computed: {
    hasNoData () {
      return !this.list.length
    }
  },
  methods: {
    handleClick (city) {
      this.changeCity(city)
      this.$router.push('/')
    },
    ...mapMutations(['changeCity'])
  },
  watch: {
    keyword () {
      if (this.timer) {
        clearTimeout(this.tiemr)
      } 
      if (!this.keyword){
        this.list = []
        return
      }
      this.timer = setTimeout(() => {
        const result = []
        for (let i in this.cities) {
          this.cities[i].forEach((value) => {
            if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword )> -1) {
              result.push(value)
            }
          })
        }
        this.list = result
      },10)
    }
  },
  mounted () {
    this.scroll = new Bscroll(this.$refs.search)
  }
}
</script>

<style lang="stylus" scoped>
  @import '~styles/varibles.styl'
  .search
    padding 0 0.1rem
    background $theThemeColor
    height .72rem
    .search-input
      width 100%
      padding 0 0.1rem
      height .62rem
      line-height .62rem
      box-sizing border-box
      color #666
      text-align center
      border-radius .1rem
  .search-content
    z-index 1
    position absolute
    overflow hidden
    background #eee
    top 1.54rem
    right 0
    left 0 
    bottom 0 
    .search-item
      background #fff
      line-height .62rem
      padding-left: .2rem
</style>
