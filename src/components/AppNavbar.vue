<template>
  <div class="navbar__container">
    <div>
      <div class="navbar__brand">
        <img src="../assets/logo.svg" alt="Nuegas logo" />
        <h1>Nuegas</h1>
      </div>
      <div class="navbar__menu">
        <ul>
          <AppNavLink icon="Category" label="Overview" to="/" />
          <AppNavLink icon="Book" label="Tasks" to="/tasks" />
          <AppNavLink icon="UserOctagon" label="Mentors" to="/mentors" />
          <AppNavLink icon="Message" label="Messages" to="/messages" />
          <AppNavLink icon="Settings" label="Settings" to="/settings" />
        </ul>
      </div>
    </div>

    <div class="navbar__help-center"></div>
  </div>
</template>

<script>
import { defineComponent } from "vue"
import BaseIcon from "@/components/BaseIcon.vue"
import AppNavLink from "@/components/AppNavLink.vue"

export default defineComponent({
  components: { AppNavLink, BaseIcon },
})
</script>

<style scoped lang="scss">
.navbar {
  &__container {
    background-color: white;
    width: 15vw;
    height: 100vh;
    padding: 32px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
  }

  &__brand {
    display: flex;
    flex-direction: row;
    gap: 15px;

    h1 {
      font-weight: 600;
      font-size: 32px;
      line-height: 150%;
    }
  }

  &__menu {
    margin-top: 62px;
    color: get-color(secondary, 300);

    font-style: normal;
    font-weight: 800;
    font-size: 14px;
    line-height: 150%;
    letter-spacing: -0.02em;

    ul {
      display: flex;
      flex-direction: column;
      gap: 24px;
    }
  }

  &__help-center {
    width: 188px;
    height: 248px;
    background: get-color(secondary, 800);
    border-radius: 10px;
  }
}

@media (max-width: 640px) {
  .navbar-container {
    display: none;
  }
}
</style>
