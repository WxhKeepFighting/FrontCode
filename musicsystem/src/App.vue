<template>
  <!-- bootstrap响应式布局grid -->
  <div id="app" class="container">
    <h1>音乐管理系统</h1>
    <div class="form-group">
      <center>
        <table class="table table-striped mt-5">
          <thead>
            <th>ID</th>
            <th>歌曲名称</th>
            <th>歌曲作者</th>
            <th>status</th>
            <th>发行日期</th>
            <th>文件</th>
            <th>操作</th>
          </thead>
          <tbody>
            <!-- v-bind 根据id变化来更新 -->
            <tr v-for="music in musicList" :key="music" v-bind="music.id">
              <td>{{music.id}}</td>
              <td>{{music.musicname}}</td>
              <td>{{music.musicauthor}}</td>
              <td>{{music.status}}</td>
              <td>{{music.release_date}}</td>
              <td>{{music.file}}</td>
              <td>
                <button
                  @click="setEditForm(music.id,music.musicname,music.musicauthor,music.musictype,music.release_date)"
                  class="btn btn-primary mr-4"
                >更新</button>
                <button @click="remove(music.id)" class="btn btn-danger">删除</button>
              </td>
            </tr>
          </tbody>
        </table>
      </center>
      <div v-if="AddForm">

        <div class="form-group row">
        <label for="author" class="col-form-label col-sm-1"><span style="color: red">*</span>ID:</label>
        <div class="col-sm-7">
        <input type="text" class="form-control" id="id" placeholder="请输入歌曲ID" v-model="mymusic.id" required />
        </div>
        <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.id != ''"
          >
              {{errors.id}}
          </div>
        </div>

        <div class="form-group row">
            <!--  col-form-label -->
        <label for="name" class="col-form-label col-sm-1"><span style="color: red">*</span>名称:</label>
        <div class="col-sm-7">
            <input
            class="form-control"
            type="text"
            id="name"
            placeholder="请输入歌曲名称"
            v-model="mymusic.musicname"
          />
          </div>
           <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.musicname != ''"
          >
              {{errors.musicname}}
          </div>
          </div>
          <div class="form-group row">
          <label for="author" class="col-sm-1 col-form-label"><span style="color: red">*</span>作者:</label>
          <div class="col-sm-7">
          <input type="text" class="form-control" id="author" placeholder="请输入歌曲作者" v-model="mymusic.musicauthor" required />
          </div>
           <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.musicauthor != ''"
          >
              {{errors.musicauthor}}
          </div>
          </div>
        
          <div class="form-group row">
          <label for="type" class="col-sm-1 col-form-label"><span style="color: red">*</span>status:</label>
          <div class="col-sm-7">
          <input type="text" class="form-control" id="type" placeholder="status" v-model="mymusic.status" />
          </div>
          <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.status != ''"
          >
              {{errors.status}}
          </div>
          </div> 

          <div class="form-group row">
          <label for="date" class="col-sm-1 col-form-label">
              <span style="color: red">*</span>日期:
          </label>
          <div class="col-sm-7">
          <input type="date" class="form-control" id="date" v-model="mymusic.release_date" />
          </div>
          <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.release_date != ''"
          >
              {{errors.release_date}}
          </div>
          </div>

          <div class="form-group row">
          <label for="description" class="col-sm-1 col-form-label"><span style="color: red">*</span>描述:</label>
          <div class="col-sm-7">
          <textarea type="text" rows="10" v-model="mymusic.description" placeholder="歌曲描述" class="form-control"  id="description" />
          </div>
          <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.description != ''"
          >
              {{errors.description}}
          </div>
          </div>

          <!-- <div class="custom-file">
              <input type="file" id="file" class="customFile col-sm-2" >
              <label for="file" class="custom-file-label col-sm-7">选择文件</label>
          </div> -->

          <div class="form-group row">
          <label for="file" class="col-sm-1 col-form-label"><span style="color: red">*</span>附件:</label>
          <div class="col-sm-7">
          <input type="file" placeholder="歌曲描述" ref="inputFile" class="form-control"  id="file" />
          </div>
          <!-- <div
            class="alert alert-danger col-sm-4"
            role="alert"
            style="padding: 6px 0 0 10px; margin: 0;"
            v-if="errors.description != ''"
          >
              {{errors.description}}
          </div> -->
          </div>

          <button @click="onSubmit()" class="btn btn-primary">添加</button>
        </div>
      
      <div v-if="EditForm">
          <div class="form-group row">    
          <label for="name" class="col-sm-2"><span style="color: red">*</span>歌曲名称:</label>
          <input
            class="col-sm-7 form-control"
            type="text"
            id="name"
            placeholder="请输入歌曲名称"
            v-model="mymusic.musicname"
          />
          </div>
          <div class="form-group row">
          <label for="author" class="col-sm-2"><span style="color: red">*</span>歌曲作者:</label>
          <input type="text" class="col-sm-7 form-control" id="author" placeholder="请输入歌曲作者" v-model="mymusic.musicauthor" required />
          </div>

          <div class="form-group row">
          <label for="type" class="col-sm-2"><span style="color: red">*</span>歌曲类型:</label>
          <input type="text" class="col-sm-7 form-control" id="type" placeholder="请输入歌曲类型" v-model="mymusic.musictype" />
          </div> 

          <div class="form-group row">
          <label for="date" class="col-sm-2"><span style="color: red">*</span>发行日期:</label>
          <input type="date" class="col-sm-7 form-control" id="date" v-model="mymusic.release_date" />
          </div>

          <button
            @click="updateById(mymusic.id,mymusic.musicname,mymusic.musicauthor,mymusic.musictype,mymusic.release_date)"
            class="btn btn-primary"
          >更新</button>
      </div>
    </div>
        <div class="form-group row">
        <label for="select_name" class="col-sm-2"><span style="color: red">*</span>查询歌曲类型:</label>
        <div class="col-sm-7 ">
        <input type="text" placeholder="请输入歌曲类型" class="form-control" id="select_type" v-model="searchtype" />
        </div>
        <button @click="findByType()"  class="btn btn-primary">查询</button>
        </div>
        
        <br />
        <br />
        <button @click="getmusic()" class="btn btn-primary">查询所有</button>
        <br />
        <button @click="AddForm = true;EditForm = false" class="btn btn-primary" style="margin: 20px 0 0 0">添加歌曲</button>
        <!-- <form id="upload" enctype="multipart/form-data" method="post">
        <el-upload action=""
                    :http-request="uploadSectionFile">
                    <el-button size="small" type="primary">点击上传</el-button>
        </el-upload>
        <el-progress v-show="showProgress" :text-inside="true" :stroke-width="18"
                            :percentage="uploadPercent"></el-progress>

        </form> -->
                <!-- <label for="select_id">删除歌曲的id</label><input type="text" placeholder="请输入歌曲id" id="select_id" class="form-control" v-model="mymusic.id"><br>
        <button @click="deleteById()" class="btn btn-primary" >删除</button>-->
    </div>
    <!-- <span>传入的json数组的长度为:{{musics.length}}</span>  -->
