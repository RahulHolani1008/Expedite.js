<template>
  <button
    :type="type"
    :disabled="
      disabled || (!isFormValid && isInsideForm && isFormValidationEnabled)
    "
    :class="[
      classList,
      'transition-04',
      'bg-' + color,
      'text-' + textColor,
      'border-' + borderColor,
      borderColor ? 'border-2' : '',
      'rounded-' + borderRadius,
      !growSize
        ? ''
        : `transition duration-${
            this.transitionDuration
          } ease-in-out transform hover:scale-${
            typeof this.growSize == 'string' ? growSize : '110'
          }`,
      'focus:ring focus:ring-' + ringColor,
    ]"
    class="align-text-bottom"
  >
    <slot name="icon-left"></slot>
    <span class="align-text-bottom">
      <slot>Submit</slot>
    </span>
    <slot name="icon-right"></slot>
  </button>
</template>
<script>
import * as buttonDefault from "@/expedite/eButton.js";
import * as colors from "@/expedite/colors.js";
export default {
  props: {
    disabled: {
      default: buttonDefault.disabled,
    },
    classList: {
      default: buttonDefault.classList,
    },
    color: {
      default: false,
    },
    textColor: {
      default: false,
    },
    borderColor: {
      default: false,
    },
    type: {
      default: buttonDefault.type,
    },
    borderRadius: {
      default: buttonDefault.borderRadius,
    },
    growSize: {
      default: buttonDefault.growSize,
    },
    transitionDuration: {
      default: buttonDefault.transitionDuration,
    },
    dark: {
      default: false,
    },
    ringColor: {
      default: false,
    },
  },
  data() {
    return {
      isFormValid: true,
      isInsideForm: true,
    };
  },
  computed: {
    isFormValidationEnabled: {
      get() {
        return this.$store.state.getters.isFormValidationEnabled || false;
      },
    },
  },
  mounted() {
    const darkMode = () => {
      return this.dark
        ? this.dark
        : colors.isDark == "media"
        ? window.matchMedia &&
          window.matchMedia("(prefers-color-scheme: dark)").matches
          ? true
          : false
        : colors.isDark;
    };
    this.textColor = buttonDefault.textColor[darkMode() ? "dark" : "light"];
    this.borderColor = buttonDefault.borderColor[darkMode() ? "dark" : "light"];
    this.color = buttonDefault.color[darkMode() ? "dark" : "light"];
    this.ringColor = buttonDefault.ringColor[darkMode() ? "dark" : "light"];
  },
};
</script>