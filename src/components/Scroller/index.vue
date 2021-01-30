<template>
  <div class="wrapper" ref="wrapper">
    <slot></slot>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "scroller",
  props:{
    handleToScroll:{
      type:Function,
      default:function(){}
    },
    handleTouchEnd:{
      type:Funtion,
      default:function(){}
    }
  },
  mounted() {
    var scroll = new BScroll(this.$refs.wrapper, {
      tap: true,
      probeType: 1,
    });

    this.scroll=scroll;

    scroll.on('scroll',(pos)=>{
      this.handleToScroll(pos);
    })

    scroll.on('scrollEnd',(pos)=>{
      this.handleToScrollEnd(pos);
    })
  },
  methods:{
    toScrollTop(y){
      this.scroll.scrollTo(0,y);
    }
  }
};
</script>

<style lang="scss" scoped>
.wrapper {
  height: 100%;
}
</style>