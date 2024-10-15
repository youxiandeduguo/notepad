<template>
    <div class="count_frame">
        <span style="font-size: 24px; font-variant-numeric: tabular-nums;">
          <n-countdown
            ref="countdownRef"
            :duration="86400000"
            :active="active"
            v-slot="{ time }"
          >
            <span>{{ time.hours }} : {{ time.minutes }} : {{ time.seconds }}</span>
          </n-countdown>
        </span>
          <n-button size="small" type="primary" @click="handleReset">
            重置
          </n-button>
          <n-switch v-model:value="active" size="small" />
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

const active = ref(true);
const countdownRef = ref<CountdownInst | null>();

function handleReset() {
  countdownRef.value?.reset();
}
</script>

<style scoped>
.count_frame{
  background: linear-gradient(135deg, #e0c3fc 0%, #8ec5fc 100%);
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 300px;
  margin: 20px auto;
  color: #2c3e50;
  gap: 10px;

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
</style>