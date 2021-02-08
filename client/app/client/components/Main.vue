<template>
  <div id="q-app">
    <div class="q-ma-md">
      Fork and make your own! Do NOT use self-closing tags here on Codepen.
    </div>

    <div class="m-96">
      <v-btn color="black" @click="notify">Notify</v-btn>
      <p class="h1">h1. Bootstrap heading</p>
    </div>

    <div class="ml-5">Running Quasar v{{ version }}</div>
  </div>
</template>

<script>
export default {
  metaInfo: {
    title: "Expedite",
  },
  data() {
    return {
      title: "Default title",
      version: "1",
    };
  },
  mounted() {
    console.log("Mounted");
  },
  serverPrefetch() {
    console.log("Run only on server");
  },
  methods: {
    notify: function () {
      this.$q.notify("Running on Quasar v" + this.$q.version);
    },
  },
};
</script>
