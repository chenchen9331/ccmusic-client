<template>
    <div class="song-list-album">
        <div class="album-slide">
            <div class="album-img">
                <img :src="attachImageUrl(tempList.pic)">
            </div>
            <div class="album-info">
                <h2>简介：</h2>
                <span>
                    {{tempList.introduction}}
                </span>
            </div>
        </div>
        <div class="album-content">
            <div class="album-title">
                <p>{{tempList.title}}</p>
            </div>

            <div class="album-score">
                <div class="album-score">
                <div>
                    <h3>歌单评分：</h3>
                    <div>
                        <el-rate v-model="average" disabled></el-rate>
                    </div>                    
                </div>
                <span>{{average*2}}</span>
                <div>
                    <h3>评价：</h3>
                    <div @click="setRank">
                        <el-rate v-model="rank" allow-half show-text></el-rate>
                    </div>                    
                </div>
            </div>
            </div>
            <div class="songs-body">
                <album-content :songList="listOfSongs">
                    <template slot="title">歌单</template>
                </album-content>
                <comment :playId="songListId" :type="1"/>
            </div>        
    </div>
</div>
</template>
<script>
import {mapGetters} from 'vuex';
import {mixin} from '../mixins/index';
import {getSongSheetBySongListId, getSongBySongId, setRank, getRank} from '../api/index';
import AlbumContent from '../components/AlbumContent';
import Comment from '../components/Comment.vue';
export default {
    name: 'song-list-album',
    mixins: [mixin],
    components: {
        AlbumContent,
        Comment
    },
   
    data() {
        return {
            songLists: [],      //当前页面需要展示的歌曲列表
            songListId: '',    //前面传来的歌单id
            average: 0,       //歌单评分
            rank: 0
        }
    },
    computed:{
        ...mapGetters([
            'tempList',         //当前歌单对象
            'listOfSongs',       //当前歌单下的歌曲列表
            'loginIn',           //是否登录
            'userId'
        ])
    },
    created() {
        this.songListId = this.$route.params.id;
        this.getSongId();
        this.getRank(this.songListId);
    },
    methods: {
        getSongId() {
            getSongSheetBySongListId(this.songListId).then(res => {
                if (res.code == 20000) {
                    for (let item of res.data) {
                        getSongBySongId(item.songId).then(res => {
                            if (res.code == 20000) {
                                this.songLists.push(res.data);
                            }
                        })
                    }
                    this.$store.commit('setListOfSongs', this.songLists);
                }
            })
        },
        setRank() {
            if(this.loginIn){
                let params = new URLSearchParams();
                params.append('songListId',this.songListId);
                params.append('consumerId',this.userId);
                params.append('score',this.rank *2);
                setRank(params)
                    .then(res => {
                        if(res.code == 20000){
                            this.notify('评分成功','success');
                            this.getRank(this.songListId);
                        }else{
                            this.notify('评分失败','error');
                        }
                    })
                    .catch(err =>{
                        this.notify('您已经评价过啦','error');
                    })
            }else{
                 this.rank = null;
                 this.$router.push('/login-in');
                 this.notify('请先登录','warning');
            }
        },
        getRank(id) {
            getRank(id).then(res => {
                if (res.code == 20000) {
                    console.log(res.data);
                    this.average = res.data / 2;
                }
            })
        }
    }

}
</script>

<style lang="scss" scoped>
@import '../assets/css/song-list-album.scss';
</style>
