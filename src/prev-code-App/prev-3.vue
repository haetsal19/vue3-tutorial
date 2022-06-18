<template>
  <!--  <div v-show="toggle">true</div>-->
  <!--  <div v-show="!toggle">false</div>-->
  <!--  <div v-if="toggle">true</div>-->
  <!--  <div v-else>false</div>-->
  <!--  v-if는 토글하는데 비용이 많이 들고 -->
  <!--  v-show는 초기렌더링에 비용이 많이 든다. -->
  <!--  v-show는 토글이 자주 필요할 때 사용-->
  <!--  v-if는 런타임동안 조건이 거의 바뀌지 않을 경우에 사용 -->
  <!--  <button @click="onToggle">Toggle</button>-->
  <div class="container">
    <!-- 부트스트랩 클래스 -->
    <h2>To-Do List</h2>
    <form @submit.prevent="onSubmit">
      <!-- .prevent를 modifier라 함
   e.preventDefault() 함수를 실행시킴 -->
      <div class="d-flex">
        <div class="flex-grow-1 mr-2">
          <input class="form-control" type="text" v-model="todo" placeholder="Type new to-do">
          <!-- v-model을 통해 양방향 데이터 바인딩을 간편하게 구현  -->
        </div>
        <div>
          <button class="btn btn-primary" type="submit">
            <!--                  form태그로 감싸진 summit버튼을 눌렀을때 onSummit이 실행-->
            Add
          </button>
        </div>
      </div>

      <div v-show="hasError" style="color: red">

        This field cannot be empty
      </div>
    </form>
    <div
        v-for="todo in todos" :key="todo.id"
        class="card mt-2">
      <!--          반복되는부분 최상위 태그에 v-for 넣어줌 -->
      <!--          v-for 사용시엔 key바인딩 필수-->
      <div class="card-body">
        {{ todo.subject }}
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
    const todos = ref([
      {id: 1, subject: '휴대폰 사기'},
      {id: 2, subject: '장보기'}
    ]);
    const hasError = ref(false);
    const onSubmit = (//e
    ) => {
      //e.preventDefault();
      //form이 submit되면 화면을 리로딩하는데 e.preventDefault()로 방어 가능
      //html form 태그안에서 @submit.prevent도 같은 효과
      if (todo.value === '') {
        hasError.value = true;
      } else {
        todos.value.push({
          id: Date.now(), //아이디는 유니크해야하기 때문에 Date.now를 통해 타임스탬프로 지정
          subject: todo.value
        });
        hasError.value = false;
      }

    };

    // const onToggle = () => {
    //   toggle.value= !toggle.value;
    // };
    return {
      todo,
      todos,
      onSubmit,
      // toggle,
      // onToggle,
      hasError,
    };
  }
}

</script>

<style>
.todo {
  color: red;
}
</style>