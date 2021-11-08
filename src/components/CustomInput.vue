<template>
  <div class="custom-input">
    <label :for="name">{{ label }}</label>
    <input type="text" :name="name" :value="value" @input="setValue" />
  </div>
</template>

<script>
export default {
  name: "CustomInput",
  props: {
    value: {
      type: String,
      default: "",
    },
    label: {
      type: String,
      default: "",
    },
    name: {
      type: String,
      default: "",
    },
  },
  inheritAttrs: false,
  methods: {
    setValue(event) {
      this.$emit("input", event.target.value);
    },
  },
};
</script>

<style scoped>
.custom-input {
  display: flex;
  flex-direction: column;
}
</style>
