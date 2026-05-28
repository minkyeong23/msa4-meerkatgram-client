<script setup>
import { reactive } from 'vue';
import MyButton from '../../components/button/MyButton.vue';
import MyInput from '../../components/input/MyInput.vue';
import MyStrikeThroughBehindWord from '../../components/decoration/MyStrikeThroughBehindWord.vue';
import { useAuthStore } from '../../store/auth/useAuthStore.js';
import { useRouter } from 'vue-router';

const router = useRouter();
const authStore = useAuthStore();
const loginForm = reactive({
  email: '',
  password: '',
})

const handleSubmit = async () => {
  await authStore.login(loginForm);
  router.replace('/posts');
}
</script>

<template>
<form @submit.prevent="handleSubmit">
  <MyInput
    :type="'email'"
    :placeholder="'Email'"
    :readonly="false"
    :required="true"
    v-model="loginForm.email"
  ></MyInput>
  <MyInput
    :type="'password'"
    :placeholder="'Password'"
    :readonly="false"
    :required="true"
    v-model="loginForm.password"
  ></MyInput>

  <MyButton
    :btnType="'submit'"
    :color="'gray'"
    :size="'middle'"
    :content="'Log In'"
  ></MyButton>

  <MyStrikeThroughBehindWord
    :content="'or'"
  ></MyStrikeThroughBehindWord>

  <MyButton
    :btnType="'button'"
    :color="'white'"
    :size="'middle'"
    :content="'Sign up'"
  ></MyButton>
</form>
</template>

<style scoped>
form {
  padding: 20px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
</style>
