<template>
    <div class="count_frame">
        <div class="line_frame">
          <span v-show="fold" @click="changefold" style="font-size: 24px; font-variant-numeric: tabular-nums;">
            <n-countdown
              ref="countdownRef"
              :duration=counttime
              :active="active"
              v-slot="{ time }"
              :on-finish="handleDiago"
            >
            <span>{{ time.hours }} : {{ time.minutes }} : {{ time.seconds }}</span>
            </n-countdown>
          </span>
          <input v-model="timeinput" class="time-input" v-show="!fold"></input>
          <n-button size="small" type="primary" @click="handleReset" v-show="fold">
            重置
          </n-button>
          <n-button size="small" type="primary" @click="confirm" v-show="!fold">
            确认
          </n-button>
          <n-switch v-model:value="active" size="small" v-show="fold"/>
        </div>

    </div>
</template>
  

<script lang="ts">
import { ref } from 'vue';
import { NCountdown, NButton, NSwitch, CountdownInst, NCard,useMessage,useDialog } from 'naive-ui';
export default {
  name:"Countdowm"
}
</script>


<script setup lang="ts">
let counttime=ref(600000);
let fold=ref(true);
let timeinput=ref('00:10:00')
const active = ref(false);
const countdownRef = ref<CountdownInst | null>();
const message = useMessage()
const dialog = useDialog()

function handleReset() {
  countdownRef.value?.reset();
}
function pick(){
  console.log(fold)
}
function confirm(){
  fold.value=!fold.value;
  changetime()
}
function changefold(){
  if(fold.value==true){
    fold.value=false
  }
}
function error(error_content) {
  message.error(error_content)
}
function handleDiago(){
    dialog.warning({
        title: '警告',
        content: '倒计时结束',
        positiveText: '确定',
      })
}
function changetime(){
  let hour=Number(timeinput.value.slice(0,2))
  let minute=Number(timeinput.value.slice(3,5))
  let second=Number(timeinput.value.slice(6,8))
  console.log(hour,minute,second)
  if(Number.isNaN(hour)||Number.isNaN(minute)||Number.isNaN(second)){
    error("非法的输入")
    counttime.value=600000
  }
  else counttime.value=hour*3600000+minute*60000+second*1000;
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
  width: 180px;
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
.time-input{
  width: 105px;
  height: 33.3px;
}
</style>