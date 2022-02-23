<template>
  <div class="container">
    <el-row :gutter="20">
      <el-col :span="24">
        <div class="header">
          <el-card shadow="never">
            <span class="header-user">
              <img :src="userIcon" />
            </span>
            <div class="header-tip">
              <p class="header-tip-title"> {{hoursTip}}，看到你好开心。 </p>
              <p class="header-tip-description"> 
                红酥手，黄縢酒，满城春色宫墙柳。东风恶，欢情薄。一怀愁绪，几年离索。错、错、错。
                春如旧，人空瘦，泪痕红浥鲛绡透。桃花落，闲池阁。山盟虽在，锦书难托。莫、莫、莫！ 
              </p>
            </div>
            <div class="header-user-list">
              <div class="user-list"></div>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="24">
        <swiper class="swiperClass"></swiper>
      </el-col>
      <el-col v-for="(item, index) in iconList" :key="index" :xs="12" :sm="6" :md="3" :lg="3" :xl="3">
        <el-card class="icon-panel" shadow="hover"  @click.native="goPath(item)">
          <i :class="item.icon" :style="{'color':item.color}"></i>
          <p>{{ item.title }}</p>
        </el-card>
      </el-col>
      <el-col :span="24">
        <plan class="planClass"></plan>
      </el-col>
    </el-row>
  </div>
</template>

<script type="text/javascript">
import Plan from './components/Plan'
import chaiIcon from '@/components/icon/index.vue';
import swiper from '../swiper/swiper2/index.vue'
  export default {
    name: "component_name",
    components: {
      chaiIcon,
      Plan,
      swiper
    },
    data() {
      return {
        userIcon:require('@/assets/img/user.gif'),
        hoursTip:'早上好',
        //卡片图标
        iconList: [
          {
            id:'0',
            icon: 'el-icon-postcard',
            title: '表单代码生成器',
            link: 'https://mrhj.gitee.io/form-generator/?hmsr=admin-plus&hmpl=&hmcu=&hmkw=&hmci=#/',
            color: '#ffc069',
          },
          {
            id:'1',
            icon: 'el-icon-video-camera',
            title: '视频播放器',
            link: '/goods/Goods',
            color: '#ffc069',
          },
          {
            id:'2',
            icon: 'el-icon-tickets',
            title: '表格',
            link: '/pay/Order',
            color: '#5cdbd3',
          },
          {
            id:'3',
            icon: 'el-icon-user',
            title: '用户',
            link: '/system/user',
            color: '#b37feb',
          },
          {
            id:'4',
            icon: 'el-icon-notebook-1',
            title: '书籍',
            link: '',
            color: '#69c0ff',
          },
          {
            id:'5',
            icon: 'el-icon-bell',
            title: '公告',
            link: '',
            color: '#ff85c0',
          },
          // {
          //   icon: 'el-icon-present',
          //   title: '礼物',
          //   link: '',
          //   color: '#ffd666',
          // },

          {
            id:'6',
            icon: 'el-icon-ship',
            title: '公平的世界',
            link: '',
            color: '#ff9c6e',
          },
          {
            id:'7',
            icon: 'el-icon-coffee-cup',
            title: '休息一下',
            link: '',
            color: '#95de64',
          },
        ],
      }
    },
    mounted() {
      this.getMycount ()
    },
    methods:{
      getMycount () {
        let self = this;
        let date = new Date();
        if (date.getHours() >= 0 && date.getHours() < 8) {
            self.hoursTip = "早上好"
        }else if (date.getHours() >= 8 && date.getHours() < 12) {
            self.hoursTip = "上午好"
        } else if (date.getHours() >= 12 && date.getHours() < 18) {
            self.hoursTip = "下午好"
        } else {
            self.hoursTip = "晚上好"
        }
      },
      goPath(val){
        if(val.id == '0'){
          window.open(val.link, "_blank");
        }else{
          this.$router.push({ path: val.link })
        }
      }
    }
  }
</script>

<style  scoped lang="less">

  .header{
    min-height: 125px;
    transition: none;
    color: #626b7d;
    /deep/.el-card__body{
      display: flex;
      flex-wrap: wrap;
      align-items: center;
    }
    .header-user{
      img{
        width: 60px;
        height: 60px;
        margin-right: 20px;
        border-radius: 50%;
      }
    }
    .header-tip{
      flex: auto;
      width: calc(100% - 200px);
      min-width: 200px;
      .header-tip-title{
        margin-bottom: 12px;
        font-size: 20px;
        font-weight: 700;
        color: #3c4a54;
      }
      .header-tip-description{
        min-height: 20px;
        font-size: 14px;
        color: #808695;
      }
    }
    .header-user-list{
      flex: 1;
      min-width: 100px;
      margin-left: 20px;
      text-align: right;
      .user-list{
        box-sizing: border-box;
        outline: none!important;
      }
    }
  }
  .planClass{
    margin-top: 20px;
  }
  .icon-panel:hover{
    i{
      transform: scale(1.2);
    }
  }
  .icon-panel {
    height: 117px;
    text-align: center;
    cursor: pointer;
    i{
      height: 50px;
      width: 50px;
      font-size: 40px;
      font-weight:400;
      transition: all .5s;  /*所有的属性在0.6秒之内完成改变*/
    }
    p {
      margin-top: 10px;
    }
  }
</style>
