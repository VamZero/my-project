<template>
  <div class="hello">
    <pull-to :top-load-method="refresh" :bottom-load-method="loadMore">
      <ul v-for="item in DataList" v-bind:key="item.id">
        <li class="list" @click="GoDetail(item)">
          <el-row style="margin-top: -10px">
            <el-col :span="4" style="height: 100%"><div class="grid-content" style="margin-left: 5px; ">
              <!--<img src="../assets/llogo.png" style="position: absolute; top: 0; bottom: 0; left: 0; right: 0; width: 100%; margin: auto">-->
              <img src="../assets/llogo.png" height="60px"/>
            </div></el-col>
            <el-col :span="14"><div class="grid-content">
              <el-row>
                <el-col :span="24" style="height: 20px"><div class="grid-content" style="margin-left: 10px; text-overflow:ellipsis; white-space:nowrap; overflow:hidden; ">{{item.NewsTitle}}</div></el-col>
              </el-row>
              <el-row>
                <el-col :span="24"><div class="grid-content" style="font-size: small; margin-top: 20px;margin-left: 10px">{{item.Author}}</div></el-col>
              </el-row>
            </div></el-col>
            <el-col :span="6"><div class="grid-content">
              <el-row>
                <el-col :span="24" style="height: 20px"><div class="grid-content" style="margin-left: 10px"><i class="el-icon-arrow-right"></i></div></el-col>
              </el-row>
              <el-row>
                <el-col :span="24" style="height: 20px"><div class="grid-content" style="font-size: 10px; margin-left: 10px; margin-top: 20px">{{item.Date.substring(0,10)}}</div></el-col>
              </el-row>
            </div></el-col>
          </el-row>
        </li>
      </ul>
    </pull-to>
  </div>
</template>

<script>
import PullTo from 'vue-pull-to'

export default {
  name: 'HelloWorld',
  data () {
    return {
      DataList: [],
      SubDataList: [],
      curPage: 0
    }
  },
  components: {
    PullTo
  },
  methods: {
    getNewsList () {
      this.$http.get('http://192.168.1.248/api/News').then((response) => {
        this.DataList = response.body
        console.log(this.DataList[0])
        var refresh = false
        var curPage = this.curPage
        for (let i = curPage * 10; i < (curPage + 1) * 10; i++) {
          if (this.DataList[i] == null) {
            break
          }
          this.SubDataList[i] = this.DataList[i]
          refresh = true
        }
        if (refresh) {
          curPage++
        }
        this.curPage = curPage
        console.log(this.SubDataList)
        console.log(this.curPage)
      })
    },
    loadMore (loaded) {
      setTimeout(() => {
        var load = false
        var curPage = this.curPage
        for (let i = curPage * 10; i < (curPage + 1) * 10; i++) {
          if (this.DataList[i] == null) {
            break
          }
          this.SubDataList[i] = this.DataList[i]
          load = true
        }
        console.log(this.SubDataList)
        if (load) {
          curPage++
        }
        this.curPage = curPage
        console.log(curPage)
        loaded('done')
      }, 500)
    },
    refresh (loaded) {
      this.getNewsList()
      loaded('done')
    },
    GoDetail (item) {
      console.log(item)
      this.$router.push({
        name: 'detail',
        path: '/detail',
        params: {
          name: 'item',
          dataObj: item
        }
      })
    }
  },
  created () {
    this.getNewsList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .list{
    position:relative;
    width:100%;
    padding-top: 1px;
    padding-bottom: 1px;
    padding-left: 0;
    height:4.5em;
    /*margin-top:5%;*/
    border-bottom: 1px solid rgba(160, 160, 160, 0.3);
  }
  .el-form-item{
    height: 10px;
  }
  .news-list-img{
    margin-left: 5px;
    position: relative;
    float: left;
    height: 100%;
    width: 10%;
    display: inline;
    vertical-align: middle;
    text-align: center;
  }
  .news-list-img img{
  }
  .news-list-title{
    margin-left: 5em;
    margin-right: 5%;
  }
  .news-list-source{
    float: left;
    width: 30%;
    /*padding-left: 10px;*/
    /*margin-right: -70%;*/
    margin-left:3em;
    font-size: 12px;
    /*margin-top: 60%;*/
  }
  .news-list-date{
    padding-left: 80%;
    /*float: left;*/
    /*width: 100%;*/
    font-size: 12px;
    margin-bottom: 5px;
  }
  .news-list-date-span{
    margin-right: 0%;
  }
  .grid-content {
    max-height: 100%;
  }
</style>
