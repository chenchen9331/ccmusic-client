<template>
  <div id="app">
    <the-header />  
    <router-view class="music-content"/>
    <song-audio />
    <the-aside />
    <play-bar />
    <scroll-top />
    <the-footer />   
  </div>
</template>
<script>
import ScrollTop from './components/scrollTop';
import TheFooter from './components/TheFooter';
import TheHeader from './components/TheHeader';
import PlayBar from './components/PlayBar';
import SongAudio from './components/SongAudio';
import TheAside from './components/TheAside';
export default {
  name: 'App',
  components: {
    TheHeader,
    ScrollTop,
    TheFooter,
    SongAudio,
    PlayBar,
    TheAside,
  },
  created () {
    //在页面加载时读取sessionStorage里的状态信息
    if (sessionStorage.getItem("store") ) {
        this.$store.replaceState(Object.assign({}, this.$store.state,JSON.parse(sessionStorage.getItem("store"))))
    } 

    //在页面刷新时将vuex里的信息保存到sessionStorage里
    window.addEventListener("beforeunload",()=>{
        sessionStorage.setItem("store",JSON.stringify(this.$store.state))
    })
  }
}
</script>
,
    SongAudio
<style  lang="scss" scoped>
@import './assets/css/app.scss';
</style>
