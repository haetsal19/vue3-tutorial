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
        <div style="color: red">{{error}}</div>

        <TodoList
            :todos="filteredTodos"
            @toggle-todo="toggleTodo"
            @delete-todo="deleteTodo"
        />

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
    const todos = ref([]);  //입력한 todo 데이터 리스트
    const error = ref('');  //에러메시지

    //DB에서 todo 리스트 받아오기
    const getTodos = async () => {
      try {
        const res = await axios.get('http://localhost:3000/todos');
        todos.value = res.data;
      } catch(err) {
        console.log(err);
        error.value = 'Something went wrong.'
      }
    };

    getTodos();


    //DB에 새로운 todo 항목 추가
    const addTodo = async (todo) => {
      error.value = '';
      try {
        const res = await axios.post('http://localhost:3000/todos', {
          subject: todo.subject,
          completed: todo.completed,
        });
        todos.value.push(res.data);
      } catch(err) {
        console.log(err);
        error.value = 'Something went wrong.'
      }
    };


    //DB에서 todo 항목 삭제
    const deleteTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id ;
      try {
        await axios.delete('http://localhost:3000/todos/' + id )
        todos.value.splice(index, 1)
      } catch (err) {
          console.log(err);
          error.value = 'Something went wrong.'
      }
    };


    //todo 항목 앞 체크박스로 completed 항목 토글 및 DB에 completed 업데이트
    const toggleTodo = async (index) => {
        error.value = '';
        const id = todos.value[index].id;
        try {
          await axios.patch('http://localhost:3000/todos/' + id, {
            completed: !todos.value[index].completed
          })
          todos.value[index].completed = !todos.value[index].completed;
        } catch (err) {
          console.log(err);
          error.value = 'Something went wrong.'
        }
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