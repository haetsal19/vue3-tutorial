<template>
  <div class="container">
    <h4>count: {{ count }}</h4>
    <h4>double count computed: {{ doubleCountComputed }}</h4>
    <h4>double count computed: {{ doubleCountComputed }}</h4>
    <h4>double count method: {{ doubleCountMethod() }}</h4>
    <h4>double count method: {{ doubleCountMethod() }}</h4>

    <!--    method는 두번실행하면 log가 두번다 찍히는데 -->
    <!--    computed는 이전 값을 캐싱하고 있기 때문에 log가 한번만 찍힘 -->

    <button @click="count++">Add One</button>
    <h2>To-Do List</h2>

    <TodoSimpleForm @add-todo="addTodo"/>

    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    <!--추가된 todos가 없을때만 표시-->


    <TodoList
        :todos="todos"
        @toggle-todo="toggleTodo"
        @delete-todo="deleteTodo"
    />
    <!--부모컴포넌트에서 자식컴포넌트로 데이터를 보낼 때 props를 이용 -->


  </div>

</template>

<script>
import {ref, computed} from 'vue';
import TodoSimpleForm from "@/components/TodoSimpleForm";
import TodoList from "@/components/TodoList";

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
      console.log(todo);
      todos.value.push(todo);
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


    const count = ref(1);
    const doubleCountComputed = computed(() => {
      console.log('computed')
      return count.value * 2;
    })

    const doubleCountMethod = () => {
      console.log('method')
      return count.value * 2;
    };

    // const doubleCountMethod = (name) => {
    //   return count.value * name;
    // };
    // method는 인자로 값을 받아와서 함수안에서 사용할 수 있으나 computed는 인자를 받을 수 없다.
    // computed는 computed함수안에 reactive state가 변경될 때만 실행되어 그 값을 변수에 저장하게됨
    // computed는 값을 캐싱하기 때문에 한번 계산하면 그 값을 저장하고 있다.


    return {
      todos,
      addTodo,
      todoStyle,
      deleteTodo,
      toggleTodo,
      count,
      doubleCountComputed,
      doubleCountMethod,
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