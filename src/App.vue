<template>
  <div id="app">
    <fu-header />
    <div class="app-podcasts">
      <div
        class="app-podcast"
        v-for="(podcast, index) in podcasts"
        :key="`podcast__${index}`"
        @click="handleClickPodcast(podcast)"
      >
        <fu-podcast
          :image="podcast.urls.logo_image.original"
          :name="podcast.title"
          :active="comparePodcastWithCurrentSelected(podcast)"
        />
      </div>
    </div>
    <div v-if="currentPodcastClips.length" class="app-player">
      <fu-player
        :name="getFirstClip.title"
        :album="getFirstClip.channel.title"
        :url="getFirstClip.urls.high_mp3"
        :image="getFirstClip.channel.urls.logo_image.original"
      />
    </div>
  </div>
</template>

<script>
import FuPlayer from "./components/FuPlayer";
import FuHeader from "./components/FuHeader";
import FuPodcast from "./components/FuPodcast";

export default {
  name: "App",

  components: {
    FuPlayer,
    FuHeader,
    FuPodcast,
  },

  data() {
    return {
      podcasts: [],
      currentPodcast: null,
      currentPodcastClips: [],
    };
  },

  created() {
    fetch("https://api.audioboom.com/channels/recommended")
      .then((res) => res.json())
      .then((data) => {
        this.podcasts = data.body;
      });
  },

  computed: {
    getFirstClip() {
      return this.currentPodcastClips[0];
    },
  },

  methods: {
    async handleClickPodcast(podcast) {
      const { id } = podcast;
      this.currentPodcast = podcast;
      const clips = await this.getAudioClips(id);

      this.currentPodcastClips = clips;
    },

    getAudioClips(id) {
      return fetch(`https://api.audioboom.com/channels/${id}/audio_clips`)
        .then((res) => res.json())
        .then((data) => data.body.audio_clips);
    },
    comparePodcastWithCurrentSelected(podcast) {
      return this.currentPodcast && podcast.id === this.currentPodcast.id;
    },
  },
};
</script>

<style>
body {
  margin: 0;
}

.app-player {
  position: fixed;
  bottom: 0;
  width: 100%;
}

.app-podcasts {
  display: flex;
  flex-wrap: wrap;
  justify-content: start;
  margin-top: 16px;
}

.app-podcast {
  margin-bottom: 32px;
  margin-left: 16px;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0;
}
</style>
