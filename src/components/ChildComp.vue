<script setup>
const props = defineProps({
  msg: String
})
const emit = defineEmits(['response'])
emit('response', 'hello from child')
</script>

<template>
  <h2>Child component</h2>
  <slot>Fallback content</slot>
</template>