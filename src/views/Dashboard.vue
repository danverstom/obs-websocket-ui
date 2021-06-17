<template>
  <section class="section">
    <div class="container">
      <div class="box">
        <Connect
          v-on:connected="
            (obs) => {
              this.acceptConnection(obs);
            }
          "
          v-if="!obs"
        />
        <div v-if="obs">
          <div v-for="scene in scene_data.scenes" :key="scene.name">
            <Scene
              :name="scene.name"
              :current_scene="current_scene"
              :ui_selected_scene="ui_selected_scene"
              v-on:changeScene="changeScene"
            />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Scene from "../components/dashboard/Scene.vue";
import Connect from "../components/dashboard/Connect.vue";

export default {
  name: "Dashboard",
  components: {
    Scene,
    Connect,
  },
  methods: {
    async acceptConnection(obs) {
      this.obs = obs;
      console.log(this.obs);

      // Register events to listen to

      this.obs.on("SwitchScenes", (data) => {
        console.log("Scene change detected")
        console.log(data)
        this.queryScenes()
      })

      this.queryScenes();
    },
    changeScene(name) {
      console.log("changing scene to " + name)
      this.ui_selected_scene = name
      this.obs.send("SetCurrentScene", {
        "scene-name": name
      })
    },
    async queryScenes() {
      const scene_data = await this.obs.send("GetSceneList");
      console.log(scene_data);
      this.scene_data = scene_data;
      this.current_scene = scene_data.currentScene;
      this.ui_selected_scene = scene_data.currentScene;

      console.log(`Current Scene: ${this.current_scene}`)
      console.log(`UI Selected Scene: ${this.ui_selected_scene}`)

    }
  },
  data() {
    return {
      scene_data: [],
      current_scene: "",
      ui_selected_scene: "",
      obs: false,
    };
  },
};
</script>

