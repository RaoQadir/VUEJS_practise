<template>
  <div>
    <Userlist />
  </div>
</template>

<script>
import Userlist from "./components/Userlist.vue";

export default {
  name: "App",
  components: {
    Userlist,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
