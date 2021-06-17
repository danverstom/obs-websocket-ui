<template>
  <div v-for="scene in scene_data.scenes" :key="scene.name">
    <Scene
      :name="scene.name"
      :current_scene="current_scene"
      :ui_selected_scene="ui_selected_scene"
      v-on:changeScene="changeScene"
    />
  </div>
</template>

<script>
import Scene from "../components/dashboard/Scene.vue";

export default {
  
}

</script>
