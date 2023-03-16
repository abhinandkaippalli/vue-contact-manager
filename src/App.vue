<template>
  <NavBar />
  <RouterView />
</template>

<script>
import NavBar from './components/NavBar.vue';

export default {
    name: "AppVue",
    components: { NavBar }
}
</script>

<style>

</style>
