<template>
  <div class="login-container">
    <div id="login-three-container"></div>
    <div class="login-plane">
      <div class="login-plane-container">
        <img class="login-plane-human" src="@/assets/img/images/login_human.png" alt="" />
        <div class="login-plane-title">
          <h3 class="title">Login Form</h3>
          <img class="login-plane-title-line" src="@/assets/img/images/login_horizontal_line.png" alt="" />
        </div>
        <div class="login-plane-form">
          <el-form label-position="left" :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px">
            <!-- <h3 class="title">用户登录</h3> -->
            <el-form-item prop="username">
              <el-input type="text" v-model="ruleForm.username" auto-complete="off" placeholder="账号"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <el-input type="password" v-model="ruleForm.password" auto-complete="off" placeholder="密码"></el-input>
            </el-form-item>
            <el-row>
              <el-col :span="16">
                <el-form-item prop="code">
                  <el-input type="text" v-model="ruleForm.code" auto-complete="off" placeholder="图形验证码" @keyup.enter.native="submitForm('ruleForm')"></el-input>
                </el-form-item>
              </el-col>
              <el-col :span="8" class="code-box">
                <canvas id="captcha" class="captcha" @click="initCaptcha"></canvas>
              </el-col>
            </el-row>
            <el-checkbox class="remember" v-model="rememberpwd">记住密码</el-checkbox>
            <el-form-item style="width:100%;">
              <el-button type="primary" class="btn" @click="submitForm('ruleForm')" :loading="logining">登录</el-button>
              <br/>
              <p style="color:#fff">随便输入</p>
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>

  </div>
</template>
<script type="text/ecmascript-6">
import CaptchaMini from 'captcha-mini'; //引入验证码生成组件​
import { login } from '../api/userMG'
import { setCookie, getCookie, delCookie } from '../utils/util'
import md5 from 'js-md5'
export default {
  name: 'login',
  data() {
    var validateCode = (rule, value, callback) => {
      if(!value){
        return  callback(new Error('请输入验证码'));
      }else {
        let val = value.compare(this.captcha)
        if(val){
           return callback();
        }else{
          return  callback(new Error('验证码输入有误'));
        }
      }
    };
    return {
      //定义loading默认为false
      logining: false,
      // 记住密码
      rememberpwd: false,
      ruleForm: {
        //username和password默认为空
        username: '',
        password: '',
        code: '',
        randomStr: '',
        codeimg: ''
      },
      //rules前端验证
      rules: {
        username: [{ required: true, message: '请输入账号', trigger: 'blur' }],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
        code:[{required: true, validator: validateCode, trigger: 'blur' },],
      },
      canvastx: null,
      fontsize: 18,
      H: null,
      W: null,
      lie: null,
      str: '01',
      text: [],
      timer: null,
      captcha:''
    }
  },
  // 创建完毕状态(里面是操作)
  created() {
    // this.$message({
    //   message: '账号密码及验证码不为空即可',
    //   type: 'success'
    // })
    // 获取存在本地的用户名密码
    this.getuserpwd()
    
  },
  mounted(){
    this.H = window.innerHeight
    this.W = window.innerWidth
    this.lie = Math.floor(this.W / this.fontsize)
    this.initCanvas()
    this.initCaptcha()
  },
  beforeDestroy() {
    clearInterval(this.timer)
  },
  // 里面的函数只有调用才会执行
  methods: {
    // 获取用户名密码
    getuserpwd() {
      if (getCookie('user') != '' && getCookie('pwd') != '') {
        this.ruleForm.username = getCookie('user')
        this.ruleForm.password = getCookie('pwd')
        this.rememberpwd = true
      }
    },
    //获取info列表
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.logining = true
          // 测试通道，不为空直接登录
          setTimeout(() => {
            this.logining = false
            this.$store.commit('login', 'true')
            this.$router.push({ path: '/largeScreen' })
          }, 1000)
          // 注释
          // login(this.ruleForm).then(res => {
          //   if (res.success) {
          //     if (this.rememberpwd) {
          //       //保存帐号到cookie，有效期7天
          //       setCookie('user', this.ruleForm.username, 7)
          //       //保存密码到cookie，有效期7天
          //       setCookie('pwd', this.ruleForm.password, 7)
          //     } else {
          //       delCookie('user')
          //       delCookie('pwd')
          //     }
          //     //如果请求成功就让他2秒跳转路由
          //     setTimeout(() => {
          //       this.logining = false
          //       // 缓存token
          //       localStorage.setItem('logintoken', res.data.token)
          //       // 缓存用户个人信息
          //       localStorage.setItem('userdata', JSON.stringify(res.data))
          //       this.$store.commit('login', 'true')
          //       this.$router.push({ path: '/goods/Goods' })
          //     }, 1000)
          //   } else {
          //     this.$message.error(res.msg)
          //     this.logining = false
          //     return false
          //   }
          // })
        } else {
          // 获取图形验证码
          this.getcode()
          this.$message.error('请输入用户名密码！')
          this.logining = false
          return false
        }
      })
    },
    initCanvas() {
      var mycanvas = document.getElementById('mycanvas')
      if(!mycanvas) return
      this.canvastx = mycanvas.getContext('2d')

      mycanvas.height = this.H
      mycanvas.width = this.W
      for (var i = 0; i < this.lie; i++) {
        this.text.push(0)
      }
      this.canvastx.font = this.fontsize + 'px 微雅软黑'
      const that = this
      this.timer = setInterval(function() {
        that.draw()
      }, 1000 / 20)
    },
    draw() {
      this.canvastx.fillStyle = 'rgba(0,0,0,0.08)'
      this.canvastx.fillRect(0, 0, this.W, this.H)
      this.canvastx.fillStyle = this.randColor()
      for (var i = 0; i < this.lie; i++) {
        var index = Math.floor(Math.random() * this.str.length)
        var x = Math.floor(this.fontsize * i)
        var y = this.text[i] * this.fontsize
        this.canvastx.fillText(this.str[index], x, y)
        if (y > this.H && Math.random() > 0.99) {
          this.text[i] = 0
        }
        this.text[i]++
      }
    },
    randColor() {
      var r = Math.ceil(Math.random() * 155) + 100
      var g = Math.ceil(Math.random() * 155) + 100
      var b = Math.ceil(Math.random() * 155) + 100
      return 'rgb(' + r + ',' + g + ',' + b + ')'
    },
    //生成验证码
    initCaptcha() {
      var captcha = new CaptchaMini({
          lineWidth: 5, //线条宽度
          lineNum: 6, //线条数量
          dotR: 2, //点的半径
          dotNum: 25, //点的数量
          preGroundColor: [10, 80], //前景色区间
          backGroundColor: [150, 250], //背景色区间
          fontSize: 70, //字体大小
          fontFamily: ["Georgia", "微软雅黑", "Helvetica", "Arial"], //字体类型
          fontStyle: "stroke", //字体绘制方法，有fill和stroke
          content: "ABCDEFGHJKLMNOPQRSTUVWXYZ1234567890", //验证码内容
          length: 4 //验证码长度
      });
      captcha.draw(document.getElementById("captcha"), r => {
          this.captcha = r; // 可通过 this.captcha 使用当前验证码（校验用户输入对否等） 
      });
    },
  }
}
</script>

