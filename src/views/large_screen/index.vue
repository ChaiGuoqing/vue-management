<template>
  <div id="data-view">
    <div class="header_top">
      <el-row>
        <el-col :span="13">
          <div class="logo1">
            <!-- <img style="width: 100px !important;" src="./img/img_logo.png" /> -->
            <span>xxxx管理平台</span>
          </div>
        </el-col>
        <el-col :span="11" style="margin-top:30px">
          <div class="menu_header">
            <!-- <el-row>
              <el-col :span="6">
                <div class="menu_header01" @click="goPath(1)">业务总览</div>
              </el-col>
              <el-col :span="6">
                <div class="menu_header02" @click="goPath(2)">业务后台</div>
              </el-col>
              <el-col :span="6">
                <div class="menu_header02" @click="goPath(3)">管理后台</div>
              </el-col>
              <el-col :span="6">
                <div class="menu_header02" @click="goPath(4)">运维部署后台</div>
              </el-col>
            </el-row> -->
          </div>
        </el-col>
      </el-row>
    </div>
    <div class="main-content">
      <el-row>
        <el-col :span="16">
          <div class="left-content">
            <div class="top-content">
              <el-row style="height:100%">
                <el-col :span="6" style="height:100%;display: table;">
                  <div class="menu" style="height:100%">
                    <div class="menu_btn" v-for="(item,index) in menuList" :key="index" @click="goToMenu(item.menuUrl)">
                      <p>{{item.menuName}}</p>
                    </div>
                  </div>
                </el-col>
                <el-col :span="18">
                  <div class="map" id="map"></div>
                </el-col>
              </el-row>
            </div>
            <div class="bottom-content">
              <div class="header">
                <el-row>
                  <el-col :span="4">
                    <!-- <p>AI监控场景</p> -->
                  </el-col>
                  <el-col :span="20">
                    <div class="header-img"></div>
                  </el-col>
                </el-row>
              </div>
              <div class="content">
                <!-- <el-row :gutter="20">
                  <el-col v-for="(item,index) in AIData" :key="index" :span="6">
                    <div class="content-1">
                      <p>{{item.eventAlarmName}}</p>
                      <el-row style="padding-top:10px">
                        <el-col :span="16">
                          <img class="alarm-img" :src="item.eventAlarmImgUrl? 'http://192.168.61.150:9090/'+ item.eventAlarmImgUrl :defoultImg " />
                        </el-col>
                        <el-col :span="7">
                          <div class="alarm-num">
                            <p style="color: #ffae00; font-size: 30px; font-family:'PangMenZhengDao';padding:30px 0 10px 0">{{item.eventAlarmCount}}</p>
                            <p style="opacity: 0.6;">告警数</p>
                          </div>
                        </el-col>
                      </el-row>
                    </div>
                  </el-col>
                </el-row> -->
                
              </div>
            </div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="right-content">
            <div class="right-top-content">
              <div class="header">
                <el-row>
                  <el-col :span="4">
                    <p>用户数据</p>
                  </el-col>
                  <el-col :span="20">
                    <div class="header-img"></div>
                  </el-col>
                </el-row>
              </div>
              <div class="content">
                <el-row>
                  <el-col :span="5">
                    <div class="content-left">
                      <br/>
                      <br/>
                      <countTo class="count" v-if="tenantData.count > 0" :startVal="tenantNumStr" :endVal="tenantData.count" :duration="3000"></countTo>
                      <div class="count"  v-else>{{tenantData.count}}</div>
                      <p>总注册用户</p>
                      <br/>
                      <br/>
                      <br/>
                      <countTo class="yestCount" v-if="tenantData.yestCount > 0" :startVal="yestTenantNumStr" :endVal="tenantData.yestCount" :duration="3000"></countTo>
                      <div class="yestCount"  v-else>{{tenantData.yestCount}}</div>
                      <p>昨日注册用户</p>
                    </div>
                  </el-col>
                  <el-col :span="19">
                    <div class="line" id="line"></div>
                  </el-col>
                </el-row>
              </div>
            </div>
            <div class="right-center-content">
              <div class="header">
                <el-row>
                  <el-col :span="4">
                    <p>设备数据</p>
                  </el-col>
                  <el-col :span="20">
                    <div class="header-img"></div>
                  </el-col>
                </el-row>
              </div>
              <div class="content">
                <div class="content-top">
                  <el-row :gutter="20">
                    <el-col :span="6">
                      <countTo class="count-1" v-if="+deviceData.channelSumCount > 0" :startVal="deviceNumStr" :endVal="+deviceData.channelSumCount" :duration="3000"></countTo>
                      <div class="count-1"  v-else>{{deviceData.channelSumCount}}</div>
                      <p>设备数量</p>
                    </el-col>
                    <el-col :span="6">
                      <countTo class="count-2" v-if="+deviceData.channelCount > 0" :startVal="channelNumStr" :endVal="+deviceData.channelCount" :duration="3000"></countTo>
                      <div class="count-2"  v-else>{{deviceData.channelCount}}</div>
                      <p>球机/半球机数量</p>
                    </el-col>
                    <el-col :span="6">
                      <countTo v-if="+deviceData.offlineRate > 0 && +deviceData.offlineRate <100" class="count-3" :startVal="offlineRateStr" :decimals="2" :endVal="+deviceData.offlineRate" :duration="3000"></countTo>
                      <div class="count-3" style="display: inline-block;" v-else>{{deviceData.offlineRate}}</div>
                      <span style="color:#ff2626; font-size: 24px;font-family:'PangMenZhengDao';">%</span>
                      <p>设备离线率</p>
                    </el-col>
                    <el-col :span="6">
                      <countTo class="count-4" v-if="+deviceData.percentageRate > 0 && +deviceData.percentageRate <100" :startVal="percentageRateStr" :decimals="2" :endVal="+deviceData.percentageRate" :duration="3000"></countTo>
                      <div class="count-4" style="display: inline-block;" v-else>{{deviceData.percentageRate}}</div>
                      <span style="color:#00efff;font-size: 24px;font-family:'PangMenZhengDao';">%</span>
                      <p>录像完整率</p>
                    </el-col>
                  </el-row>
                </div>
                <div class="bar" id="bar"></div>
              </div>
            </div>
            <div class="right-bottom-content">
              <div class="header">
                <el-row>
                  <el-col :span="4">
                    <p>订单列表</p>
                  </el-col>
                  <el-col :span="20">
                    <div class="header-img"></div>
                  </el-col>
                </el-row>
              </div>
              <div class="table-table">
                <el-row>
                  <el-col :span="14">
                    <p>地址</p>
                  </el-col>
                  <el-col :span="4" style="opacity: 0.6;font-size: 12px;">
                    <p>销量</p>
                  </el-col>
                  <el-col :span="6" style="text-align:end;opacity: 0.6;font-size: 12px;">
                    <p>日期</p>
                  </el-col>
                </el-row>
              </div>
              <div class="content">
                <vue-seamless-scroll :data="alarmData" class="seamless-warp" :class-option="classOption">
                    <ul>
                      <li class="DataList_top" v-for="(item,index) in alarmData" :key="index">
                        <el-row>
                          <el-col :span="14">
                            <p>{{item.name}}</p>
                          </el-col>
                          <el-col :span="4" style="opacity: 0.6;font-size: 12px;">
                            <p>{{item.value}}</p>
                          </el-col>
                          <el-col :span="6" style="text-align:end;opacity: 0.6;font-size: 12px;">
                            <p>{{item.time}}</p>
                          </el-col>
                        </el-row>
                      </li>
                    </ul>
                </vue-seamless-scroll>
              </div>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import ScaleBox from '@/components/scale_box';
