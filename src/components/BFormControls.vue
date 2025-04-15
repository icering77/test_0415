<template>
  <div>
    <label for="" class="form-label"></label>
    <input 
      :type="type"
      class="form-control"
      id=""
      placeholder=""
      :value="modelValue"
      @input="$emit('update:modelValue', ($event.target as HTMLInputElement)?.value)"
      >
  </div>
</template>

<script setup lang="ts">
  defineProps({
    label: String,
    type: {
      type: String,
      default: 'text',
    },
    value: String,
    modelValue: [String, Number]
  })
  defineEmits(['update:modelValue'])
</script>

<style lang="scss" scoped>

</style>