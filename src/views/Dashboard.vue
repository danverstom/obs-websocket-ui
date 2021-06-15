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
              :current_scene="scene_data.currentScene"
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
      const scene_data = await this.obs.send("GetSceneList");
      console.log(scene_data);
      this.scene_data = scene_data;
    },
  },
  data() {
    return {
      scene_data: [],
      obs: false,
    };
  },
};
</script>

