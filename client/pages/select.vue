<template>
  <div class="relative select h-min">
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
        color == 'transparent' ? 'bg-white' : 'bg-' + color,
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
    <select
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
    >
      <slot
        name="options-key-value"
        v-if="hasSlot('options-key-value') || options[0].key"
      >
        <option v-for="opt in options" :key="opt.id">
          {{ opt.label }}
        </option>
      </slot>
      <slot name="options" v-if="hasSlot('options') || !options[0].key">
        <option v-for="opt in options" :key="opt">
          {{ opt }}
        </option>
      </slot>
    </select>
    <span
      class="absolute z-10 select-none"
      v-if="hasSlot('icon-right')"
      :style="dense ? 'right: 10px; top: 7px;' : 'right: 15px; top: 20px;'"
      ><slot name="icon-right"></slot
    ></span>
  </div>
</template>
<script>
import * as selectDefault from "@/expedite/eSelect.js";
import * as colors from "@/expedite/colors.js";
export default {
  props: {
    outlined: {
      default: selectDefault.outlined,
    },
    solid: {
      default: selectDefault.solid,
    },
    underlined: {
      default: selectDefault.underlined,
    },
    dense: {
      default: selectDefault.dense,
    },
    value: {
      default: selectDefault.value,
    },
    ringColor: {
      default: false,
    },
    label: {
      default: selectDefault.label,
    },
    placeholder: {
      default: selectDefault.placeholder,
    },
    shadow: {
      default: selectDefault.shadow,
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
      default: selectDefault.borderRadius,
    },
    borderColor: {
      default: false,
    },
    realTimeError: {
      default: selectDefault.realTimeError,
    },
    error: {
      default: selectDefault.error,
    },
    multiple: {
      default: selectDefault.multiple,
    },
    classList: {
      default: selectDefault.classList,
    },
    options: {
      required: true,
    },
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
  data() {
    return {
      isFocused: false,
      count: 0,
      content: this.value,
    };
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
    this.textColor = selectDefault.textColor[darkMode() ? "dark" : "light"];
    this.labelColor = selectDefault.labelColor[darkMode() ? "dark" : "light"];
    this.borderColor = selectDefault.borderColor[darkMode() ? "dark" : "light"];
    this.color = selectDefault.color[darkMode() ? "dark" : "light"];
    this.ringColor = selectDefault.ringColor[darkMode() ? "dark" : "light"];
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