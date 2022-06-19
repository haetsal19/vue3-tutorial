<template>
  <div class="container">

    <h2>To-Do List</h2>

    <input class="form-control"
           type="text" v-model="searchText"
           placeholder="Search">

    <hr />

    <TodoSimpleForm @add-todo="addTodo"/>

    <div v-if="!filteredTodos.length">
      There is nothing to display.
    </div>
    <!--추가된 todos가 없을때만 표시-->


    <TodoList
        :todos="filteredTodos"
        @toggle-todo="toggleTodo"
        @delete-todo="deleteTodo"
    />
    <!--부모컴포넌트에서 자식컴포넌트로 데이터를 보낼 때 props를 이용 -->

    <div style="color: red">{{error}}</div>


  </div>

</template>

<script>
import { ref, computed } from 'vue';
import TodoSimpleForm from "@/components/TodoSimpleForm";
import TodoList from "@/components/TodoList";
import axios from 'axios';

export default {
  components: {
    TodoSimpleForm,
    TodoList
  },
  setup() {
    const todos = ref([]);
    const error = ref('');

    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray'
    };

    const addTodo = async (todo) => {
      error.value = '';
      try {
        const res = await axios.post('http://localhost:3000/todos', {
          subject: todo.subject,
          completed: todo.completed,
        });
        todos.value.push(res.data);
      } catch(err) {
        error.value = 'Something went wrong.'
      }
      //     .then(res => {
      //   //요청한 작업이 모두 끝나고 응답이 왔을 때 실행되는 부분
      //   console.log(res);
      //   todos.value.push(res.data); //.then 밖에 위치할 경우 비동기로 응답을 받기도 전에 실행되어버림
      // }).catch(err => {
      //   console.log(err);
      //   error.value = 'Something went wrong.'
      // });

    };

    const toggleTodo = (index) => {
      console.log(todos.value[index].completed);
      todos.value[index].completed = !todos.value[index].completed;
      // <label class="form-check-lable" :class="{ todo : todo.completed }">
      // 체크하면 회색밑줄로 항목 지우는 부분 토글
      console.log(todos.value[index].completed);
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1) //todos.value array에 splice로 한개만 지워줌
    };


    const searchText = ref('');

    const filteredTodos = computed(() => {
      if(searchText.value) {
        return todos.value.filter( todo => {
          return todo.subject.includes(searchText.value);
        });
      }

      return todos.value;
    })


    return {
      todos,
      addTodo,
      todoStyle,
      deleteTodo,
      toggleTodo,
      searchText,
      filteredTodos,
      error,
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