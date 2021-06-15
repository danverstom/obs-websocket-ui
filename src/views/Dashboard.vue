<template>
  <section class="section">
    <div class="container">
      <div class="box">
        <div v-for="scene in scenes" :key="scene.name">
          <Scene :name="scene.name" :is_active="scene.is_active"/> 
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Scene from '../components/dashboard/Scene.vue'

export default {
  name: "Dashboard",
  components: {
    Scene
  },
  data() {
    return {
      scenes: [
        {
          name: "Minecraft",
          is_active: true
        },
        {
          name: "Starting Soon",
          is_active: false
        }
      ]
    }
  }
}
</script>

