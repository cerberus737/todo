<template>
  <div id="app">
    <h1 class="title">{{ title }}</h1>
    <div class="todo">
      <input v-model="todo" type="text" placeholder="Введите задачу" />
      <button @click="addTodo(id++)">Добавить</button>
      <div v-for="(todo, index) in todos" :key="todo.id">
        <p class="todo_item">
          <span class="todo_id">{{ index + 1 }}.</span>
          <span class="todo_text" :class="{ todo_show: todo.isComplete }">
            {{ todo.text }}
          </span>
          <input v-model="todo.isComplete" class="todo_check" type="checkbox" />
          <button class="remove_button" @click="removeTodo(index)">X</button>
        </p>
      </div>
      <hr />
      <p>Список задач: {{ todos.length }}</p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      title: 'Todo list',
      todo: '',
      todos: [],
      isDone: false,
      id: 0,
    };
  },
  async mounted() {
    const data = await localStorage.getItem('todos');
    data ? (this.todos = JSON.parse(data)) : null;
  },
  methods: {
    addTodo() {
      if (this.todo != '') {
        this.todos.push({
          id: this.id,
          text: this.todo,
          isComplete: this.isDone,
        });
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
      this.todo = '';
    },
    removeTodo(index) {
      console.log(index);
      this.todos.splice(index, 1);
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
  },
};
</script>

<style>
.todo_item {
  display: flex;
  flex-flow: row wrap;
}

.remove_button {
  order: 999;
  margin-left: auto;
}
.title {
  text-align: center;
}
.todo {
  width: 500px;
  height: 100%;
  font-size: 30px;
  background: #fff2f2;
  padding: 30px;
  outline: 1px solid #000;
  min-height: 300px;
}
.todo_id {
  color: rgb(38, 0, 255);
  font-weight: bold;
}
.todo_text {
  color: #444;
  font-size: 30px;
  text-transform: capitalize;
}
.todo_show {
  color: #ccc;
  text-decoration: line-through;
}
.todo_check {
  display: inline-block;
  margin-left: 10px;
  transform: scale(1.5);
}
</style>
