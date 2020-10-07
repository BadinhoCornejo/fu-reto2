<template>
  <div class="fu-player">
    <img :src="image" :alt="name" class="fu-player__image" />
    <div class="fu-player__details">
      <p class="fu-player__name">{{ name }}</p>
      <p class="fu-player__album">{{ album }}</p>
    </div>
    <div class="fu-player__play" @click="handlePodcast">
      <i v-show="isPaused" class="fas fa-play"></i>
      <i v-show="!isPaused" class="fas fa-pause"></i>
    </div>
    <div class="fu-player__volume">
      <input
        v-model="volume"
        @input="setVolume"
        class="fu-player__volumeRange"
        type="range"
        min="0"
        max="100"
      />
      <div class="fu-player__volumeIcon" @click="toggleVolume">
        <i v-show="volume > 0" class="fas fa-volume-up"></i>
        <i v-show="volume == 0" class="fas fa-volume-mute"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FuPlayer",

  props: {
    name: {
      type: String,
      required: true,
    },
    album: {
      type: String,
      required: true,
    },
    url: {
      type: String,
      required: true,
    },
    image: {
      type: String,
      required: true,
    },
  },

  data() {
    return {
      audio: new Audio(this.url),
      isPaused: true,
      volume: 50,
    };
  },

  methods: {
    handlePodcast() {
      if (this.isPaused) {
        this.audio.play();
        this.isPaused = false;
      } else {
        this.audio.pause();
        this.isPaused = true;
      }
    },
    setVolume() {
      this.audio.volume = this.volume / 100;
    },
    toggleVolume() {
      if (this.audio.muted) {
        this.volume = 100;
        this.audio.muted = false;
      } else {
        this.volume = 0;
        this.audio.muted = true;
      }

      this.setVolume();
    },
  },
};
</script>

<style scoped>
.fu-player {
  display: grid;
  grid-template-columns: 100px repeat(3, 1fr);
  background: #2196f3;
  width: 100%;
  height: 100px;
}

.fu-player__image {
  width: 100px;
  height: 100px;
  object-fit: cover;
  vertical-align: top;
}

.fu-player__details {
  margin-left: 20px;
  color: #fff;
  align-self: center;
  justify-self: start;
}

.fu-player__name {
  font-weight: bold;
  font-size: 14px;
}

.fu-player__album {
  font-weight: 400;
  font-size: 12px;
}

.fu-player__play {
  align-self: center;
  justify-self: center;
  color: #fff;
  font-size: 20px;
  background: #6abdff;
  height: 60px;
  width: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  cursor: pointer;
}

.fu-player__volume {
  justify-self: end;
  display: flex;
  align-items: center;
  margin-right: 32px;
}

.fu-player__volumeIcon {
  color: #fff;
  font-size: 20px;
  margin-left: 40px;
  cursor: pointer;
}

.fu-player__volumeRange {
  appearance: none;
  outline: none;
  border-radius: 8px;
  height: 6px;
  cursor: pointer;
}
</style>