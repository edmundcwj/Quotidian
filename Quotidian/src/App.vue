<template>
<v-app>
  <div id="app" data-app>
    <Navbar :key="$route.fullPath" />
    <div class="container">
      <router-view />
    </div>
  </div>
</v-app>
</template>

<script>
import Navbar from "./components/Navbar";
import Home from "./components/Home";

export default {
  name: "app",
  components: {
    Navbar,
    Home,
  },
};
</script>

<style>
nav {
  margin-bottom: 10px;
}
</style>
