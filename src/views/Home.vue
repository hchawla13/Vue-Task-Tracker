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
    import Header from '../components/Header.vue';
    import Tasks from '../components/Tasks.vue';
    import AddTask from '../components/AddTask.vue';
    export default {
        name:'Home',
        components: {
            Header,
            Tasks,
            AddTask,
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
<style scoped>
    h1{
        background: green;
    }
</style>