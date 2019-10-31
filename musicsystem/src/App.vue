<template>
  <div id="app">
    <div class="from_bos">
        <!-- form表单默认有个提交到当前页操作 -->
        <div class="form-group">
            <label for="name" class="col-sm-2 control-label">歌曲名称:</label><input type="text" id="name" placeholder="请输入歌曲名称" class="form-control" v-model="mymusic.musicname">
            <label for="author" class="col-sm-2 control-label">歌曲作者:</label><input type="text" placeholder="请输入歌曲作者" id="author" class="form-control" v-model="mymusic.musicauthor">
            <label for="type" class="col-sm-2 control-label">歌曲类型:</label><input type="text" placeholder="请输入歌曲类型" id="type" class="form-control" v-model="mymusic.musictype">
            <label for="date" class="col-sm-2 control-label">发行日期:</label><input type="date" id="date" class="form-control" v-model="mymusic.release_date">
        </div>
        <button @click="onSubmit()" class="btn btn-primary">提交</button>
    </div>
    <div>
        <label for="select_name">查询歌曲名称</label><input type="text" placeholder="请输入歌名" id="select_name" class="form-control" v-model="mymusic.musicname">
        <button @click="findByType()" class="btn btn-primary">查询</button>
    </div>
    <!-- <span>传入的json数组的长度为:{{musics.length}}</span>  -->
    <table class="table table-striped">
      <thead>
          <th>歌曲名称</th>
          <th>歌曲作者</th>
          <th>音乐类型</th>
          <th>发行日期</th>
      </thead>
      <tbody>
          <!-- <tr>
              <td>{{musics[0].musicname}}</td>
              <td>{{musics[0].musicauthor}}</td>
              <td>{{musics[0].musictype}}</td>
              <td>{{musics[0].data}}</td>
          </tr> -->
          <!-- v-bind 根据id变化来更新 -->
          <tr v-for="music in musicList" :key = "music" v-bind="music.id">
              <td>{{music.musicname}}</td>
              <td>{{music.musicauthor}}</td>
              <td>{{music.musictype}}</td>
              <td>{{music.relase_date}}</td>
              <td></td>
          </tr>
      </tbody>
  </table>
  </div>
</template>

<script>

import Axios from "axios";
export default {
    //页面加载之前就执行的操作
  created(){
       this.getmuseic();
  },
  data(){
      return{
          count:20,
          musicList:[],
          mymusic:{
              "id":0,
              "musicname":"",
              "musicauthor":"",
              "musictype":"",
              "release_date":""
          },
      };
  },
  methods:{
      getmuseic(){
          Axios.get("http://localhost:8080/musics")
          .then(response => (this.musicList = response.data));//这里传入的是一个json数组
      },
      onSubmit(){
          Axios.post("http://localhost:8080/musics",[{
              musicname: this.mymusic.musicname,
              musicauthor: this.mymusic.musicauthor,
              musictype: this.mymusic.musictype,
              release_date: this.mymusic.release_date,
          }])
          .then(this.musicList.push({
              musicname: this.mymusic.musicname,
              musicauthor: this.mymusic.musicauthor,
              musictype: this.mymusic.musictype,
              release_date: this.mymusic.release_date,
          }));
      },
      findByType(){
          Axios.get("http://localhost:8080/musics/type/"+this.mymusic.musicname)
          .then(function(){
            //   
              response => 
              (this.musicList = [],
              this.musicList = response.data);
            //   this.getmuseic();
              });
      }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2976c4;
  margin-top: 60px;
}

#app table {
    margin-top: 50px;
}

#app input{
    width: 70%;
    margin-left: 50px;
    margin-right: 0%;
}

</style>
