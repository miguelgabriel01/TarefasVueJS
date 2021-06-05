<template>
  <div id="app">
    <h1>Tarefas</h1>
    <TaskProgress :progress="progress"/>
    <NewTask @taskAdded="addTask"/>
    <TaskGrid
      :tasks="tasks"
      @taskDeleted="deletedTask"
      @taskStateChanged="toggleTaskState"
    />
  </div>
</template>

<script>
import TaskProgress from '../src/components/TaskProgess.vue';
import TaskGrid from '../src/components/TaskGrid.vue';
import NewTask from './components/NewTask.vue';

export default {
  name: 'App',
  components: {
    TaskGrid,
    NewTask,
    TaskProgress,
  },
  data(){
    return{
      tasks:[
        {name: 'Comprar uma camisa',pending: true},
        {name: 'Lavar a bicicleta',pending: false},
        {name: 'Estudar',pending: false},
        {name: 'Ler um livro',pending: true},
      ]
    }
  },
  watch:{
   tasks:{
     deep:true,
     handler(){
        localStorage.setItem('tasks',JSON.stringify(this.tasks))
     }
   }
  },
  created(){
    const json = localStorage.getItem('tasks')
    const array = JSON.parse(json)
    if(Array.isArray(array)){
    this.tasks = array 
    }else{
      this.task = []
    }
  },
  methods: {
    addTask(task){
      const sameName = t => t.name === task.name
      const reallyNew = this.tasks.filter(sameName).length === 0

      if(reallyNew){
        this.tasks.push({
          name:task.name,
          pending: task.pending || true
        })
      }
    },
    deletedTask(i){
      this.tasks.splice(i,1)
    },
    toggleTaskState(i){
      this.tasks[i].pending = !this.tasks[i].pending
    }
  },
  computed:{
    progress(){
      const total = this.tasks.length
      const done = this.tasks.filter(t => !t.pending).length
      return Math.round(done / total * 100 ) || 0
    }
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app h1{
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
}




</style>
