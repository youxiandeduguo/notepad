<template>
  <n-message-provider>
  <content/>
  </n-message-provider>
  <div class="home">
    <span class="leftspan">
      <span class="pend">
        <Weather/><Countdown/>
      </span>
      <span>
        <Calendar/>
      </span>
      <span>
        <!-- <Time_Plant></Time_Plant> -->
      </span>
    </span>
    <span class="rightspan">
      <Notepad ref="the_notepad"/>
    </span>
  </div>
</template>
<script lang="ts">
  export default {
    name:'MainView',
  }
</script>

<script setup lang="ts">
  import Calendar from "../components/Calendar.vue";
  import { NMessageProvider } from "naive-ui";
  import Countdown from "../components/Countdown.vue";
  import Notepad from "../components/Notepad.vue"
  import Weather from "../components/Weather.vue";
  import Time_Plant from "../components/TimePlant.vue";
</script>

<style>


  .home {
  min-height:800px;
  background: linear-gradient(to right,#a1c4fd, #a1c4fd, #c2e9fb);
  background-size: 100% 100%;
  border:'3px solid #B5E2E5';
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
