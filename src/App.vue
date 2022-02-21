<template>
  <body class="board-app">
    <header class="board-app__header">
      <div class="board-app__inner">
        <h1>Канбан-доска</h1>
      </div>
    </header>
    <main class="board-app__main">
      <tasks-container></tasks-container>
    </main> 
  </body>
</template>

<script>
  import tasksContainer from './components/tasksContainer.vue';

  export default {
    name: "Kanban-board",
    components: {tasksContainer},
    data() {
      return {
        categoriesArray: [
          {id: 1, title: "Бэклог"},
          {id: 2, title: "В процессе"},
          {id: 3, title: "Готово"},
          {id: 4, title: "Корзина"},
        ],
        tasksArray: JSON.parse(localStorage.getItem('tasksItems')) || [
          {id: 1, name: 'Отдых', categoryId: 1},
          {id: 2, name: 'Приготовить здоровый обед', categoryId: 1},
          {id: 3, name: 'Покормить собаку', categoryId: 1},
          {id: 4, name: 'Закончить первую главу учебного курса', categoryId: 2},
          {id: 5, name: 'Послушать музыку', categoryId: 2},
          {id: 6, name: 'Принять лекарства', categoryId: 2},
          {id: 7, name: 'Отправить 5 писем с обратной связью', categoryId: 3},
          {id: 8, name: 'Обновить бухгалтерский учет', categoryId: 3},
          {id: 9, name: 'Позвонить или написать другу', categoryId: 3},
          {id: 10, name: 'Курсовой проект', categoryId: 4},
          {id: 11, name: 'Обработка почты', categoryId: 4},
        ],
      }
    },
    provide() {
      return {
        tasks: this.tasksArray,
        categories: this.categoriesArray,
      }
    },
  };
</script>