<template>
  <div id="app">
    <div class="from_bos">
        <center>
    <table class="table table-striped" >
      <thead>
          <th>id</th>
          <th>歌曲名称</th>
          <th>歌曲作者</th>
          <th>音乐类型</th>
          <th>发行日期</th>
          <th>操作</th>
      </thead>
      <tbody>
          <!-- v-bind 根据id变化来更新 -->
          <tr v-for="music in musicList" :key = "music" v-bind="music.id">
              <td>{{music.id}}</td>
              <td>{{music.musicname}}</td>
              <td>{{music.musicauthor}}</td>
              <td>{{music.musictype}}</td>
              <td>{{music.release_date}}</td>
              <td>
              <button @click="remove(music.id)">删除</button>
              <button @click="setEditForm(music.id,music.musicname,music.musicauthor,music.musictype,music.release_date)">更新</button>
              </td>
          </tr>
      </tbody>
  </table>
  </center>

        <!-- form表单默认有个提交到当前页操作 -->
        <!-- <div class="form-group">
            <center>
            <label for="name" class="col-sm-2 control-label">歌曲名称:</label><input type="text" id="name" placeholder="请输入歌曲名称" class="form-control" v-model="mymusic.musicname">
            <label for="author" class="col-sm-2 control-label">歌曲作者:</label><input type="text" placeholder="请输入歌曲作者" id="author" class="form-control" v-model="mymusic.musicauthor">
            <label for="type" class="col-sm-2 control-label">歌曲类型:</label><input type="text" placeholder="请输入歌曲类型" id="type" class="form-control" v-model="mymusic.musictype">
            <label for="date" class="col-sm-2 control-label">发行日期:</label><input type="date" id="date" class="form-control" v-model="mymusic.release_date">
            </center>
        </div> -->

        <div v-if="AddForm">
            <center>
            <label for="name" >歌曲名称:</label><input type="text" id="name" placeholder="请输入歌曲名称"  v-model="mymusic.musicname"><br>
            <label for="author" >歌曲作者:</label><input type="text" id="author" placeholder="请输入歌曲作者"  v-model="mymusic.musicauthor"><br>
            <label for="type" >歌曲类型:</label><input type="text" id="type" placeholder="请输入歌曲类型"  v-model="mymusic.musictype"><br>
            <label for="date" >发行日期:</label><input type="date" id="date"  v-model="mymusic.release_date"><br>
            <button @click="onSubmit()">添加</button>
            </center>
        </div>
        <div v-if="EditForm">
            <center>
            <label for="name" >歌曲名称:</label><input type="text" id="name" placeholder="请输入歌曲名称"  v-model="mymusic.musicname"><br>
            <label for="author" >歌曲作者:</label><input type="text" placeholder="请输入歌曲作者" id="author"  v-model="mymusic.musicauthor"><br>
            <label for="type" >歌曲类型:</label><input type="text" placeholder="请输入歌曲类型" id="type"  v-model="mymusic.musictype"><br>
            <label for="date" >发行日期:</label><input type="date" id="date"  v-model="mymusic.release_date"><br>
            <button @click="updateById(mymusic.id,mymusic.musicname,mymusic.musicauthor,mymusic.musictype,mymusic.release_date)">更新</button>
            </center>
        </div>
        
    </div>
    <div>
        <center>
        <label for="select_name">查询歌曲类型</label><input type="text" placeholder="请输入歌曲类型" id="select_type"  v-model="mymusic.musictype"><br>
        <button @click="findByType()" >查询</button><br><br>
        <button @click="getmusic()" >查询所有</button><br>
        <!-- <label for="select_id">删除歌曲的id</label><input type="text" placeholder="请输入歌曲id" id="select_id" class="form-control" v-model="mymusic.id"><br>
        <button @click="deleteById()" class="btn btn-primary" >删除</button> -->
        </center>
    </div>
    <!-- <span>传入的json数组的长度为:{{musics.length}}</span>  -->
  </div>
</template>

<script>

import Axios from "axios";
//也是json格式
export default {
    //页面加载之前就执行的操作
  created(){
       this.getmusic();
  },
  data(){
      return{
          EditForm:false,
          AddForm:true,
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
      getmusic(){
          Axios.get("http://localhost:8080/musics")
          .then(response => (this.musicList = response.data));//这里传入的是一个json数组
      },
      onSubmit(){
          //传入一个json数据
          let body = {
              musicname: this.mymusic.musicname,
              musicauthor: this.mymusic.musicauthor,
              musictype: this.mymusic.musictype,
              release_date: this.mymusic.release_date
          };
          Axios.post("http://localhost:8080/musics",body)
          .then(this.musicList.push(body));
      },
      findByType(){
          Axios.get("http://localhost:8080/musics/type/"+this.mymusic.musictype)
          .then(response => (this.musicList = response.data));
      },
      remove(id){
          Axios.delete(`http://localhost:8080/musics/${id}`).then(
              this.getmusic
            //   this.musicList = this.musicList.filter()
          )
      },
      updateById(id,name,author,type,date){
          let body = {
              id: id,
              musicname: name,
              musicauthor: author,
              musictype: type,
              release_date: date
          };
          Axios.put(`http://localhost:8080/musics/${id}`,body).then(
              this.getmusic,
              this.EditForm = false
          )
      },
      setEditForm(id,musicname,musicauthor,musictype,release_date){
          this.EditForm = false;
          this.AddForm = !this.AddForm;
          this.EditForm = !this.EditForm;
          this.mymusic.id = id;
          this.mymusic.musicname = musicname;
          this.mymusic.musicauthor = musicauthor;
          this.mymusic.musictype = musictype;
          this.mymusic.release_date = release_date;
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
    width: 70%;
    /* margin: auto; */
}

#app input{
    width: 30%;
    margin-left: 50px;
    margin-right: 0%;
}

</style>
