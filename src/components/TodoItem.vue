<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="todo.completed" />
      <div
        v-if="!todo.editing"
        @dblclick="editTodo(todo)"
        class="todo-item-label"
        :class="{ completed : todo.completed }"
      >{{ todo.title }}</div>
      <input
        v-else
        @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)"
        @keyup.esc="cancelEdit(todo)"
        v-focus
        class="todo-item-edit"
        type="text"
        v-model="todo.title"
      />
    </div>
    <div class="remove-item" @click="removeTodo(index)">&times;</div>
  </div>
</template>


<script>
export default {
  name: "todo-item",
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  }
};
</script>