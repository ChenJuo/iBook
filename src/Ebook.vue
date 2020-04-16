<template>
  <div class="ebook">
       <title-bar
         :ifTitleAndMenuShow = "ifTitleAndMenuShow"
       ></title-bar>
    <div class="read-wrapper">
      <div id="read">
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
      </div>
    </div>
      <menu-bar
        :ifTitleAndMenuShow = "ifTitleAndMenuShow"
        ref="menuBar"
      ></menu-bar>
  </div>
</template>

<script>
  import MenuBar from '@/components/MenuBar';
  import TitleBar from '@/components/TitleBar';
  import Epub from 'epubjs';
  //const DOWNLOAD_URL = '/static/wxxcx.epub';
  const DOWNLOAD_URL = '/static/394.epub';
  global.ePub = Epub;
  export default {
    name: "Ebook",
    components:{
      MenuBar,
      TitleBar
    },
    data(){
      return{
        ifTitleAndMenuShow:false
      }
    },
    methods: {
      toggleTitleAndMenu(){
        this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
        if(!this.ifTitleAndMenuShow){
          this.$refs.menuBar.hideSetting()
        }
      },
      prevPage(){
        //reation.prev上一页
        if(this.rendition){
          this.rendition.prev()
        }
      },
      nextPage(){
        //reation.next下一页
        if(this.rendition){
          this.rendition.next()
        }
      },
      //电子书的解析和渲染
      showEpub() {
        //生成Book
        this.book = new Epub(DOWNLOAD_URL);
        console.log(this.book);
        //生成Rendition
        this.rendition = this.book.renderTo('read', {
          width: window.innerWidth,
          height: window.innerHeight
        });
        //通过Rendition.display渲染电子书
        this.rendition.display()
      },
    },
    mounted() {
      this.showEpub()
    }
  }
</script>

<style lang="scss" scoped>
  @import "assets/styles/global";
  .ebook{
    position: relative;
    .read-wrapper{
      .mask{
        position: absolute;
        top: 0;
        left: 0;
        z-index: 100;
        width: 100%;
        height: 100%;
        display: flex;
      }
      .left{
        flex: 0 0 px2rem(100);
      }
      .center{
        flex: 1;
      }
      .right{
        flex: 0 0 px2rem(100);
      }
    }
  }
</style>
