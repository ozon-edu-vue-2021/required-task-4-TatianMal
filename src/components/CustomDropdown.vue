<template>
  <div v-click-outside="closeDropdown" class="dropdown-wrapper">
    <custom-input
      :value="inputValue"
      :label="label"
      :name="name"
      @focus="openDropdown"
    ></custom-input>
    <div v-if="isDropdownOpen" class="dropdown-content-wrapper">
      <ul v-if="items.length" class="dropdown-menu">
        <li
          v-for="item in items"
          :key="item[valueField]"
          :class="['dropdown-menu-item', { selected: checkIsSelected(item) }]"
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
      this.closeDropdown();
    },
    checkIsSelected(item) {
      if (!this.value) {
        return false;
      }
      return this.value[this.valueField] === item[this.valueField];
    },
  },
};
</script>

<style scoped>
.dropdown-wrapper {
  position: relative;
}

.dropdown-content-wrapper {
  position: absolute;
  top: 0;
  display: block;
  background: #fff;
  width: 100%;
  max-height: 240px;
  overflow: auto;
  border: 1px solid #e8e8e8;
  z-index: 1000;
}

.dropdown-menu {
  list-style: none;
  display: inline-block;
  padding: 0;
  margin: 0;
  min-width: 100%;
}

.dropdown-menu-item {
  cursor: pointer;
  padding: 5px;
}

.dropdown-menu-item:hover {
  background-color: rgb(168, 192, 236);
}

.selected {
  background-color: rgb(71, 71, 212);
  color: #fff;
}
</style>
