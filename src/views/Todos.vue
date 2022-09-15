<template>
  <div>
    <AddTodo @add-todo="addTodo" />
    <select v-model="filter">
      <option value="all">Все</option>
      <option value="completed">Выполненные</option>
      <option value="not-completed">Не выполненные</option>
    </select>
    <hr />
    <Loader v-if="loading" />
    <TodoList
      v-else-if="filteredTodos.length"
      v-bind:todos="filteredTodos"
      @remove-todo="removeTodo"
    />
    <p v-else>Нет задач!</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList';
import AddTodo from '@/components/AddTodo';
import Loader from '@/components/Loader';
export default {
  name: 'app',
  data() {
    return {
      todos: [
        { id: 1, title: 'Купить хлеб', completed: false },
        { id: 2, title: 'Купить молоко', completed: false },
        { id: 2, title: 'Купить мало', completed: false },
      ],

      filter: 'all',
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos;
      }

      if (this.filter === 'completed') {
        return this.todos.filter((t) => t.completed);
      }

      if (this.filter === 'not-completed') {
        return this.todos.filter((t) => !t.completed);
      }
    },
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter((t) => t.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    },
  },
  components: {
    TodoList,
    AddTodo,
    Loader,
  },
};
</script>
