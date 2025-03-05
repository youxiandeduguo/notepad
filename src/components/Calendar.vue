<template>
  <section class="date">
    <div class="data-box" :class="{'date-box-unflold':!fold}">
      <div class="tocenter">
        <span class="year-month">
        <span  @click="perMonth"><</span>
        <span>{{ getCurrentDate.year}}年{{ getCurrentDate.month }}月</span>
        <span  @click="nextMonth">></span>
      </span>
      </div>
      
      <div class="date-time">
        <div class="date-select">
          <div class="week-box">
            <span class="week" v-for="(item,index) in foldVisitDate" :key="index" :class="{'current-week':item.select &&fold}">
              <span>{{ weeks[index] }}</span>
              <transition name="include-week-day">
                <span class="include-week-day" v-show="fold">{{ item.day }}</span>
              </transition>
            </span>
          </div>

          <div class="month-everyday-box">
            <transition name="month-everyday">
              <div class="month-everyday" v-show="!fold">
                <span class="" v-for="(i,index) in visitDate" :key="index" @click="selectDate(index)">
                  <span class="day" :class="{select:i.select,notInMonth:!i.notInMonth}">
                    {{i.day}}
                  </span>

                </span>
              </div>
            </transition>
          </div>
          <div class="toggle-btn" @click="foldOrUnfold">
          <button class="btn"></button>
          <Teleport to="body">
            <!-- <div class="shade" v-show="!fold" @click="fold=!fold"></div> -->
          </Teleport>
        </div>
        </div>
      </div>
    </div>
  </section>



</template>
  
<script lang="ts">
import { ref,computed } from 'vue';
import useDate from '../utils/useDate'
export default {
    name:'Calendar'
}
</script>

<script setup lang="ts">
    const weeks=['星期日','星期一','星期二','星期三','星期四','星期五','星期六'];
    const date=useDate().date;
    const formatDate=useDate().formatDate
    const getCurrentDate=computed(()=>formatDate(date.value))
    const getVisitDate=computed(()=>{
      let arr:Date[]=[]
      const{year,month}=formatDate(date.value)
      let firstDay=new Date(year,month-1,1);
      let firstDayWeek=firstDay.getDay()
      const firstVistDay=new Date(firstDay.getTime()-firstDayWeek*24*60*60*1000)
      for(let i=0;i<42;i++){
        let day=new Date(firstVistDay.getTime()+i*24*60*60*1000)
        arr.push(day)
      }
      return arr;
    })

    const visitDate=computed(()=>{
      let arr:any=[]
      // console.log(getCurrentDate)
      for(let i=0;i<getVisitDate.value.length;i++){
        const year=getVisitDate.value[i].getFullYear()
        const month=getVisitDate.value[i].getMonth()+1
        const day=getVisitDate.value[i].getDate()
        // console.log(year,month,day)
        
        // console.log(date.value.getMonth(),date.value.getDate())
        arr.push({
          year,
          month,
          day,
          select:month===date.value.getMonth()+1&&day===date.value.getDate()?true:false,
          notInMonth:month===date.value.getMonth()+1?true:false
        })
      }
      return arr
    })
    const foldVisitDate=computed(()=>{
      for(let i=0;i<6;i++){
        let arr=visitDate.value.slice(i*7,i*7+7)
        console.log(arr)
        for(let j=0;j<arr.length;j++){
          if(arr[j].select){
            return arr
          }
        }
      }
    })

    function selectDate(index:number){
      let day=getVisitDate.value[index]
      date.value=new Date(date.value.setFullYear(day.getFullYear()))
      date.value=new Date(date.value.setMonth(day.getMonth()))
      date.value=new Date(date.value.setDate(day.getDate()))

    }

    function nextMonth(){
      date.value=new Date(date.value.setMonth(date.value.getMonth()+1))
    }

    function perMonth(){
      date.value=new Date(date.value.setMonth(date.value.getMonth()-1))
    }

    let fold=ref<boolean>(true)
    function foldOrUnfold():void{
      fold.value=!fold.value
    }

</script>



<style scoped>
section{
    position: relative;
    color: #007bff;
    font-weight: bold;
    font-family: '微软雅黑';
    width: 100%;
    
}
.date-box{
    position: absolute;
    z-index: 10;
    display: flex;
    flex-flow: column;
    align-items: center;
    height: 100%;
    width: 100%;
    background-color: #a8d8ea;
    border-radius: 20px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    overflow: hidden;
}
.date-box-unfold{
    height: 445px;
}
.tocenter{
  display: flex;
  align-items: center;
  flex-flow: column;
}
.year-month{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 300px;
  margin: 18px 0;
  padding: 5px 10px;
  border-radius: 20px;
  background-color: #e3fdfd;
}

.date-time{
    display: flex;
    width: 100%;
}
.date-select{
    width: 100%;
    padding: 0 20px;
    text-align: center;
}
.week-box{
    display: grid;
    grid-template-columns: repeat(7,70px);
    justify-content: space-between;
}
.week{
    display: flex;
    flex-flow: column;
    padding: 20px 10px;
    border-radius: 20px;
    transition: all 0.3s ease;
}
.include-week-day{
    margin-top: 18px;
}
.include-week-day-enter-from,
.include-week-day-leave-to{
    opacity: 0;
}
.include-week-day-enter-active{
    transition: opacity 0.1s ease;
}
.current-week{
    color: #f5f5f5;
    background-color: #cca8e9;
}
.month-everyday-box{
    overflow: hidden;
}
.month-everyday{
    display: grid;
    grid-template-columns: repeat(7,70px);
    grid-template-rows: auto auto auto auto auto auto;
    grid-row-gap:10px;
    justify-content: space-between;
}
.month-everyday-enter-from,
.month-everyday-leave-to{
    transform: translateY(-300px);
}
.month-everyday-enter-active,
.month-everyday-leave-active{
    transition: all 0.3s ease;
}
.month-everyday>span{
    padding: 5px 0;
}
.day{
    width: 30px;
    line-height: 30px;
    display: inline-block;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.4s;
}
.day.select{
    background-color: #cca8e9;
}
.day.notInMonth{
    color:#ff9999;
}
.time-select{
    display: flex;
    flex-flow: column;
    align-items: center;
    flex: 0 0 200px;
    padding: 0 20px;
}



.btn {
    width: 60px;
    height: 8px;
    border-radius: 20px;
    background-color: #c9d6df;
    transition: transform 0.3s ease;
}

.toggle-btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    margin: 10px 0;
    position: relative;
    
}


.date-box-unfold .btn {
    transform: rotate(180deg);
}


.shade{
    position:absolute;
    z-index: 1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
.date{
    box-sizing: border-box;
    background-color: #a5dee5;
    border-radius: 30px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.05);
    transition: all 0.4s;
}

</style>