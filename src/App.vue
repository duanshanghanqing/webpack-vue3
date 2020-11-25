<template>
  <div>
    <button @click="increase">
      <!-- vue 内部对 count 做了处理，能直接使用-->
      Clicked {{ count }} times.
    </button>
    <p>{{ double }}</p>
    <br />

    <ul>
        <li v-for="number of numbers" :key="number">{{ number }}</li>
    </ul>
    <div>{{ person.name }}</div>
  </div>
</template>

<style scoped>
h2 {
  color: aqua;
}
</style>

<style scoped lang="less" rel="stylesheet/less">
@import "./assets/less/app.less";
h2 {
  font-weight: bold;
  background-color: red;
}
</style>

<script lang="ts">
/*
import { defineComponent, ref, computed } from "vue";
// 1
export default defineComponent({
  // 访问不到this
  setup() {
    const count = ref(0); // 返回一个响应式对象
    const increase = () => {
      count.value++;
    };
    // computed: 计算方法，内部依赖的值变了返回新值
    const double = computed(() => {
      return count.value * 2;
    });
    return {
      count,
      double,
      increase,
    };
  },
});
*/

/*
import { defineComponent, ref, computed, reactive, toRefs } from "vue";
// 2 改写1
export default defineComponent({
    setup() {
        const data = reactive({
            count: 0,
            increase: () => { data.count++; }, // 每次加1
            double: computed(() => data.count * 2), // count 变化 * 2
        });

        // 直接这样返回，没有用。因为丢失了响应式模仿
        // return {
        //     ...data,
        // }

        // 需要用 toRefs 包一下
        return {
            ...toRefs(data)
        }

    }
})
*/

import { defineComponent, ref, computed, reactive, toRefs } from "vue";
export default defineComponent({
    setup() {
        const data = reactive({
            count: 0,
            increase: () => { data.count++; }, // 每次加1
            double: computed(() => data.count * 2), // count 变化 * 2

            // vue2 返回的data是个响应式对象，不支持对象动态添加属性成响应式，数组添动态加元素成响应式，
            // 需要使用 Vue.set(vm.obj, 'b', 2), this.set(this.obj, 'b', 2) 来解决
            // 因为 vue2 使用了 Object.defineProperty(data, 'count', { get() {}, set() {} }) , 
            // 这个方法比较弱，对新新增加的key需要手动调一下

            // vue3使用了 new Proxy(data, { get(key) {}, set(key, value) {}, }), 对任意key进行拦截

            numbers: [],
            person: {},
        });

        // 两秒后就渲染在模版上了, 让 Vue.set 成了过去式
        setTimeout(() => {
            data.numbers[0] = 1;
            data.numbers.push(2);
            data.person.name = '张三';
        }, 2000);

        // 需要用 toRefs 包一下
        return {
            ...toRefs(data)
        }

    }
})
</script>
