<template>
  <div class="custom-input">
    <label :for="name">{{ label }}</label>
    <input
      type="text"
      :name="name"
      :placeholder="placeholder"
      :value="value"
      @input="setValue"
      @focus="$emit('focus', $event)"
      @blur="$emit('blur', $event)"
    />
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
    placeholder: {
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
