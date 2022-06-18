<template>
  <div
      v-for="(todo, index) in todos"
      :key="todo.id"
      class="card mt-2">
    <!--입력할 to do list v-for 루프 돌리기 -->
    <div class="card-body p-1 d-flex align-items-center">
      <div class="form-check flex-grow-1">
        <input class="form-check-input"
               type="checkbox"
               :value="todo.completed"
               @change="toggleTodo(index)">
        <!--todo를 v-for로 돌릴 때 생기는 index-->

        <!--        v-model="todo.completed">-->
        <!--        모든 props는 부모->자식으로만 데이터를 내려보내줌 one-way-down-binding (단방향)-->
        <!--        부모컴포넌트의 property가 변경되면 props로 받은 데이터도 자식컴포넌트에서 같이 변경된다. 그 반대는 불가-->
        <!--        자식컴포넌트에서 props로 받은 데이터를 직접적으로 변경할 수 없다.-->
        <!--        v-model은 양방향 바인딩-->
        <!--        자식컴포넌트에서 v-model로 props를 직접 변경할 수 없다-->
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
            @click="delete_Todo(index)"
        >
          X
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // props : ['todos']
  //부모컴포넌트에서 받아온 props
  props: {
    todos: {
      type: Array, //props의 타입 지정 가능
      required: true,
    }
  },

  emits: ['toggle-todo', 'delete-todo'],

  setup(props, context) {

    const toggleTodo = (index) => {
      context.emit('toggle-todo', index);
    };

    const delete_Todo = (index) => {
      context.emit('delete-todo', index);
    };


    return {
      toggleTodo,
      delete_Todo,
    }
  }
}
</script>

<style>

</style>