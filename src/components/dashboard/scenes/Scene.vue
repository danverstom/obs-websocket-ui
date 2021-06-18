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
    <span class="icon right clickable" @click="dropDownClick(name)">
      <i
        :class="
          detail_scene === name ? 'fas fa-chevron-up' : 'fas fa-chevron-down'
        "
      ></i>
    </span>

    <div v-if="detail_scene === name">
      <div class="box">
        <div v-for="source in scene_items" :key="source.itemId">
          <Source
            :source_details="source"
            :obs="obs"
            v-on:OpenSourceSettings="openSourceSettings"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Source from "@/components/dashboard/sources/Source.vue";

export default {
  name: "Scene",
  components: {
    Source,
  },
  props: {
    name: String,
    current_scene: String,
    ui_selected_scene: String,
    detail_scene: String,
    obs: Object,
  },
  data() {
    return {
      scene_items: false,
    };
  },
  computed: {
    isActive() {
      return this.name === this.current_scene;
    },
    showDetail() {
      return this.name === this.detail_scene;
    },
  },
  methods: {
    openSourceSettings(source_name) {
      console.log("Scene component got open settings event", source_name)
      this.$emit('OpenSourceSettings', source_name, this.name)
    },
    changeScene(name) {
      this.$emit("changeScene", name);
    },
    async dropDownClick(name) {
      this.$emit("dropDownClick", name);
      if (!this.showDetail) {
        // (!) Take the opposite since the state has flipped due to the emit above
        await this.getSceneItems(name);
      }
    },
    async getSceneItems(name) {
      const data = await this.obs.send("GetSceneItemList", {
        sceneName: name,
      });
      this.scene_items = data.sceneItems;
      console.log(data.sceneItems);
      return data.sceneItems;
    },
  },
  emits: ["changeScene", "dropDownClick", "OpenSourceSettings"],
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
