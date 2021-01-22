<template>
  <div class="home">
    <form @submit.prevent>
      <input type="file" @change="preview" />
    </form>
    <pre>
      {{ event }}
      {{ playlist }}
    </pre>
  </div>
</template>

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
        const channel = { name: parse[i].split("|")[1], link: parse[j + 1] };
        playlist.push(channel);
        i = i + 2;
      }
      console.log(playlist);
    },
  },
};
</script>