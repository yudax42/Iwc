<template>
  <div id="nav">
    <img src="./assets/Iwc.png" alt="iptv web player" />
  </div>
  <router-view />
</template>

<style lang="css">
body,
html {
  font-family: "Jura", sans-serif;
  margin: 0;
  padding: 0;
}
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}

#nav {
  background-image: linear-gradient(120deg, #89f7fe 0%, #66a6ff 100%);
  padding: 20px;
}
#nav img {
  width: 342px;
}
</style>
