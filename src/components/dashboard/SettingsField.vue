<template>
  <div class="box field" v-if="!isObject">
    <label class="label">{{ setting_name }}</label>
    <p class="control">
      <input class="input" v-model="new_value" @change="broadcastChange" />
    </p>
  </div>
  <div class="box" v-else>
    <h2 class="subtitle">{{ setting_name }}</h2>
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
.box {
  margin: 10px;
}
</style>