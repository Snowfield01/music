<template>
  <div>
    <van-pull-refresh v-model="refreshing" @refresh="onRefresh">
      <van-list
        v-model="loading"
        :finished="finished"
        
        finished-text="没有更多了"
        @load="onLoad"
      >
        <h4 style="margin:10px">{{this.$route.params.title}}</h4>
        <router-link
          :to="'/PlayMusic/'+item.song_id"
          class="musicBox"
          v-for="item in picList"
          :key="item.id"
        >
          <img :src="item.pic_big" alt />
          <p class="line">{{item.title}}</p>
        </router-link>
      </van-list>
    </van-pull-refresh>
  </div>
</template>
<script>
export default {
  data() {
    return {
      picList: [],
      loading: false,
      finished: false,
      refreshing: false,
      offset:0
    };
  },
  created() {
    // this.getMusicList();
  },
  methods: {
    onLoad() {
      // console.log(11111111)
      // console.log(this.$route.params.type);
      // console.log(this.$route.params.title);
      const musicListUrl =
        this.HOST +
        `/v1/restserver/ting?method=baidu.ting.billboard.billList&size=10&offset=${this.offset}&type=${this.$route.params.type}` ;
      setTimeout(()=>{
        this.$axios
        .get(musicListUrl)
        .then(res => {
          console.log(res);
          this.picList = this.picList.concat(res.data.song_list);
          this.offset+=10
          this.loading = false;
        })
        .catch();
      },1000)
    },
    onRefresh() {
    
      this.finished = false;

      // 重新加载数据
      // 将 loading 设置为 true，表示处于加载状态
      this.loading = true ;
      this.onLoad();
    }
  }
};
</script>
<style scoped>
.musicBox {
  width: 50%;
  float: left;
  text-align: center;
}
.line{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>

