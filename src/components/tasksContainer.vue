<template>
  <div class="board-app__inner">
    <add-task></add-task>
    <section class="taskboard">
      <h2 class="visually-hidden">Ваши задачи:</h2>
      <article v-for="category in categories" :key="category.id" class="taskboard__group">
        <h3 class="taskboard__group-heading" :class="headingClass(category.id)">{{ category.title }}</h3>
        <div class="taskboard__list taskboard__list--sorted" @drop="onDrop($event, category.id)" @dragover.prevent @dragenter.prevent>
          <div class="taskboard__item task task--empty" v-if="sortedTasks(category.id).length === 0">
            <p>{{ category.id !== 4 ? 'Перетащите карточку' : 'Корзина пуста'}}</p>
          </div>
          <task-item v-for="task in sortedTasks(category.id)" :key="task.id" :task="task" v-else></task-item>
        </div>
        <clear-button @click="clearBasket" v-if="category.id === 4"></clear-button>
      </article>
    </section>
  </div>
</template>

<script>
  import addTask from './addTask.vue';
  import taskItem from './taskItem.vue';
  import clearButton from './UI/clearButton.vue';

  export default {
    inject: ['tasks', 'categories'],
    components: {addTask, taskItem, clearButton},
    methods: {
      sortedTasks(categoryId) {
        let newArray = this.tasks.filter(task => task.categoryId === categoryId);
        return newArray;
      },
      clearBasket() {
        let tasksArray = this.tasks;
        const condition = task => task.categoryId !== 4;

        tasksArray.filter(condition);
        for (let i = tasksArray.length - 1; i >= 0; i -= 1) {
          if (!condition(tasksArray[i])) tasksArray.splice(i, 1);
        }

        localStorage.setItem('tasksItems', JSON.stringify(tasksArray))
      },
      onDrop(e, categoryId) {
        const itemId = parseInt(e.dataTransfer.getData('itemId'));
        for(let taskIndex in this.tasks) {
          const task = this.tasks[taskIndex];
          if(task.id === itemId) {
            task.categoryId = categoryId;
            localStorage.setItem('tasksItems', JSON.stringify(this.tasks));
          }
        }
      },
      headingClass(categoryId) {
        if(categoryId === 1) return "taskboard__group-heading--backlog";
        if(categoryId === 2) return "taskboard__group-heading--processing";
        if(categoryId === 3) return "taskboard__group-heading--done";
        if(categoryId === 4) return "taskboard__group-heading--basket";
      }
    },
}
</script> 