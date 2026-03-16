<template>
  <AppLoader
    :visible="visible"
    :text="text"
    :show-text="showText"
    :overlay="false"
    :show-overlay="showOverlay"
    :fullscreen="false"
    :centered="true"
    :inline="true"
    :direction="direction"
    :block-pointer-events="false"
    :text-color="textColor"
    :spinner-color="spinnerColor"
    :track-color="trackColor"
    :size="size"
    :thickness="thickness"
    :gap="gap"
    :text-size="textSize"
    :font-weight="fontWeight"
    :min-height="minHeight"
    :border-radius="borderRadius"
    :padding="padding"
  >
    <template #before>
      <slot name="before" />
    </template>

    <template #spinner>
      <slot name="spinner" />
    </template>

    <template #text>
      <slot name="text" />
    </template>

    <slot />

    <template #after>
      <slot name="after" />
    </template>
  </AppLoader>
</template>

<script setup>
import AppLoader from './AppLoader.vue'

defineProps({
  visible: { type: Boolean, default: false },
  text: { type: String, default: 'Chargement...' },
  showText: { type: Boolean, default: true },
  showOverlay: { type: Boolean, default: false },
  direction: {
    type: String,
    default: 'row',
    validator: (value) => ['column', 'row'].includes(value),
  },
  textColor: { type: String, default: '#0d2251' },
  spinnerColor: { type: String, default: '#02ed77' },
  trackColor: { type: String, default: 'rgba(13, 34, 81, 0.12)' },
  size: { type: [String, Number], default: 24 },
  thickness: { type: [String, Number], default: 3 },
  gap: { type: [String, Number], default: 10 },
  textSize: { type: [String, Number], default: 14 },
  fontWeight: { type: [String, Number], default: 600 },
  minHeight: { type: [String, Number], default: 'auto' },
  borderRadius: { type: [String, Number], default: 0 },
  padding: { type: String, default: '0' },
})
</script>
