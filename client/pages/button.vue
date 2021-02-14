<template>
  <button
    :type="type"
    :disabled="
      disabled || (!isFormValid && isInsideForm && isFormValidationEnabled)
    "
    :class="[
      classList,
      !color.split('-')[1] ? 'bg-' + color + '-500' : 'bg-' + color,
      !textColor.split('-')[1]
        ? 'text-' + textColor + '-500'
        : 'text-' + textColor,
      !borderColor.split('-')[1]
        ? 'border-' + borderColor + '-500'
        : 'border-' + borderColor,
      borderColor != '' && borderColor ? 'border-2' : '',
      'rounded-' + borderRadius,
      !growSize
        ? ''
        : `transition duration-${
            this.transitionDuration
          } ease-in-out transform hover:scale-${
            typeof this.growSize == 'string' ? growSize : '110'
          }`,
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
export default {
  props: {
    disabled: {
      default: false,
    },
    classList: {
      default: "px-5 py-2 select-none outline-none",
    },
    color: {
      default: "white",
    },
    textColor: {
      default: "blue",
    },
    borderColor: {
      default: "blue",
    },
    type: {
      default: "submit",
    },
    borderRadius: {
      default: "xl",
    },
    growSize: {
      default: false,
    },
    transitionDuration: {
      default: "500",
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
};
</script>