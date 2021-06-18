<template>
  <div class="container">
    <div class="modal is-active">
      <div class="modal-background" @click="closeModal"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Source Settings: {{ source_name }}</p>
          <button
            class="delete"
            aria-label="close"
            @click="closeModal"
          ></button>
        </header>
        <section class="modal-card-body">
          <h1 class="subtitle">Source Settings</h1>
          <div v-for="(value, setting_name) in source_settings" :key="setting_name">
            <SettingsField :setting_name="setting_name" :value="value" v-on:SettingValueChange="sourceSettingChange"/>
          </div>
          <hr>
          <h1 class="subtitle">Scene Specific Properties</h1>
          <div v-for="(value, setting_name) in source_properties" :key="setting_name">
            <SettingsField :setting_name="setting_name" :value="value" v-on:SettingValueChange="sourceScenePropertyChange"/>
          </div>
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success" :disabled="!(settingChange || propertyChange)" @click="saveChanges(true)">Save</button>
          <button class="button is-success is-light" :disabled="!(settingChange || propertyChange)" @click="saveChanges(false)">Apply</button>
          <button class="button is-light" @click="closeModal">Cancel</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import SettingsField from "@/components/dashboard/SettingsField.vue"

export default {
  name: "SourceDetailModal",
  props: {
    source_name: String,
    scene_name: String,
    obs: Object,
  },
  components: {
    SettingsField
  },
  data() {
    return {
      source_settings: false,
      source_properties: false,

      old_source_settings: false,
      old_source_properties: false
    }
  },
  async created() {
    await this.getSettings()
    await this.getProperties()

    console.log("Current settings", this.source_settings)
    console.log("Old settings", this.old_source_settings)
    console.log(this.settingChange, this.propertyChange)
  },
  unmounted() {
    console.log("Source Detail Modal destroyed");
  },
  emits: ["CloseSourceDetailModal"],
  methods: {
    async saveChanges(close) {
      if (this.settingChange) {
        const result = await this.obs.send("SetSourceSettings", {
          sourceName: this.source_name,
          sourceSettings: this.source_settings
        });
        console.log("Sent Setting Change. Response:", result)
        this.source_settings = result.sourceSettings
        this.old_source_settings = JSON.parse(JSON.stringify(result.sourceSettings))  // hate this but it works
      }
      if (this.propertyChange) {
        var data = JSON.parse(JSON.stringify(this.source_properties));
        data["item"] = this.source_name
        console.log(data)
        const result = await this.obs.send("SetSceneItemProperties", data);
        console.log("Sent Property Change. Response:", result)
        this.getProperties()
      }
      if (close) {
        this.closeModal();
      }
      
    },
    sourceSettingChange(setting_name, new_value) {
      console.log("Source Setting Change Detected:", setting_name, new_value)
      this.source_settings[setting_name] = new_value
      console.log("Current settings", this.source_settings)
      console.log("Old settings", this.old_source_settings)
      console.log(this.settingChange, this.propertyChange)
    },
    sourceScenePropertyChange(setting_name, new_value) {
      console.log("Source Property Change Detected:", setting_name, new_value)
      this.source_properties[setting_name] = new_value
      console.log("Current settings", this.source_properties)
      console.log("Old settings", this.old_source_properties)
      console.log(this.settingChange, this.propertyChange)
    },
    closeModal() {
      this.$emit("CloseSourceDetailModal");
    },
    async getSettings() {
      const data = await this.obs.send("GetSourceSettings", {
        sourceName: this.source_name,
      });
      console.log(
        "Source settings for source ", this.source_name, data.sourceSettings
      );
      this.source_settings = data.sourceSettings
      this.old_source_settings = JSON.parse(JSON.stringify(data.sourceSettings))  // hate this but it works
      return data.sourceSettings
    },
    async getProperties() {
      console.log(this.scene_name)
      console.log(this.source_name)
      const data = await this.obs.send("GetSceneItemProperties", {
        'scene-name': this.scene_name,
        'item': this.source_name,
      });
      this.removeUnwantedKeys(data);
      console.log(
        "Scene-specific properties for source", this.source_name, data
      );
      this.source_properties = data
      this.old_source_properties = JSON.parse(JSON.stringify(data))  // hate this but it works
      return data;
    },
    removeUnwantedKeys(data) {
      delete data["messageId"]
      delete data["itemId"]
      delete data["message-id"]
      delete data["status"]
      delete data["bounds"]
      delete data["crop"]
      delete data["position"]
      delete data["scale"]
    }
  },
  computed: {
    settingChange() {
      return (JSON.stringify(this.source_settings) !== JSON.stringify(this.old_source_settings))
    },
    propertyChange() {
      return (JSON.stringify(this.source_properties) !== JSON.stringify(this.old_source_properties))
    }
  }
};
</script>
