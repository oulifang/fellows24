<template>
    <div>
        <!-- 专辑信息 -->
        <div class="album">
            <div class="album-mask" :style="{background: 'url('+albumImg+') no-repeat center/cover'}"></div>
            <div class="album-img">
                <img :src="albumImg" alt="">
            </div>
            <div class="album-info">
                <p class="album-info-title">{{albumTitle}}</p>
                <p class="album-info-author">{{albumAuthor}}</p>
                <div class="album-info-control clearfix">
                    <div class="album-info-control-icon">
                        <i class="icon iconfont icon-shangyishou" @click="prev"></i>
                        <i class="icon iconfont icon-bofang" v-show="!isPlay" @click="play"></i>
                        <i class="icon iconfont icon-zanting" v-show="isPlay" @click="pause"></i>
                        <i class="icon iconfont icon-xiayishou" @click="next"></i>
                    </div>
                    <span @click="toggleList=!toggleList" class="album-info-control-menu">歌单</span>
                </div>
            </div> 
        </div>

        <!-- 歌单 -->
        <transition name="slide">
            <ul class="music-list" v-show="toggleList">
                <li @click="selectMusic(index)" :class="['music-list-item', nowIndex == index?'selected': '']" v-for="(music,index) in musicList" :key="index">
                    <span class="music-list-item-title">{{music.title}}&nbsp;-&nbsp;</span>
                    <span class="music-list-item-author">{{music.author}}</span>
                </li>
            </ul>
        </transition>
        
        <!-- 播放控件 -->
        <div class="audio">
            <audio ref="musicAudio" @play="isPlay=true" @pause="isPlay=false" class="audio-ctrl" :src="musicSrc" autoplay controls></audio>
        </div>
    
    </div>
   
</template>

<script>
import "@/assets/font/iconfont.css";
export default {
  props: ["musicList"],
  data() {
    return {
      nowIndex: -1, // 当前选中歌曲的索引
      albumImg:
        "http://img3.imgtn.bdimg.com/it/u=1039246244,1205520727&fm=27&gp=0.jpg", //专辑的封面
      albumTitle: "", //歌曲名称
      albumAuthor: "", //歌曲歌手
      isPlay: false, // 是否在播放
      toggleList: true, //歌单的是否显示
      musicSrc: "" // 歌曲的URL
    };
  },
  methods: {
    selectMusic(index) {
      this.nowIndex = index;
    },
    play() {
      this.$refs.musicAudio.play();
    },
    pause() {
      this.$refs.musicAudio.pause();
    },
    prev() {
      this.nowIndex--;
      if (this.nowIndex == -1) {
        this.nowIndex = this.musicList.length - 1;
      }
    },
    next() {
      this.nowIndex++;
      if (this.nowIndex == this.musicList.length) {
        this.nowIndex = 0;
      }
    }
  },
  watch: {
    nowIndex() {
      let nowMusic = this.musicList[this.nowIndex];
      this.albumImg = nowMusic.musicImgSrc;
      this.albumTitle = nowMusic.title;
      this.albumAuthor = nowMusic.author;
      this.musicSrc = nowMusic.src;
    }
  }
};
</script>

<style lang="scss" scoped>
.clearfix::after {
  content: "";
  display: block;
  clear: both;
}
.music-list {
  position: fixed;
  bottom: 2rem;
  width: 100%;
  background-color: #2a2929;
  height: 4rem;
  overflow-y: scroll;
  &-item {
    color: #dcdbdb;
    border-bottom: 0.02rem solid #343433;
    padding: 0.2rem;

    &.selected {
      color: #299557;
    }
  }
}

.album {
  display: flex;
  text-align: center;
  position: relative;
  color: #fff;
  &-mask {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    filter: blur(20px);
    z-index: -1;
  }

  &-img {
    flex-grow: 1;
    width: 0;
    margin-left: 0.2rem;
    img {
      width: 100%;
    }
  }
  &-info {
    width: 0;
    flex-grow: 2;
    &-title {
      font-size: 0.5rem;
    }
    &-control {
      position: relative;
      height: 0.9rem;
      line-height: 0.9rem;
      &-icon {
        float: left;
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        .icon {
          font-size: 0.5rem;
        }
      }
      &-menu {
        float: right;
        margin-right: 0.4rem;
      }
    }
  }
}

.slide {
  &-enter {
    transform: translateY(100%);
    &-to {
      transform: translateY(0);
    }
    &-active {
      transition: transform 1s ease;
    }
  }

  &-leave {
    transform: translateY(0);
    &-to {
      transform: translateY(100%);
    }
    &-active {
      transition: transform 1s ease;
    }
  }
}

.audio {
  background: #fff;
  height: 1rem;
  position: fixed;
  bottom: 1rem;
  width: 100%;
  &-ctrl {
    width: 100%;
  }
}
</style>
