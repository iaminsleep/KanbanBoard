<template>
  <div class="taskboard__item task" :class="[activeTask, taskType]" @dragstart="onDragStart($event, task)" draggable="true">
    <div class="task__body">
      <p class="task__view">{{ this.taskName }}</p>
      <input class="task__input" type="text" :value="task.name" ref="editTaskInput" @keyup.enter="editTask">
    </div>
    <button class="task__edit" type="button" aria-label="Изменить" @click="editTask"></button>
  </div>
</template>

<script>
  export default {
    inject: ['tasks'],
    data() {
      return {
        taskName: this.task.name,
        whichTaskIsActive: null,
      }
    },
    methods: {
      saveChanges() {
        const tasksArray = this.tasks;
        const inputValue = this.$refs.editTaskInput.value;
        if(inputValue !== '') {
          this.taskName = inputValue;
          tasksArray.find(task => task.id === this.whichTaskIsActive).name = this.taskName;
          this.whichTaskIsActive = null;
          localStorage.setItem('tasksItems', JSON.stringify(tasksArray));
        }
      },
      editTask() {
        if(this.whichTaskIsActive === null) this.whichTaskIsActive = this.task.id;
        else {
          this.saveChanges();
        }
      },
      onDragStart(e, item) {
        e.dataTransfer.dropEffect = 'move';
        e.dataTransfer.effectAllowed = 'move';
        e.dataTransfer.setData('itemId', item.id);
      }
    },
    computed: {
      activeTask() {
        return this.whichTaskIsActive === this.task.id ? 'task--active' : null;
      },
      taskType() {
        let taskClass = '';
        const taskCategory = this.task.categoryId;
        if(taskCategory === 1) taskClass = null;
        if(taskCategory === 2) taskClass = 'task--processing';
        if(taskCategory === 3) taskClass = 'task--done';
        if(taskCategory === 4) taskClass = 'task--basket';
        return taskClass;
      }
    },
    props: {
      task: Object, 
      type: String
    }
  }
</script>