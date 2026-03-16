<template>
  <Transition :name="transitionName">
    <div
      v-if="visible"
      :class="rootClasses"
      :style="rootStyle"
      role="status"
      :aria-live="ariaLive"
      :aria-label="ariaLabel"
    >
      <div
        v-if="showOverlay"
        class="app-loader__overlay"
      />

      <div
        class="app-loader__inner"
        :class="innerClasses"
        :style="innerStyle"
      >
        <slot name="before" />

        <slot name="spinner">
          <div
            class="app-loader__spinner"
            :style="spinnerStyle"
          />
        </slot>

        <slot
          v-if="showText"
          name="text"
        >
          <div
            class="app-loader__text"
            :style="textStyle"
          >
            {{ text }}
          </div>
        </slot>

        <slot />
        <slot name="after" />
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  visible: { type: Boolean, default: false },
  text: { type: String, default: 'Chargement...' },
  showText: { type: Boolean, default: true },
  showOverlay: { type: Boolean, default: true },
  fullscreen: { type: Boolean, default: true },
  centered: { type: Boolean, default: true },
  inline: { type: Boolean, default: false },
  direction: {
    type: String,
    default: 'column',
    validator: (value) => ['column', 'row'].includes(value),
  },
  blockPointerEvents: { type: Boolean, default: true },
  transitionName: { type: String, default: 'app-loader-fade' },
  ariaLabel: { type: String, default: 'Chargement en cours' },
  ariaLive: { type: String, default: 'polite' },
  zIndex: { type: [String, Number], default: 2000 },
  overlayColor: { type: String, default: 'rgba(0, 0, 0, 0.45)' },
  textColor: { type: String, default: '#ffffff' },
  spinnerColor: { type: String, default: '#ffffff' },
  trackColor: { type: String, default: 'rgba(255, 255, 255, 0.15)' },
  size: { type: [String, Number], default: 56 },
  thickness: { type: [String, Number], default: 6 },
  gap: { type: [String, Number], default: 8 },
  textSize: { type: [String, Number], default: 14 },
  fontWeight: { type: [String, Number], default: 500 },
  minHeight: { type: [String, Number], default: 'auto' },
  borderRadius: { type: [String, Number], default: 0 },
  padding: { type: String, default: '0' },
})

function toUnit(value, fallback = 'px') {
  if (typeof value === 'number') return `${value}${fallback}`
  return value
}

const rootClasses = computed(() => ({
  'app-loader': true,
  'app-loader--fullscreen': props.fullscreen,
  'app-loader--centered': props.centered,
  'app-loader--interactive': !props.blockPointerEvents,
}))

const rootStyle = computed(() => ({
  '--app-loader-overlay-color': props.overlayColor,
  '--app-loader-text-color': props.textColor,
  '--app-loader-gap': toUnit(props.gap),
  '--app-loader-z-index': String(props.zIndex),
  '--app-loader-min-height': props.minHeight === 'auto' ? 'auto' : toUnit(props.minHeight),
  '--app-loader-radius': toUnit(props.borderRadius),
  '--app-loader-padding': props.padding,
}))

const innerStyle = computed(() => ({
  color: props.textColor,
  flexDirection: props.direction,
}))

const innerClasses = computed(() => ({
  'app-loader__inner--inline': props.inline,
}))

const spinnerStyle = computed(() => ({
  width: toUnit(props.size),
  height: toUnit(props.size),
  borderWidth: toUnit(props.thickness),
  borderColor: props.trackColor,
  borderTopColor: props.spinnerColor,
}))

const textStyle = computed(() => ({
  color: props.textColor,
  fontSize: toUnit(props.textSize),
  fontWeight: String(props.fontWeight),
}))
</script>

<style scoped>
.app-loader {
  position: relative;
  min-height: var(--app-loader-min-height);
  border-radius: var(--app-loader-radius);
  padding: var(--app-loader-padding);
  overflow: hidden;
}

.app-loader--fullscreen {
  position: fixed;
  inset: 0;
  z-index: var(--app-loader-z-index);
}

.app-loader--centered {
  display: flex;
  align-items: center;
  justify-content: center;
}

.app-loader:not(.app-loader--interactive) {
  pointer-events: auto;
}

.app-loader__overlay {
  position: absolute;
  inset: 0;
  background: var(--app-loader-overlay-color);
  border-radius: inherit;
}

.app-loader__inner {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--app-loader-gap);
  text-align: center;
  position: relative;
  z-index: 1;
}

.app-loader__inner--inline {
  width: 100%;
}

.app-loader__spinner {
  border-style: solid;
  border-radius: 50%;
  animation: app-loader-spin 1s linear infinite;
  box-sizing: border-box;
}

.app-loader__text {
  line-height: 1.3;
  opacity: 0.95;
}

.app-loader-fade-enter-active,
.app-loader-fade-leave-active {
  transition: opacity 180ms ease;
}

.app-loader-fade-enter-from,
.app-loader-fade-leave-to {
  opacity: 0;
}

@keyframes app-loader-spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
