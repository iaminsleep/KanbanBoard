<template>
  <section class="add-task">
    <h2 class="visually-hidden">Добавить задачу</h2>
    <form class="add-task__form" aria-label="Форма добавления задачи" @submit.prevent="addTask">
      <div class="add-task__input-wrapper">
      <label for="add-task">Новая задача</label>
        <input type="text" name="task-name" id="add-task" placeholder="Название задачи..." v-model="newTask" @keyup.enter="addTask" required autocomplete="off">
      </div>
      <add-button></add-button>
    </form>
  </section>
</template>

<script>
  import addButton from './UI/addButton.vue';

  export default {
    inject: ['tasks'],
    components: {addButton},
    data() {
      return {
        newTask: '',
      }
    },
    methods: {
      addTask() {
        if(this.newTask !== '') {
          const taskObj = {
            id: new Date().getTime(), 
            name: this.newTask,
            categoryId: 1,
          }
          this.tasks.push(taskObj);
          localStorage.setItem('tasksItems', JSON.stringify(this.tasks));
          this.newTask = '';
        }
      },
    }
  }
</script>