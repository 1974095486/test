<template>
  <li class="dropdwon-option" :class="{ 'is-disabled': disabled }">
    <slot></slot>
  </li>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'DropdownItem',
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  }
})
</script>

<style scoped>
.dropdwon-option {
  color: #6c757d;
  pointer-events: none;
  background-color: transparent;
}
</style>
