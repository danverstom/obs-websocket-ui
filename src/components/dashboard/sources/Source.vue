<template>
  <div class="source-container">
    <span>{{ source_details.sourceName }}</span>
    <span class="icon right clickable" v-if="data_loaded">
      <i class="fas fa-chevron-down"></i>
    </span>
    <span class="icon right clickable" v-if="data_loaded">
      <i
        :class="scene_properties.visible ? 'fas fa-eye' : 'fas fa-eye-slash'"
        @click="toggleVisibility"
      ></i>
    </span>
  </div>
</template>

<script>
export default {
  name: "Source",
  data() {
    return {
      data_loaded: false,
      settings: false,
      scene_properties: false,
    };
  },
  props: {
    source_details: Object,
    obs: Object,
  },
  async created() {
    console.log("source object created");
    console.log(this.source_details);
    this.settings = await this.getSettings();
    this.scene_properties = await this.getProperties();
    this.data_loaded = true;
  },
  methods: {
    async getSettings() {
      const data = await this.obs.send("GetSourceSettings", {
        sourceName: this.source_details.sourceName,
      });
      console.log(
        "Source settings for source " + this.source_details.sourceName
      );
      console.log(data.sourceSettings);
      return data.sourceSettings;
    },
    async getProperties() {
      const data = await this.obs.send("GetSceneItemProperties", {
        item: this.source_details.sourceName,
      });
      console.log(
        "Scene-specific properties for source " + this.source_details.sourceName
      );
      this.scene_properties = data;
      console.log(data);
      return data;
    }
  },
};
</script>


<style scoped>
.right {
  float: right;
}

.clickable {
  cursor: pointer;
}

.source-container {
  border: 1px solid rgba(0, 0, 0, 0.2);
  margin-top: 10px;
  margin-bottom: 10px;
  padding: 8px;
  border-radius: 10px;
}
</style>