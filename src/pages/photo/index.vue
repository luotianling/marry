<template>
    <div class="photo">
        <h-swiper :list="list" :isGif="isGif"></h-swiper>
    </div>
</template>

<script>
import HSwiper from '@/components/swiper'
export default {
  name:'photo',
  components:{
    HSwiper
  },
  data(){
      return{
          list:[],
          isGif:false
      }
  },
  onShow(){
      const that = this
      that.isGif = !that.isGif
      that.getList()
  },
  methods:{
      getList(){
          const that = this
          const db = wx.cloud.database()
          const banner = db.collection('banner')
          banner.get().then(res=>{
              that.list = res.data[1].photo
          })
      }
  }
}
</script>

<style lang="stylus" scoped>
.photo
    height 100%
</style>