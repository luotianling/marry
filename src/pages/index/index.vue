<template>
  <div class="index">
    <div class="bg-swiper">
      <index-swiper :list="list"></index-swiper>
    </div>
    <div class="bg_music" v-if="isPlay" @tap="audioPlay">
      <image class="musicImg music_icon" src="../../static/images/music_icon.png" alt="" />
      <image class="music_play pauseImg" src="../../static/images/music_play.png" alt="" />
    </div>
    <div class="bg_music" v-else @tap="audioPlay">
      <image class="musicImg" src="../../static/images/music_icon.png" alt="" />
      <image class="music_play playImg" src="../../static/images/music_play.png" alt="" />
    </div>
    <div class="info">
      <div class="content">
        <h1>Mr.韩 & Miss.李</h1>
        <p>谨定于 2019年2月8日 （星期五）下午17:00</p>
        <p>农历 正月初四 下午五点 举办婚礼</p>
        <p>席设：玉山县冰溪镇下徐村</p>
        <p>地址：玉山县冰溪镇下徐村下坞亭142号</p>
        <image class="img_footer" src="../../static/images/we.png" alt="" />
      </div>
    </div>
    <audio id="myAudio" :src="audioUrl" autoplay loop></audio>
  </div>
</template>

<script>
import IndexSwiper from '@/components/indexSwiper'
import tools from '@/common/js/h_tools'
export default {
  name:'Index',
  components: {
    IndexSwiper
  },
  data () {
    return {
      list:[],
      isPlay: true,
      audioCtx: '',
      audioUrl: ''
    }
  },
  onShow () {
    const that = this
    that.getList()
    that.audioCtx = wx.createAudioContext('myAudio')
    that.isPlay = true
    that.getMusicUrl()
  },
  methods: {
    getList(){
      const that = this
      const db = wx.cloud.database()
      const banner = db.collection('banner')
      banner.get().then(res =>{
         that.list = res.data[0].indexBanner
      })
    },
    audioPlay () {
      const that = this
      if (that.isPlay) {
        that.audioCtx.pause()
        that.isPlay = false
        tools.showToast('您已暂停音乐播放~')
      } else {
        that.audioCtx.play()
        that.isPlay = true
        tools.showToast('背景音乐已开启~')
      }
    },
    getMusicUrl () {
      const that = this
      const db = wx.cloud.database()
      const music = db.collection('music')
      music.get().then(res => {
        that.audioUrl  = res.data[0].music
        that.audioCtx.play()
      })
    }
  },
  onShareAppMessage: function (res) {
    return {
      path: '/pages/index/main'
    }
  }
}
</script>

<style scoped lang="stylus">
@-webkit-keyframes musicRotate
  from
    -webkit-transformb rotate(0deg)
  to
    -webkit-transform rotate(360deg)
@-webkit-keyframes musicStop
  from
    -webkit-transform rotate(20deg)
  to
    -webkit-transform rotate(0deg)
@-webkit-keyframes musicStart
  from
    -webkit-transform rotate(0deg)
  to
    -webkit-transform rotate(20deg)
@-webkit-keyframes infoAnimation
  0%
    -webkit-transform scale(1) translate(0, 0)
  50%
    -webkit-transform scale(.9) translate(5px, 5px)
  100%
    -webkit-transform scale(1) translate(0, 0)
.index
  height 100%
  position relative
  .img
    width 100%
    height 100%
  .bg-swiper
    width 100%
    height 100%
  // .inv
  //   position absolute
  //   top 20rpx
  //   left 89rpx
  //   width 572rpx
  //   height 69rpx
  //   z-index 9
  .bg_music
    position fixed
    right 0
    top 20rpx
    width 100rpx
    z-index 99
    display flex
    justify-content flex-start
    align-items flex-start
    .musicImg
      width 60rpx
      height 60rpx
    .music_icon
      animation musicRotate 3s linear infinite
    .music_play
      width 28rpx
      height 60rpx
      margin-left -10rpx
      transform-origin top 
      -webkit-transform rotate(20deg)
    .playImg
      animation musicStop 1s linear forwards
    .pauseImg
      animation musicStart 1s linear forwards 
  .info
    width 630rpx
    background rgba(255,255,255,0.75)
    z-index 9
    position fixed
    bottom 40rpx
    left 50rpx
    padding 10rpx
    animation infoAnimation 12s linear infinite 
    .content
      width:626rpx
      border 1rpx solid #000
      display flex
      flex-direction column
      justify-content flex-start
      align-items center
      position relative
      padding-bottom 30rpx
      h1 
        font-size 50rpx
        height 100rpx
        line-height 100rpx
      p 
        font-size 24rpx
        height 60rpx
        line-height 60rpx
      .img_footer 
        position absolute 
        bottom 0
        left 53rpx
        height 14rpx
        width 520rpx 
        z-index 99
</style>
