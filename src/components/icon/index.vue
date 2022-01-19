<template>
  <div class="container">
    <i :class="iconClassName" :style="{'color':color,'font-size':fontSize,'font-weight':fontWeight}"></i>
  </div>
</template>

<script type="text/javascript">
 //import index from '@/components/index.vue';
  export default {
    name: "component_name",
    components: {},
    props:{
      iconClassName:{
        type:String,
        default() {
          return null;
        }
      },
      color:{
        type:String,
        default() {
          return '#606266';
        }
      },
      fontSize:{
        type:String,
        default() {
          return '16px';
        }
      },
      fontWeight:{
        type:Number,
        default() {
          return 400;
        }
      }
    },
    data() {
      return {

      }
    },
    created() {

    },
    mounted() {

    },
    methods:{

    }
  }
</script>

<style  scoped lang="less">
</style>
