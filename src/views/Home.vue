<template>
  <div class="home">
    <form @submit.prevent>
      <input type="file" @change="preview" />
    </form>
    <div class="container">
      <div class="playlists">
        <div v-show="playlist.length > 1">
          <div class="channel" v-for="i in playlist" :key="i.link">
            <p>{{ i.name }}</p>
            <!-- <p>{{ i.link }}</p> -->
          </div>
        </div>
      </div>
      <div class="player"></div>
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
  background: blue;
  width: 70%;
  height: 20px;
}
</style>

<script>
export default {
  data() {
    return {
      event: "",
      playlist: "",
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
};
</script>