<template>
    <el-calendar v-model="value" />
</template>
  
  
<script lang="ts">
import { ElCalendar} from "element-plus";
export default {
    name:'Calendar'
}
</script>

<script setup lang="ts">
    import { ref } from 'vue'
    const value = ref(new Date())
</script>



<style>
.is-selected {
color: #1989fa;
}
</style>