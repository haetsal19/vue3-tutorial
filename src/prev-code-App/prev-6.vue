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

    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray'
    };

    const addTodo = (todo) => {
      // 데이터베이스 투두를 저장
      // 이부분에 axios를 이용해 post request를 데이터베이스 json 서버에 보내볼 것
      // inspect > network > status : 201 created 일 때 데이터가 서버에 잘 저장된 것
      // 데이터를 함께 보낼 때 post request를 사용
      axios.post('http://localhost:3000/todos', {
        subject: todo.subject,
        completed: todo.completed,
        //id는 입력하지 않아도 db에서 id:1부터 차례대로 저장하게 됨
      }).then(res => {
        //요청한 작업이 모두 끝나고 응답이 왔을 때 실행되는 부분
        console.log(res);
        todos.value.push(todo); //.then 밖에 위치할 경우 비동기로 응답을 받기도 전에 실행되어버림
      }).catch(err => {
        console.log(err);
      });

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