<template>
  <div></div>
</template>

<script>
export default {
  metaInfo: {
    title: "About page",
  },
  serverPrefetch() {
    console.log("Run only on server 2");
  },
};
</script>
