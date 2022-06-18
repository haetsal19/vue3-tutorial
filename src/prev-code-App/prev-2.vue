<template>
    <!-- 양방향 데이터 바인딩 -->
    <input type="text" :value="name" @input="updateName"> 
    <button class="btn btn-primary" @click="onSubmity">
        Click
    </button>
</template>

<script>
import { ref } from 'vue';
export default {
    setup() {
        const name = ref('Kossie');

        const onSubmity = () => {
            console.log(name.value)
        };

          //이벤트를 사용할 때 함수에서 e(event)라는 객체를 받아옴
          //input 박스에 대한 정보가 들어있는 객체
        const updateName = (e) => {
            // console.log(e.target.value)
            name.value = e.target.value;
        };

        return {
            name,
            onSubmity,
            updateName
        };
    }
}

</script>

<style>
.name {
    color: red;
}
</style>