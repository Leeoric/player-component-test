<template>
  <div class="player">
    <ali-player :source="src"
                :isLive="isLive"
                :aliplayerSdkPath="sdk"
                :useH5Prism="useH5Prism"
                :autoplay="autoPlay"
                @ready="onready"
                @play="onplay"
                @pause="onpause"
                @playing="onplaying"
                @waiting="onwaiting"
                @ended="onended"
                @m3u8Retry="onm3u8Retry"
                @liveStreamStop="onliveStreamStop"
                @timeupdate="ontimeupdate"
                @error="onerror"
                style="display: none;"
                ref="player"></ali-player>
    <div class="player-box">
      <div class="player-inner">
        <span class="player-control-btn player-btn-play" @click="triggle">
         <i :class="playBtnTriggle ? 'icon-iconbofangqizanting' : 'icon-iconbofangqibofang'"></i>
         </span>
        <span class="player-tips">{{tips}}</span>
        <span class="player-control-btn play-btn-reload" @click="reload">
          <i class="icon-refresh_light"></i>
        </span>
      </div>
    </div>
    <div class="control-bar">
      <button @click="clickPause">暂停</button>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import VueAliplayer from 'vue-aliplayer'
  export default {
    name: 'player',
    props: {
      src: {
        type: String,
        default: ''
      }
    },
    data () {
      return {
        isLive: true,
        autoPlay: false,
        sdk: 'static/aliplayer-min.js',
        useH5Prism: true,
        tips: 'LIVE',
        status: '',
        playBtnTriggle: true
      }
    },
    watch: {
      status() {
        console.log('%c ---status has changed---' + this.status, 'background:black;color:#fff')
        switch (this.status) {
          case 'init':
            this.tips = '初始化完成'
            break
          case 'ready':
//            this.tips = '准备就绪'
            this.tips = '直播中'
            break
          case 'loading':
//            this.tips = 'loading...'
            break
          case 'play':
//            this.tips = '已停止'
            break
          case 'pause':
//            this.tips = '开始播放'
            break
          case 'playing':
            this.tips = '直播中'
            break
          case 'waiting':
            this.tips = '加载中...'
            break
          case 'error':
            this.tips = '发生错误'
            break
          case 'ended':
            this.tips = '直播结束'
            break
        }
      }
    },
    mounted() {
      console.log('src-----', this.src)
    },
    methods: {
      triggle() {
        if (this.playBtnTriggle) {
          this.clickPlay()
        } else {
          this.clickPause()
        }
        this.playBtnTriggle = !this.playBtnTriggle
      },
      reload() {
        console.log('%c click replay btn ', 'background:green;color:#fff')
        this.$refs.player.instance.replay()
      },
      onready() {
//        this.tips = '准备就绪...'
        console.log('%c onready & get status:---', 'background:#f00;color:#fff', this.$refs.player.instance.getStatus())
        this.status = this.$refs.player.instance.getStatus()
      },
      clickPlay() {
        console.log('%c click play btn ', 'background:green;color:#fff')
        this.$refs.player.instance.play()
      },
      clickPause() {
        console.log('%c clickPause btn ', 'background:green;color:#fff')
        this.$refs.player.instance.pause()
      },
      onplaying() {
//        this.tips = '播放中...'
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c onplaying ', 'background:#f00;color:#fff')
      },
      onplay() {
//        this.tips = '开始播放'
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c onplay ', 'background:#f00;color:#fff')
      },
      onpause() {
        this.tips = '已停止'
        this.playBtnTriggle = true
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c onpause ', 'background:#f00;color:#fff')
      },
      onwaiting() {
        this.tips = '加载中...'
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c onwaiting ', 'background:#f00;color:#fff')
      },
      onended() {
//        this.tips = '已停止'
        this.playBtnTriggle = false
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c onended ', 'background:#f00;color:#fff')
      },
      onliveStreamStop() {
        this.tips = '无直播流'
        this.playBtnTriggle = false
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c onliveStreamStop ', 'background:#f00;color:#fff')
      },
      onm3u8Retry() {
//        this.tips = '重新连接中...'
        this.status = this.$refs.player.instance.getStatus()
        console.log('onm3u8Retry --status: ', this.status)
      },
      ontimeupdate() {
//        console.log(this.$refs.player.instance.getCurrentTime())
        this.status = this.$refs.player.instance.getStatus()
        console.log('%c ontimeupdate ', 'background:#f00;color:#fff', this.$refs.player.instance.getStatus())
      },
      onerror(e) {
        console.log('error', e)
      }
    },
    components: {
      'ali-player': VueAliplayer
    }
  }
</script>

<style scoped lang="scss">
  $player-width: 100%;
  $player-height: 45px;
  $player-bg: #000;
  $player-btn-font-size: 32px;
  $player-btn-font-color: #fff;

  .player {
    .player-box {
      width: $player-width;
      height: $player-height;
      line-height: $player-height;
      background: $player-bg;
      .player-inner {
        display: flex;
        height: 100%;
        color: $player-btn-font-color;
        span {
          display: inline-block;
          height: 100%;
          vertical-align: middle;
        }
        .player-control-btn {
          cursor: pointer;
          font-size: $player-btn-font-size;
          line-height: 150%;
        }
        .player-btn-play {
          flex: 1;
        }
        .player-tips {
          flex: 5;
          text-align: left;
        }
        .play-btn-reload {
          flex: 1;
        }
      }
    }
  }

  .control-bar {
    position: absolute;
    bottom: 0;
    z-index: 9;
  }
</style>
