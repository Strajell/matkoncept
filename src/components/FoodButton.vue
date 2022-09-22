<template>
  <v-btn @click="onClick()" :style="{ background: color }">{{
    buttontext
  }}</v-btn>
</template>

<script>
export default {
  name: "FoodButton",
  props: {
    buttontext: String,
    color: String,
  },
  methods: {
    onClick() {
      this.$emit("btn-click");
    },
  },
};
</script>
