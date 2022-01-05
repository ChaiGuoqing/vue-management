<template>
  <div :style="style" class="scale">
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name:'ScaleBox',
    props:{
      width:{
          type: Number,
          default: 1920
      },
      height:{
        type: Number,
        default: 1080
      }
    },
    data(){
      return{
        scale:1,
        style:{
          transform: `scale(1) translate(-50%, -50%)`,
          transformOrigin: "left top",
        }
      }
    },
    mounted(){
      window.addEventListener('resize', this.setScale);
      this.setScale();
    },
    methods:{
      getScale(){
         let ww = window.innerWidth / this.width
          let wh = window.innerHeight / this.height
          let scale = ww < wh ? ww : wh;
          return scale;
      },
      setScale(){
        this.scale=this.getScale();
        this.style={
          transform: `scale(${this.scale}) translate(-50%, -50%)`,
          transformOrigin: "left top",
          width:this.width +'px',
          height:this.height  +'px'
        }
      },
      debounce(fn, delay) {
        let delays = delay || 500;
        let timer;
        return  ()=> {
          let th = this;
          let args = arguments;
          if (timer) {
            clearTimeout(timer);
          }
          timer = setTimeout( ()=> {
            timer = null;
            fn.apply(th, args);
          }, delays);
        };
      }
    },
    destroyed(){
      window.removeEventListener('resize', this.setScale)
    }

  }
</script>

<style lang="less" scoped>
.scale{
   display: flex;
  flex-direction: column;
  transform-origin: 0 0;
  position: fixed;
  left: 50%;
  top: 50%;
  transition: 0.3s;
  z-index: 999;
}
</style>