<template>
    <div class="search">
        <nav class="searchList-nav" ref="change">
            <span :class="{active : toggle=='Songs'}" @click="handleChangeView('Songs')">歌曲</span>
            <span :class="{active: toggle=='SongLists'}" @click="handleChangeView('SongLists')">歌单</span>
        </nav>
        <component :is="currentView"></component>
    </div>
</template>
<script>
import searchSongs from '../components/search/SearchSongs'
import searchSongLists from '../components/search/SearchSongLists'
export default {
    name: 'search',
    components: {
        searchSongs,
        searchSongLists,
    },
    data() {
        return {
            toggle: 'Songs',
            currentView: 'searchSongs'
        }
    },
    methods: {
        handleChangeView(component) {
            this.currentView = 'search' + component;
            this.toggle = component;
        }
    }
}
</script>
<style lang="scss" scoped>
@import '../assets/css/search.scss';
@import '../assets/css/the-header.scss';
</style>