<template>
  <div :class="[isActive ? 'active-scene' : '', 'scene-container']" @dblclick="changeScene(name)">
    <span class="icon clickable" @click="changeScene(name)">
      <i :class="ui_selected_scene === name ? 'fas fa-check-square': 'far fa-square'"></i>
    </span>
    <span class="subtitle">{{ name }}</span>
    <span class="icon right clickable">
      <i class="fas fa-chevron-down"></i>
    </span>
    
  </div>
</template>

<script>
export default {
  name: "Scene",
  props: {
    name: String,
    current_scene: String,
    ui_selected_scene: String
  },
  computed: {
    isActive() {
      return this.name === this.current_scene;
    },
  },
  methods: {
    changeScene(name) {
      this.$emit("changeScene", name);
    }
  },
  emits: ["changeScene"]
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
