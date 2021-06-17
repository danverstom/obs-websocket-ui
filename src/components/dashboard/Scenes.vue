<template>
  <div v-for="scene in scene_data.scenes" :key="scene.name">
    <Scene
      :name="scene.name"
      :current_scene="current_scene"
      :ui_selected_scene="ui_selected_scene"
      :detail_scene="detail_scene"
      v-on:changeScene="changeScene"
      v-on:dropDownClick="dropDownClick"
    />
  </div>
</template>

<script>
import Scene from "@/components/dashboard/Scene.vue";

export default {
  name: "Scenes",
  components: {
    Scene,
  },
  props: {
    obs: Object,
  },
  data() {
    return {
      scene_data: [],
      current_scene: "",
      ui_selected_scene: "",
      detail_scene: ""
    }
  },
  created() {
    this.obs.on("SwitchScenes", (data) => {
      console.log("Scene change detected");
      console.log(data);
      this.queryScenes();
    });

    this.queryScenes();
  },
  methods: {
    async queryScenes() {
      const scene_data = await this.obs.send("GetSceneList");
      console.log(scene_data);
      this.scene_data = scene_data;
      this.current_scene = scene_data.currentScene;
      this.ui_selected_scene = scene_data.currentScene;

      console.log(`Current Scene: ${this.current_scene}`)
      console.log(`UI Selected Scene: ${this.ui_selected_scene}`)

    },
    changeScene(name) {
      console.log("changing scene to " + name)
      this.ui_selected_scene = name
      this.obs.send("SetCurrentScene", {
        "scene-name": name
      })
    },
    dropDownClick(name) {
      if ( this.detail_scene !== name ){
        this.detail_scene = name
      } else {
        // The scene is already selected with the dropdown
        this.detail_scene = ""  // Set to none
      }
      
    }
  }
};
</script>
