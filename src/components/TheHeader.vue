<script setup>
import topImg from "./../assets/topimg.png";
</script>
<template>
  <header>
    <img :src="topImg" alt="トップ画像" class="topimg" />
  </header>
</template>

<style scoped>
.topimg {
  margin: 20px auto;
  max-height: 70px;
}
</style>
