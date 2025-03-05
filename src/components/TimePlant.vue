<template>
  <div class="grass-container">
    <div class="grass" :style="{ height: grassHeight + 'px', transform: `rotate(${windRotation}deg)` }"></div>
    <p>草的高度：{{ grassHeight }}px</p>
  </div>
</template>

<script lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
export default {
  name: "TimePlant"
}
</script>

<script lang="ts" setup>

const grassHeight = ref(10); 


const windRotation = ref(0);


const growGrass = () => {
  if (grassHeight.value < 100) { 
    grassHeight.value += 1; 
  }
};


const windEffect = () => {
  windRotation.value = Math.sin(Date.now() / 200) * 10; 
};


let growIntervalId;
let windIntervalId;


onMounted(() => {
  growIntervalId = setInterval(growGrass, 200); 
  windIntervalId = setInterval(windEffect, 50); 
});

onUnmounted(() => {
  clearInterval(growIntervalId);
  clearInterval(windIntervalId);
});
</script>

<style scoped>
.grass-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-family: Arial, sans-serif;
  background: linear-gradient(to bottom, #87CEEB, #90EE90);
}

.grass {
  width: 10px;
  background: linear-gradient(to bottom, #32CD32, #228B22); 
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 100, 0, 0.5); 
  transition: height 0.2s ease, transform 0.1s ease; 
  position: relative;
}

.grass::before {
  content: '';
  position: absolute;
  top: -10px;
  left: -5px;
  width: 20px;
  height: 20px;
  background: radial-gradient(circle, #32CD32, #228B22); 
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(0, 100, 0, 0.5); 
}

.grass::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: -5px;
  width: 20px;
  height: 10px;
  background: linear-gradient(to bottom, #228B22, #006400); 
  border-radius: 5px;
}
</style>