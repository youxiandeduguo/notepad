<template>
    <div>
        {{str}}
    </div>
</template>


<script lang="ts">
import { defineProps } from 'vue';
export default {
    name:"Timeago"
}

</script>

<script lang="ts" setup>

    const Props=defineProps(['timestamp'])
    let timestamp=Props.timestamp
    let curtime= changeDate(new Date().getTime())
    let pretime= changeDate(timestamp)
    console.log("当前时间")
    console.log(curtime)
    console.log("过去时间")
    console.log(pretime)
    let nume=0
    let str=""
    if(curtime.year!=pretime.year){
        let nume=Number(curtime.year)-Number(pretime.year)
        str=nume.toString()+"年以前"
    }else if(curtime.month!=pretime.month){
        let nume=Number(curtime.month)-Number(pretime.month)
        str=nume.toString()+"月以前"
    }else if(curtime.day!=pretime.day){
        let nume=Number(curtime.day)-Number(pretime.day)
        str=nume.toString()+"天以前"
    }else if(curtime.hour!=pretime.hour){
        let nume=Number(curtime.hour)-Number(pretime.hour)
        str=nume.toString()+"小时以前"
    }else if(curtime.minute!=pretime.minute){
        let nume=Number(curtime.minute)-Number(pretime.minute)
        str=nume.toString()+"分钟以前"
    }else str="刚刚"

    function changeDate(datetimes){
        let date = new Date(datetimes);
        let Y = date.getFullYear();
        let M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1);
        const timer={
            year:Y,
            month:M,
            day:date.getDate(),
            hour:date.getHours(),
            minute:date.getMinutes(),
        } 
        return timer;
    }


</script>