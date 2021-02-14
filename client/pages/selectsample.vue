<template>
  <eSelect
    :options="['Cat', 'Mouse', 'Dog', 'Ghost']"
    class="ml-8 mt-8 w-1/4"
    label="Hello!"
    :multiple="true"
  >
    <template slot="icon-right">
      <span class="material-icons">face</span>
    </template>
  </eSelect>
</template>
<script>
import eSelect from "./select.vue";
export default {
  components: {
    eSelect,
  },
};
</script>