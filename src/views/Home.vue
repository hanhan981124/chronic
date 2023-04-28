<template>
  <div>
    <el-row :gutter="24" style="height: 800px;">
  <el-col :span="9">
    <el-card style="margin: 0;">
      <div>
        <el-col :span="12">智能问答</el-col>
        <el-col :span="12" style="text-align: end;">
          <el-switch
            v-model="value1"
            active-text="链接图谱"
            >
          </el-switch>
        </el-col>
      </div>
      <div style="padding-top: 50px;padding-left: 0;">
          <img src='../assets/img/2.webp' alt="" class="user">
          <span class="chat1" >你好,我是慢病健康小助手,有什么问题可以直接问我。但基于患者自述病情所发表的言论仅供参考,不能作为诊断和治疗的直接依据。</span>
        </div>
      <div style="padding: 0;height: 635px;max-height: 635px;overflow-y: scroll;" ref="chatContainer">
        <div v-for="(item,index) in history" v-if="index%2==0" style="float: right;width: 100%; text-align: right;" :key="index">
          <span class="chat">{{ item }}</span><img src='../assets/img/1.webp' alt="" class="user">
        </div>
        <div v-else>
          <img src='../assets/img/2.webp' alt="" class="user">
          <span class="chat1" >{{ item }}</span>
        </div>
      </div>
      <form @submit.prevent="submitQuestion">
    <input type="text" v-model="question" id="question" name="question" placeholder="请输入病情" style="border-radius: 5px;height: 30px;width: 70%;"/>
    <el-button type="primary"  icon="el-icon-check" circle class="submit"></el-button>
  </form>
</el-card>
  </el-col>
  <el-col :span="15">
  <el-card>
    <el-row style="height:165px;overflow:auto;display: flex;">
    <el-col><img src='../assets/img/dietray.png' alt="" style="height:146px;width: 150px;"></el-col>
    <el-col><el-card class="card1">膳食补充剂</el-card> </el-col>
    <el-col><el-card class="card1">膳食补充剂</el-card></el-col>
    <el-col><el-card class="card1">膳食补充剂</el-card></el-col>
    
  </el-row>
  </el-card>
  <el-card>
    <el-row style="height:165px;overflow:auto;display: flex;">
    <el-col><img src='../assets/img/food.jpg' alt="" style="height:146px;width: 150px;"></el-col>
    <el-col><el-card class="card2">食谱</el-card></el-col>
    <el-col><el-card class="card2">食谱</el-card></el-col>
    <el-col><el-card class="card2">食谱</el-card></el-col>
  </el-row>
  </el-card>
  <el-card>
    <el-row style="height:165px;overflow:auto;display: flex;">
    <el-col><img src='../assets/img/doctor.jpg' alt="" style="height:146px;width: 150px;"></el-col>
    <el-col><el-card class="card3">医疗</el-card></el-col>
    <el-col><el-card class="card3">医疗</el-card></el-col>
    <el-col><el-card class="card3">医疗</el-card></el-col>
  </el-row>
  </el-card>
  <el-card>
    <el-row style="height:165px;overflow:auto;display: flex;">
    <el-col><img src='../assets/img/life.png' alt="" style="height:146px;width: 150px;"></el-col>
    <el-col>
      <el-carousel indicator-position="outside">
        <el-carousel-item v-for="item in 4" :key="item">
          <el-card class="card3">{{ 1 }}</el-card>
        </el-carousel-item>
      </el-carousel>
    </el-col>
  </el-row>
  </el-card>
  </el-col>
</el-row>
  </div>
</template>

<script>
import innerHttp from "../network/innerHttp.js";

export default {
  name: "Home",
  
  data() {
    return {
      value1:false,
      question: '',
      history:[],
      history1:[],
      answer:''
    };
  },
  mounted() {
     // 滚动到底部
     this.$refs.chatContainer.scrollTop = this.$refs.chatContainer.scrollHeight;
  },
    updated() {
    this.$refs.chatContainer.scrollTop = this.$refs.chatContainer.scrollHeight;
  },
   // 定义向下滚动的方法
 scrollToTop() {
    const container = this.$refs.chatContainer;
    container.scrollTop -= container.clientHeight;
  },
  methods: {
    submitQuestion: function() {
      if (this.question==''){
        this.$message('请输入内容再发送')
      }else{
      axios.post('http://10.5.83.162:8000', {
        prompt: this.question,
        history: this.history1
      }, {
        headers: {
          'Content-Type': 'application/json'
        }
      }).then(response => {
        // console.log(response.data)
        this.answer = response.data.response
        this.history1 = response.data.history
        // console.log(this.answer)
        this.history.push(this.question);
        this.history.push(this.answer)
        // console.log(this.history)
        this.question = '';
      }).catch(error => {
        console.log(error);
      });
    }
    },
  },
  
  activated() {
    document.title = "慢病助手";
  }
};
</script>

<style scoped>
.clearfix {
  align-items: center;
  color: #131313;
  font-size: 24px;
  padding-top: 30px;
}
.chat{
  display: inline-block; 
  color: #fff;
  background-color: #2570bf;
  opacity:0.9; 
  border: 0.5px solid white; 
  border-radius: 5px;
  font-size: 15px;
  margin-bottom: 10px;
  padding: 5px;
}
.chat1{
  display: inline-block; 
  color: #0c0c0c;
  background-color: #d6d0d0;
  opacity:0.9; 
  border: 0.5px solid white; 
  border-radius: 5px;
  font-size: 15px;
  margin-bottom: 10px;
  padding: 5px;
  width: 80%;
}
  .user{
  width: 25px;
  height: 25px;
  border-radius: 50%;
}
.submit{
  border-radius: 50%;
  height: 40px; 
  background-color: #4397f0;
   color: #fff; 
   border: #fff;
}
.card1{
  height:146px;
  width: 200px;
  background-color:lemonchiffon;
  margin-left: 10px;
}
.card2{
  height:146px;
  width: 200px;
  background-color:rgb(173, 235, 240);
  margin-left: 10px;
}.card3{
  height:146px;
  width: 200px;
  background-color:rgb(238, 197, 211);
  margin-left: 10px;
}.card4{
  height:146px;
  width: 200px;
  background-color:rgb(205, 241, 213);
  margin-left: 10px;
}
.el-carousel__item h3 {
    color: #2570d1;
    font-size: 14px;
    margin: 0;
  }
</style>
