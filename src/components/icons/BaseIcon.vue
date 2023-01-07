<template>
  <div
    :class="`color-${color} variant-${variant} color-override`"
    :style="style"
  >
    <component :is="dynamicIcon" />
  </div>
</template>

<script setup lang="ts">
import { computed, defineAsyncComponent } from "vue"
import type { PropType } from "vue"
import type { Icons } from "../../utils/types/icons.type"
import type { Color, Variant } from "@/utils/types/color.type"

const props = defineProps({
  icon: {
    type: String as PropType<Icons>,
    required: true,
  },
  color: {
    type: String as PropType<Color>,
    default: "primary",
  },
  variant: {
    type: String as PropType<Variant>,
    default: "900",
  },
  height: {
    type: Number,
    default: 24,
  },
  width: {
    type: Number,
    default: 24,
  },
})

const dynamicIcon = computed(() =>
  defineAsyncComponent(() => import(`./${props.icon}Icon.vue`)),
)
const style = computed(() => {
  return {
    height: props.height + "px",
    width: props.width + "px",
  }
})
</script>

<style lang="scss">
$colors: (primary, secondary, information, error, warning, success);
$variants: (100, 200, 300, 400, 500, 600, 700, 800, 900);

@each $color in $colors {
  @each $variant in $variants {
    .color-#{$color}.variant-#{$variant} {
      display: flex;
      justify-content: center;
      align-items: center;
      svg {
        stroke: get-color($color, $variant);
      }
    }
  }
}
</style>
