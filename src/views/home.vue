<template>
  <div class="mainContainer">
    <imageUpload />
  </div>
</template>
<script>
import imageUpload from "../components/imageUpload.vue";
export default {
  name: "home",
  components: {
    imageUpload,
  },
  data() {
    return {};
  },
};
</script>
<style scoped>
.mainContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}
</style>
