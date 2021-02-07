<template>
    <transition name="slide-fade">
        <div class="the-aside" v-if="showAside">
            <h2 class="title">播放列表</h2>
            <ul class="menus">
                <li v-for="(item,index) in listOfSongs" :key="index" :class="{'is-play': id==item.id}"
                @click="toplay(item.id,item.url,item.pic,item.index,item.name,item.lyric)">
                    {{replaceFName(item.name)}}
                </li>
            </ul>
        </div>
    </transition>
</template>
<script>
import {mapGetters} from 'vuex';
import {getCollectOfUserId} from '../api/index'
export default {
    name: 'the-aside',
    computed: {
        ...mapGetters([
            'showAside',         //是否显示播放中的歌曲列表
            'listOfSongs',       //当前歌曲列表
            'id',                //播放中的音乐id
            'loginIn',              //用户是否已登录
            'userId',               //当前登录用户的id
            'isActive',             //当前播放的歌曲是否已收藏
        ])
    },
    mounted(){
        let _this = this;
        document.addEventListener('click',function(){
            _this.$store.commit('setShowAside',false)
        },true);
    },
    methods:{
        //获取名字前半部分--歌手名
        replaceLName(str){
            let arr = str.split('-');
            return arr[0];
        },
        //获取名字后半部分--歌名
        replaceFName(str){
            let arr = str.split('-');
            return arr[1];
        },
        //播放
        toplay: function(id,url,pic,index,name,lyric){
            this.$store.commit('setId',id);
            this.$store.commit('setUrl',this.$store.state.configure.HOST+url);
            this.$store.commit('setPicUrl',this.$store.state.configure.HOST+pic);
            this.$store.commit('setListIndex',index);
            this.$store.commit('setTitle',this.replaceFName(name));
            this.$store.commit('setArtist',this.replaceLName(name));
            this.$store.commit('setLyric',this.parseLyric(lyric));
            this.$store.commit('setIsActive',false);
                if(this.loginIn){
                    getCollectOfUserId(this.userId)
                        .then(res =>{
                            for(let item of res.data){
                                if(item.songId == id){
                                    this.$store.commit('setIsActive',true);
                                    break;
                                }
                            }
                        })
                }
        }
    }
}
</script>
<style  lang="scss" scoped>
@import '../assets/css/the-aside.scss';
</style>
