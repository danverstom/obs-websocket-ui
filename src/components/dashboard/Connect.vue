<template>
  <div class="field">
    <label class="label">Host: {{ host }}</label>
    <div class="control">
      <input
        type="text"
        class="input"
        placeholder="hostname i.e 'localhost'"
        v-model="host"
        :disabled="loading"
        @keydown.ctrl.enter="connectOBS"
      />
    </div>
  </div>
  <div class="field">
    <label class="label">Port</label>
    <div class="control">
      <input
        type="number"
        class="input"
        min="0"
        max="65535"
        v-model="port"
        :disabled="loading"
        @keydown.ctrl.enter="connectOBS"
      />
    </div>
  </div>
  <div class="field">
    <label class="label">Password</label>
    <div class="control">
      <input
        type="password"
        class="input"
        v-model="password"
        :disabled="loading"
        @keydown.ctrl.enter="connectOBS"
      />
    </div>
  </div>
  
  <div class="field is-grouped">
    <div class="control">
      <button
        :class="['button', 'is-link', loading ? 'is-loading' : '']"
        :disabled="buttonStatus"
        @click="connectOBS"
      >
        Connect
      </button>
    </div>
  </div>
  <p class="help is-danger" v-if="error">{{ error }}</p>
</template>

<script>
import OBSWebSocket from "obs-websocket-js";

export default {
  name: "Connect",
  data() {
    return {
      host: this.$route.query.host ? this.$route.query.host : "",
      port: this.$route.query.port ? this.$route.query.port : 4444,
      password: "",
      error: "",
      loading: false,
    };
  },
  computed: {
    buttonStatus() {
      return !(this.host && this.port);
    },
  },
  methods: {
    async connectOBS() {
      this.loading = true;
      const obs = new OBSWebSocket();
      try {
        const status = await obs.connect({
          address: `${this.host}:${this.port}`,
          password: this.password,
        });
        console.log(status);
        console.log("Emitting connected event");
        this.$emit("connected", obs); // Emit connected event to dashboard component
        this.loading = false;
      } catch (e) {
        console.log(e);
        this.error = e.error
        this.loading = false;
      }
    },
  },
  emits: ["connected"],
};
</script>