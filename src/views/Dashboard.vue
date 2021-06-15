<template>
  <section class="section">
    <div class="container">
      <div class="box">
        <div v-for="scene in scenes" :key="scene.name">
          <Scene :name="scene.name" /> 
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Scene from '../components/dashboard/Scene.vue'
import OBSWebSocket from 'obs-websocket-js'

export default {
  name: "Dashboard",
  components: {
    Scene
  },
  async created(){
    const obs = new OBSWebSocket();
    console.log(obs)
    await obs.connect({ address: 'localhost:4444', password: '$up3rSecretP@ssw0rd' });
    const scenes_data = await obs.send('GetSceneList');
    console.log(scenes_data)
    this.scenes = scenes_data.scenes
  },
  data() {
    return {
      scenes: [
        {
          name: "Minecraft"
        },
        {
          name: "Starting Soon"
        }
      ]
    }
  }
}
</script>

