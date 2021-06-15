<template>
  <nav class="navbar">
    <div class="container">
      <div class="navbar-brand">
        <router-link class="navbar-item" to="/">
        <img alt="Vue logo" src="../assets/logo.png">
        <p class="navbar-item">OBS Web UI</p>
        </router-link>
        <a
          role="button"
          class="navbar-burger"
          data-target="navMenu"
          aria-label="menu"
          aria-expanded="false"
          :class="[dropdown_active ? 'is-active' : '', 'navbar-burger']"
          @click="toggleDropdown"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      <div :class="[dropdown_active ? 'is-active' : '', 'navbar-menu']">
        <div class="navbar-start">
            <router-link
              v-for="item in navbar_items" :key="item.title"
              class="navbar-item"
              active-class="is-active"
              :to="item.route"
              @click="() => {
                this.dropdown_active = False
              }"
              >{{ item.title }}</router-link
            >
        </div>

        <div class="navbar-end">
          <div class="navbar-item">
            <div class="buttons">
              <a
                class="button is-dark"
                href="https://github.com/TomD53/obs-websocket-ui"
                target="blank"
              >
                GitHub
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {
      dropdown_active: false,
      navbar_items: [
        {
          title: "Home",
          route: "/",
        },
        {
          title: "Dashboard",
          route: "/dashboard",
        },
      ],
    };
  },
  methods: {
    toggleDropdown() {
      this.dropdown_active = !this.dropdown_active;
    },
  },
};
</script>

