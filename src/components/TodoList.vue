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
               :checked="todo.completed"
               @change="toggleTodo(index)">
                                  <!--todo를 v-for로 돌릴 때 생기는 index-->


        <label class="form-check-lable" :class="{ todo : todo.completed }">
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
  props: {
    todos: {
      type: Array, //props의 타입 지정 가능
      required: true,
    }
  },

  emits: ['toggle-todo', 'delete-todo'],

  setup(props, { emit }) {
    // context는 객체{} 이므로
    // context--> {emit} 으로 구조분해할당 하여
    // context.emit을 emit으로 간편하게 사용
      const toggleTodo = (index) => {
        emit('toggle-todo', index);
      };

      const delete_Todo = (index) => {
        emit('delete-todo', index);
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