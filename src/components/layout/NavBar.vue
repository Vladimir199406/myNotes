<template>
  <nav class="navbar is-success" role="navigation" aria-label="main navigation">
    <div class="container is-max-desktop px-2">
      <div class="navbar-brand">
        <div class="navbar-item is-size-4 is-family-monospace">MyNotes</div>

        <a
          @click.prevent="showMobileNav = !showMobileNav"
          role="button"
          :class="{ 'is-active': showMobileNav }"
          class="navbar-burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbarBasicExample"
          ref="navbarBurgerRef"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div
        id="navbarBasicExample"
        class="navbar-menu"
        :class="{ 'is-active': showMobileNav }"
        ref="navBarMenuRef"
      >
        <div class="navbar-start">
          <button
            v-if="storeAuth.user.id"
            @click="logout"
            class="button is-small is-info mt-3 ml-3"
          >
            LOG OUT {{ storeAuth.user.email }}
          </button>
        </div>
        <div class="navbar-end">
          <RouterLink
            @click="showMobileNav = false"
            class="navbar-item"
            to="/"
            active-class="is-active"
          >
            Notes
          </RouterLink>
          <RouterLink
            @click="showMobileNav = false"
            class="navbar-item"
            to="/stats"
            active-class="is-active"
          >
            Stats
          </RouterLink>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
/*
  imports
*/
import { ref } from "vue";
import { onClickOutside } from "@vueuse/core";
import { useStoreAuth } from "@/stores/storeAuth";

/*
 data
*/
let showMobileNav = ref(false);
const navBarMenuRef = ref(null);
const navbarBurgerRef = ref(null);

/*
 store
*/
const storeAuth = useStoreAuth();

/*
 methods
*/
const logout = () => {
  showMobileNav.value = false;
  storeAuth.logoutUser();
};

/*
 other logic
*/
//composable
onClickOutside(
  navBarMenuRef,
  () => {
    showMobileNav.value = false;
  },
  { ignore: [navbarBurgerRef] }
);
</script>

<style>
@media (max-width: 1023px) {
  .navbar-menu {
    position: absolute;
    left: 0;
    width: 100%;
  }
}
</style>
