<script setup>
import { defineEmits, reactive } from "vue";
import TodoButton from "./TodoButton.vue";

const emit = defineEmits(["create-Todo"]);

const todoState = reactive({
  todo: "",
  invalid: null,
  errorMessage: "",
});

const createTodo = () => {
  todoState.invalid = null;
  if (todoState.todo !== "") {
    emit("create-Todo", todoState.todo);
    todoState.todo = "";
    return;
  }
  todoState.invalid = true;
  todoState.errorMessage = "Please enter a task";
};
</script>

<template>
  <div>
    <div class="input-wrap" :class="{ 'input-error': todoState.invalid }">
      <input type="text" v-model="todoState.todo" />
      <TodoButton @click="createTodo()" />
    </div>

    <p v-show="todoState.invalid" class="error-Message">
      {{ todoState.errorMessage }}
    </p>
  </div>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-error {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}
.error-Message {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
