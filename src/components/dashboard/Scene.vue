<template>
  <div class="box">
    <span class="subtitle">{{ name }}</span>
    <span class="">{{ is_active }}</span>
  </div>
</template>

<script>
export default {
  name: "Scene",
  props: {
    name: String,
    is_active: Boolean,
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

</style>
