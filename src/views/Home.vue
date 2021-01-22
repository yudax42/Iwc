<template>
  <div class="home">
    <form @submit.prevent>
      <input type="file" @change="preview" />
    </form>
    <pre>
      {{ event }}
    </pre>
  </div>
</template>

<script>
export default {
  data() {
    return {
      event: "",
    };
  },
  methods: {
    preview(ev) {
      const file = ev.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => (this.event = e.target.result);
      reader.readAsText(file);
    },
  },
};
</script>