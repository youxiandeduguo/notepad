<template>
    <div class="count_frame">
        <div class="line_frame">
          <span v-show="fold" @click="fold=!fold" style="font-size: 24px; font-variant-numeric: tabular-nums;">
            <n-countdown
              ref="countdownRef"
              :duration="86400000"
              :active="active"
              v-slot="{ time }"
            >
              <span >{{ time.hours }} : {{ time.minutes }} : {{ time.seconds }}</span>
            </n-countdown>
          </span>
          <span class="time-input" v-show="!fold" @click="fold=!fold">23:59:46</span>
          <n-button size="small" type="primary" @click="handleReset">
            重置
          </n-button>
          <n-switch v-model:value="active" size="small" />
        </div>

    </div>
</template>
  

<script lang="ts">
import { ref } from 'vue';
import { NCountdown, NButton, NSwitch, CountdownInst, NCard } from 'naive-ui';
export default {
  name:"Countdowm"
}
</script>


<script setup lang="ts">
let fold=ref(true);
const active = ref(true);
const countdownRef = ref<CountdownInst | null>();

function handleReset() {
  countdownRef.value?.reset();
}
function pick(){
  console.log(fold)
}

</script>

<style scoped>
.count_frame{
  background: linear-gradient(135deg, #e0c3fc 0%, #8ec5fc 100%);
  padding: 15px;
  border-radius: 15px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 300px;
  margin: 20px auto;
  color: #2c3e50;
  gap: 10px;
  flex-flow: column;

}
.line_frame{
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 300px;
  margin: 20px auto;
  color: #2c3e50;
  gap: 10px;

}
.time-input{
  font-weight: bold;
  color: #007bff;
  font-size: 24px; 
}
span {
  font-weight: bold;
  color: #007bff; 
}
.n-button {
  background-color: #007bff; 
  color: white;
}
.n-switch {
  --n-switch-height: 20px;
  --n-switch-width: 40px;
}
.time-select{
  display:inline
}
</style>