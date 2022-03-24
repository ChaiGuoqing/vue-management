<template>
  <div class="container">
    <div style="height:60px; width:100%">
      <p>（1）reserve-selection -->仅对 type=selection 的列有效，类型为 Boolean，为 true 则会在数据更新之后保留之前选中的数据（需指定 row-key）</p>
      <br/>
      <p>（2）配合row-key -->行数据的 Key，用来优化 Table 的渲染；在使用 reserve-selection 功能的情况下，该属性是必填的。类型为 String 时，支持多层访问：user.info.id，但不支持 user.info[0].id，此种情况请使用 Function。</p>
    </div>
    <el-button type="text" @click="dialogFormVisible = true">添加</el-button>
    <!--列表-->
    <el-table size="small" @selection-change="selectChange" :row-key="getRowKeys" :data="userData" highlight-current-row v-loading="loading" border element-loading-text="拼命加载中" style="width: 100%;">
      <el-table-column align="center" type="selection" :reserve-selection="true" width="50">
      </el-table-column>
      <el-table-column align="center" sortable prop="deptName" label="公司" width="120">
      </el-table-column>
      <el-table-column align="center" sortable prop="userName" label="用户名" width="120">
      </el-table-column>
      <el-table-column align="center" sortable prop="userRealName" label="姓名" width="120">
      </el-table-column>
      <el-table-column align="center" sortable prop="userMobile" label="手机号" width="120">
      </el-table-column>
      <el-table-column align="center" sortable prop="userSex" label="性别" min-width="50">
      </el-table-column>
      <el-table-column align="center" sortable prop="userEmail" label="邮件" min-width="120">
      </el-table-column>
      <el-table-column align="center" sortable prop="editTime" label="修改时间" min-width="120">
        <template slot-scope="scope">
          <div>{{scope.row.editTime|timestampToTime}}</div>
        </template>
      </el-table-column>
      <!-- <el-table-column align="center" sortable prop="isLock" label="状态" min-width="50">
        <template slot-scope="scope">
          <el-switch :v-model="scope.row.isLock=='N'?nshow:fshow" active-color="#13ce66" inactive-color="#ff4949" @change="editType(scope.$index, scope.row)"></el-switch>
        </template>
      </el-table-column> -->
      <!-- <el-table-column label="操作" min-width="300">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="deleteUser(scope.$index, scope.row)">删除</el-button>
          <el-button size="mini" type="success" @click="resetpwd(scope.$index, scope.row)">重置密码</el-button>
          <el-button size="mini" type="success" @click="dataAccess(scope.$index, scope.row)">数据权限</el-button>
          <el-button size="mini" type="success" @click="offlineUser(scope.$index, scope.row)">下线</el-button>
          <el-button size="mini" type="success" @click="refreshCache(scope.$index, scope.row)">刷新缓存</el-button>
        </template>
      </el-table-column> -->
    </el-table>
    <pagination :total="pageData.total" :pageSize="pageData.pageSize" :pageIndex="pageData.currentPage" @getPageSize="getPageSize" @pagination="getPageIndex" />

    <el-dialog title="收货地址" :visible.sync="dialogFormVisible" width="30%">
      <el-form :model="form" :rules="rules" ref="form" style="width:60%;margin:0 auto">
        <el-form-item label="活动名称" prop="name">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="活动区域">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="IP地址" prop="ip">
          <ipInput @validate="validateIP" :ip.sync="form.ip"></ipInput>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script type="text/javascript">
