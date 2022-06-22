<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :btnText="showAddTask?'Close':'Add Task'" :btnColor="showAddTask?'red':'green'"></Header>
    <div v-if="showAddTask">
      <AddTask @add-task="addTask"></AddTask>
    </div>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"></Tasks>
  </div>
</template>

<script>
import { CLOSING } from 'ws';
import Header from './Header.vue';
import Tasks from './Tasks.vue';
import AddTask from './AddTask.vue';

export default {
  name: 'App',
  components:{
    Header,
    Tasks,
    AddTask
  },
  data(){
    return {
      tasks:[],
      showAddTask:false
    }
  },
  created(){
    this.tasks = [
      {
          id:1,
          text:'Doctors appointment',
          day:'March 3rd at 2:30pm',
          reminder:true
        },
        {
          id:2,
          text:'Meeting at school',
          name:'March 4rd at 1:30pm',
          reminder:true
        },
        {
          id:3,
          text:'Food Shopping',
          name:'March 1rd at 12:30pm',
          reminder:false
        }
    ]
  },
  methods:{
    deleteTask(id){
      if(confirm('ays')){
        this.tasks = this.tasks.filter(item=>item.id !== id)
      }
      console.log("newTasks",this.tasks)
    },
    toggleReminder(id){
      console.log("reminder for ",id)
      this.tasks = this.tasks.map((item)=>{
        if(item.id === id){
          item.reminder = !item.reminder;
        }
        return item;
      })
    },
    addTask(taskObj){
      this.tasks.push(taskObj);
    },
    toggleAddTask(){
      this.showAddTask = !this.showAddTask;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>