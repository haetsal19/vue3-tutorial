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