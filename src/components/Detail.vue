<template>
   <div>
  <br/>
    <br/>
<div class="block" v-loading.fullscreen.lock="loading">
    
  <el-timeline>
    <el-timeline-item  placement="top" type="success"> 
          <h1 >任务编号:{{tid}}</h1>
    </el-timeline-item>
    <el-timeline-item  placement="top" type="success">
      <el-card>
        <p align="left">投诉单号:{{cid}}</p>
      </el-card>
    </el-timeline-item>
   
    
    <el-timeline-item  placement="top">
      <el-card>
          <div>
          <p align="left">投诉人学号:{{sid1}}</p>
          <p align="left">被投诉人学号:{{sid2}}</p></div>
      </el-card>
    </el-timeline-item>
<el-timeline-item  placement="top">
      <el-card>
          <h3 align="left">投诉细节:</h3>
          <p align="left">{{reason}}</p>
      </el-card>
    </el-timeline-item>

    <el-timeline-item  placement="top">
      <el-card>
          <h3 align="left">截图:</h3>
          <div v-for="pict in photo_list"  >
           
           <img  :src="'data:image/png;base64,'+pict.photo0">
             

          </div>
      </el-card>
    </el-timeline-item>
        <el-timeline-item>
          <el-button @click="$router.go(-1)">返回</el-button>
        </el-timeline-item>
      </el-timeline>

    </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            cid:this.$route.params.id,
            tid:0,
            sid1:"",
            sid2:"",
            reason:"",
            photo_list:[],
           loading: true
        }
    },
    methods:{
        //查看所有被投诉的任务，待审核
    getInfo: function(vm) {
     
      vm.loading=true;
      var URL = "http://localhost:8082/module/user/get_complaint/"+parseInt(vm.cid);
      
      var axios = {
        method: "get",
        url: URL,
        widthCredentials: false,
      };

      vm.$http(axios)
        .then(function(res) {
          if (res.status == 200) {
            if (res.data.code == 200) {
             
                vm.tid = res.data.tid;
                vm.sid1= res.data.sid1;
                vm.sid2 = res.data.sid2;
                vm.reason=res.data.reason;
                var number=res.data.number;
               vm.photo_list=res.data.photo;
                 console.log(number);
                
               
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

  
    },
    created(){
      this.getInfo(this);
    }
}
</script>

<style scoped>
  .box{
      position: relative;
      width:50%;
      height:50%;
      border: 1px solid #eee
  }

  .block {
  position: relative;
  padding: 15px 15px 15px 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
  border: 1px solid #eee;
  background-color: #ffffff;
  left: 20%;
  width: 60%;
}
</style>
