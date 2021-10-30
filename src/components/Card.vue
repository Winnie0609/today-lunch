<template>
  <div>I'm {{ props.text }} Card</div>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  text: { type: String, default: 'MCD'},
})

</script>