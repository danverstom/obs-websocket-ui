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
          <Scenes :obs="obs" />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Scenes from "@/components/dashboard/scenes/Scenes.vue";
import Connect from "../components/dashboard/Connect.vue";

export default {
  name: "Dashboard",
  components: {
    Scenes,
    Connect,
  },
  methods: {
    async acceptConnection(obs) {
      this.obs = obs;
      console.log(this.obs);
    }
  },
  data() {
    return {
      obs: false,
    };
  },
};
</script>

