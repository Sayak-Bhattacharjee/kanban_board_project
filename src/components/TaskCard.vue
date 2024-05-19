<template>
  <div :class="getTaskBackground(task.type)" @input="updateContent">
    <div class="cardHeader">
    <div class="headerContent">
      <p class="taskDescription" contenteditable="true"><b>Task:</b> {{task.title}}</p>
      <button class="deleteButton" @click="deleteTask(task.type, task.id)" contenteditable="false">&#128465;</button>
    </div>
    </div>
    <div class="flex mt-4 justify-between items-center">
      <span class="text-sm text-gray-600" contenteditable="true"><b>Due by:</b> {{task.date}}</span><br>
      <span class="text-sm text-gray-600" contenteditable="true"><b>Description:</b> {{task.description}}</span><br>
      <span class="text-sm text-gray-600" contenteditable="true"><b>Assigned to:</b> {{task.assigned || "Not assigned yet"}}</span><br>
    </div>
  </div>
</template>


<script>
export default {
  props: {
    task: {
      type: Object,
      default: () => ({})
    },
    columns: {
      type: Array,
      default: () => ([])
    }
  },
  methods: {
    updateContent(event) {
      this.editableContent = event.target.innerHTML;
    },
    deleteTask: function(columnTitle, taskId){
      this.columns.forEach(element => {
        if(element.title === columnTitle){
          const taskIndexToRemove = element.tasks.findIndex(obj => obj.id === taskId);
          if (taskIndexToRemove !== -1) {
              element.tasks.splice(taskIndexToRemove, 1);
          }
        }
      })
    },
    getTaskBackground(type) {
      const colors = {
        ToDo: 'todoClassCard',
        InProgress: 'inProgressClassCard',
        Done: 'doneClassCard'
      };
      return colors[type] || 'bg-gray-200';
    }
  }

};
</script>
<style scoped>
.cardHeader{
    display: flex;
    justify-content: space-between;
  }
  .headerContent {
      display: flex;
      align-items: center;
    }

    .deleteButton {
        margin-left: auto;
        font-size: xx-large;
        background: none;
        border: none;
        cursor: pointer;
        font-weight: bolder;
        //margin-right: -25vw;
    }
    .taskDescription {
      color: #4a5568;
      font-weight: 600;
      font-family: sans-serif;
      letter-spacing: 0.025em;
      font-size: 0.875rem;
  }
</style>
