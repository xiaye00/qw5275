<template>
  <div class="itemMusicList"> 
            <div class="itemListTop">
      <div class="listLeft">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-bofanganniu"></use>
        </svg>
        <span
          >播放全部<span>(共{{ songs.length }}首)</span></span>
      </div>
      <div class="listRight">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-jiahao"></use>
        </svg>
        <span>收藏({{ subscribedCount }})</span>
      </div>
    </div>
    <div  class="itemList">
        <div class="item" v-for="(item,index) in songs" :key="index" >
            <div class="itemLeft" @click="playMusic(index)">
                <span class="leftSpan">{{index+1}}</span>
                <div>
                    <p>{{item.name}}</p>
                    <span v-for="(item1,i) in item.ar" :key="i" >
                        {{item1.name}}
                    </span>
                </div>
            </div>
            <!--  -->
        <div class="itemRight">
          <svg class="icon bofang" aria-hidden="true" v-if='item.mv !=0'>
            <use xlink:href="#icon-shipin"></use>
          </svg>
          <svg class="icon liebiao" aria-hidden="true">
            <use xlink:href="#icon-31liebiao"></use>
          </svg>
        </div>
        </div>
    </div>
  </div>

</template>

<script>
import { mapMutations } from "vuex";
export default {
    setup(props) {
         console.log(props,'子组件'); 

         
    },
     props: ["songs",'subscribedCount'],
     methods: {
       ...mapMutations(['updatePlayList','updatePlayListIndex']),
       playMusic(index){
         this.updatePlayList(this.songs)
         this.updatePlayListIndex(index)

       }
     },
}
</script>

<style lang="less" scoped>
.itemMusicList {
  width: 100%;
  height: 10rem;
  background-color: #fff;
  padding: 0 0.2rem;
  margin-top: 0.2rem;
  border-top-left-radius: 0.4rem;
  border-top-right-radius: 0.4rem;
  .itemListTop {
    width: 100%;
    height: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .listLeft {
      width: 3rem;
      height: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      .icon {
        stroke: #333333;
        stroke-width: 20;
      }
      span {
        font-weight: 700;
        span {
          font-weight: 400;
          font-size: 0.24rem;
          color: #999;
        }
      }
    }
    .listRight {
      display: flex;
      align-items: center;
      background-color: red;
      padding: 0.2rem;
      border-radius: 0.4rem;
      color: #fff;
      .icon {
        width: 0.3rem;
        height: 0.3rem;
        fill: #fff;
        margin-right: 0.1rem;
        stroke: #fff;
        stroke-width: 50;
      }
    }
  }
  .itemList {
    width: 100%;
    .item {
      width: 100%;
      height: 1.4rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      .itemLeft {
        width: 85%;
        height: 100%;
        display: flex;
        align-items: center;
        .leftSpan {
          display: inline-block;
          width: 0.2rem;
          text-align: center;
        }
        div {
          p {
            width: 4.54rem;
            height: .4rem;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            font-weight: 700;
          }
          span{
            font-weight: 400;
            font-size: .24rem;
            color: #999;
          }
          margin-left: 0.3rem;
        }
      }
      .itemRight{
        width: 20%;
        height: 100%;
        display: flex;
        // justify-content: space-between;
        align-items: center;
        position: relative;
        .icon{
          fill: #999;
        }
         .bofang{
            position: absolute;
            left: 0;
          }
         .liebiao{
            position: absolute;
            right: 0;
          }
      }
    }
  }
}
</style>