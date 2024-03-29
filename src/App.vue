<template>
  <p>{{ count }}</p>
  <p>{{ state.count }}</p>
  <button @click="incrementCount">button</button><br />
  <br />{{ name }} <input type="text" v-model="name" /><br />
  <br />{{ email }} <input type="text" v-model="email" /><br />
  <br />{{ phone }} <input type="text" v-model="phone" /><br />
  <br />{{ text }}
</template>

<script>
import { ref, reactive, toRefs, watch } from 'vue';

export default {
  name: 'App',
  setup() {
    const count = ref(0);
    const state = reactive({
      count: 0,
    });
    const name = ref('');
    const user = reactive({
      email: '',
      phone: '',
      text: '',
    });

    function incrementCount() {
      count.value++;
      state.count = state.count + 5;
    }

    // 以下監聽適用於ref
    watch(
      [name],
      (newValue, oldValue) => {
        console.log('name', newValue[0], oldValue[0]);
      },
      {
        immediate: true, //如果變數一開始有值，就會被 watch
      }
    );

    // 以下監聽適用於reactive
    watch(
      () => {
        return { ...user };
      },
      (newValue, oldValue) => {
        console.log('user', newValue, oldValue);
        if (user.phone.length === 10) {
          user.text = '電話格式正確';
        } else {
          user.text = '電話格式有誤';
        }
      }
    );

    return {
      count,
      state,
      name,
      ...toRefs(user),
      incrementCount,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
