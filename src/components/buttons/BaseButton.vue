<template>
  <component
    :is="tag"
    :class="buttonClasses"
    :type="type"
    v-bind="$attrs"
    @click="$emit('click', $event)"
  >
    <div :class="`v-btn__slot ${!iconOnly ? `v-btn__slot--${position}` : ''}`">
      <BaseIcon v-if="icon" :icon="'Message'" color="primary" />
      <p><slot v-if="!iconOnly" /></p>
    </div>
  </component>
</template>

<script setup lang="ts">
import type { PropType } from "vue"
import type {
  ButtonPosition,
  ButtonSize,
  ButtonTag,
  ButtonType,
} from "@/utils/types/buttons.type"
import { computed } from "vue"
import type { Color } from "@/utils/types/color.type"
import type { Icons } from "@/utils/types/icons.type"
import BaseIcon from "@/components/icons/BaseIcon.vue"

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
    type: String as PropType<Icons>,
    required: false,
  },
  iconOnly: {
    type: Boolean,
    default: false,
  },
  position: {
    type: String as PropType<ButtonPosition>,
    default: "left",
  },
})

const buttonClasses = computed(() => {
  return {
    "v-btn": true,
    [`v-btn--${props.size + (props.iconOnly ? "-icon" : "")}`]: props.size,
    [`v-btn--${props.color}`]: props.color,
    "v-btn--block": props.block,
    "v-btn--disabled": props.disabled,
    "v-btn--loading": props.loading,
    "v-btn--outlined": props.outline,
  }
})
</script>

<style scoped lang="scss">
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
    padding: 5px 10px,
    gap: 10px,
    "::v-deep svg": (
      width: 70%,
      height: 70%,
    ),
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
    "::v-deep svg": (
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
    "::v-deep svg": (
      width: 80%,
      height: 80%,
    ),
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
    "::v-deep svg": (
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
    "::v-deep svg": (
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
    "::v-deep svg": (
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

.v-btn__slot {
  display: flex;
  width: fit-content;
  align-items: center;

  &--left {
    flex-direction: row;
    gap: 10px;
  }

  &--right {
    flex-direction: row-reverse;
    gap: 10px;
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
    @each $theme-name in $button-theme-names {
      $theme: recursive-map-merge($all, map-get($theme-map, $theme-name));

      &.v-btn#{$theme-name} {
        @include styles-from-map(map-get($theme, "buttonStyles"));

        ::v-deep svg {
          @include styles-from-map(map-get($theme, "svgStyles"));
        }

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
