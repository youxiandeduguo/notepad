<template>
  <div class="title">
    <div class="name">记事本</div>
    <n-button strong secondary round type="info" class="title-button_add" @click="addNote">+</n-button>
  </div>
  <div class="notes-list">
    <div class="notes" v-for="(note, index) in notes" :key="note.id">
      <Timeago :timestamp=note.id></Timeago>
      <textarea 
        v-model="note.text"
        @input="saveNotes"
        placeholder="请添加内容..."
        :style="{'background-color':note.highlight=='0'? '#fbfbfc':'#20B2AA'}"
      >
      </textarea>
      <div>
        <n-button size="small" strong secondary round type="success" @click="highlightNote(index)">标记笔记</n-button>
        <n-button size="small" strong secondary round type="error" @click="removeNote(index)">删除笔记</n-button>
      </div>

      
    </div>
  </div>
</template>

<script lang="ts">
  import {ref} from "vue"
  import { NButton,NTime } from "naive-ui"
  import Timeago from "./Timeago.vue"
  export default {
    name:"Notepad"
  }
</script>

<script lang="ts" setup>
  let notes=ref(JSON.parse(localStorage.getItem("notes")||"0")||[]);
  if(notes !=null){
    notes=ref(notes["_value"])
  }
  // let notes=ref([{id:111,text:'ccc'}]);
  function saveNotes() {
      localStorage.setItem("notes", JSON.stringify(notes))
  }
  function highlightNote(index){
    this.notes[index]['highlight']=(!Number(this.notes[index]['highlight']))
    this.saveNotes()
  }
  function addNote() {
    console.log(notes)
    let getTime=Number(new Date().getTime())
    
    // console.log(JSON.parse(localStorage.getItem("notes")||""))
    const newNote = {
      id: getTime,
      text:"",
      highlight:"0"
    }
    notes.value.push(newNote)
    saveNotes()
  }
  function removeNote(index) {
      this.notes.splice(index, 1)
      this.saveNotes()
  }
  defineExpose({addNote})

</script>


<style>
.title {
  display: flex;
  width: 100;
  align-items: center;

  gap: 10px;

  padding: 15px;

  color: aliceblue;
  margin: 0 auto;
}
.name{
  color: black;
  font-family: "Helvetica Neue", Arial, sans-serif;
}
.notes-list {
  display: flex;
  flex-wrap: wrap;

  width: 100%;
  margin: 0;
}
.notes {
  display: flex;
  flex-direction: column;
  align-items: center;

  margin: 10px;
  width: 27%;

  border: 2px solid rgb(106, 104, 104);
  border-radius: 5px;

  & textarea {
    width: 100%;
    height: 250px;
    padding: 10px;

    box-sizing: border-box;
    resize: none;
    font-size: 15px;
    font-family: "Helvetica Neue", Arial, sans-serif;
    color: #0c0a0a;
    background-color: #fbfbfc;
  }

}
</style>
