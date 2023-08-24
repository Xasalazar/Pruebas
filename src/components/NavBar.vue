<template>
  <nav class="navbar">
    <div class="navbar-left">
      <img
        src="../assets/logo.png"
        alt="Logo"
        class="logo"
        :class="{ 'logo-mobile': ShowText }"
      />
    </div>
    <div class="navbar-right">
      <img
        v-show="ShowText"
        src="../assets/Smart.png"
        alt="Logo"
        class="logo"
        style="width: 200px"
      />
      <button @click="toggleMenu" class="menu-toggle">
        <svg viewBox="0 0 24 24" :width="30" :height="30">
          <path :fill="iconcolor" :d="iconClass" />
        </svg>
      </button>
      <div v-if="false" class="dropdown-menu"></div>
    </div>
  </nav>
  <slot />
</template>

<script>
import { mdiMenu } from "@mdi/js";

export default {
  data() {
    return {
      showMenu: false,
    };
  },
  methods: {
    toggleMenu() {
      this.showMenu = !this.showMenu;
    },
  },
  computed: {
    iconClass() {
      return mdiMenu;
    },
    iconcolor() {
      return "#007bff";
    },
  },
  props: {
    ShowText: {
      type: Boolean,
      default: true,
    },
  },
};
</script>

<style>
.logo-mobile {
  width: 80%;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--secondary-color);
  color: var(--secondary-color);
  padding: 10px 20px;
}

.navbar-left {
  display: flex;
  align-items: center;
}

.logo {
  width: 50%;
  margin-right: 10px;
}

.navbar-right {
  display: flex;
  align-items: center;
}

.menu-toggle {
  background-color: transparent;
  border: none;
  color: var(--secondary-color);
  cursor: pointer;
  margin-right: 10px;
  margin-left: 10px;
}

.dropdown-menu {
  top: 100%;
  right: 0;
  background-color: var(--secondary-color);
  display: flex;
  flex-direction: column;
  padding: 10px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.nav-link {
  margin: 5px 0;
  color: var(--secondary-color);
  text-decoration: none;
}
</style>
