<template>
  <div class="login_container" :style="note">
    <div class="login_box">
      <!--头像区域 -->
      <div class="avater-box">
        <img src="../assets/home1.png" alt="">
      </div>
      <!-- 登陆表单区域-->
      <el-form ref="loginformref" :model="loginform" :rules="loginformroles" label-width="0px" class="login_form">
        <!--用户名-->
        <el-form-item prop="username">
          <el-input v-model="loginform.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <!--密码-->
        <el-form-item prop="password">
          <el-input v-model="loginform.password" prefix-icon="el-icon-more" type="password" show-password></el-input>
        </el-form-item>
        <!--按钮-->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetloginform">重置</el-button>
        </el-form-item>
<!--        <el-button type="primary" @click="fe">fe</el-button>-->
      </el-form>
    </div>
  </div>
</template>

<script>
  export default {

    data() {
      return {
        //登陆数据绑定对象
        loginform: {
          username: '',
          password: ''
        },
        list:[],
        //表单验证规则
        loginformroles: {
          //验证用户名是否合法
          username: [
            {required: true, message: "请输入用户名", trigger: "blur"},
            {min: 1, max: 10, message: "长度在1到10个字符", trigger: "blur"}
          ],
          //验证密码是否合法
          password: [
            {required: true, message: "请输入密码", trigger: "blur"},
            {min: 2, max: 60, message: "长度在2到60个字符", trigger: "blur"}
          ]

        },
        note:{
          backgroundImage:' url('+require( '../assets/hometu.jpg')+ ')',
          backgroundSize:'cover',
          backgroundRepeat:'no-repeat',
          backgroundPosition:'center',
        },
        f:''
      }
    },

    methods: {
      resetloginform() {
        this.$refs.loginformref.resetFields();
      },
      login() {
        this.$refs.loginformref.validate(async valid => {
          if (!valid) return;
          const {data: res} = await this.http.post("re", this.loginform);
          if (res.status == 240) return this.$message.error('密码错误');
          if (res.status == 241) return this.$message.error('用户不存在');
          this.$message.success('登陆成功');
          //登陆成功token保存到客户端的sessionStorage中
          window.sessionStorage.setItem("token", res.token);
          window.sessionStorage.setItem("name", res.name);
          window.sessionStorage.setItem("role", res.role);
          //编程式导航跳转
          await this.$router.push("/home");
        });
      },
      get() {
        this.$refs.loginformref.validate(async valid => {
          if (!valid) return;
          const {data: res} = await this.http.post("allrecord");
          this.list = res;
        });
      },
      fe(){
        this.axios({
          url: 'todata',
          method: 'post',
          data: {
            a:'sqdq'
          }
        }).then(res => {
          this.getlist()
          this.$message.success("修改成功")
        })
      },
    }
  }

</script>

<style lang="less" scoped>
  .login_container {
    background-color: #2b4b6b;
    height: 100%;

  }

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .avater-box {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;

      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }

  .btns {
    display: flex;
    justify-content: flex-end;
  }

  .login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }
</style>
