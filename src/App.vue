<template>
  <MainView></MainView>
</template>
<script lang="ts">
  import MainView from "./views/MainView.vue";
  import {ref} from "vue"
  export default {
    name:'App',
  }
</script>

<script setup lang="ts">
  const the_notepad=ref<any>();
  function AddNote() {
    the_notepad.value.addNote()
  }
</script>
<style scoped>
#app {
display: flex;
flex-direction: column;
align-items: center;

font-family: Avenir, Helvetica, Arial, sans-serif;
}

</style>
