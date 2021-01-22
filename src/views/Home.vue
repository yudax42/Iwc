<template>
  <div class="home">
    <form @submit.prevent>
      <input type="file" @change="preview" />
    </form>
    <div class="container">
      <div class="playlists">
        <div v-show="playlist && playlist.length > 1">
          <div class="channel" v-for="i in playlist" :key="i.link">
            <p @click="selected = i.link">
              {{ i.name }}
            </p>
            <!-- <p>{{ i.link }}</p> -->
          </div>
        </div>
      </div>
      <div class="player" v-if="selected">
        <video-player
          ref="videoPlayer"
          class="vjs-custom-skin vid"
          :options="playerOptions"
        >
        </video-player>
      </div>
    </div>
  </div>
</template>

<style lang="css" scoped>
.container {
  width: 100%;
  overflow: hidden;
  margin-top: 10%;
}
.container div {
  display: inline-block;
}
.vid {
  width: 100%;
}
.playlists {
  /* background: red; */
  width: 29%;
  /* height: 20px; */
  float: left;
}
.playlists .channel {
  background: #eee;
  border: 0.5px solid #333;
  margin-bottom: 5px;
  padding: 10px;
  margin: 10px;
  width: 100%;
  display: block;
}
.playlists .channel:hover {
  cursor: pointer;
}
.player {
  width: 70%;
  /* height: 20px; */
}
</style>

<script>
import { videoPlayer } from "vue-videojs7";

export default {
  components: {
    videoPlayer,
  },
  data() {
    return {
      event: "",
      playlist: "",
      selected: "",
    };
  },
  methods: {
    preview(ev) {
      const file = ev.target.files[0];
      const reader = new FileReader();
      reader.readAsText(file, "utf-8");
      reader.onload = () => {
        this.event = reader.result;
        this.process();
      };
    },

    process() {
      let parse = this.event.split("\n");
      let i = 1;
      let playlist = [];
      // console.log(parse);
      while (i < parse.length) {
        let j = i;
        const channel = { name: parse[i].split(",")[1], link: parse[j + 1] };
        playlist.push(channel);
        i = i + 2;
      }
      this.playlist = playlist;
    },
  },
  computed: {
    playerOptions() {
      console.log("hhfdsaf", this.selected);
      return {
        // videojs and plugin options
        // height: "360",
        width: "1000",
        sources: [
          {
            withCredentials: false,
            type: "application/x-mpegURL",
            src: this.selected,
            // src: this.selected + ".m3u8",
          },
        ],
        controlBar: {
          timeDivider: false,
          durationDisplay: false,
        },
        flash: { vhs: { withCredentials: true } },
        html5: { vhs: { withCredentials: true } },
        // poster:
        // "https://surmon-china.github.io/vue-quill-editor/static/images/surmon-5.jpg",
      };
    },
    player() {
      return this.$refs.videoPlayer.player;
    },
  },
};
</script>