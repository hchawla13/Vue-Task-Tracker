<template>
    <div :class="`${taskDetails.reminder ? 'reminder' : ''} task`">
        <h3 @dblclick="$emit('toggle-reminder',taskDetails.id)">
            {{taskDetails.text}}
            <i class="fas fa-times" @click="$emit('delete-task',taskDetails.id)"></i>
        </h3>
        <p> {{taskDetails.day}}</p>
    </div>
</template>
<script>
    export default {
        name:'Task',
        props:{
            taskDetails:Object
        },
        
    }
</script>
<style scope>
.fas{
    color:red
}
    .task {
        background :#f4f4f4;
        margin: 5px;
        padding: 10px 20px;
        cursor:pointer;
    }
    .task.reminder {
        border-left : 5px solid green;
    }
    .task h3 {
        display : flex ;
        align-items : center;
        justify-content :space-between ;
    }
</style>