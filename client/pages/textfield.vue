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
        !labelColor.split('-')[1]
          ? 'text-' + labelColor + '-500'
          : 'text-' + labelColor,
        !color.split('-')[1]
          ? color == 'transparent'
            ? 'bg-white'
            : 'bg-' + color + '-500'
          : color.test(/transparent\-([1-9]|[1-9][0-9]|[1-9][0-9][0-9]|1000)/g)
          ? 'bg-white'
          : 'bg-' + color,
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
          ? `ring ring-${showValidation ? 'red' : primary}-300`
          : solid
          ? `focus:outline-none focus:ring-${primary}-300`
          : underlined
          ? !borderColor.split('-')[1]
            ? 'border-' + borderColor + '-500'
            : 'border-' + borderColor
          : `${showValidation ? '' : 'focus:'}ring ${
              showValidation ? '' : 'focus:'
            }ring-${showValidation ? 'red' : primary}-300`,
        shadow ? 'shadow-' + shadow : '',
        dense ? 'h-10 p-2' : 'h-16 p-4',
        hasSlot('icon-right') ? (dense ? 'pr-7' : 'pr-10') : '',
        hasSlot('icon-left') ? (dense ? 'pl-7' : 'pl-10') : '',
        !textColor.split('-')[1]
          ? 'text-' + textColor + '-500'
          : 'text-' + textColor,
        !color.split('-')[1] ? 'bg-' + color + '-500' : 'bg-' + color,
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
export default {
  props: {
    outlined: {
      default: false,
    },
    solid: {
      default: false,
    },
    underlined: {
      default: false,
    },
    dense: {
      default: false,
    },
    value: {
      default: "",
    },
    primary: {
      default: "indigo",
    },
    label: {
      default: false,
    },
    placeholder: {
      default: false,
    },
    shadow: {
      default: false,
    },
    color: {
      default: "transparent",
    },
    textColor: {
      default: "black",
    },
    labelColor: {
      default: "black",
    },
    borderRadius: {
      default: "xl",
    },
    borderColor: {
      default: "transparent",
    },
    realTimeError: {
      default: false,
    },
    error: {
      default: false,
    },
    classList: {
      default: "",
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