<template>
  <div class="field" v-if="!isObject">
    <label class="label">{{ setting_name }}</label>
    <p class="control">
      <input class="input" type="text" v-model="new_value" @change="broadcastChange" v-if="typeof new_value === 'string'"/>
      <input class="input" type="number" v-model="new_value" @change="broadcastChange" v-else-if="typeof new_value === 'number'"/>
      <label class="checkbox" v-else-if="typeof new_value === 'boolean'">
        <input type="checkbox" v-model="new_value" @change="broadcastChange">
        {{ new_value }}
      </label>
    </p>

  </div>
  <div class="box" v-else>
    <label class="label" style="text-decoration: underline">{{ setting_name }}</label>
    <div v-for="(sub_value, sub_setting_name) in value" :key="sub_setting_name">
      <SettingsField :setting_name="sub_setting_name" :value="sub_value" v-on:SettingValueChange="childValueChange" />
    </div>
  </div>
</template>

<script>
import SettingsField from "@/components/dashboard/SettingsField.vue";

export default {
  name: "SettingsField",
  props: ["setting_name", "value"],
  emits: ["SettingValueChange"],
  data() {
    return {
      new_value: Object,
    }
  },
  components: {
    SettingsField
  },
  created() {
    this.new_value = this.value
  },
  methods: {
    childValueChange(setting_name, value) {
      this.new_value[setting_name] = value
      this.broadcastChange()
    },
    broadcastChange() {
      console.log("Broadcasting change", this.setting_name, this.new_value)
      this.$emit("SettingValueChange", this.setting_name, this.new_value)
    }
  },
  computed: {
    isObject() {
      return typeof this.value === "object" && this.value !== null;
    },
  },
};
</script>

<style scoped>
div {
  margin: 10px
}
</style>