import Pagination from '@/components/Pagination/index.vue';
import ipInput from '@/components/ipInput/src/IPInput.vue';
  export default {
    name: "component_name",
    components: {
      Pagination,
      ipInput
    },
    data() {
      var  validateIP = (rule, value, callback) =>{
        if (!value){
          callback(new Error('请输入ip地址'))
        }else{
          const reg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/
          if ((!reg.test(value))) {
            callback(new Error('请输入正确ip地址'))
          } else {
            callback()
          }
        }
      }
      return {
        loading:false,
        userData:[],
        pageData: {
          currentPage: 1,
          pageSize: 10,
          total: 0
        },
        dialogFormVisible:false,
        form: {
          name: '',
          region: '',
          ip:''
        },
        rules:{
          name:[{required: true, message: '请输入名称', trigger: 'blur' },],
          ip:[{required: true, validator:validateIP, trigger: 'blur' },],
        },
      }
    },
    created() {

    },
    mounted() {
      this.getdata()
    },
    methods:{
      //校验ip
      validateIP() {
        this.$refs.form.validateField('ip');
      },
      // 获取row的key值
      getRowKeys(row) {
        return row.id;
      },
      // 获取数据方法
      getdata() {
        this.loading = true
        // 模拟数据开始
        let res = {
          code: 0,
          count:12,
          data: [
            {
              addUser: '1',
              editUser: '1',
              addTime: null,
              editTime: 1527411068000,
              userId: 1,
              systemNo: 'pmd',
              userName: 'root',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '超级管理员',
              userSex: '女',
              userMobile: '138123456789',
              userEmail: '111@qq.com',
              isLock: 'N',
              deptId: 1,
              deptName: 'xxxx',
              roleId: 1
            },
            {
              addUser: '1',
              editUser: '1',
              addTime: null,
              editTime: 1527410579000,
              userId: 3,
              systemNo: 'mc',
              userName: 'zengzhuo',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '系统管理员',
              userSex: 'M',
              userMobile: '18616988966',
              userEmail: '222@qq.com',
              isLock: 'N',
              deptId: 2,
              deptName: 'xxxx',
              roleId: 101
            },
            {
              addUser: '1',
              editUser: '4',
              addTime: null,
              editTime: 1527411586000,
              userId: 4,
              systemNo: 'ec',
              userName: 'admin',
              userPassword: '59ba8b7dda9ff79186311a5a9fa155ca',
              userRealName: '超级管理员',
              userSex: '女',
              userMobile: '138123456789',
              userEmail: 'huangxuekun@founder.com',
              isLock: 'N',
              deptId: 2,
              deptName: 'xxxx',
              roleId: 3
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526275128000,
              editTime: 1526284402000,
              userId: 28,
              systemNo: null,
              userName: 'eee111',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '123111',
              userSex: '男',
              userMobile: '12354342345',
              userEmail: '111232@qq.com',
              isLock: 'N',
              deptId: 4,
              deptName: 'zxxxxx',
              roleId: 1
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526284533000,
              editTime: 1526284533000,
              userId: 37,
              systemNo: null,
              userName: 'ces',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: 'sesfg',
              userSex: '男',
              userMobile: '12312312312',
              userEmail: '122111111',
              isLock: 'N',
              deptId: 1,
              deptName: 'xxxx',
              roleId: 1
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526285228000,
              editTime: 1526285228000,
              userId: 43,
              systemNo: null,
              userName: '22',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '22',
              userSex: '男',
              userMobile: '222',
              userEmail: '222',
              isLock: 'N',
              deptId: 1,
              deptName: 'xxxx',
              roleId: 1
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526448593000,
              editTime: 1526448593000,
              userId: 58,
              systemNo: null,
              userName: '1',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '1',
              userSex: '女',
              userMobile: '13607118810',
              userEmail: '1@qq.com',
              isLock: 'N',
              deptId: 1,
              deptName: 'xxxx',
              roleId: 1
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526452698000,
              editTime: 1526520341000,
              userId: 60,
              systemNo: null,
              userName: '222222222',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '222222222222',
              userSex: '男',
              userMobile: '13607118810',
              userEmail: '111@qq.com',
              isLock: 'N',
              deptId: 1,
              deptName: 'xxxx',
              roleId: 1
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526452731000,
              editTime: 1526452731000,
              userId: 61,
              systemNo: null,
              userName: '33333333333333',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '4444444444444444444',
              userSex: '女',
              userMobile: '13607118810',
              userEmail: 'qqq@qq.com',
              isLock: 'N',
              deptId: 1,
              deptName: 'xxxx',
              roleId: 2
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526452756000,
              editTime: 1527128981000,
              userId: 62,
              systemNo: null,
              userName: '211111111',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '21111111111',
              userSex: '男',
              userMobile: '13601478451',
              userEmail: '222222@qq.com',
              isLock: 'N',
              deptId: 17,
              deptName: 'v',
              roleId: 2
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526452756000,
              editTime: 1527128981000,
              userId: 62,
              systemNo: null,
              userName: '2111111113',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '21111111111',
              userSex: '男',
              userMobile: '13601478451',
              userEmail: '222222@qq.com',
              isLock: 'N',
              deptId: 17,
              deptName: 'v',
              roleId: 2
            },
            {
              addUser: null,
              editUser: null,
              addTime: 1526452756000,
              editTime: 1527128981000,
              userId: 62,
              systemNo: null,
              userName: '2111111112',
              userPassword: 'e10adc3949ba59abbe56e057f20f883e',
              userRealName: '21111111111',
              userSex: '男',
              userMobile: '13601478451',
              userEmail: '222222@qq.com',
              isLock: 'N',
              deptId: 17,
              deptName: 'v',
              roleId: 2
            }
          ]
        }
        this.loading = false
        this.userData = res.data.slice((this.pageData.currentPage - 1)*this.pageData.pageSize, (this.pageData.currentPage*this.pageData.pageSize))
        this.pageData.total = res.count
      },
      // 选择复选框事件
      selectChange(val) {
        this.selectdata = val
        console.log(val)
      },
      //获取当前点击的页码
      getPageIndex(page) {
        this.pageData.currentPage = page;
        this.getdata()
      },
      // 切换页码数量
      getPageSize(val) {
        this.pageData.currentPage = 1;
        this.pageData.pageSize = val;
        this.getdata()
      },
    }
  }
</script>

<style   scoped>
.container{
  background: #fff;
  padding: 20px;
}
.container >>>.el-select{
  width: 100%;
}
</style>
