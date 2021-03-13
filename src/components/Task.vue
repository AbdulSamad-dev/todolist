<template>
    <div @click="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder': '', 'task' ]">
        <h3>{{ task.text }}
             <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
         </h3>
         <p>{{ task.day }} </p> 
    </div>
</template>

<script>
    export default{
        name:'Task',
        props:{
            task:Object,
        }
         
    }
</script>

<style scoped>
.fas{
    color:red;
    cursor: pointer;
}
.task{
    display:flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 5px 20px; 
    margin-bottom: 5px; 
    cursor:pointer;
    background: #fbe2f0;
}
.reminder{
    border-left:5px solid green;
}
.task h3{
    display:flex;
    align-items: center;
    justify-content: space-between;

}
</style>