<template>
  <div class="container">
    <h2>Traditional,<br> Yet Completely New</h2>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide" v-for="(item,index) in imgArr" :key="index">
          <div class="title">{{item.message}}</div>
          <img :src="item.url"  class="swiper-lazy">
          <div class="swiper-lazy-preloader"></div>
        </div>
      </div>
      <div class="swiper-pagination"></div>
      <div class="swiper-button-prev"></div>
      <div class="swiper-button-next"></div>
    </div>
  </div>
</template>

<script type="text/javascript">
 import { Swiper } from '@/assets/js/swiper.min.js';
  export default {
    name: "component_name",
    components: {},
    data() {
      return {
        swiper:null,
        imgArr:[]
      }
    },
    created() {

      this.imgArr = [
        { url:require('@/assets/imgs/picture-1.jpg'), url2x:require('@/assets/imgs/picture-1-2x.jpg'), message:'#01 _ The entrance'},
        { url:require('@/assets/imgs/picture-2.jpg'), url2x:require('@/assets/imgs/picture-2-2x.jpg'), message:'#02 _ A tatami-matted hallway'},
        { url:require('@/assets/imgs/picture-3.jpg'), url2x:require('@/assets/imgs/picture-3-2x.jpg'), message:'#03 _ A guestroom'},
        { url:require('@/assets/imgs/picture-4.jpg'), url2x:require('@/assets/imgs/picture-4-2x.jpg'), message:'#04 _ The ochanoma lounge'},
        { url:require('@/assets/imgs/picture-5.jpg'), url2x:require('@/assets/imgs/picture-5-2x.jpg'), message:'#05 _ Reception'},
        { url:require('@/assets/imgs/picture-6.jpg'), url2x:require('@/assets/imgs/picture-6-2x.jpg'), message:'#06 _ The entrance garden'}
      ] 
    },
    mounted() {
      console.log(this.imgArr)
      this.initSwiper()
      window.onresize=function(){
        this.swiper.update();
      }
    },
    methods:{
      initSwiper(){
        this.swiper = new Swiper('.swiper-container',{
          autoplay: {
            delay: 5000,  
            disableOnInteraction: false,
          },
          speed: 700,
          allowTouchMove: false,
          lazy: {
            loadPrevNext: true,
            loadPrevNextAmount: 3,
          },
          centeredSlides: true,
          spaceBetween : 50,
          slidesOffsetBefore: 40,
          loop: true,
          slidesPerView : 'auto',
          on: {
            slideChangeTransitionEnd: function(){
              console.log(this.slides)
              this.slides.transition(this.params.autoplay.delay+this.params.speed).transform('translate3d(-60px, 0, 0)');
            },
            slideChangeTransitionStart: function(){
              this.slides.transition(this.params.speed).transform('translate3d(0, 0, 0)');
            },
          },
          pagination: {
            el: '.swiper-pagination',
            clickable :true,
            renderBullet: function (index, className) {
              console.log(className)
              return '<div class="' + className + '"><span></span><i></i></div>';
            },
          },
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
          },
        });
      }
    }
  }
</script>
<style>
@import './css/swiper.min.css';
</style>
<style  scoped>
  body {
    margin: 0;
  }
	h2{	
	  font-family: -webkit-pictograph;
	  font-size:30px;
	  font-style:italic;
	  text-align:center;
    }
    .swiper-container {
	    padding-bottom:40px;
    }
	/* .swiper-wrapper{
	} */
    .swiper-slide {
	    width:978px;
		  transition-timing-function:linear;
    }
	@media only screen and (max-width:1200px){
	    .swiper-slide {
	        width:770px;
        }
	}
	@media only screen and (max-width:980px){
	    .swiper-slide {
	        width:471px;
        }
	}
	@media only screen and (max-height:480px){
	    .swiper-slide {
	        width:471px;
        }
	}
	.swiper-slide img{
		width:100%;
		border-radius: 4px;}
	.swiper-slide .title{
      position:absolute;
      transform:rotate(90deg);
      transform-origin:left top;
      left:-3px;
      font-size:11px;
      color: rgb(102, 102, 102);
    }	
    .swiper-button-next, .swiper-button-prev{
      width:86px;
      height:112px;
      background-size:60px 112px;
      margin-top:-56px;
      outline:none;
    }
	.swiper-button-next{
		background-image:url('~@/assets/imgs/cursor-next.png');}
	.swiper-button-prev{
		background-image:url('~@/assets/imgs/cursor-prev.png');}		
	.swiper-pagination-bullet{
		background:none;
		opacity:1;
		margin:0 6px !important;
		width:9px;
		height:9px;
		position:relative;
		outline:none;
		vertical-align:middle;}
	.swiper-pagination-bullet span{
		width:3px;
		height:3px;
		background:#CCC;
		display:block;
		border-radius:50%;
		margin-top:3px;
		margin-left:3px;
	}
	.swiper-pagination-bullet i{
		background:#000;
		height:1px;
		width:20px;
		position:absolute;
		top:4px;
		transform:scaleX(0);
		transform-origin:left;
		z-index:3;
		transition-timing-function:linear;
		}
	.swiper-pagination-bullet-active span,.swiper-pagination-bullet:hover span{
		width:9px;
		height:9px;
		margin-top:0;
		margin-left:0;
		background:#000;
		position:relative;
		z-index:1;
		}
	.swiper-pagination-bullet-active i{
		animation:middle 6s;
		}
	.swiper-pagination-bullet:first-child.swiper-pagination-bullet-active i{
		animation:first 6s;
		}
	.swiper-pagination-bullet:last-child.swiper-pagination-bullet-active i{
		animation:last 6s;
		}				
	@keyframes first{
	  0% {transform:scaleX(0.5);left:0px;}/*091*/
	  100% {transform:scaleX(1);left:2px;} /*0915*/
	}
	@keyframes last{
	  0% {transform:scaleX(0.7);left:-10px;}/*1090*/
	  20% {transform:scaleX(0.3);left:2px;} /*090*/
	  100% {transform:scaleX(0.3);left:0px;} /*090*/
	}		
	@keyframes middle{
	  0% {transform:scaleX(0.7);left:-10px;}/*1091*/
	  20% {transform:scaleX(0.45);left:2px;}/*092*/
	  100% {transform:scaleX(1);left:2px;} /*0913*/
	}	
</style>
