<template>
  <div class="relative textfield h-min">
    <span
      v-if="label"
      style="padding-left: 5px; padding-right: 5px"
      class="absolute z-8 select-none rounded-xl"
      :class="[
        isFocused || hasText || placeholder
          ? 'label-focused'
          : hasSlot('icon-left')
          ? dense
            ? 'label-icon-dense'
            : 'label-icon'
          : dense
          ? 'label-dense'
          : 'label',
        'text-' + labelColor,
        color == 'transparent' ? 'bg-white' : 'bg-' + color + '-500',
      ]"
      >{{ label }}</span
    >
    <span
      v-if="hasSlot('icon-left')"
      class="absolute z-10 select-none"
      :style="
        label
          ? dense
            ? 'left: 3px; top: 7px;'
            : 'left: 10px; top: 20px;'
          : dense
          ? 'left: 3px; top: 7px;'
          : 'left: 10px; top: 20px;'
      "
      ><slot name="icon-left"></slot
    ></span>
    <input
      v-model="content"
      @focus="isFocused = true"
      @blur="isFocused = false"
      @input="handleInput"
      type="text"
      :placeholder="placeholder"
      :class="[
        outlined
          ? `ring ring-${showValidation ? 'red' : ringColor}-300`
          : solid
          ? `focus:outline-none focus:ring-${ringColor}-300`
          : underlined
          ? 'border-' + borderColor
          : `${showValidation ? '' : 'focus:'}ring ${
              showValidation ? '' : 'focus:'
            }ring-${showValidation ? 'red' : borderColor}-300`,
        shadow ? 'shadow-' + shadow : '',
        dense ? 'h-10 p-2' : 'h-16 p-4',
        hasSlot('icon-right') ? (dense ? 'pr-7' : 'pr-10') : '',
        hasSlot('icon-left') ? (dense ? 'pl-7' : 'pl-10') : '',
        'text-' + textColor,
        'bg-' + color,
        classList,
        'rounded-' + borderRadius,
      ]"
      class="w-full outline-none placeholder-gray-600 transition-04 focus:placeholder-gray-400"
    />
    <span
      class="absolute z-10 select-none"
      v-if="hasSlot('icon-right')"
      :style="dense ? 'right: 3px; top: 7px;' : 'right: 10px; top: 20px;'"
      ><slot name="icon-right"></slot
    ></span>
  </div>
</template>
<script>
import * as textfieldDefault from "@/expedite/eTextField.js";
import * as colors from "@/expedite/colors.js";
export default {
  props: {
    outlined: {
      default: textfieldDefault.outlined,
    },
    solid: {
      default: textfieldDefault.solid,
    },
    underlined: {
      default: textfieldDefault.underlined,
    },
    dense: {
      default: textfieldDefault.dense,
    },
    value: {
      default: textfieldDefault.value,
    },
    ringColor: {
      default: false,
    },
    label: {
      default: textfieldDefault.label,
    },
    placeholder: {
      default: textfieldDefault.placeholder,
    },
    shadow: {
      default: textfieldDefault.shadow,
    },
    color: {
      default: false,
    },
    textColor: {
      default: false,
    },
    labelColor: {
      default: false,
    },
    borderRadius: {
      default: textfieldDefault.borderRadius,
    },
    borderColor: {
      default: false,
    },
    realTimeError: {
      default: textfieldDefault.realTimeError,
    },
    error: {
      default: textfieldDefault.error,
    },
    classList: {
      default: textfieldDefault.classList,
    },
  },
  data() {
    return {
      isFocused: false,
      count: 0,
      content: this.value,
    };
  },
  computed: {
    hasText() {
      return !!this.value;
    },
    hasBeenFocused() {
      if (this.isFocused) {
        this.count += 1;
        return false;
      } else {
        return this.count ? true : false;
      }
    },
    showValidation() {
      return (
        (this.hasBeenFocused && this.error) ||
        (this.isFocused && this.realTimeError && this.error)
      );
    },
  },
  mounted() {
    const darkMode = () => {
      return colors.isDark == "media"
        ? window.matchMedia &&
          window.matchMedia("(prefers-color-scheme: dark)").matches
          ? true
          : false
        : colors.isDark;
    };
    this.textColor = textfieldDefault.textColor[darkMode() ? "dark" : "light"];
    this.labelColor =
      textfieldDefault.labelColor[darkMode() ? "dark" : "light"];
    this.borderColor =
      textfieldDefault.borderColor[darkMode() ? "dark" : "light"];
    this.color = textfieldDefault.color[darkMode() ? "dark" : "light"];
    this.ringColor = textfieldDefault.ringColor[darkMode() ? "dark" : "light"];
  },
  methods: {
    hasSlot(name = "default") {
      return !!this.$slots[name] || !!this.$scopedSlots[name];
    },
    handleInput(e) {
      this.$emit("input", this.content);
    },
  },
};
</script>