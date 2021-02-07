<template>
  <div class="home">
    <swiper />
    <div class="section" v-for="(item,index) in songsList" :key="index">
      <div class="section-title">{{item.name}}</div>
      <content-list :contentList="item.list"></content-list>
    </div>
  </div>
</template>

<script>
import Swiper from '@/components/Swiper';
import contentList from '../components/ContentList';
import {findAll, findAllSongSheet} from '@/api/index'
export default {
  name: 'home',
  components: {
      Swiper,
      contentList
  },
  data () {
    return {
      songsList: [
        {name:"歌单",list: []},
        {name:"歌手",list: []}
      ]
    }
  },
  created () {
     this.getSinger();
     this.getSongList();
  },
  methods: {
    getSinger() {
      findAll().then(res => {
        this.songsList[1].list = res.data.slice(0,10);
      }).catch((err) => {
        console.log(err);
      })
    },
    getSongList() {
      findAllSongSheet().then(res => {
        this.songsList[0].list = res.data.slice(0,10);
      }).catch((err) => {
        console.log(err);
      })
    },
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/css/home.scss';
</style>
