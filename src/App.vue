<template>
    <h2>This is a Vue 3 component!</h2>
    <button @click="increase">Clicked {{ count }} times.</button>
</template>
<style scoped>
h2 {
    color: aqua;
}
</style>
<style scoped lang="less" rel="stylesheet/less">
@import './assets/less/app.less';
h2 {
    font-weight: bold;
    background-color: blue;
}
</style>
<script lang="ts">
import { defineComponent, ref } from "vue";
// This class break the webpack vue-loader
class Test {
    myTest = "ok";
}
export default defineComponent({
    setup() {
        const count = ref(0);
        const increase = () => {
            count.value++;
        };
        return {
            count,
            increase,
        };
    },
});
</script>