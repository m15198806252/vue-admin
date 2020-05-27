<template>
  <div id="Login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          v-for="item in loginmenu"
          :key="item.id"
          :class="{'current':item.isActive}"
          @click="loginsub(item)"
        >{{item.txt}}</li>
      </ul>
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        size='medium'
      >
        <el-form-item  prop='userName'>
            <label>邮箱</label>
            <el-input type="text" v-model="ruleForm.userName" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="Password" >
            <label>密码</label>
            <el-input type="password" v-model="ruleForm.Password" autocomplete="off"></el-input>
        </el-form-item>
        <!-- 注册表单 -->
         <el-form-item prop="checkPass"  v-if='this.model=="register"'>
            <label>重复密码</label>
            <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
        </el-form-item>
      <el-form-item  prop="Code" >
            <label>验证码</label>
            <el-row :gutter="11">
              <el-col :span="15"><el-input v-model.number="ruleForm.Code"></el-input></el-col>
              <el-col :span="6"><el-button type="success">获取验证码</el-button></el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
                <el-button type="primary" @click="onLogin">{{subutton}}</el-button>
       </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  name: "login",
  data() {
    //   登陆表单验证
      var checkCode = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('验证码不能为空'));
        }
        setTimeout(() => {
          if (!Number.isInteger(value)) {
            callback(new Error('请输入数字值'));
          } else {
            if (value < 18) {
              callback(new Error('必须年满18岁'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      var validateUser = (rule, value, callback) => {
    //   var reg=/^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
        if (value === '') {
          callback(new Error('请输入邮箱'));
         }//else if(reg.text(value)){
        //     callback(new Error('邮箱格式不对'));
        // } 
        else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
        var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
         }
        else {
          if (this.ruleForm.checkPass !== '') {
            this.$refs.ruleForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请确认密码'));
        } else if (value !== this.ruleForm.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
    return {
      loginmenu: [
        { txt: "登录", isActive: true,type:'login',Subtn:'立即登录' },
        { txt: "注册", isActive: false,type:'register',Subtn:'立即注册' }
      ],
      model:'login',
      subutton:'立即登录',
      isActive: true,//登录注册按钮选中切换
    //   登陆表单验证
     ruleForm: {
          userName: '',
          Password:'',
          checkPass: '',
          Code: ''
        },
        rules: {
          userName: [
            { validator: validateUser, trigger: 'blur' }
          ],
          Password:[
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          Code: [
            { validator: checkCode, trigger: 'blur' }
          ]
        }
     
    
    };
  },
  methods: {

      //点击切换登陆注册按钮
    loginsub(e) {
      this.loginmenu.forEach(elem => {
        elem.isActive = false;
      });
      e.isActive = true;
      this.model=e.type;
      this.subutton=e.Subtn;
      console.log(this.subutton,123)
    },
    //点击登录
    onLogin(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!',8888);
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
<style lang="scss" scoped>
#Login {
  background-color: #344a5f;
  height: 100vh;
}
.login-wrap {
  width: 330px;
  margin: auto;
  .menu-tab {
    text-align: center;
    li {
      display: inline-block;
      line-height: 36px;
      font-size: 14px;
      width: 88px;
      border-radius: 2px;
      color: #fff;
      cursor: pointer; //鼠标指针变成手势
    }
    .current {
      background-color: rgba(0, 0, 0, 0.1);
    }
  }
  label{
    display:block;
    color:#fff;
    font-size: 13px;
  }
  .el-form-item{
      margin-bottom: 10px;
  }
  .el-button--primary{
      width: 100%;
      margin: 4% 0;
  }

}
</style>