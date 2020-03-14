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
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
          <div
            v-if="!todo.editing"
            @dblclick="editTodo(todo)"
            class="todo-item-label"
          >{{ todo.title}}</div>
          <input
            v-else
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            v-focus
            class="todo-item-edit"
            type="text"
            v-model="todo.title"
          />
        </div>
        <div class="remove-item" @click="removeTodo(index)">&times;</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  props: {},

  data: function() {
    return {
      newTodo: "",
      idForTodo: 3,
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
      todo.editing = true;
    },

    doneEdit(todo) {
      todo.editing = false;
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
</style>
