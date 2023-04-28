<!--
 * @Author      : YaleXin
 * @Email       : me@yalexin.top
 * @LastEditors : YaleXin
-->
<template>
  <div>
    <el-divider content-position="center">
      <h3>慢性病调查问卷</h3>
    </el-divider>
    <div class="survey">
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
  <el-form-item label="名字" prop="name">
    <el-input v-model="ruleForm.name"></el-input>
  </el-form-item>
  <el-form-item label="年龄" prop="age">
    <el-input v-model="ruleForm.age"></el-input>
  </el-form-item>
  <el-form-item label="性别" prop="region">
    <el-select v-model="ruleForm.region" placeholder="请选择性别">
      <el-option label="男" value="shanghai"></el-option>
      <el-option label="女" value="beijing"></el-option>
    </el-select>
  </el-form-item>
  
 
  <el-form-item label="慢性病" prop="type">
    <el-checkbox-group v-model="ruleForm.type">
      <el-checkbox label="糖尿病" name="type"></el-checkbox>
      <el-checkbox label="高血压" name="type"></el-checkbox>
      <el-checkbox label="冠心病" name="type"></el-checkbox>
      <el-checkbox label="肝硬化" name="type"></el-checkbox>
    </el-checkbox-group>
  </el-form-item>
  <el-form-item label="生活方式" prop="desc">
    <el-input type="textarea" v-model="ruleForm.desc" placeholder="例如：素食者，熬夜作息不规律..."></el-input>
  </el-form-item>
  <el-form-item label="当前症状" prop="desc1">
    <el-input type="textarea" v-model="ruleForm.desc1" placeholder="例如：失眠、焦虑..."></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
    <el-button @click="resetForm('ruleForm')">重置</el-button>
  </el-form-item>
</el-form>
  </div>
  </div>
</template>

<script>
import innerHttp from "../network/innerHttp.js";
export default {

  name: "Link",
  components: {},
  created() {
    innerHttp
      .get("/link")
      .then(res => {
        console.log(res.data.links);
        this.linkList = res.data.links;
        this.linkList[0].content.sort(() => {
          return 0.5 - Math.random();
        });
      })
      .catch(e => {});
  },
  activated() {
    document.title = "调查问卷";
    this.linkList[0].content.sort(() => {
      return 0.5 - Math.random();
    });
  },
  methods: {},
  data() {
    return {
      ruleForm: {
          name: '',
          age:'',
          region: '',
          type: [],
          resource: '',
          desc: '',
          desc1: ''

        },
        rules: {
          name: [
            { required: true, message: '请输入名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          age: [
            { required: true, message: '请输入年龄', trigger: 'blur' },
          ],
          region: [
            { required: true, message: '请选择性别', trigger: 'change' }
          ],
          type: [
            { type: 'array', required: true, message: '请至少选择一个慢性病', trigger: 'change' }
          ],
          desc: [
            { required: true, message: '请填写生活方式', trigger: 'blur' }
          ],
          desc1: [
            { required: true, message: '请填写当前症状', trigger: 'blur' }
          ]
        },
      tableData: [
        {
          name: "黄阿信的个人博客",
          description: "要么改变世界，要么适应世界",
          link: "https://www.yalexin.top/blog/",
          avatar: "https://www.yalexin.top/img/cat_mouse.jpg"
        }
      ],
      linkList: [
        {
          id: 1,
          type: "博客链接",
          content: [
            {
              id: 1,
              name: "黄阿信的博客",
              description: "要么改变世界，要么适应世界",
              url: "www.yalexin.com",
              avatar: "https://www.yalexin.top/images/cat_mouse.jpg"
            },
            {
              id: 2,
              name: "黄阿信的hexo博客",
              description: "要么改变世界，要么适应世界",
              url: "https://yalexin.gitee.io/",
              avatar: "https://qiniu.yalexin.top/home.png"
            }
          ]
        }
      ]
    };
  },
  methods:{
    submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
  }
};
</script>

<style scoped>
.link-item {
  align-items: flex-start;
  display: flex;
  text-align: left;
}

.avatar-wrapper {
  margin-right: 1rem;
  margin-left: 1rem;
  width: 80px;
  height: 80px;
  margin-top: 15px;
}
.info-wrapper {
  flex-basis: auto;
  flex-grow: 1;
  flex-shrink: 1;
  text-align: left;
}
.link-avatar {
  max-width: 100%;
  height: auto;
  display: block;
  margin: auto;
  border-radius: 9px;
}
</style>
