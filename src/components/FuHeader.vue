<template>
  <div class="header">
    <img class="header__logo" src="../assets/logo.png" alt="FractalUp Logo" />
  </div>
</template>

<script>
export default {
  name: "FuHeader",
};
</script>

<style scoped>
.header {
  width: 100%;
  height: 70px;
  background: #2196F3;
  display: flex;
  justify-content: center;
  align-items: center;
}

.header__logo {
  width: 150px;
  height: 42px;
}
</style>


