<template>
  <div class="container">
    <h2>To-Do List</h2>
    <form @submit.prevent="onSubmit">
      <div class="d-flex">
        <!-- 할일입력과 제출-->
        <div class="flex-grow-1 mr-2">
          <input class="form-control"
                 type="text" v-model="todo"
                 placeholder="Type new to-do">
        </div>
        <div>
          <button class="btn btn-primary" type="submit">Add</button>
        </div>
      </div>
      <div v-show="hasError"
           style="color: red">
        <!--빈문자열 제출 시 에러메시지 -->
        This field cannot be empty
      </div>
    </form>

    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    <!--추가된 todos가 없을때만 표시-->

    <div
        v-for="(todo, index) in todos"
        :key="todo.id"
        class="card mt-2">
      <!--입력할 to do list v-for 루프 돌리기 -->
      <div class="card-body p-1 d-flex align-items-center">
        <div class="form-check flex-grow-1">
          <input class="form-check-input"
                 type="checkbox"
                 v-model="todo.completed">
          <label class="form-check-lable" :class="{ todo : todo.completed }">
            <!--:style="todo.completed ? todoStyle :  {}"-->
            <!--스타일 바인딩 : 체크박스 true 시 list에 선긋기 -->

            {{ todo.subject }}
          </label>
        </div>
        <!--삭제버튼/클릭시 todos 배열에서 제거-->
        <div>
          <button
              class="btn btn-danger btn-sm"
              @click="deleteTodo(index)">X</button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    // const toggle = ref(false);
    const todo = ref('');
    const todos = ref([]);
    const hasError = ref(false);
    const todoStyle = {
      textDecoration: 'line-through',
      //css에서 입력시 text-decoration 자바스크립트로 입력시 textDecoration
      color: 'gray'
    };
    const onSubmit = (//e
    ) => {
      if (todo.value === '') {
        hasError.value = true;
      } else {
        todos.value.push({
          id: Date.now(),
          subject: todo.value,
          completed: false,

        });
        hasError.value = false;
        todo.value = '';
      }

    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1) //todos.value array에 splice로 한개만 지워줌
    };

    return {
      todo,
      todos,
      onSubmit,
      hasError,
      todoStyle,
      deleteTodo
    };
  }
}

</script>

<style>
.todo {
  color: gray;
  text-decoration: line-through;
}

</style>