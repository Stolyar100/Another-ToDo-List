<template>
  <div class="container">
    <h1>ToDo list go here omg</h1>
    <input
      type="text"
      class="todo-input"
      placeholder="What needs to be done"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <div class="todo-list">
      <transition-group
        name="fade"
        enter-active-class="animated fadeInUp faster"
        leave-active-class="animated fadeOutDown faster"
      >
        <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index"></todo-item>
      </transition-group>

      <div class="extra-container">
        <div>
          <label>
            <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos" /> Check all
          </label>
        </div>
        <div>{{ remaining }} items left</div>
      </div>

      <div class="extra-container">
        <div>
          <button :class="{active: filter == 'all' }" @click="filter = 'all' ">All</button>
          <button :class="{active: filter == 'active' }" @click="filter = 'active' ">Active</button>
          <button :class="{active: filter == 'completed' }" @click="filter = 'completed' ">Completed</button>
        </div>

        <div>
          <transition name="fade">
            <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Comleted</button>
          </transition>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem";

export default {
  name: "todo-list",
  props: {},
  components: {
    TodoItem
  },

  data: function() {
    return {
      newTodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Finish Vue Screentest",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "Read Vue documentation",
          completed: false,
          editing: false
        }
      ]
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },

    anyRemaining() {
      return this.remaining != 0;
    },

    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter(todo => todo.completed);
      }

      return this.todos;
    },

    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        this.newTodo = "";
        return;
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false
      });

      this.newTodo = "";

      this.idForTodo++;
    },

    removeTodo(index) {
      this.todos.splice(index, 1);
    },

    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },

    doneEdit(todo) {
      if (todo.title.trim().length == 0) {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },

    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },

    checkAllTodos() {
      this.todos.forEach(todo => (todo.completed = event.target.checked));
    },

    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css);

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo-input:focus {
  outline: 0;
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.todo-list {
  width: 100%;
  display: flex;
  flex-direction: column;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
}

.remove-item:hover {
  color: black;
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  color: #2c3e50;
  margin-left: 12px;
  padding: 10px 18px;
  font-size: 18px;
}

.todo-item-edit:focus {
  outline: 0;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: row;
  font-size: 14px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}

button {
  font-size: 14px;
  background-color: white;
  appearance: none;
  border: 1px solid lightgrey;
}

button:hover {
  background: #40b883;
}

button:focus {
  outline: none;
}

.active {
  background: #40b883;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
