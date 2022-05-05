<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
    class="app-input"
  />
</template>

<script setup lang="ts">
defineProps<{ modelValue: string }>();
defineEmits(['update:modelValue'])
</script>

<style lang="scss" scoped>
.app-input {
  width: 100%;
  margin-right: 20px;
  padding: 0 10px;
  border: 1px solid #dadada;
  border-radius: 5px;
}
</style>
