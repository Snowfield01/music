<template>
  <div>
    <form action="/">
      <van-search
        v-model="value"
        show-action
        placeholder="请输入歌名、歌手、专辑"
        @search="onSearch"
        @cancel="onCancel"
      />
    </form>
    <div>
      <ul>
        <li v-for="item in resultList" :key="item.id">
          <router-link :to="'/PlayMusic/'+item.songid">
            <span style="font-size:14px">歌名：{{item.songname}}</span>
            <span style="font-size:12px">作者：{{item.artistname}}</span>
          </router-link>
        </li>
      </ul>

      <div>
        <p>搜索记录</p> 
        <span v-for="item in historyList" :key="item.id" style="display: inline-block;width:30px">{{item.person}}</span>
        
      </div>
      <div @click="clear">清空历史记录</div>
    </div>
  </div>
</template>
<script>
import { Toast } from "vant";
export default {
  data() {
    return {
      value: "",
      resultList: [],
      historyList: []
    };
  },
  created() {
    // this.onSearch()
    // localStorage.clear()
    this.historyList = JSON.parse(localStorage.getItem("history") || "[]");
  },
  methods: {
    clear(){
      localStorage.clear()
      this.historyList = []
      console.log(111)
    },
    onSearch(val) {
      const searchListUrl =
        this.HOST +
        "/v1/restserver/ting?method=baidu.ting.search.catalogSug&query=" +
        this.value;
      this.$axios
        .get(searchListUrl)
        .then(result => {
          // console.log(result);
          this.resultList = result.data.song;
        })
        .catch();

      //将历史记录存到localStorage里

      var con = { id: Date.now(), person: this.value };
      // console.log(con);
      // var list = JSON.parse(localStorage.getItem("history") || "[]");
      // console.log(list);
      // list.push(con);
      this.historyList.push(con)
      // localStorage.setItem("history", JSON.stringify(list));
      localStorage.setItem("history", JSON.stringify(this.historyList));
      
     /*  this.historyList = list;
      console.log(this.historyList) */
      // Toast(val);
    },
    onCancel() {
      Toast("取消");
    }

    //搜索功能实现
  }
};
</script>
<style scoped>
.a{
  display: inline-block;
}
</style>