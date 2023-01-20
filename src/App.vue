<template>
  <div id="app">
    <numberScroll
      target="12987423134"
      :width="500"
      :height="37"
      :font-size="37"
    ></numberScroll>
  </div>
</template>

<script>
import numberScroll from "@/components/numberScroll.vue";
export default {
  name: "App",
  components: {
    numberScroll,
  },
};
</script>

<style></style>
