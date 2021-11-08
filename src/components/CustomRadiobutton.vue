<template>
  <div class="custom-radio">
    <input
      type="radio"
      v-model="radioButtonValue"
      :name="name"
      :value="value"
    />
    <label :for="name">{{ label }}</label>
  </div>
</template>

<script>
export default {
  name: "CustomRadiobutton",
  model: {
    prop: "checked",
    event: "change",
  },
  props: {
    checked: {
      type: [String, Boolean],
    },
    value: {
      type: [String, Boolean],
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
  computed: {
    radioButtonValue: {
      get() {
        return this.checked;
      },
      set() {
        this.setValue();
      },
    },
  },
  methods: {
    setValue() {
      this.$emit("change", this.value);
    },
  },
};
</script>

<style scoped>
.custom-radio {
  display: flex;
}
</style>