import vueSeamlessScroll from 'vue-seamless-scroll'
import * as echarts from 'echarts';
import 'echarts-gl';
import countTo from 'vue-count-to';
const shandongJson = require('./js/shandong-area')
echarts.registerMap('shandong', shandongJson)
export default {
  components: {
    countTo,
    vueSeamlessScroll,
    ScaleBox
  },
  data () {
    return {
      psdOpendialog:false,
      intevalID:null,
      // userName: mUtils.getStore('userInfo').name,
      clickMenuIndex:0,
      locationHost: window.location.origin,
      menuList:[],
      dataList: [],
      geoCoordMap:{
         "济南市":[117.31935455868401, 36.75203069188615],
         "东营市":[118.66016251629284, 37.42110422115865],
         "菏泽市":[115.4798563731728, 35.23904370760091],
         "威海市":[122.12056064576934, 37.51178592770231],
         "烟台市":[121.43172067644802, 37.06366333031291],
         "滨州市":[118.02235604702042, 37.3784731157343],
         "德州市":[116.31027297830168, 37.44295581899174],
         "青岛市":[120.38258384377774, 36.56506753161324],
         "潍坊市":[119.16239923201515, 36.70385771880752],
         "淄博市":[118.05823493709174, 36.80430856346772],
         "泰安市":[117.12238503514725, 36.05295296616621],
         "聊城市":[115.98455035188512, 36.4552481870342],
         "日照市":[119.55126243147176, 35.39255344996701],
         "临沂市":[118.34804042739705, 35.05042493461895],
         "济宁市":[116.58919978222104, 35.412855370184126],
         "枣庄市":[117.33137561523205, 34.81264602477674]
      },
      tenantData:{
        count:0,
        yestCount:0,
        echartData:[],
      },
      img:require('./img/img_dialog.png'),
      defoultImg:require('./img/img_no_pic.png'),
      deviceData:{
        channelSumCount: 0,
        onlineChannelCount: 0,
        offlineChannelCount: 0,
        channelCount: 0,
        offlineRate: 0,
        percentageRate: 0
      },
      tenantNumStr:0,
      yestTenantNumStr:0,
      deviceNumStr:0,
      channelNumStr:0,
      offlineRateStr:0,
      percentageRateStr:0,
      alarmData:[],
      AIData:[],
    }
  },
computed: {
  classOption () {
    return {
      step: 0.5, // 数值越大速度滚动越快
      limitMoveNum: 2, // 开始无缝滚动的数据量 this.dataList.length
      hoverStop: true, // 是否开启鼠标悬停stop
      direction: 1, // 0向下 1向上 2向左 3向右
      openWatch: true, // 开启数据实时监控刷新dom
      singleHeight: 0, // 单步运动停止的高度(默认值0是无缝不停止的滚动) direction => 0/1
      singleWidth: 0, // 单步运动停止的宽度(默认值0是无缝不停止的滚动) direction => 2/3
      waitTime: 1000 // 单步运动停止的时间(默认值1000ms)
    }
  },
},
  mounted(){
    this.getMenuData()
    this.getMapData()
    this.getTenantData()
    this.getDeviceData()
    this.getAlarmData()
    this.getAIData()
    // 轮询
    // this.intevalID = setInterval(() => {
    //   // this.getMapData()
    //   this.getTenantData()
    //   this.getDeviceData()
    //   this.getAlarmData()
    //   this.getAIData()
    // }, 30000);
    // console.log(window.location.host)
  },
  beforeDestroy(){
    this.$once("hook:beforeDestroy", () => { clearInterval(this.intevalID)});
  },
  methods: {
    getMenuData(){
      let data = [
        {menuName:'公司管理',menuUrl:'/system/Dept'},
        {menuName:'商品管理',menuUrl:'/goods/Goods'},
        {menuName:'交易订单',menuUrl:'/pay/Order'},
        {menuName:'用户管理',menuUrl:'/system/user'},
        {menuName:'菜单管理',menuUrl:'/system/Module'},
        {menuName:'权限管理',menuUrl:'/system/Permission'},
      ]
      this.menuList = data
      // menuInfoIFS().then(res => {
      //   if(res.code == 1000){
      //    
      //   }
      // })
    },
    getMapData(){
      this.dataList = []
      let data = [
        {name:'德州市',value:'0'},
        {name:'济南市',value:'69'},
        {name:'青岛市',value:'1'},
        {name:'烟台市',value:'1'},
        {name:'泰安市',value:'1'},
        {name:'威海市',value:'30'},
      ]
      this.dataList = data
      this.initMapEchart()
      // areaInfoIFS().then(res =>{
      //   if(res.code == 1000){
      //     this.dataList = []
      //     res.data&& res.data.length > 0 &&
      //     res.data.forEach(item => {
      //       this.dataList.push({name:item.areaName,value:item.channelCount})
      //     })
      //     this.initMapEchart()
      //   }
      // })
    },
    initMapEchart(){
      var chartDom = document.getElementById('map');
      let img05 = 'path://d="M10,1A10,10,0,0,0,0,11C0,15.92,8.06,25.35,9.69,27.2a.37.37,0,0,0,.57,0c1.62-1.85,9.68-11.28,9.68-16.23A10,10,0,0,0,10,1Zm0,14.9a4,4,0,1,1,4-4A4,4,0,0,1,10,15.9Z"';
      console.log(chartDom)
      var myChart = echarts.init(chartDom);
      var that = this
      var convertData = function(data) {
        var res = [];
        for (var i = 0; i < data.length; i++) {
          var geoCoord = that.geoCoordMap[data[i].name];
          if (geoCoord) {
            res.push({
              name: data[i].name,
              value: geoCoord.concat(data[i].value)
            });
          }
         }
        return res;
      }; 
      var option = {
        backgroundColor: 'rgb(128,128,128,0)',
        tooltip: {
          trigger: "item",
          position: "top",
          padding: 0,
          borderWidth:0,
          axisPointer: { // 坐标轴指示器，鼠标放上时变阴影，而不是线
            type: '' // 默认为直线，可选为：'line' | 'shadow'
          },
          formatter: function (params) {
            if(!params.value)return
            return  `<div style="background-image: url('${that.img}');background-repeat:no-repeat;background-size: 100%;width:120px;height:120px;padding:15px 20px;text-align:center" >
                      <h1 style='font-size:16px;'>${params.name}</h1>
                      <p style="font-size: 20px; font-family:'PangMenZhengDao';color:#ffae00">${params.value[2]}</p>
                      <p style="font-size:14px;">分销商数量</p>
                    </div>`;
          },
          backgroundColor:'transparent',
          textStyle: {
            color: "#FFFFFF",
            textalign: "center",
            fontSize: "16px",
          },
        },
        geo3D: {
          map: 'shandong',
          roam: true,
          itemStyle: {
            color: "transparent", //地图颜色
          },
          viewControl: {
            projection: 'perspective',
              center: [0, 0, 0],
              alpha: 60,//上下旋转角度
              beta: 0,//左右旋转角度
              distance: 95,//视角到主题距离
              rotateSensitivity: 4,//旋转操作的灵敏度
              zoomSensitivity: 1.5, //缩放操作的灵敏度
              panSensitivity: 4,// 平移操作的灵敏度
            },
          },
        series: [
          {
            name: '山东',
            map: 'shandong',
            type: "scatter3D",
            coordinateSystem: "geo3D",
            symbol: img05,
            // symbolOffset:[100,110],
            symbolSize: [25,35],
            regionHeight: 4, //地图高度
            data: convertData(that.dataList),
            itemStyle: {
              color: '#ffae00',
            },
            // itemStyle: {
            //   color: "rgba(100,149,237,.3)", //地图颜色
            //   borderColor: "rgba(0,255,255,1)",
            //   shadowColor: "rgba(0,54,255, 1)",
            //   shadowBlur: 150,
            //   borderWidth: 1, //分界线wdith
            // },
            label: {
              normal: {
                show: false,
              },
              emphasis: {
                label: {
                  show: false, //是否显示高亮
                }
              },
            },
          },{
            name: "山东",
            type: "map3D",
            selectedMode: 'single', //地图高亮单选
            regionHeight: 4, //地图高度
            // boxWidth:105,
            map: 'shandong',
            viewControl: {
              projection: 'perspective',
                center: [-2, 0, 0],
                alpha: 60,//上下旋转角度
                beta: 0,//左右旋转角度
                distance: 90,//视角到主题距离
              //  autoRotateDirection: 'ccw',
              //  autoRotateSpeed: 10,
                rotateSensitivity: 4,//旋转操作的灵敏度
                zoomSensitivity: 1.5, //缩放操作的灵敏度
                panSensitivity: 4,// 平移操作的灵敏度
              //  panMouseButton: 'left',
              //  rotateMouseButton: 'right'
              },
            label: {
              normal: {
                show: true,
                color: "#fff",
                fontSize: "14",
              },
              emphasis: {
                label: {
                  show: true, //是否显示高亮
                  textStyle: {
                    color: '#fff', //高亮文字颜色
                  },
                },
              },
            },
            itemStyle: {
              color: "rgba(100,149,237,.3)", //地图颜色
              borderColor: "rgba(0,255,255,1)",
              borderWidth: 1, //分界线wdith
              // shadowColor: "rgba(0,54,255, 1)",
              shadowColor: '#000',
              shadowBlur: 0,
              shadowOffsetX:10,
              shadowOffsetY:10
            },
            data: convertData(that.dataList),
            silent: false, // 不响应和触发鼠标事件
          }
          ]
      }
      option && myChart.setOption(option,true);
    },
    getTenantData(){
      let data = { 
          sumRegisterCount:1132, 
          yesterdayRegisterCount:56,
          basicTenantInfos:[
            { registerCount:'78',cancelCount:'29',createTime:'12/25'},
            { registerCount:'42',cancelCount:'42',createTime:'12/26'},
            { registerCount:'44',cancelCount:'32',createTime:'12/27'},
            { registerCount:'87',cancelCount:'11',createTime:'12/28'},
            { registerCount:'10',cancelCount:'36',createTime:'12/29'},
            { registerCount:'87',cancelCount:'25',createTime:'12/30'},
            { registerCount:'56',cancelCount:'9',createTime:'12/31'}
          ]
        }
      this.tenantData.count = data.sumRegisterCount
      this.tenantData.yestCount = data.yesterdayRegisterCount
      this.tenantData.echartData = data.basicTenantInfos || []
      this.tenantNumStr = this.tenantData.count
      this.yestTenantNumStr = this.tenantData.yestCount
      let registerCount = []
      let cancelCount = []
      let createTime = []
      this.tenantData.echartData.forEach(item =>{
        registerCount.push(item.registerCount)
        cancelCount.push(item.cancelCount)
        createTime.push(item.createTime)
      })
      this.initLineEchart(registerCount,cancelCount,createTime)

      // tenantInfoIFS().then(res =>{
      //   if(res.code == 1000){
      //     this.tenantData.count = res.data.sumRegisterCount
      //     this.tenantData.yestCount = res.data.yesterdayRegisterCount
      //     this.tenantData.echartData = res.data.basicTenantInfos || []
      //     this.tenantNumStr = this.tenantData.count
      //     this.yestTenantNumStr = this.tenantData.yestCount
      //     console.log(this.tenantNumStr)
      //     let registerCount = []
      //     let cancelCount = []
      //     let createTime = []
      //     this.tenantData.echartData.forEach(item =>{
      //       registerCount.push(item.registerCount)
      //       cancelCount.push(item.cancelCount)
      //       createTime.push(item.createTime)
      //     })
      //     this.initLineEchart(registerCount,cancelCount,createTime)
      //   }
      // })
    },
    initLineEchart(registerCount,cancelCount,createTime){
      var chartDom = document.getElementById('line');
      var myChart = echarts.init(chartDom);
      var option = {
        legend: {
          right: 10,
          top: 10,
          itemHeight:10,
          textStyle: {
            color: '#fff',
          }
        },
        grid: {
          top: '20%',
          left: '5%',
          right: '5%',
          bottom: '12%',
        },
        xAxis: [{
          type: 'category',
          axisLine: {
            show: true
          },
          splitArea: {
            color: '#f00',
            lineStyle: {
              color: '#f00'
            },
          },
          axisLabel: {
            color: 'rgba(255,255,255,0.8)',
            fontSize:10,
          },
          splitLine: {
            show: false
          },
          boundaryGap: false,
          data: createTime,
        }],
        yAxis: [{
          type: 'value',
          min: 0,
          // max: 140,
          splitNumber: 4,
          splitLine: {
            show: true,
            lineStyle: {
              color: 'rgba(255,255,255,0.1)'
            }
          },
          axisLine: {
            show: false,
          },
          axisLabel: {
            show: false,
            margin: 20,
            textStyle: {
              color: '#d1e6eb',
            },
          },
          axisTick: {
            show: false,
          },
        }],
        series: [{
          name: '流失用户',
          type: 'line',
          smooth: true, //是否平滑
          showAllSymbol: true,
          symbol: 'circle',
          symbolSize: 8,
          lineStyle: {
            normal: {
              color: "#229aff",
              shadowColor: 'rgba(0, 0, 0, .3)',
              shadowBlur: 0,
              shadowOffsetY: 5,
              shadowOffsetX: 5,
            },
          },
          label: {
            show: true,
            position: 'top',
            textStyle: {
              color: '#229aff',
            },
            formatter: [ ' {c}人', ].join(','),
          },
          itemStyle: {
            color: "#229aff",
            // borderColor: "#fff",
            // borderWidth: 2,
            shadowColor: 'rgba(0, 0, 0, .3)',
            shadowBlur: 0,
            shadowOffsetY: 2,
            shadowOffsetX: 2,
          },
          tooltip: {
            show: false
          },
          areaStyle: {
            normal: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                  offset: 0,
                  color: 'rgba(0,179,244,0.3)'
                },{
                  offset: 1,
                  color: 'rgba(0,179,244,0)'
                }
              ], false),
              shadowColor: 'rgba(0,179,244, 0.9)',
              shadowBlur: 20
            }
          },
          data: cancelCount
        },{
            name: '注册用户',
            type: 'line',
            smooth: true, //是否平滑
            showAllSymbol: true,
            symbol: 'circle',
            symbolSize: 8,
            lineStyle: {
              normal: {
                color: "#00ffd4",
                shadowColor: 'rgba(0, 0, 0, .3)',
                shadowBlur: 0,
                shadowOffsetY: 2,
                shadowOffsetX: 2,
              },
            },
            label: {
              show: true,
              position: 'top',
              textStyle: {
                color: '#00ffd4',
              },
              formatter: [ ' {c}人', ].join(','),
            },
            itemStyle: {
              color: "#00ffd4",
              // borderColor: "#fff",
              // borderWidth: 2,
              shadowColor: 'rgba(0, 0, 0, .3)',
              shadowBlur: 0,
              shadowOffsetY: 2,
              shadowOffsetX: 2,
            },
            tooltip: {
              show: false
            },
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                    offset: 0,
                    color: 'rgba(0,202,149,0.3)'
                  },{
                    offset: 1,
                    color: 'rgba(0,202,149,0)'
                  }
                ], false),
                shadowColor: 'rgba(0,202,149, 0.9)',
                shadowBlur: 20
              }
            },
            data: registerCount,
        }]
      };
      option && myChart.setOption(option,true);
    },
    getDeviceData(){
      let data ={
        channelSumCount:'74',
        channelCount:'66',
        offlineRate:'36.49',
        percentageRate:'12.50',
        onlineChannelCount:'47',
        offlineChannelCount:'27'
      }
      this.deviceNumStr = +data.channelSumCount
      this.channelNumStr = +data.channelCount
      this.offlineRateStr = +data.offlineRate
      this.percentageRateStr = +data.percentageRate
      this.deviceData = data
      let list = [
        { name:'在线设备', value:data.onlineChannelCount,color:['#00abee','#62E6F6']},
        { name:'离线设备', value:data.offlineChannelCount,color:['#f51818','#ee6666']}
      ]
      this.initBarEchart(data,list)
      // deviceIFS().then(res =>{
      //   if(res.code == 1000){
      //     this.deviceNumStr = +res.data.channelSumCount
      //     this.channelNumStr = +res.data.channelCount
      //     this.offlineRateStr = +res.data.offlineRate
      //     this.percentageRateStr = +res.data.percentageRate
      //     this.deviceData = res.data
      //     let list = [
      //       { name:'在线设备', value:res.data.onlineChannelCount,color:['#00abee','#62E6F6']},
      //       { name:'离线设备', value:res.data.offlineChannelCount,color:['#f51818','#ee6666']}
      //     ]
      //     this.initBarEchart(res.data,list)
      //   }
      // })
    },
    initBarEchart(data,list){
      var chartDom = document.getElementById('bar');
      var myChart = echarts.init(chartDom);
      let barWidth = 18;
      let yName = list.map((item) => item.name);
      let xData = list.map((item) => item.value);
      var option = {
          xAxis: {
            max: data.channelSumCount,
            splitLine: {
              show: false
            },
            axisLabel: {
              show: false
            },
            axisTick: {
              show: false
            },
            axisLine: {
              show: false
            }
          },
          grid: {
            top: '14%',
            left: '14%',
            right: '15%',
            bottom: '20%',
          },
          yAxis: [{
            inverse: true,
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              margin: 10,
              textStyle: {
                fontSize: 14,
                color: '#fff'
              }
            },
            data: yName,
          }],
          series: [{ //内
            type: 'bar',
            barWidth,
            legendHoverLink: false,
            symbolRepeat: true,
            silent: true,
            itemStyle: {
              color: (param) =>{
                return {
                  type: 'linear',
                  x: 0,
                  y: 0,
                  x2: 1,
                  y2: 0,
                  colorStops: [{
                    offset: 0,
                    color: param.data.color[0] // 0% 处的颜色
                  },{
                    offset: 1,
                    color: param.data.color[1] // 100% 处的颜色
                  }]
                }
              }
            },
            data: list,
            z: 1,
            animationEasing: 'elasticOut'
          },{ // 背景
            type: 'pictorialBar',
            animationDuration: 0,
            symbolRepeat: 'fixed',
            symbolMargin: '10%',
            symbol: 'roundRect',
            symbolSize: [6, barWidth],
            itemStyle: {
              normal: {
                color: '#56787c',
              }
            },
            label: {
              normal: {
                show: true,
                position: 'right',
                offset: [0, 2],
                distance: 15,
                textStyle: {
                  color: '#7AF8FF',
                  fontSize: 20,
                  fontFamily:'PangMenZhengDao'
                },
                formatter: function(a) {
                  return `${a.value} 台`
                }
              },
            },
            data: xData,
            z: 0,
            animationEasing: 'elasticOut'
          },{ //分隔
            type: 'pictorialBar',
            itemStyle: {
              color: '#000'
            },
            symbolRepeat: 'fixed',
            symbolMargin: 4,
            symbol: 'roundRect',
            symbolClip: true,
            symbolSize: [2, barWidth],
            symbolPosition: 'start',
            symbolOffset: [0, 0],
            data: list,
            z: 2,
            animationEasing: 'elasticOut'
          }]
        };
      option && myChart.setOption(option,true);
    },
    getAlarmData(){
      let data = [
        {name:'金陵寺',value:'304',time:'2021-12-20 14:11:20'},
        {name:'济南市白云湖镇白云中学',value:'402',time:'2021-12-20 10:11:20'},
        {name:'济南市白云湖镇',value:'362',time:'2021-12-22 14:18:21'},
        {name:'济南市白云湖镇白云中学',value:'520',time:'2021-12-23 17:08:20'},
        {name:'杨家庄北',value:'953',time:'2021-12-25 15:17:29'},
        {name:'白云湖',value:'653',time:'2021-12-30 14:09:30'},
      ]
      this.alarmData = data
      // findEventIFS().then(res => {
      //   if(res.code == 1000){
      //     this.alarmData = []
      //     if(res.data && res.data.length > 0){
      //       res.data.forEach(item =>{
      //         this.alarmData.push({name:item.alarmDescribe, time:dayjs(item.alarmTime).format('YYYY-MM-DD HH:mm:ss')})
      //       })
      //     }
      //   }
      // })
    },
    getAIData(){
      let data = [
        {eventAlarmName:'视频遮挡',eventAlarmImgUrl:'',eventAlarmCount:'64'},
        {eventAlarmName:'移动侦测',eventAlarmImgUrl:'',eventAlarmCount:'74'},
        {eventAlarmName:'区域入侵',eventAlarmImgUrl:'',eventAlarmCount:'37'},
        {eventAlarmName:'环境监测',eventAlarmImgUrl:'',eventAlarmCount:'161'}
      ]
      this.AIData = data
      // eventInfoIFS().then(res =>{
      //   if(res.code == 1000){
      //     this.AIData = res.data
      //   }
      // })
    },
    goPath(val){
      this.clickMenuIndex = val
      switch(val){
        case 1:
          this.$router.push('/large_screen');
          break
        case 2:
          this.$emit('updataMenu','businessUser')
          this.menuItem = this.$t('headerPage.ywgl');
          localStorage.setItem('menuItem', this.menuItem);
          this.$router.push('/index');
          break 
        case 3: 
          this.$emit('updataMenu','systemUser')
          this.menuItem = this.$t('headerPage.xtgl');
          localStorage.setItem('menuItem', this.menuItem);
          this.$router.push('/index');
         break  
        case 4: 
          var reg = /\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}/;
          var ip = reg.exec(window.location.host)[0];
          var newUrl = "http://"+ip + ':9070';
          window.open(newUrl, "_blank");
         break  
      }
    },
    goToMenu(val){
      this.$router.push(val);
    }
  }
}
</script>

