<template>
    <!--搜索  -->
    <div class="searchTop">
    <svg class="icon" aria-hidden="true" @click="$router.go(-1)">
      <use xlink:href="#icon-zuojiantou"></use>
    </svg>
    <input type="text"  
    placeholder="霜序十二" 
    v-model="searchKey" 
    @keydown.enter="enterKey" />
  </div>
  <!-- 历史搜索 -->
    <div class="searchHistory">
    <span class="searchSpan">历史</span>
    <span v-for="(item,index) in keyWorldList" :key="index"  
    @click="searchHistory(item)"
    class="spanKey">
    {{ item }}
    </span>
    <svg class="icon" aria-hidden="true" @click="delHistory">
      <use xlink:href="#icon-shanchu"></use>
    </svg>
  </div>
  <!-- 列表 -->
       <div class="itemList">
      <div class="item" v-for="(item, i) in searchList" :key="i">
        <div class="itemLeft" @click="updateIndex(item)">
          <span class="leftSpan">{{ i + 1 }}</span>
          <div>
            <p>{{ item.name }}</p>
            <span v-for="(item1, index) in item.artists" :key="index">{{
              item1.name
            }}</span>
          </div>
        </div>
        <div class="itemRight">
          <svg class="icon bofang" aria-hidden="true" v-if='item.mvid !=0'>
            <use xlink:href="#icon-shipin"></use>
          </svg>
          <svg class="icon liebiao" aria-hidden="true">
            <use xlink:href="#icon-31liebiao"></use>
          </svg>
        </div>
      </div>
    </div>
</template>

<script>
import { getSearchMusic } from "@/request/api/home";
import { mapMutations } from "vuex";
export default {
data() {
    return {
      keyWorldList: [],
      searchKey: "",
      searchList: [],
    }
},
mounted() {
    this.keyWorldList = JSON.parse(localStorage.getItem('keyWorldList'))
    ? JSON.parse(localStorage.getItem("keyWorldList"))
    : [];
},
methods: {
    ...mapMutations(['pushPlayList','updatePlayListIndex']),
    async enterKey(){
        if(this.searchKey !==""){
            this.keyWorldList.unshift(this.searchKey)
            //去重
            this.keyWorldList = [...new Set(this.keyWorldList)]
            //固定长度
            if(this.keyWorldList.length>10){
                this.keyWorldList.splice(this.keyWorldList.length-1,1)
            }
            //本地存储
            localStorage.setItem("keyWorldList", JSON.stringify(this.keyWorldList));
            let res = await getSearchMusic(this.searchKey)
            // console.log(res) 
            this.searchList = res.data.result.songs
            this.searchKey=''
        }
    },
    //删除
    delHistory() {
      localStorage.removeItem("keyWorldList");
      this.keyWorldList = [];
    },
    //历史搜索
    async searchHistory(item){
        let res = await getSearchMusic(item)
        console.log(res) 
        this.searchList = res.data.result.songs;
    },
    updateIndex(item){
        item.al=item.album
        item.al.picUrl=item.album.artist.img1v1Url
        // this.$store.commit("pushPlayList",item)
        // this.$store.commit("updatePlayListIndex",this.$store.state.playList.length-1)
        //  this.updatePlayList(this.songs)
        //  this.updatePlayListIndex(index)
        this.pushPlayList(item)
        this.updatePlayListIndex(this.$store.state.playList.length-1)
    }
    
},
}
</script>

<style lang="less" scoped>
.searchTop {
  width: 100%;
  height: 1rem;
  padding: 0 0.2rem;
  display: flex;
  align-items: center;
  input {
    margin-left: 0.2rem;
    border: none;
    border-bottom: 1px solid #999;
    width: 90%;
    padding: 0.1rem;
  }
}
.searchHistory {
  width: 100%;
  padding: 0.2rem;
  position: relative;
  .searchSpan {
    font-weight: 700;
  }
  .spanKey {
    padding: 0.1rem 0.2rem;
    background-color: rgb(185, 169, 169);
    border-radius: 0.4rem;
    margin: 0.1rem 0.2rem;
    display: inline-block;
  }
  .icon {
    width: 0.3rem;
    height: 0.3rem;
    position: absolute;
    right: 0.2rem;
  }
}
.itemList {
    width: 100%;
    padding: .2rem;
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
</style>