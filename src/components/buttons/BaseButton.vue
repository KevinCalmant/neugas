<template>
  <component
    :is="tag"
    :class="buttonClasses"
    :type="type"
    v-bind="$attrs"
    @click="$emit('click', $event)"
  >
    <p class="v-btn__slot"><slot /></p>
  </component>
</template>

<script setup lang="ts">
import type { PropType } from "vue"
import type {
  ButtonSize,
  ButtonTag,
  ButtonType,
} from "@/utils/types/buttons.type"
import { computed } from "vue"
import type { Color } from "@/utils/types/color.type"

const props = defineProps({
  block: {
    type: Boolean,
    default: false,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  color: {
    type: String as PropType<Color>,
    default: "primary",
  },
  loading: {
    type: Boolean,
    default: false,
  },
  type: {
    type: String as PropType<ButtonType>,
    default: "button",
  },
  outline: {
    type: Boolean,
    default: false,
  },
  size: {
    type: String as PropType<ButtonSize>,
    default: "large",
  },
  tag: {
    type: String as PropType<ButtonTag>,
    default: "button",
  },
  icon: {
    type: Boolean,
    default: false,
  },
})

const buttonClasses = computed(() => {
  return {
    "v-btn": true,
    [`v-btn--${props.size + (props.icon ? "-icon" : "")}`]: props.size,
    [`v-btn--${props.color}`]: props.color,
    "v-btn--block": props.block,
    "v-btn--disabled": props.disabled,
    "v-btn--loading": props.loading,
    "v-btn--outlined": props.outline,
  }
})
</script>

<style lang="scss">
$button-size-map: (
  null: (
    padding: 1rem,
  ),
  "--small": (
    height: 32px,
    font-size: 80%,
    display: flex,
    flex-direction: row,
    justify-content: center,
    align-items: center,
    padding: 5px 16px,
    gap: 10px,
  ),
  "--small-icon": (
    width: 32px,
    height: 32px,
    font-size: 80%,
    display: flex,
    flex-direction: row,
    justify-content: center,
    align-items: center,
    padding: 8px,
    gap: 8px,
    svg: (
      width: 70%,
      height: 70%,
    ),
  ),
  "--medium": (
    height: 37px,
    font-size: 90%,
    display: flex,
    flex-direction: row,
    justify-content: center,
    align-items: center,
    padding: 13px 24px,
    gap: 10px,
  ),
  "--medium-icon": (
    width: 37px,
    height: 37px,
    font-size: 90%,
    display: flex,
    flex-direction: row,
    justify-content: center,
    align-items: center,
    padding: 16px,
    gap: 8px,
    svg: (
      width: 80%,
      height: 80%,
    ),
  ),
  "--large": (
    font-size: 110%,
    height: 52px,
    display: flex,
    flex-direction: row,
    justify-content: center,
    align-items: center,
    padding: 16px,
    gap: 8px,
    svg: (
      width: 80%,
      height: 80%,
    ),
  ),
  "--large-icon": (
    font-size: 110%,
    height: 52px,
    width: 52px,
    display: flex,
    flex-direction: row,
    justify-content: center,
    align-items: center,
    padding: 16px,
    gap: 8px,
    svg: (
      width: 80%,
      height: 80%,
    ),
  ),
) !default;

$button-theme-names: (
  "--primary",
  "--secondary",
  "--information",
  "--success",
  "--warning",
  "--error"
) !default;

$default-theme-map: (
  "all": (
    "buttonStyles": (
      color: white,
    ),
    "svgStyles": (
      stroke: white,
    ),
  ),
  "--primary": plain-button-style(primary),
  "--secondary": plain-button-style(secondary, true),
  "--information": plain-button-style(information),
  "--success": plain-button-style(success),
  "--warning": plain-button-style(warning),
  "--error": plain-button-style(error),
) !default;

$outline-theme-map: (
  "all": (
    "buttonStyles": (
      background-color: transparent,
    ),
    "hover": (
      background-color: transparent,
    ),
  ),
  "--primary": outlined-button-style(primary),
  "--secondary": outlined-button-style(secondary),
  "--information": outlined-button-style(information),
  "--success": outlined-button-style(success),
  "--warning": outlined-button-style(warning),
  "--error": outlined-button-style(error),
) !default;

$button-type-theme-maps: (
  null: $default-theme-map,
  "--outlined": $outline-theme-map,
) !default;

$button-default-styles: (
  border: 1px solid transparent,
  border-radius: 10px,
  outline: none,
) !default;

.v-btn {
  cursor: pointer;
  @include styles-from-map($button-default-styles);
}

.v-btn--block {
  display: block;
  width: 100%;
}

.v-btn--disabled {
  cursor: not-allowed;
}

.v-btn--loading {
  pointer-events: none;
  position: relative;

  .v-btn__slot {
    color: transparent;
  }

  &::after {
    animation: animSpin 0.5s infinite linear;
    border-radius: 50%;
    border: 2px solid currentColor;
    border-right-color: transparent;
    border-top-color: transparent;
    content: "";
    display: block;
    height: 1em;
    left: calc(50% - 0.5em);
    position: absolute;
    top: calc(50% - 0.5em);
    width: 1em;
  }
}

/** Sizes */
@each $size-name, $style-map in $button-size-map {
  .v-btn#{$size-name} {
    @include styles-from-map($style-map);
  }
}

@each $type-name, $theme-map in $button-type-theme-maps {
  $all: ();
  @if map-has-key($theme-map, "all") {
    $all: map-get($theme-map, "all");
  }

  .v-btn#{$type-name} {
    transition: 0.2s all;

    @each $theme-name in $button-theme-names {
      $theme: recursive-map-merge($all, map-get($theme-map, $theme-name));

      .v-btn__slot {
        svg {
          @include styles-from-map(map-get($theme, "svgStyles"));
        }
      }

      &.v-btn#{$theme-name} {
        @include styles-from-map(map-get($theme, "buttonStyles"));

        &:hover:not(.v-btn--disabled) {
          @include styles-from-map(map-get($theme, "hover"));
        }

        &:active:not(.v-btn--disabled) {
          @include styles-from-map(map-get($theme, "active"));
        }

        &.v-btn--disabled {
          @include styles-from-map(map-get($theme, "disabled"));
        }
      }
    }
  }
}
</style>
