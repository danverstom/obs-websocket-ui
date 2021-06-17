<template>
  <div
    :class="[isActive ? 'active-scene' : '', 'scene-container']"
    @dblclick="changeScene(name)"
  >
    <span class="icon clickable" @click="changeScene(name)">
      <i
        :class="
          ui_selected_scene === name ? 'fas fa-check-square' : 'far fa-square'
        "
      ></i>
    </span>
    <span class="subtitle">{{ name }}</span>
    <span
      class="icon right clickable"
      @click="dropDownClick(name)"
    >
      <i :class="detail_scene === name ? 'fas fa-chevron-up' : 'fas fa-chevron-down'"></i>
    </span>

    <div v-if="detail_scene === name">
      <div class="box">
        <h2 class="subtitle">Detailed View</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Scene",
  props: {
    name: String,
    current_scene: String,
    ui_selected_scene: String,
    detail_scene: String
  },
  data() {
    return {
      detail: false,
    };
  },
  computed: {
    isActive() {
      return this.name === this.current_scene;
    }
  },
  methods: {
    changeScene(name) {
      this.$emit("changeScene", name);
    },
    dropDownClick(name){
      this.$emit("dropDownClick", name);
    }
  },
  emits: ["changeScene", "dropDownClick"],
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
  border-radius: 10px;
  user-select: none;
}

.active-scene {
  background-color: rgba(0, 255, 0, 0.1);
}

.right {
  float: right;
}

.clickable {
  cursor: pointer;
}
</style>
