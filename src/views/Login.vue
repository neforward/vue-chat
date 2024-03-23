<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const nameVal = ref("");
const router = useRouter();

const login = () => {
  localStorage.setItem("login", nameVal.value);
  router.push("/");
};
</script>
<template>
  <div class="login">
    <input type="text" placeholder="type ur name" v-model="nameVal" @keyup.enter="login" />
    <button @click="login">click</button>
  </div>
</template>

<style>
.login {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  gap: 20px;
}
</style>
