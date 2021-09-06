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
          v-on:remove="remove(index)"
          v-on:savechange="savechange(todo, $event)"
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
      localStorage.setItem("listOfTodos", JSON.stringify(this.todos));
      this.newTodoText = "";
    },
    remove: function(index) {
      this.todos.splice(index, 1);
      localStorage.setItem("listOfTodos", JSON.stringify(this.todos));
    },
    savechange: function(todo, event) {
      todo.title = event;
      localStorage.setItem("listOfTodos", JSON.stringify(this.todos));
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

  mounted() {
    if (
      !localStorage.getItem("listOfTodos") ||
      localStorage.getItem("listOfTodos") === "[]"
    ) {
      localStorage.setItem("listOfTodos", JSON.stringify(this.todos));
      return;
    }
    this.todos = JSON.parse(localStorage.getItem("listOfTodos"));
    this.nextTodoId = this.todos[this.todos.length - 1].id + 1;
  },
};
</script>

<style scoped>
input {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-size: large;
}
::placeholder {
  font-size: larger;
}
#todo-list-example {
  /* color: blue; */
}
.newTask-input {
  width: 25%;
  padding: 0.4em;
}
button {
  padding: 0.4em 1.6em;
  /* margin-right: 1em; */
  font-size: large;
}
label {
  font-size: x-large;
}
form {
  margin-bottom: 32px;
}
li {
  margin-bottom: 12px;
}
</style>
