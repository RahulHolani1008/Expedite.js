<template>
  <div id="q-app">
    <div class="q-ma-md">
      Fork and make your own! Do NOT use self-closing tags here on Codepen.
    </div>

    <div class="q-ma-md">
      <q-btn label="Notify" color="primary" @click="notify"></q-btn>
    </div>

    <div class="q-ma-xl">Running Quasar v{{ version }}</div>
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
