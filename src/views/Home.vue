<template>
  <div class="home">
    <div class="container">
      <div class="playlists">
        <div class="holder">
          <form class="import" @submit.prevent>
            <label>Import m3u file : </label>
            <input type="file" @change="preview" />
          </form>
          <input
            class="search"
            v-if="playlist.length > 0"
            type="text"
            v-model="search"
            placeholder="search for a channel"
          />
          <div class="channel" v-for="i in filterchannel" :key="i.link">
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
.home {
  background: linear-gradient(to right, #ffffff 0%, #d6f8ff 100%);
  height: calc(100vh - 70px);
}
.import {
  text-align: left;
  background: #ffffff;
  width: 100%;
  padding: 21px;
}
.container {
  width: 100%;
  overflow: hidden;
}
.container div {
  display: inline-block;
}
.vid {
  width: 100%;
}
.search {
  width: 100%;
  padding: 20px;
  border: none;
  background: black;
  color: #fff;
}
.playlists {
  height: calc(100vh - 80px);
  width: 29%;
  float: left;
  background: #dff8ff;
  overflow-y: auto;
  overflow-x: hidden;
  border-right: 8px solid #a3dcf5;
}
.holder {
  width: 100%;
}
.playlists .channel {
  background: #fff;
  width: 100%;
  display: inline-block;
  font-weight: bold;
  border-bottom: 0.5px solid #eee;
}
.playlists .channel:hover {
  cursor: pointer;
}
.player {
  width: 70%;
  /* height: 20px; */
}
.videoWrapper {
  position: relative;
  padding-bottom: 56.25%; /* 16:9 */
  height: 0;
}
.videoWrapper .video-js {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
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
      playlist: [],
      selected: "",
      search: "",
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
        const channel = {
          name: parse[i].split(",")[1] || "no name",
          link: parse[j + 1],
        };
        playlist.push(channel);
        i = i + 2;
      }
      this.playlist = playlist;
    },
  },
  computed: {
    filterchannel() {
      return this.search
        ? this.playlist.filter((pl) => pl.name.match(RegExp(this.search, "i")))
        : this.playlist;
    },
    playerOptions() {
      console.log("hhfdsaf", this.selected);
      return {
        // videojs and plugin options
        height: "900",
        width: "1413",
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