<template>
    <div v-bind:class="nameClass">
        <!-- v-bind대신 속성앞에 :만 넣어줘도 됨 
        v-on대신 속성앞에 @만 넣어주면 됨  -->

        {{ name }}
    </div>
    <input :type="type" :value="name">
    <button class="btn btn-primary" @click="updateName">
        Click
    </button>
</template>

<script>
import { ref } from 'vue';  //string이나 number 등 기본 자료형은 ref를 사용하지만, object나 array도 사용가능함 
// import { reactive } from 'vue';    object나 array 사용시에는 reactive를 사용 reactive는 기본자료형(숫자,문자) 사용불가 
export default {
    setup() {
        const name = ref('Kossie'); //단방향 바인딩
        const type = ref('number');
        const nameClass = ref('')

        // const name = reactive ({
        //     id : 1 
        // })
        // const updataName = () => {
        //   name.id = 2;
        //   console.log(name);
        // }
       // const greeting = (name) => {
        //     return 'Hello, ' + name ;
        // };

        // const greet = greeting(name);

        const updateName = () => {
            name.value = 'Coder';
            type.value = 'text';
            nameClass.value = 'name'

            //reactive를 사용할 떈 name.id = 2;
            //ref를 사용할 땐 name.value.id = 2;
            console.log(name);
        };

        return {
            name,
            // greeting,
            // greet
            updateName,
            type,
            nameClass
        };
    }
}

</script>

<style>
.name {
    color: red;
}
</style>