</template>

<script>
import Axios from "axios";
//也是json格式
export default {
  name: "App", //将vue命名为App
  //页面加载之前就执行的操作
  created() {
    this.getmusic();
  },
  data() {
    return {
      EditForm: false,
      AddForm: false,
      musicList: [],
      //用来定义错误类型，方便输出
      errors:{
          id:"",
          musicname:"",
          musicauthor:"",
          status:"",
          release_date:"",
          description:""
      },
      mymusic: {
        id: 0,
        musicname: "",
        musicauthor: "",
        status: "",
        release_date: "",
        description:""
      },
      searchtype: ""
    };
  },
  methods: {
    getmusic() {
      Axios.get("http://localhost:8080/musics").then(
        response => (this.musicList = response.data)
      ); //这里传入的是一个json数组
    },
    onSubmit() {
      //字段验证
    //   if(body.id == ''||body.id<=0){
    //       this.errors = "";
    //   }
    let body = new FormData();
    body.append("id",this.mymusic.id);
    body.append("musicname",this.mymusic.musicname);
    body.append("musicauthor",this.mymusic.musicauthor);
    body.append("status",this.mymusic.status);
    body.append("release_date",this.mymusic.release_date);
    body.append("file",this.$refs.inputFile.files[0]);

    let header = {
        "Content-Type": "application/form-data"
    };

      Axios.post("http://localhost:8080/musics", body, header).then(
        // this.getmusic,
        response => 
        this.musicList.push({
            id: this.mymusic.id,
            musicname: this.mymusic.musicname,
            musicauthor: this.mymusic.musicauthor,
            status: this.mymusic.status,
            release_date: this.mymusic.release_date,
            description: this.mymusic.description,
            file: response.data
        })
        //   this.musicList.push(body),
      ).catch(
          (error) => {
              let messages = error.response.data;
              window.console.log(messages);
            //   写成一个通用的错误信息提取的库
              messages.forEach(element => {
                  if(element.field == "id") {
                      this.errors.id = element.defaultMessage;
                  }
                  if(element.field == "musicname"){
                      this.errors.musicname = element.defaultMessage;
                  }
              });
          }
      );
        // this.AddForm = false;
    },
    findByType() {
      Axios.get("http://localhost:8080/musics/type/" + this.searchtype).then(
        response => ((this.musicList = response.data), (this.searchtype = ""))
      );
    },
    remove(id) {
      Axios.delete(`http://localhost:8080/musics/${id}`).then(
        this.getmusic
        //   this.musicList = this.musicList.filter()
      );
    },
    updateById(id, name, author, type, date) {
      let body = {
        id: id,
        musicname: name,
        musicauthor: author,
        musictype: type,
        release_date: date
      };
      Axios.put(`http://localhost:8080/musics/${id}`, body).then(
        this.getmusic,
        (this.EditForm = false)
      );
      this.mymusic.id = "";
      this.mymusic.musicname = "";
      this.mymusic.musicauthor = "";
      this.mymusic.musictype = "";
      this.mymusic.release_date = "";
    },
    setEditForm(id, musicname, musicauthor, musictype, release_date) {
      this.EditForm = !this.EditForm;
      if(this.AddForm){
        this.AddForm = !this.AddForm;
      }
      this.mymusic.id = id;
      this.mymusic.musicname = musicname;
      this.mymusic.musicauthor = musicauthor;
      this.mymusic.musictype = musictype;
      this.mymusic.release_date = release_date;
    },
    uploadSectionFile(param){
        let form = new FormData();
        form.append('file', param.file);
        form.append('dir', 'temp1');
        Axios.post('http://localhost/upload',form,{
            headers:{
                'Content-Type':'multipart/form-data'
            },
            onUploadProgress:(ProgressEvent.loaded/ProgressEvent.total * 100)|0
        }).then(()=>{
            window.console.log('上传结束')
        }).catch(()=>{
            window.console.log('上传错误')
        })
    }
    // fileUpload(data) {
    //   this.file = data;
    //   let formData = new FormData();
    //   formData.append(
    //     "userfile",
    //     this.file
    //   );
    //   let config = {
    //       headers:{
    //           'Content-Type': 'multipart/form-data'
    //       }
    //   }
    //   Axios.post('',formData,config).then();
    // }
  }
};
</script>

<style>
#app table {
  margin-top: 80px;
  /* margin: auto; */
}

label{
    font-size: 3ex;
    padding:5px 0 0 0;
}
</style>
