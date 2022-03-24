
<!--
<el-form-item :label="$t('platform.ip')" prop="ip">
	<my-ip-address :ip.sync="platform.ip" @validate="validateIP"></my-ip-address>
</el-form-item>
验证规则：
	roules: [
		ip: [
				{ validator: Validator.requiredVal },
				{ validator: Validator.checkIP }
		]
	]
验证触发
	validateIP(value) {
		this.$refs['platform'].validateField('ip');
	},
校验ip值：每个input里只能输入数值，且数值大小为0-255
按下箭头左会跳转到前一个input，当backspace把数值删完后也会向前一个input跳转
按下箭头右、空格、enter键、冒号会向下一个input跳转
防止用户输入空值，当input失去焦点时，自动赋值为0
-->

<template>
  <div>
    <ul :class="isEdit? 'ipAddress-disable':'ipAddress'">
      <li
        v-for="(item,index) in ipAddress"
        :key="index"
      >
        <input
          ref="ipInput"
          :class="isEdit? 'ipInputClass-disable':'ipInputClass'"
          v-model="item.value"
          type="text"
          @keyup="keyup(index, $event)"
          @focus="focus(item)"
          @blur="blur(index)"
          :disabled="isEdit"
        >
        <span v-show="(ipAddress.length - 1) !== index" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ip-input',
  props: {
    ip: {
      type: String,
      required: false,
      default: ''
    },
    isEdit:{
      type:Boolean,
      default: false
    }
  },
  
  data() {
    return {
      ipAddress: [
        {
          value: ''
        },
        {
          value: ''
        },
        {
          value: ''
        },
        {
          value: ''
        }
      ],
      ipValue: this.ip,
      exceptKey: '=37=38=39=40=17=18=9=8=46=33=34=35=36=144=16=13=',
      cursorPosition_down: -1,
      cursorPosition_up: -1,
      isFocus: false
    }
  },

  watch: {
    ip: {
      handler: function(val) {
        this.ipValue = val
        this.setIpAddress()
      }
    }
  },
  mounted() {
    this.setIpAddress()
  },
  methods: {
    keyup(index, e) {
      let valLength=this.ipAddress[index].value.length
      const keycode = e.keyCode
      this.cursorPosition_up = -1
      var val = this.$refs.ipInput[index].value.toString()
        if (this.exceptKey.indexOf('=' + keycode + '=') < 0) {
        val = parseInt(val.replace(/\D/g, ''))
        if (val > 255) val = 255
        this.ipAddress[index].value = val === 0 ? val : (val || '')
        this.checkAndTab(index, keycode)
      }
      if (keycode === 110||valLength==3) {
        if (val !== '') this.tabLeftRight('right', index)
      }
      else if (
        keycode === 37 ||
				keycode === 39 ||
				keycode === 8 ||
				keycode === 46
      ) {
        this.checkAndTab(index, keycode)
      }
    },
    blur(index) {
      this.isFocus = false
      let val = this.ipAddress[index].value.toString()
      val = val.replace('.', '')
      this.ipAddress[index].value = val
      const ipValue = this.getIpValue()
      // 如果焦点不在IP框上面
      setTimeout(() => {
        if (!this.isFocus) {
          this.$emit('update:ip', ipValue)
          this.$emit('validate')
        }
      }, 10)
    },
    focus() {
      this.isFocus = true
    },
    getCursorPosition(obj) {
      if (obj.selectionStart !== undefined) {
        return obj.selectionStart
      } else {
        var rang = document.selection.createRange()
        rang.moveStart('character', -obj.value.length)
        return rang.text.length
      }
    },
    tabLeftRight(LR, index) {
      if (LR === 'left') {
        index--
        index = index < 0 ? 3 : index
      } else if (LR === 'right') {
        index++
        index = index >= 4 ? 0 : index
      }
      this.$refs.ipInput[index].focus()
    },
    checkAndTab(index, keycode) {
      const obj = this.$refs.ipInput[index]
      this.cursorPosition_up = this.getCursorPosition(obj)

      if (this.cursorPosition_down === this.cursorPosition_up) { // 37左；39右；8退格；46删除
        if (keycode === 37 || keycode === 8) {
          this.tabLeftRight('left', index)
        } else if (keycode === 39) {
          this.tabLeftRight('right', index)
        } else {
          this.tabLeftRight('right', index)
        }
      }
    },
    setIpAddress() {
      if (this.ipValue) {
        const arr = this.ipValue.split('.')
        arr.forEach(function(element, index) {
          this.ipAddress[index].value = element
        }, this)
      } else {
        this.ipAddress.forEach(function(obj) {
          obj.value = ''
        })
      }
    },
    getIpValue() {
      const arr = []
      this.ipAddress.forEach(function(element) {
        if(element.value) arr.push(element.value)
      })
      return arr.join('.')
    }
  }
}
</script>

<style  scoped>
.ipAddress {
	display: flex;
	align-items: center;
	border: 1px solid #e4e7ed;
	border-radius: 4px;
	width: 100%;
	margin-right: 10px;
	list-style: none;
	padding: 0;
	margin: 0;
	/* background: white; */
}
.ipAddress-disable{
  display: flex;
	align-items: center;
	border: 1px solid #e4e7ed;
	border-radius: 4px;
	width: 100%;
	margin-right: 10px;
	list-style: none;
	padding: 0;
	margin: 0;
  background-color:'#f5f7fa'
}
.ipAddress:hover {
	border: 1px solid #C0CCDA;
}
.ipAddress li {
	position: relative;
	height: 32px;
	width: 25%;
	margin: 0;
}
ul[class='ipAddress'] input[type='text'] {
	/* position: absolute; */
	border: none;
	width: 100%;
	height: 32px;
	text-align: center;
	background: transparent;
}
.el-input__inner {
	padding: 0 !important;
}
.ipAddress li span {
	position: absolute;
	bottom: 10px;
	right: 0;
	border-radius: 50%;
	background: #C0CCDA;
	width: 2px;
	height: 2px;
}
/* ip4 ui */
.ip4 {
	float: left;
	border: 1px solid #c9c9c9;
	background: #fff;
	width: 200px;
	height: 36px;
}
.ip4 input {
	float: left;
	width: 100%;
	height: 18px;
	line-height: 18px;
	margin-top: 1px;
	padding: 2px 5px 3px;
	border: 0 none;
	background: #fff;
	text-align: center;
}
.ip4 i {
	float: left;
	width: 8px;
	height: 24px;
	line-height: 20px;
	overflow: hidden;
	text-align: center;
}
.table1 .hover td .ip4 i {
	color: #000;
}
.ipInputClass{
  border: none;
  width: 100%;
  height: 30px;
  text-align: center;
  background: transparent;
}
.ipInputClass-disable{
  border: none;
  width: 100%;
  height: 30px;
  text-align: center;
  background-color: #f5f7fa;
  color: #c0c4cc;
  cursor: not-allowed;
}
</style>
