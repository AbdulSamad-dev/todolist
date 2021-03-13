<template> 
<div class="container">
    <Header :showAddTask="showAddTask" @toggle-add-task="toggleAddTask" title="To Do List"/>
    <div v-if="showAddTask"  >
        <AddTask @add-task="addTask" />
    </div>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</div>

</template>

<script>
import Header from './components/Header'
import Tasks from './components/Tasks' 
import AddTask from './components/AddTask' 
export default {
  name: 'App',
  components: {
      Header,
      Tasks,
      AddTask
  },
  data(){
      return {
          tasks: [],
          showAddTask: false
      } 
  },
  methods:{

      toggleAddTask(){
          this.showAddTask = !this.showAddTask
      },
     async addTask(task){
          const res = await fetch('api/tasks', {
              method:'POST',
              headers:{
                  'Content-type': 'application/json'   
              },
              body: JSON.stringify(task)
          })
          const data = await res.json()
          this.tasks = [...this.tasks,data]

      },
     async deleteTask(id)
      {
          if(confirm('Are you sure?'))
          {
              const res = await fetch(`api/tasks/${id}`, {
                  method: 'DELETE',
              })
              res.status ===200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('error deleting task')
          }

      },
       async toggleReminder(id)
      {

        const taskToToggle = await this.fetchTask(id)
          const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}
        
          const res = await fetch(`api/tasks/${id}`, {
              method: 'PUT',
              headers:{'Content-type': 'application/json'},
              body: JSON.stringify(updTask)
          })
        
            const data = await res.json()

         this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: !data.reminder} : task)
          
      },
      async fetchTasks(){
        const res = await fetch('api/tasks')
        const data = await res.json()
        return data
    },
     async fetchTask(id){
        const res = await fetch(`api/tasks/${id}`)
        const data = await res.json()
        return data
    }
  },
 async created(){
      this.tasks = await this.fetchTasks()
  },

}
</script>

<style>
*{
    box-sizing: border-box;
    margin: O;
    padding: 0;
}
html,body{
    font-family: Arial, Helvetica, sans-serif;
    font-size: 65.2;
}
.container{
    max-width:500px;
    margin:30px auto;
    overflow: auto;
    min-height: 300px;
    border: 2px solid slategrey;
    padding:30px;
    border-radius: 5px;
}
#app{
    width: 50%;
    height: 100vh;
    margin:auto;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
img{
    border-radius: 50%;
    border: 5px #333 solid;
    margin-bottom: 1rem;
}
button{
    cursor: pointer;
    display: inline-block;
    background-color: #333;
    color:white;
    font-size: 1rem;
    border:0;
    border-radius: 5px;
    padding: .5rem 1rem;
}
h1,h3{
    margin-bottom: 1rem; 
    font-weight: normal;
   
}
.female{
    border: 5px solid #F521DE;
    background-color: #F521DE;

}
.male{
    border: 5px solid #494868;
    background-color: #494868
}
</style>
