<template>
  <AppLoader
    :visible="visible"
    text="Chargement..."
    :show-overlay="true"
    :fullscreen="true"
    :centered="true"
    :block-pointer-events="true"
    overlay-color="rgba(0, 0, 0, 0.45)"
    text-color="#ffffff"
    spinner-color="#ffffff"
    track-color="rgba(255, 255, 255, 0.15)"
    :size="56"
    :thickness="6"
    :gap="8"
    :text-size="14"
    :font-weight="500"
    :z-index="2000"
  />
</template>

<script setup>
import { computed } from 'vue'
import AppLoader from './AppLoader.vue'
import { globalLoading } from '../stores/loading'

const visible = computed(() => (globalLoading.value || 0) > 0)
</script>
