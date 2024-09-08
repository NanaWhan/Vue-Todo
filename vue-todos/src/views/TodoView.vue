<script setup>
import CreateTodo from "../components/CreateTodo.vue";
import { uid } from "uid";
import { ref, watch, computed } from "vue";
import TodoItem from "../components/TodoItem.vue";
import { Icon } from "@iconify/vue";

const todoList = ref([]);

watch(
  todoList,
  () => {
    setTodoListLocalStorage();
  },
  {
    deep: true,
  }
);

const todoListCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

const getTodoListLocalStorage = () => {
  const todoListData = JSON.parse(localStorage.getItem("todoList"));
  if (todoListData) {
    todoList.value = todoListData;
  }
};

getTodoListLocalStorage();

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });
  // setTodoListLocalStorage();
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
  // setTodoListLocalStorage();
};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
  setTodoListLocalStorage();
};

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
  // setTodoListLocalStorage;
};

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todo.id);
  // setTodoListLocalStorage();
};
</script>

<template>
  <main>
    <h1>Create a task</h1>
    <CreateTodo @create-todo="createTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="fluent-emoji-flat:sad-but-relieved-face" width="22" />
      <span class="">Oops...You have no todo's to complete. Add one!</span>
    </p>
    <p v-if="todoListCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="fluent-emoji-flat:smiling-face-with-sunglasses" width="22" />
      <span class="">You have completed all your todo's. Great job!</span>
    </p>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