<style lang="scss" scope>
@font-face{
  font-family:'PangMenZhengDao';
  src:url('./font/PangMenZhengDaoBiaoTiTi-1.ttf');
 }
#data-view {
  overflow: hidden;
  height: 1080px;
  color: #fff;
  overflow-y: auto;
  background-image: url('./img/bg.png');
  background-repeat:no-repeat ;
  background-size: cover;
}
::-webkit-scrollbar {width: 0 !important}
.header_top{
  height: 100px;
  background-image: url('./img/img_header_bg.png');
  background-repeat:no-repeat ;
  background-size: cover;
  .logo1{
    padding: 20px;
    span{
      padding: 0 20px;
      font-size: 40px;
      font-family:'PangMenZhengDao';
      color: #00FFFF;
      // background: linear-gradient(0deg, #BDE1FF 0%, #FFFFFF 100%);
      background-image:-webkit-linear-gradient(bottom,#BDE1FF 50%,#fff 100%); 
      -webkit-background-clip:text; 
      -webkit-text-fill-color:transparent; 
    }
  }
  // .menu_header{
  //   font-size: 20px;
  //   height: 56px;
  //   .menu_header01{
  //     height: 56px;
  //     width: 200px;
  //     background-image: url('./img/img_btn_h.png');
  //     background-repeat:no-repeat ;
  //     background-size: 100%;
  //     text-align: center;
  //     font-weight: bold;
  //     line-height: 55px;
  //   }
  //   .menu_header02{
  //     height: 56px;
  //     width: 200px;
  //     background-image: url('./img/img_btn.png');
  //     background-repeat:no-repeat ;
  //     background-size: 100%;
  //     text-align: center;
  //     line-height: 50px;
  //   }
  //   .menu_header02:hover{
  //     background-image: url('./img/img_btn_h.png');
  //   }
  // }
}
.main-content{
  height: 90%;
  margin: 0 20px;
  background-image: url('./img/img_border_bg.png');
  background-repeat:no-repeat ;
  background-size: 100%;
  .left-content{
    padding: 10px 30px;
    height: 935px;
    .top-content{
      height: 690px;
      .menu{
        padding: 20px 0 20px 20px;
        line-height: 20px;
        width: 220px;
        display: table-cell;
        vertical-align: middle;
        .menu_btn{
          height: 114px;
          width: 114px;
          background-image: url('./img/img_menu_btn.png');
          background-repeat:no-repeat ;
          background-size: 100%;
          text-align: center;
          margin-right:25px ;
          margin-bottom: 20px;
          float: left;
          p{
            width: 40px;
            font-size: 16px;
            line-height: 24px;
            margin-top: 35px;
            margin-left: 35px;
          }
        }
        .menu_btn:hover{
          background-image: url('./img/img_menu_btn_h.png');
        }
      }
      .map{
        height: 680px;
      }
    }
    .bottom-content{
      // padding: 10px 10px 0 20px;
      margin-top: 20px;
      height: 220px;
      background-image: url('./img/img_left_bottom_bg.png');
      background-repeat:no-repeat ;
      background-size: 100% 100%;
      .header{
        p{
          font-size: 20px;
          font-family:'PangMenZhengDao';
          background-image:-webkit-linear-gradient(bottom,#BDE1FF 30%,#fff 100%); 
          -webkit-background-clip:text; 
          -webkit-text-fill-color:transparent; 
        }
        .header-img{
          width: 100%;
          height: 20px;
          // background-image: url('./img/img_title2.png');
          // background-repeat:no-repeat ;
          // background-size: 100%;
        }
      }
      .content{
        padding: 10px 0 0 5px;
        height: 140px;
        .content-1{
          .alarm-img{
            width: 180px;
            height: 100px;
          }
          .alarm-num{
            height: 100px;
            text-align: center;
            background-color: rgba(6,36,108,0.43);
            text-align: center;
          }
        }
      }
    }
  }
  .right-content{
    padding: 0px 20px 0 0px;
    .header{
      p{
        font-size: 20px;
        font-family:'PangMenZhengDao';
        background-image:-webkit-linear-gradient(bottom,#BDE1FF 30%,#fff 100%); 
        -webkit-background-clip:text; 
        -webkit-text-fill-color:transparent; 
        // padding: 10px 0 0 10px;
      }
    }
    .right-top-content{
      background-image: url('./img/img_right_bg.png');
      background-repeat:no-repeat ;
      background-size: 100%;
      padding: 10px 20px;
      height: 310px;
      margin-top: 20px;
      .content{
        // height: 280px;
        .content-left{
          padding: 30px 10px;
          text-align: center;
          p{
            padding: 10px 0;
            opacity: 0.6;
            color: #FFFFFF
          }
          .count{
            color:#00ffd4;
            font-size: 30px;
            font-family:'PangMenZhengDao';
          }
          .yestCount{
            color: #ffae00;
            font-size: 30px;
            font-family:'PangMenZhengDao';
          }
        }
        .line{
          height: 260px;
        }
      }
    }
    .right-center-content{
      background-image: url('./img/img_right_bg.png');
      background-repeat:no-repeat ;
      background-size: 100% 100%;
      padding: 10px 20px;
      height: 280px;
      margin-top: 20px;
      .content{
        // height: 280px;
        .content-top{
          padding: 40px 0 0 0;
          text-align: center;
          p{
            color: #FFFFFF;
            opacity: 0.6;
            padding: 10px 0;
          }
          .count-1{
            color:#00ffd4;
            font-size: 28px;
            font-family:'PangMenZhengDao';
          }
          .count-2{
            color:#ffae00;
            font-size: 28px;
            font-family:'PangMenZhengDao';
          }
          .count-3{
            color:#ff2626;
            font-size: 28px;
            font-family:'PangMenZhengDao';
          }
          .count-4{
            color:#00efff;
            font-size: 28px;
            font-family:'PangMenZhengDao';
          }          
        }
        .bar{
          height: 160px;
        }
      }
    }
    .right-bottom-content{
      background-image: url('./img/img_right_bg.png') ;
      background-repeat:no-repeat ;
      background-size: 100% 100%;
      padding: 10px 20px;
      height: 290px;
      margin-top: 20px;
      .header{
        padding-bottom: 10px;
      }
      .table-table{
        height: 30px;
        line-height: 30px;
      }
      .content{
        height: 190px;
        overflow: hidden;
        .DataList_top{
          background-image: url('./img/img_li_bg.png');
          background-repeat:no-repeat ;
          background-size: 100%;
          height: 40px;
          line-height: 22px;
          padding-left: 10px;
          p{
            text-align: 14px;
            color: beige;
          }
        }
      }
    }
  }
}
</style>
