<template>
  <n-message-provider>
  <content />
  </n-message-provider>
  <div class="home">
      <span class="leftspan">
        <span class="pend">
          <Weather/><Countdown/>
        </span>
        <span>
          <Calendar/>
        </span>
      </span>
      <span class="rightspan">
        <Notepad ref="the_notepad"/>
      </span>
  </div>
</template>
<script lang="ts">
  import Calendar from "./components/Calendar.vue";
  import { NMessageProvider } from "naive-ui";
  import Countdown from "./components/Countdown.vue";
  import Notepad from "./components/Notepad.vue"
  import Weather from "./components/Weather.vue";
  import { ElContainer } from "element-plus";
  import {ref} from "vue"
  export default {
    name:'app'
  }
</script>

<script setup lang="ts">
  const the_notepad=ref<any>();
  function AddNote() {
    the_notepad.value.addNote()
  }

</script>

<style>
body {
  background-color: #eebe77;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;

  font-family: Avenir, Helvetica, Arial, sans-serif;
}
.home {
  display: flex;
  justify-content: space-between;
  align-items: flex-start; 
  width: 100%; 
  padding: 20px;
  box-sizing: border-box; 
}


.leftspan {
  width: 45%;
  display: flex;
  flex-direction: column; 
}
.pend{
  display: flex;
  flex-direction: row;
}
.rightspan {
  width: 45%;
  display: flex;
  flex-direction: column; 
  justify-content: flex-start;
}
</style>
