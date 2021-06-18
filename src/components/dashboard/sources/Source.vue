<template>
  <div class="source-container">
    <span>{{ source_details.sourceName }}</span>
    <span
      class="icon right clickable"
      @click="openSourceSettings"
    >
      <i class="fas fa-cog"></i>
    </span>
    
  </div>
</template>

<script>
export default {
  name: "Source",
  props: {
    source_details: Object,
    obs: Object,
  },
  emits: ["OpenSourceSettings"],
  data() {
    return {
      view_settings: false,
    };
  },
  async created() {
    console.log("source object created");
    console.log(this.source_details);
  },
  methods: {
    openSourceSettings() {
      console.log("Cog icon pressed on source", this.source_details.sourceName)
      this.$emit('OpenSourceSettings', this.source_details.sourceName)
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