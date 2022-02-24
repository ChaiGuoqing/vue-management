<template>
  <div class="container">
    <el-row :gutter="20">
      <el-col :span="9">
        <div>
          <el-card :body-style="{ padding: '0px' }" shadow="hover" class="card">
            <div class="info">
              <img src="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png" class="image">
              <h3>柴国庆</h3>
              <p>有道无术，术尚可求，有术无道止于术，富在术数，不在劳身；</p>
              <el-button type="primary" size="small" round @click="followMe">Follow me</el-button>
            </div>
            <div style="padding: 14px;">
              <div class="msg"><i class="el-icon-user"></i><span>前端小白白</span></div>
              <div class="msg"><i class="el-icon-female"></i><span>男</span></div>
              <div class="msg"><i class="el-icon-date"></i><span>1993/11/13</span></div>
              <div class="msg"><i class="el-icon-suitcase"></i><span>web前端工程师</span></div>
              <div class="msg"><i class="el-icon-map-location"></i><span>中国·山东省·济南市</span></div>
              <div class="msg"><i class="el-icon-document-checked"></i><span>JavaScript、HTML、CSS、Vue、Echart、Element-UI、PS</span></div>
              <el-divider></el-divider>
              <div class="bottom clearfix">
                <p>个人标签</p>
                <el-tag class="tag" size="small">匪石匪席</el-tag>
                <el-tag class="tag" size="small">怕麻烦</el-tag>
                <el-tag class="tag" size="small">穷叉叉</el-tag>
                <el-tag class="tag" size="small">躺平</el-tag>
              </div>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="15">
        <div>
          <el-card :body-style="{ padding: '0px' }" shadow="hover" style="padding:20px 15px;">
            <el-tabs v-model="activeName" @tab-click="handleClick">
              <el-tab-pane label="基本信息" name="first">
                <div class="account"></div>
              </el-tab-pane>
              <el-tab-pane label="账号绑定" name="second">
                <div class="account">
                  <el-row>
                    <el-col :span="2" style="text-align:center">
                      <img :src="img[0]" width="42" height="42" />
                    </el-col>
                    <el-col :span="20">
                      <div class="grid-content"> 绑定QQ </div>
                      <div class="grid-content"> 969636913 </div>
                    </el-col>
                    <el-col :span="2">
                      <el-button type="text">更换绑定</el-button>
                    </el-col>
                  </el-row>
                  <el-divider></el-divider>
                  <el-row>
                    <el-col :span="2" style="text-align:center">
                      <img :src="img[1]" width="42" height="42" />
                    </el-col>
                    <el-col :span="20">
                      <div class="grid-content"> 绑定微信 </div>
                      <div class="grid-content"> 当前未绑定绑定微信账号 </div>
                    </el-col>
                    <el-col :span="2">
                      <el-button type="text">更换绑定</el-button>
                    </el-col>
                  </el-row>
                  <el-divider></el-divider>
                  <el-row>
                    <el-col :span="2" style="text-align:center">
                      <img :src="img[2]" width="42" height="42" />
                    </el-col>
                    <el-col :span="20">
                      <div class="grid-content"> 绑定支付宝 </div>
                      <div class="grid-content"> 当前未绑定绑定支付宝账号 </div>
                    </el-col>
                    <el-col :span="2">
                      <el-button type="text">更换绑定</el-button>
                    </el-col>
                  </el-row>
                  <el-divider></el-divider>
                  <el-row>
                    <el-col :span="2" style="text-align:center">
                      <img :src="img[3]" width="42" height="42" />
                    </el-col>
                    <el-col :span="20">
                      <div class="grid-content"> 绑定微博 </div>
                      <div class="grid-content"> 当前未绑定绑定微博账号 </div>
                    </el-col>
                    <el-col :span="2">
                      <el-button type="text">更换绑定</el-button>
                    </el-col>
                  </el-row>
                  <el-divider></el-divider>
                  <el-row>
                    <el-col :span="2" style="text-align:center">
                      <img :src="img[4]" width="42" height="42" />
                    </el-col>
                    <el-col :span="20">
                      <div class="grid-content"> 绑定Github </div>
                      <div class="grid-content"> 
                        <el-link @click="followMe" :underline="false" type="primary">https://github.com/ChaiGuoqing</el-link>
                      </div>
                    </el-col>
                    <el-col :span="2">
                      <el-button type="text">更换绑定</el-button>
                    </el-col>
                  </el-row>
                  <el-divider></el-divider>
                </div>
              </el-tab-pane>
            </el-tabs>
          </el-card>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script type="text/javascript">
 //import index from '@/components/index.vue';
  export default {
    name: "component_name",
    components: {},
    data() {
      return {
        activeName:'second',
        img:[
          require("@/assets/icon/qq.svg"),
          require("@/assets/icon/weixin.svg"),
          require("@/assets/icon/zhifubao.svg"),
          require("@/assets/icon/xinlang.svg"),
          require("@/assets/icon/github.svg")
        ]
      }
    },
    created() {},
    mounted() {},
    methods:{
      handleClick(tab, event) {
        console.log(tab, event);
      },
      followMe(){
        window.open('https://github.com/ChaiGuoqing', "_blank");
      }
    }
  }
</script>

<style lang="less"  scoped >
@font-color:#262626;
@font-size:14px;
.container{
  padding: 10px;
}
.card{
  padding: 20px 30px;
  .info{
    text-align:center;
    padding: 20px 10px;
    img{
      width: 100px;
      border-radius: 50px;
    }
    h3{
      font-size: 24px;
      font-weight: 500;
      color: @font-color;
      padding-top: 15px;
    }
    p{
      padding: 10px 0;
      font-size: @font-size;
      color: @font-color;
    }
  }
  .msg{
    padding: 10px 30px;
    font-size: @font-size;
    span{
      padding-left: 5px;
    }
  }
  .bottom{
    p{
      font-weight: bold;
      font-size: @font-size;
      color: @font-color;
      padding-bottom: 10px;
    }
    .tag{
      margin-left: 5px;
    }
  }
}
.account{
  padding-top:12px ;
  height:568px;
  .grid-content{
    font-size:16px;
    color: #626b7d;
    height: 26px;
    // line-height: 26px;
  }
}
</style>
