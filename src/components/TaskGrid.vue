<template>
  <div class="tasks-grid">
    <template v-if="tasks.length">
      <Task
       v-for="(task,i) in tasks" 
       :key="task.name"
       @taskDeleted="$emit('taskDeleted',i)" 
       :task="task" 
       @taskStateChanged="$emit('taskStateChanged',i)"
      ></Task>
    </template>
    <small v-else>Sua vida esta em dia</small>
  </div>
</template>

<script>
import Task from './Task.vue'

export default {
  components:{
    Task,
  },
props:{
  tasks:{
    type:Array,
    required:true,
  }
}
}
</script>

<style>
.tasks-grid{
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.tasks-grid .task{
  margin: 10px;
}
</style>