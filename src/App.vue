<template>
  <div id="app">
    <img src="./assets/logo.png">
    <player :src="src"></player>
  </div>
</template>

<script type="text/ecmascript-6">
  import Player from './components/player/player'

  export default {
    name: 'App',
    data() {
      return {
        //        sdk: 'http://g.alicdn.com/de/prismplayer/2.5.0/aliplayer-min.js',

//        src: 'http://sk.cri.cn/887.m3u8',
        src: 'http://localhost:8080/hls/qingtian.m3u8'
//        src: 'http://www.meng10xian.com/sound.mp3',
//        src: 'http://ting666.yymp3.com:86/new27/liyugang6/6.mp3',
      }
    },
    components: {
      Player
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
