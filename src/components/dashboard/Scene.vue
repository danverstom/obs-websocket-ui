<template>
  <div :class="[isActive ? 'active-scene' : '', 'scene-container']">
    <span class="icon">
      <i class="fas fa-photo-video"></i>
    </span>
    <span class="subtitle">{{ name }}</span>
    <span class="icon right clickable">
      <i class="fab fa-github"></i>
    </span>
    <span class="icon right clickable">
      <i class="fas fa-photo-video"></i>
    </span>
  </div>
</template>

<script>
export default {
  name: "Scene",
  props: {
    name: String,
    current_scene: String,
  },
  computed: {
    isActive() {
      return this.name === this.current_scene;
    },
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

.scene-container {
  background-color: rgba(0, 0, 0, 0.1);
  margin: 5px;
  padding: 5px;
}

.active-scene {
  background-color: rgba(0, 255, 0, 0.1);
}

.right{
  float: right;
}

.clickable{
  cursor: pointer;
}
</style>
