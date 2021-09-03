<template>
  <div id="todo-list-example">
    <form v-on:submit.prevent="addNewTodo">
      <label for="new-todo">Задача: </label>
      <input
        v-model="newTodoText"
        id="new-todo"
        placeholder="Введите Вашу новую задачу"
        class="newTask-input"
      />
      <button>Добавить</button>
    </form>
    <ul>
      <li v-for="(todo, index) in filteredTodos" v-bind:key="todo.id">
        <TodoItem
          v-bind:title="todo.title"
          v-on:remove="todos.splice(index, 1)"
          v-on:savechange="todo.title = $event"
        />
      </li>
    </ul>
  </div>
</template>

<script>
import TodoItem from "./TodoItem";

export default {
  name: "TodoListExample",
  components: {
    TodoItem,
  },
  data() {
    return {
      newTodoText: "",
      todos: [
        {
          id: 1,
          title: "Помыть посуду",
        },
        {
          id: 2,
          title: "Вынести мусор",
        },
        {
          id: 3,
          title: "Подстричь газон",
        },
        {
          id: 4,
          title: "Купить машину",
        },
        {
          id: 5,
          title: "Почитать книгу",
        },
      ],
      nextTodoId: 6,
    };
  },
  methods: {
    addNewTodo: function() {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText,
      });
      this.newTodoText = "";
    },
  },
  computed: {
    filteredTodos() {
      if (!this.newTodoText) return this.todos;

      return this.todos.filter((todo) => {
        return todo.title.includes(this.newTodoText);
      });
    },
  },
};
</script>
