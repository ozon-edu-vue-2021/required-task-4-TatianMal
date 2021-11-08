<template>
  <div v-click-outside="closeDropdown" class="dropdown-wrapper">
    <custom-input
      :value="inputValue"
      :label="label"
      :name="name"
      @focus="openDropdown"
    ></custom-input>
    <div v-if="isDropdownOpen" class="dropdown-menu">
      <ul v-if="items.length">
        <li
          v-for="item in items"
          :key="item[valueField]"
          @click="setValue(item)"
        >
          {{ item[textField] }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";

import CustomInput from "./CustomInput.vue";

export default {
  name: "CustomDropdown",
  directives: {
    ClickOutside,
  },
  props: {
    value: {
      type: Object,
    },
    items: {
      type: Array,
      default: () => [],
    },
    label: {
      type: String,
    },
    name: {
      type: String,
    },
    valueField: {
      type: String,
      default: "id",
    },
    textField: {
      type: String,
      default: "name",
    },
  },
  components: {
    CustomInput,
  },
  data() {
    return {
      isDropdownOpen: false,
    };
  },
  computed: {
    inputValue: {
      get() {
        return this.value && this.value[this.textField];
      },
      set() {
        console.log("try to search");
      },
    },
  },
  methods: {
    openDropdown() {
      this.isDropdownOpen = true;
    },
    closeDropdown() {
      this.isDropdownOpen = false;
    },
    setValue(item) {
      this.$emit("input", item);
    },
  },
};
</script>

<style scoped>
.dropdown-wrapper {
  position: relative;
  max-height: 200px;
  box-shadow: 10px 10px 0 0 rgba(black, 0.03);
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  width: 100%;
  min-width: 100px;
  min-height: 10px;
  overflow-y: auto;
  box-shadow: 10px 10px 0 0 rgba(black, 0.03);
  background: white;
  animation: menu 0.3s ease forwards;
  z-index: 1000;
}
</style>
