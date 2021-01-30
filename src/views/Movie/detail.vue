<template>
    <div id="detailContrainer" class="slide-enter-active">
        <Header title="影片详情">
            <i class="iconfont icon-left" @touchstart="handleToBack"></i>
        </Header>
        <Loading v-if="isLoading" />
        <!-- <div v-else id="content" class="contentDetail">
            <div class="detail_list">
                <div class="detail_list_bg"></div>
                <div class="detail_list_filter"></div>
                <div class="detail_list_content">
                    <div class="detail_list_img">
                        <img src="/images/龙猫1.jpg" alt="">
                    </div>
                    <div class="detail_list_info">
                        <h2>无名之辈</h2>
                        <p>A Cool Fish</p>
                        <p>9.2</p>
                        <p>剧情,喜剧,犯罪</p>
                        <p>中国大陆 / 108分钟</p>
                        <p>2018-10-16大陆上映</p>
                    </div>
                </div>
            </div>
            <div class="detail_intro">
                <p>该片围绕一把丢失的老枪，讲述了一对低配版的劫匪，一个落魄的保安，一个身体残疾却性格彪悍的毒舌女，这些“无名之辈”身上发生的一系列荒诞故事</p>
            </div>
            <div class="detail_player swiper-container">
                <ul class="swiper-wrapper">
                    <li class="swiper-slide" v-for="data in 10" :key="data">
                        <div>
                            <img src="/images/龙猫1.jpg" alt="">
                        </div>
                        <p>陈建赋</p>
                        <p>马先勇</p>
                    </li>
                </ul>
            </div>
        </div> -->

        <div v-else id="content" class="contentDetail">
            <div class="detail_list">
                <div class="detail_list_bg"></div>
                <div class="detail_list_filter"></div>
                <div class="detail_list_content">
                    <div class="detail_list_img">
                        <img :src="detailMovie.img | setWH('148.208')" alt="">
                    </div>
                    <div class="detail_list_info">
                        <h2>{{detailMovie.nm}}</h2>
                        <p>{{detailMovie.enm}}</p>
                        <p>{{detailMovie.sc}}</p>
                        <p>{{detailMovie.cat}}</p>
                        <p>{{detailMovie.src}} / {{detailMovie.dur}}</p>
                        <p>{{detailMovie.pubDesc}}</p>
                    </div>
                </div>
            </div>
            <div class="detail_intro">
                <p>{{detailMovie.dra}}</p>
            </div>
            <div class="detail_player swiper-container" ref="detail_player">
                <ul class="swiper-wrapper">
                    <li v-for="(item,index) in detailMovie.photos" :key="index" class="swiper-slide">
                        <div>
                            <img :src="item | setHW('140.127')" alt="">
                        </div>
                        <p>陈建赋</p>
                        <p>马先勇</p>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import Header from "@/components/Header";
import axios from "axios"

export default {
  name: "Detail",
  data(){
      return {
          detailMovie:{},
          isLoading:true
      }
  },
  components:{
      Header
  },
  props:['movieId'],
  methods:{
      handleToBack(){
        //   this.$router.back();
        console.log(this.$router.back())
      }
  },
  mounted(){
    //   console.log(this.movieId)
    this.axios.get('/api/detailmovie?movieId='+this.movieId).then((res)=>{
        var msg=res.data.msg;
        if(msg === 'ok'){
            this.isLoading=false;
            this.detailMovie=res.data.data.detailMovie;
            this.$nextTik(()=>{
                new Swiper(this.$refs.detail_player,{
                    slidesPerView:'auto',
                    freeMode:true,
                    freeModeSticky:true
                })
            })
        }
    })
  }
};
</script>

<style scoped>
#detailContrainer{ position:absolute; left:0; top:0; z-index:100; width:100%; min-height:100%;background:white;}
#detailContraner.slide-enter-active{ animation:.3s slideMove;}
@keyframes slideMove{
    0%{ transform:translateX(100%); }
    100%{ transform:translateX(0); }
}
#content.contentDetail{ display:block; margin-bottom:0;}
#content .detail_list{ height:200px; width:100%; position: relative; overflow:hidden;}
.detail_list .detail_list_bg{width:100%; height:100%; background:url(/images/龙猫1.jpg) 0 40%;filter:blur(200px);}
.detail_list .detail_list_filter{ width:100%; height:100%;position:absolute; background-color:#40454d;opacity:0.5;}
.detail_list .detail_list_content{display:flex; width:100%; height:100%; position:absolute; left:0; top:0; z-index:50;}
.detail_list .detail_list_img{width:108px; height:150px; border:1px #f0f2f3 solid; margin:20px;}
.detail_list .detail_list_img img{width:100%;height:100%;}
.detail_list .detail_list_info{margin-top:20px;}
.detail_list .detail_list_info h2{font-size:20px;color:white; font-weight:normal; line-height:40px;}
.detail_list .detail_list_info p{ color:white; line-height:20px; font-size:14px; color:#ccc;}
.swiper-slide div:nth-of-type(1){ width: 100px; height:100px; overflow:hidden; }
.detail_player img{ width: 100%; height: 100%; }
#content .detail_list_intro{ padding:10px;}
#content .detail_player{ margin:20px;}
.detail_player .swiper-slide{ width:70px; margin-right:20px; text-align:center; font-size:14px;}
.detail_player .swiper-slide img{ width:100%; margin-bottom:5px;}
.detail_player .swiper-slide p:nth-of-type(2){ color:#999;}
</style>