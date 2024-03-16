<script>
import Vue from "vue";

export default {
  name: "layoutError",
  props: {
    nameTitle: String,
    message: String,
    status: {
      type: String,
      validator: (value) => ["Error", "success"].includes(value),
    },
  },
};
</script>

<template>
  <div class="wrap--error">
    <slot> </slot>
    <span class="error">{{ message }}</span>
  </div>
</template>

<style lang="sass">
.wrap--error
  height: 50px
.error
  color: red
  position: relative
  margin-top: 10px
</style>