<style lang="scss" scoped>
$bg:#2d3a4b;
$dark_gray:#889aa4;
$light_gray:#eee;
.login-container {
  width: 100%;
  height: 100vh;
  position: relative;
  background-image: url('~@/assets/img/images/img_bg2.jpg');
  background-repeat:no-repeat ;
  background-size: 100%;
  #login-three-container {
    width: 100%;
    height: 100%;
  }
  .login-plane {
    position: absolute;
    z-index: 9999;
    width: 600px;
    height: 500px;
    background-image: url('~@/assets/img/images/login_border.png');
    background-repeat: no-repeat;
    background-size: 100% 100%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    .login-plane-container {
      width: 100%;
      height: 100%;
      border-radius: 18px;
      // background-color: #007eff2e;
      position: relative;
      @keyframes humanMove {
        0% {
          top: -100px;
        }
        25% {
          top: -120px;
        }
        50% {
          top: -100px;
        }
        75% {
          top: -80px;
        }
        100% {
          background: -100px;
        }
      }
      .login-plane-human {
        position: absolute;
        width: 260px;
        right: -120px;
        top: -100px;
        animation: humanMove 8s linear 0s infinite normal;
      }
      .login-plane-title {
        width: 100%;
        height: 100px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        font-size: 35px;
        color: #fff;
        font-weight: 700;
        img {
          width: 50%;
        }
        .login-plane-title-line {
          width: 80%;
          position: absolute;
          bottom: 0;
        }
      }
      .login-plane-form {
        padding: 45px 55px;
        box-sizing: border-box;
        .code-box{
          .captcha {
            margin-left: 10px;
            width: 150px;
            height: 40px;
            border-radius: 6px;
          }
        }
        .btn{
          margin-top:10px ;
          width:100%;
          background:#01101e2e;
          border-color:#26292c;
        }
        .btn:hover{
          background:#0d223685;
        }
      }
    }
  }

  // .login-ground {
  //   position: absolute;
  //   z-index: 9998;
  //   width: 100%;
  //   height: 400px;
  //   background-image: url('~@/assets/img/images/ground.png');
  //   background-repeat: no-repeat;
  //   background-size: 100% 100%;
  //   bottom: 0;
  //   left: 0;
  // }
}
</style>
<style lang='less' scoped>
    /deep/.el-input__inner,/deep/.el-checkbox__inner{
      background-color:#fff0 !important;
      color: #fff;
    }
    /deep/::-webkit-scrollbar {width: 0 !important}
</style>