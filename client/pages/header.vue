<template>
  <nav
    :class="[
      'shadow-' + shadow,
      sticky ? 'sticky' : fixed ? 'fixed' : absolute ? 'absolute' : '',
      top ? 'top-0' : 'bottom-0',
      classList,
      'w-screen',
    ]"
  >
    <div class="self-center pl-2 select-none flex justify-around">
      <slot name="left">Expedite.js</slot>
    </div>
    <div
      class="self-center select-none px-2 flex justify-around"
      v-if="hasSlot('left-middle')"
    >
      <slot name="left-middle"></slot>
    </div>
    <div class="self-center select-none px-2 flex justify-around">
      <slot name="middle"></slot>
    </div>
    <div
      class="self-center select-none px-2 flex justify-around"
      v-if="hasSlot('right-middle')"
    >
      <slot name="right-middle"></slot>
    </div>
    <div class="self-center pr-2 flex justify-around">
      <slot name="right"></slot>
    </div>
  </nav>
</template>
<script>
import * as headerDefault from "@/expedite/eheader";
export default {
  props: {
    sticky: {
      default: headerDefault.sticky,
    },
    fixed: {
      default: headerDefault.fixed,
    },
    absolute: {
      default: headerDefault.absolute,
    },
    shadow: {
      default: headerDefault.shadow,
    },
    top: {
      default: headerDefault.top,
    },
    classList: {
      default: headerDefault.classList,
    },
  },
  methods: {
    hasSlot(name = "default") {
      return !!this.$slots[name] || !!this.$scopedSlots[name];
    },
  },
};
</script>