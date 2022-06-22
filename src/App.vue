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
  async created(){
    this.tasks = await this.fetchTasks();
  },
  methods:{
    async deleteTask(id){
      if(confirm('ays')){
        const res = await fetch(`api/tasks/${id}`,{
          headers:{
            'Content-type':'application/json',
          },
          method:'DELETE'
        })
        const data = await res.json()
        console.log("data",data)
        this.tasks = this.tasks.filter(item=>item.id !== id)
        console.log("newTasks",this.tasks)
      }
    },
    async toggleReminder(id){
      console.log("reminder for ",id)
      const taskToToggle = await this.fetchTaskById(id);
      let newTaskList = {...taskToToggle,reminder:!taskToToggle.reminder}
    console.log("newTaskList",newTaskList)
      //writing the updated code to json. 
      const res = await fetch(`api/tasks/${id}`,{
          method:'PUT',
          headers:{
            'Content-type':'application/json'
          },
          body:JSON.stringify(newTaskList),
      })
      this.tasks = this.tasks.map((item)=>{
        if(item.id === id){
          item.reminder = !item.reminder;
        }
        return item;
      })
      
    },
    async addTask(taskObj){
      const res = await fetch('api/tasks',{
        method:'POST',
        headers:{
          'Content-type':'application/json'
        },
        body:JSON.stringify(taskObj)
      })
      const data = await res.json();
      this.tasks.push(data);
    },
    toggleAddTask(){
      this.showAddTask = !this.showAddTask;
    },
    async fetchTasks(){
      return await fetch('api/tasks').then(res=>res.json());
    },
    async fetchTaskById(id){
      return await fetch(`api/tasks/${id}`).then(res=>res.json());
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