<template>
<div>
  <h1 class="title">管理员系统</h1>
  <br/>

<div class="box" >
  <el-table
    :data="tableData"
    style="width: 100%"
    v-loading.fullscreen.lock="loading">
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

<el-table-column >
      <template slot-scope="scope">
        <el-button class="button1"
         
          @click="Success(scope.row)">投诉成功</el-button>
      </template>
</el-table-column>

<el-table-column>
  <template slot-scope="scope">
        <el-button class="button2"
          
          type="danger"
          @click="Fail(scope.row)">投诉失败</el-button>
  </template>
</el-table-column>
<el-table-column>
  <template slot-scope="scope">
          <el-button class="button3"
         
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
        tableData: [],
        loading:true
      }
    },
    methods:{

       //查看所有被投诉的任务，待审核
    getComplain: function(vm) {
      if (this.user_id === "") return;
      
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
           this.ApplyDatabase(parseInt(row.id), this);
           this.$message({
          type: "success",
          message: "通过"
        });
      },

      Fail(row){
          this.ApplyDatabase2(parseInt(row.id), this);
           this.$message({
          type: "warning",
          message: "不通过"
        });
      },

      ApplyDatabase: function(id, vm) {

 var jsonData = {
        cid: id,
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
  
              var table = vm.tableData;
              for (var i = 0; i < table.length; i++) {
                if (table[i].id == id) {
                  vm.tableData.splice(i, 1);
                  break;
                }
              }
            
          }
          else alert("网络错误");
        }).catch(function(err){
          console.log(err);
          alert("发生了一个异常");
        });

        
      },

      ApplyDatabase2: function(id, vm){
         var jsonData = {
        cid: id,
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
            var table = vm.tableData;
              for (var i = 0; i < table.length; i++) {
                if (table[i].id == id) {
                  vm.tableData.splice(i, 1);
                  break;
                }
              }
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
  left:0%
}
 .box{
   position: relative;
   left: 25%;
   width: 50%;
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

  .button1{
    width:100px;
  }

  .button2{
    width:100px;
  }

  .button3{
    width:100px;
  }
</style>