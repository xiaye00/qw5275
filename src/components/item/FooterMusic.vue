<template>
  <div class="FooterMusic">
    <div class="footerLeft" @click="updateDetailShow">
      <img :src="playList[playListIndex].al.picUrl" alt="" />
      <div>
        <p>{{ playList[playListIndex].name }}</p>
        <span>横滑切换上下页哦</span>
      </div>
    </div>
    <div class="footerRight">
      <svg class="icon liebiao" aria-hidden="true" @click="play" v-if="isbtnShow">
        <use xlink:href="#icon-bofanganniu"></use>
      </svg>
      <svg class="icon liebiao" aria-hidden="true"  @click="play" v-else>
        <use xlink:href="#icon-weibiaoti--"></use>
      </svg>
      <svg class="icon liebiao" aria-hidden="true">
        <use xlink:href="#icon-zu"></use>
      </svg>
    </div>
    <!-- 音乐播放 -->
    <audio
      ref="audio"
      :src="`https://music.163.com/song/media/outer/url?id=${playList[playListIndex].id}.mp3`"
    ></audio>
    <van-popup v-model:show="detailShow" position="right" :style="{ height: '100%',width:'100%' }" >
      <MusicDetail  :musicList="playList[playListIndex]" 
      :play="play" 
      :isbtnShow="isbtnShow"
      :addDuration="addDuration" />
    </van-popup>
  </div>
</template>

<script>
import MusicDetail from '@/components/item/MusicDetail'
import { mapState, mapMutations } from "vuex";
export default {
  data() {
    return {
      interVal:0
    }
  },
  components:{MusicDetail},
  computed: {
    ...mapState(["playList", "playListIndex", "isbtnShow",'detailShow']),
  },
  mounted() {
    console.log(this.$refs);
    this.$store.dispatch("getLyric", this.playList[this.playListIndex].id);
    this.updateTime()
  },
    updated() {
    this.$store.dispatch("getLyric", this.playList[this.playListIndex].id);
    this.addDuration()
  },
  methods: {
      ...mapMutations(["ISBTNSHOW",'updateDetailShow','updateCurrentTime','updateDuration']),
    
    //获取当前时间
    updateTime(){
      this.interVal = setInterval(() => {
        this.updateCurrentTime(this.$refs.audio.currentTime)
      }, 1000);
    },
    //获取歌曲总时长
    addDuration:function(){
    this.updateDuration(this.$refs.audio.duration)
    },
    //音乐播放
    play() {
      // 判断音乐是否播放
      if(this.$refs.audio.paused){
          this.$refs.audio.play()
          this.ISBTNSHOW(false)
          this.updateTime()//播放 调用函数进行传值
      }else{
        this.$refs.audio.pause();
        this.ISBTNSHOW(true)
        clearInterval(this.interVal)//暂停清除定时器
      }
      console.log(1);
      
    },
  
  },
  watch:{
    playListIndex(){
      //监听如果下标发生改变,就自动播放音乐
      this.$refs.audio.autoplay = true
      if(this.$refs.audio.paused){
        //如果暂停状态,改变图标
        this.ISBTNSHOW(false)
      }
    },
        playList: function () {
      if (this.isbtnShow) {
        this.$refs.audio.autoplay = true;
        this.ISBTNSHOW(false);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.FooterMusic {
  width: 100%;
  height: 1.4rem;
  background-color: #fff;
  position: fixed;
  bottom: 0;
  border-top: 1px solid #999;
  display: flex;
  padding: 0.2rem;
  justify-content: space-between;
  .footerLeft {
    width: 60%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-content: center;
    img {
      width: 1rem;
      height: 1rem;
      border-radius: 50%;
    }
  }
  .footerRight {
    width: 20%;
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .icon {
      width: 0.6rem;
      height: 0.6rem;
    }
  }
}
</style>