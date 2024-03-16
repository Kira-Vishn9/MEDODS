<script>
import Vue from "vue";

export default {
  name: "toggleInput",
  props: {
    checked: Boolean,
  },
  emits: ["update:value"],
  methods: {
    onChange(event) {
      if (!(event.target instanceof HTMLInputElement)) return;
      this.$emit("update:value", event.target.checked);
    },
  },
};
</script>

<template>
  <div class="container">
    <span  :class="{ active: !checked }">Женский</span>
    <label class="switch">
      <input type="checkbox" :checked="checked" @change="onChange" />
      <span class="slider round"></span>
    </label>
    <div class="wrap_toggle">
      <span  :class="{ active: checked }"> Мужской </span>
    </div>
  </div>
</template>

<style lang="sass">

.wrap_toggle
  margin-left: 6px

.container
  display: flex
  align-items: center
  justify-content: center

.container .switch
  position: relative
  display: inline-block
  width: 39px
  height: 19px
  margin: 0 10px 0 10px

  input
    opacity: 0
    width: 0
    height: 0

  .slider
    position: absolute
    cursor: pointer
    top: 0
    left: 0
    right: 0
    bottom: 0
    background-color: #ccc
    transition: 0.4s

    &:before
      position: absolute
      content: ""
      height: 17px
      width: 17px
      left: -5px
      bottom: 1px
      background-color: white
      transition: 0.4s
      margin-right: 5px

  input:checked + .slider
    background-color: #2196f3

  input:focus + .slider
    box-shadow: 0 0 1px #2196f3

  input:checked + .slider:before
    transform: translateX(26px)

  .slider.round
    border-radius: 34px

  .slider.round:before
    border-radius: 50%
</style>
