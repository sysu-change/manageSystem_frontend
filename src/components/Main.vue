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
          <el-form-item label="投诉原因">
            <span>{{ props.row.desc }}</span>
          </el-form-item>
          <el-form-item label="截图">
            <span>{{ props.row.desc }}</span>
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
        tableData: [{
          id: '5',
          number_from:'16341018',
          number_to:'16341025',
          task_id:'7',
          desc: '未按时完成',
          
        }, {
          id: '4',
          number_from:'16341011',
          number_to:'16341025',
          task_id:'8',
          desc: '未按时完成',
        }, {
         id: '2',
          number_from:'16341019',
          number_to:'16341025',
          task_id:'7',
          desc: '未按时完成',
        }, {
          id: '5',
          number_from:'16341018',
          number_to:'16341025',
          task_id:'7',
          desc: '未按时完成',
        }]
      }
    },
    methods:{
      Success(row){

      },

      Fail(row){
        
      }
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
