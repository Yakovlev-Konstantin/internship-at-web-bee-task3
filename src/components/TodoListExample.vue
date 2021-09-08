<template>
  <div id="todo-list-example">
    <NewTodoForm
      @createTodo="addNewTodo()"
      :newTodoText="newTodoText"
      @newTodoText:update="newTodoText = $event"
    />
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id">
        <TodoItem
          :title="todo.title"
          @remove="remove(todo.id)"
          @savechange="saveChange(todo, $event)"
        />
      </li>
    </ul>
  </div>
</template>

<script>
import TodoItem from "./TodoItem";
import NewTodoForm from "./NewTodoForm";

export default {
  name: "TodoListExample",
  components: {
    TodoItem,
    NewTodoForm,
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
    };
  },

  methods: {
    addNewTodo: function() {
      this.todos.push({
        id: this.nextTodoId,
        title: this.newTodoText,
      });
      this.saveItems();
      this.newTodoText = "";
    },
    remove: function(id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id != id;
      });
      this.saveItems();
    },
    saveChange: function(todo, event) {
      todo.title = event;
      this.saveItems();
    },
    saveItems: function() {
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
    nextTodoId() {
      if (!this.todos || this.todos.length == 0) return 1;
      return this.todos[this.todos.length - 1].id + 1;
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
