<template>
  <section class="section">
    <div class="container 2">
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
          <Scenes :obs="obs" v-on:OpenSourceSettings="openSourceSettings" />
          <SourceDetailModal
            v-if="view_source_detail"
            v-on:CloseSourceDetailModal="() => {
              view_source_detail = false
            }"
            :source_name="detail_source_name"
            :obs="obs"
            :scene_name="detail_scene_name"
          />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Scenes from "@/components/dashboard/scenes/Scenes.vue";
import Connect from "../components/dashboard/Connect.vue";

import SourceDetailModal from "@/components/dashboard/sources/SourceDetailModal.vue";

export default {
  name: "Dashboard",
  components: {
    Scenes,
    Connect,
    SourceDetailModal,
  },
  methods: {
    async acceptConnection(obs) {
      this.obs = obs;
      console.log(this.obs);
    },
    openSourceSettings(source_name, scene_name) {
      console.log("Dashboard got source settings open command", source_name);
      this.detail_source_name = source_name
      this.detail_scene_name = scene_name
      this.view_source_detail = true
    },
  },
  data() {
    return {
      obs: false,
      detail_source_name: "",
      detail_scene_name: "",
      view_source_detail: false,
    };
  },
  unmounted() {
    clearInterval(this.interval)
  }
};
</script>

