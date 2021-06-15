<template>
  <div class="scene-container">
    <span class="subtitle">{{ name }}</span>
  </div>
</template>

<script>
export default {
  name: "Scene",
  props: {
    name: String
  },
};
</script>

<style scoped>
.box {
  margin: 10px;
}

span {
  margin-left: 5px;
  margin-right: 5px;
}

.scene-container{
  background-color: rgba(0,0,0,0.1);
  margin: 5px;
  padding: 5px;
}

</style>
