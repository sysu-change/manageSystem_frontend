<template>
<div>
  <h1 class="title">管理员系统</h1>
  <br/>

<div class="box">
  <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column type="expand">
      <template slot-scope="props">
        <el-form  label-position="left" inline class="demo-table-expand">
          <el-form-item label="投诉单编号">
            <span>{{ props.row.id }}</span>
          </el-form-item>
          <el-form-item label="任务编号">
            <span>{{ props.row.task_id }}</span>
          </el-form-item>
          <el-form-item label="投诉人学号">
            <span>{{ props.row.number_from }}</span>
          </el-form-item>
          <el-form-item label="被投诉人学号">
            <span>{{ props.row.number_to }}</span>
          </el-form-item>
         <el-form-item label="截图">
            <span>详见'细节查看'</span>
          </el-form-item>
        </el-form>
      </template>
    </el-table-column>

    <el-table-column
      label="投诉单编号"
      prop="id">
    </el-table-column>

<el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="Success(scope.row)">投诉成功</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="Fail(scope.row)">投诉失败</el-button>
          <el-button
          size="mini"
          @click="Detail(scope.row)">查看细节</el-button>
      </template>
    </el-table-column>
  </el-table>
</div>
</div>
</template>



<script>
  export default {
    data() {
      return {
        ico: 'R0lGODlhWAAfAJEAAAAAAP////8AAGZmZiH5BAAHAP8ALAAAAABYAB8AAALfhI+py+0PX5i02ouz3rxn44XiSHJgiaYqdq7uK7bwTFtyjcN3zqd7D4wBgkTSr4i87AQCCrPCfFoG1IGIWqtabUOLNPCVfgNY8rZTzp4py+Yk7B6nL9pp3T6f3O3KLrQJ6OYlqLdGUTaHuKZYwcjHdfEUOEh4aGiW4Vi4acnZeNkGBlb5Ror5mbmVqLrISgfq9zcqO4uxmup5enuKChk56RRHqKnbmktMnDvxI1kZRbpnmZccXTittXaUtB2gzY3k/U0ULg5EXs5zjo6jvk7T7q4TGz8+T28eka+/zx9RAAA7'
        ,
        tableData: []
      }
    },
    methods:{

       //查看所有被投诉的任务，待审核
    getComplain: function(vm) {
      if (this.user_id === "") return;
      vm.loading=true;
      var URL = "http://localhost:8082/module/user/get_complaint/all";
      var jsonData = { offset: 0, number: 100 };
      var axios = {
        method: "get",
        url: URL,
        widthCredentials: false,
        params: jsonData
      };
      this.$http(axios)
        .then(function(res) {
          if (res.status == 200) {
            if (res.data.code == 200) {
              var number = res.data.number;
              var content = res.data.content;
              var jsonContent = content;
              for (var i = 0; i < number; i++) {
                var temp = jsonContent[i];
                var tempIndex = {
                  id: 0,
                  task_id: 0,
                  number_from:"",
                  number_to:""
                };
                tempIndex.id = temp.cid;
                tempIndex.task_id = temp.tid;
                tempIndex.number_from = temp.sid1;
                tempIndex.number_to = temp.sid2;
                
                vm.tableData.push(tempIndex);
              }
              vm.loading=false;
            } else {
              alert(res.data.msg);
            }
          } else alert("网络出错");
        })
        .catch(function(err) {
          console.log(err);
        });
    },

      Success(row){
           var jsonData = {
        cid: parseInt(row.id),
        verify:1
      };
var axios = {
        method: "put",
        url: "http://localhost:8082/module/user/complaint_handle",
        widthCredentials: false,
        data: jsonData
      };
     
    this.$http(axios).then(function(res){
          if(res.status==200) {
           alert("已提交！");
          }
          else alert("网络错误");
        }).catch(function(err){
          console.log(err);
          alert("发生了一个异常");
        });
      },

      Fail(row){
         var jsonData = {
        cid: parseInt(row.id),
        verify:2
      };
var axios = {
        method: "put",
        url: "http://localhost:8082/module/user/complaint_handle",
        widthCredentials: false,
        data: jsonData
      };
     
    this.$http(axios).then(function(res){
          if(res.status==200) {
           alert("已提交！");
          }
          else alert("网络错误");
        }).catch(function(err){
          console.log(err);
          alert("发生了一个异常");
        });
      },

      Detail(row){
        this.$router.push({
          name: 'Detail',
          params: {
            id: row.id
          }
        })
      },

      

    },
    created(){
      this.getComplain(this);

    }
  }
</script>

<style>
.title{
  position: relative;
  left:10%
}
 .box{
   position: relative;
   left: 20%;
   width: 60%;
   border: 1px solid #eee;
   border-radius: 16px;
 }
  .demo-table-expand {
    font-size: 0;
  }
  .demo-table-expand label {
    width: 100px;
    color: #99a9bf;
  }
  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 100%;
  }
</style>