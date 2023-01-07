<template>
  <RouterLink :to="to" custom v-slot="{ href, navigate, isActive }">
    <li :class="`nav-link__menu-item nav-link${isActive ? '--active' : ''}`">
      <BaseIcon
        :icon="icon"
        color="secondary"
        :variant="isActive ? '500' : '300'"
      />
      <h2>
        <a :href="href" @click="navigate">{{ label }}</a>
      </h2>
    </li>
  </RouterLink>
</template>

<script setup lang="ts">
import type { PropType } from "vue"
import type { Icons } from "@/utils/types/icons.type"
import BaseIcon from "@/components/BaseIcon.vue"

defineProps({
  to: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
  icon: {
    type: String as PropType<Icons>,
    required: true,
  },
})
</script>

<style lang="scss">
.nav-link {
  &__menu-item {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    gap: 12px;

    width: 200px;
    height: 55px;

    border-radius: 10px;
    padding: 10px 20px;
  }

  &--active {
    color: get-color(secondary, 500);
    background-color: #f5f5f7;
    border-radius: 20px;
  }
}
</style>
