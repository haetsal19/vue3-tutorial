<template>
  <form @submit.prevent="onSubmit">
    <div class="d-flex">
      <!-- 할일입력과 제출-->
      <div class="flex-grow-1 mr-2">
        <input class="form-control"
               type="text" v-model="todo"
               placeholder="Type new to-do">
      </div>
      <div>
        <button
            class="btn btn-primary"
            type="submit">
          Add
        </button>
      </div>
    </div>
    <div v-show="hasError"
         style="color: red">
      <!--빈문자열 제출 시 에러메시지 -->
      This field cannot be empty
    </div>
  </form>
</template>

<script>
import {ref} from "vue";

export default {
  emits: ['add-todo'], //이 컴포넌트에서 부모 컴포넌트로 전달하는 어떤 이벤트들이 있는지 한눈에 확인 가능

  setup(pros, { emit }) {
    //context : 자식컴포넌트에서 부모컴포넌트로 데이터를 보낼 때 필요한 것이 들어있음
    // context는 객체{} 이므로
    // context--> {emit} 으로 구조분해할당 하여
    // context.emit을 emit으로 간편하게 사용

    const todo = ref('');
    const hasError = ref(false);

    const onSubmit = () => {

      if (todo.value === '') {
        hasError.value = true;
      } else {
        emit('add-todo', {
          //add-todo라는 이벤트 이름을 부모컴포넌트에서 사용
          id: Date.now(),
          subject: todo.value,
          completed: false,
          //자식컴포넌트에서 submit하여 이부분 실행되면 이 데이터 오브젝트를 부모 컴포넌트로 보내줌
        });
        //context객체 내장함수 emit()을 통해 자식컴포넌트에서 부모컴포넌트로 이벤트, 데이터를 보낸다
        //context.emit(이벤트, 데이터)
        hasError.value = false;
        todo.value = '';
      }
    };

    return {
      todo,
      hasError,
      onSubmit,
    };
  }

}

</script>

<style>

</style>