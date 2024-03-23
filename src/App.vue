<script setup>
import { RouterView } from "vue-router";
</script>

<template>
  <div>
    <Router-view />
  </div>
</template>

<style lang="scss">
.wrapper {
  display: flex;
  max-width: 80%;
  margin: 0 auto;
  padding-top: 40px;
  gap: 30px;
}
.people {
  max-width: 320px;
  &-item {
    display: flex;
    gap: 16px;
    padding: 16px;
    align-items: center;

    &.active {
      background: #24334b;
      box-shadow: 0px 10px 20px 0px #acbbd2;
      h2 {
        color: #f6f6f6;
      }
      span {
        color: #1baa75;
      }
    }

    &-pfp {
      height: 40px;
      width: 40px;
      border-radius: 100%;
      overflow: hidden;
      img {
        max-height: 100%;
        max-width: 100%;
      }
    }

    h2 {
      color: #24334b;
      font-size: 16px;
      font-weight: 500;
      margin: 0;
    }
    span {
      color: #bdbdbd;
      font-size: 14px;
      font-weight: 500;
    }
  }
  header {
    background: #efefef;
    padding: 16px;
    color: #24334b;
    font-size: 18px;
    font-weight: 600;
  }
}
</style>
