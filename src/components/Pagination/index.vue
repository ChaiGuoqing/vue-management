<template>
    <div class="pagination-container">
        <div v-if="showTotal" class="pageTotal"> 共 {{Math.ceil(total/pageCurrentPageSize)}} 页 / {{total}}  条<span></span></div>
        <div v-else></div>
       
        <div style="float: right; display:flex;align-items: center;">
            <div class="el-pagination btn-fore">
                <button type="button" class="btn-prev" @click="toFore" :disabled="pageCurrentIndex==1">首页</button>
            </div>
            <el-pagination
                background
                prev-text="<"
                next-text=">"
                layout="prev, pager, next"
                :pager-count=5
                @current-change="handleCurrentChange"
                :current-page.sync="elPageIndex"
                :page-size.sync="pageSize"
                :total="total">
            </el-pagination>
            <div class="el-pagination btn-fore">
                <button type="button" class="btn-prev" @click="toAft" :disabled="pageCurrentIndex==Math.ceil(this.total==0?1:this.total/this.pageCurrentPageSize)">末页</button>
            </div>
             <!-- 分页设置 -->
            <div style="padding-left:8px" v-if="showImg">
                <el-select class="yesize" v-model="pageCurrentPageSize" @change="updatePageSize">
                    <el-option
                    v-for="item in pagesizes"
                    :key="item"
                    :label="`${item} ` + '条/页'"
                    :value="item">
                    </el-option>
                </el-select>
            </div>
            <div class="pageJump">
                <span>跳至</span>
                <el-input class="yema" v-model="pageCurrentIndex" @change="inputBlur"></el-input>
                <span>页</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MyPagination',
    props: {
        total: {
            required: true,
            type: Number
        },
        pageSize: {
            type: Number,
            default: 15
        },
        pageIndex: {
            type: Number,
            default: 1
        },
        showTotal: {
            type: Boolean,
            default: true
        },
        showImg: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            layout: 'total,  prev, pager, next, jumper',
            background: true,
			pageCurrentTotal:0,
			pageCurrentPageSize:10,
            pageCurrentIndex: 1,
			elPageIndex:1,
			pagesizes:[15,30,45,60,75,100,200]
        };
    },
    created(){},
    watch: {
        pageIndex: {
            handler(newVal, oldVal) {
                this.pageCurrentIndex = newVal;
                this.elPageIndex = newVal;
            },
            immediate: true
        },
        pageSize: {
            handler(newVal, oldVal) {
                this.pageCurrentPageSize = newVal;
            },
            immediate: true
        },
        total: {
            handler(newVal, oldVal) {
                this.pageCurrentTotal = newVal;
            },
            immediate: true
        },
		pageCurrentIndex(newVal, oldVal){
			if(newVal==""){
				// this.pageCurrentIndex = 1;
			}else{
				if(isNaN(parseInt(newVal))){
					this.pageCurrentIndex = 1;
				}else{
					if(parseInt(newVal)==0){
						this.pageCurrentIndex = 1;
					}else{
						this.pageCurrentIndex = parseInt(newVal);
					}
				}
			}
			
		},
    },
    methods: {
        handleCurrentChange(val) {
            this.$emit('pagination', val);
        },
		updatePageSize(val){
			this.$emit('getPageSize', val);
		},
		updataCurrentIndex(){
			this.$emit('pagination', parseInt(this.pageCurrentIndex));
		},
		inputBlur(){
			if(this.pageCurrentIndex==""){
				this.pageCurrentIndex = 1;
			}else{
				if(parseInt(this.pageCurrentIndex)>Math.ceil(parseInt(this.total)/parseInt(this.pageCurrentPageSize))){
					this.pageCurrentIndex = Math.ceil(this.total==0?1:this.total/this.pageCurrentPageSize);
				}
            }
            this.updataCurrentIndex();
        },
        toFore(){
            this.pageCurrentIndex = 1;
            this.updataCurrentIndex();
        },
        toAft(){
            this.pageCurrentIndex = Math.ceil(this.total==0?1:this.total/this.pageCurrentPageSize);
            this.updataCurrentIndex();
        }
    }
};
</script>

<style lang="less" scoped>

/deep/.el-input__inner{
	height: 32px;
}
.pagination-container{
    padding-top: 10px;
}
// 分页总数样式
.pageTotal{
    margin-right: 32px;
    color: #282B33;
    font-size: 14px;
    color:#999;
    display: inline-block;
    line-height: 35px;
}
// 分页设置样式
.yesize{
	width: 105px;
	/deep/.el-input__icon{
		line-height: 32px;
	}
}
// 上下一页样式
/deep/ .el-pagination.is-background .btn-prev,/deep/ .el-pagination.is-background .btn-next{
    background-color: unset;
    height: unset;
    line-height: unset;
    span{
        height: 32px;
        line-height: 32px;
        color: #999;
    }
}
// 分页页码样式
/deep/ .el-pagination.is-background .el-pager li{
    background-color: unset;
    height: 32px;
    line-height: 32px;
    // width: unset;
    color: #999;
    &.active {
        width: 32px;
    }
}
// 首页尾页样式
.btn-fore.el-pagination{
    padding: 0;
    button{
        color: #999;
        padding: 0;
        margin: 0;
        height: 32px;
        line-height: 32px;
        background-color: unset;
    }
}
// 分页跳转样式
.pageJump{
    display:flex;
    align-items:center;
    margin-left:10px;
    color: #999;
}
.yema{
	width: 46px;
    font-size: 14px;
    margin: 0 6px;
	/deep/.el-input__inner{
		padding: 0;
		text-align: center;
        color: #B8BECC;
        background-color: unset;
        min-width: auto;
	} 
}


</style>
