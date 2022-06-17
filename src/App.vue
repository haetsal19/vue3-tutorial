<template>
    <div class="container">
        <!-- 부트스트랩 클래스 -->
        <h2>To-Do List</h2>
        <form @submit.prevent="onSubmit" class="d-flex">
            <!-- .prevent를 modifier라 함
         e.preventDefault() 함수를 실행시킴 -->
            <div class="flex-grow-1 mr-2">
                <input class="form-control" type="text" v-model="todo" placeholder="Type new to-do">
                <!-- v-model을 통해 양방향 데이터 바인딩을 간편하게 구현  -->

            </div>
            <div>
                <button class="btn btn-primary" type="submit">
                    Add
                </button>
            </div>
        </form>
        {{todos}}
        <div class="card">
            <div class="card-body">
                휴대폰 사기
            </div>
        </div>
    </div>

</template>

<script>
import { ref } from 'vue';  
export default {
    setup() {
        const todo = ref ( '' );
        const todos = ref( [] );

        const onSubmit = () => {
            //e.preventDefault(); 
            //form이 submit되면 화면을 리로딩하는데 e.preventDefault()로 방어 가능
            //html form 태그안에서 @submit.prevent도 같은 효과 
            todos.value.push({
                id : Date.now(),
                subject: todo.value
            });
        };

      
        return {
            todo,
            todos,
            onSubmit
        };
    }
} 

</script>

<style>
    .todo {
        color : red;
    }
</style>