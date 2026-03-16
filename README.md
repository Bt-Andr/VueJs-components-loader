# Loader Components

## Files
- `src/components/loader/AppLoader.vue`
- `src/components/loader/AppInlineLoader.vue`
- `src/components/loader/LoadingOverlay.vue`

## Purpose
- `AppLoader.vue`: generic, reusable loader component
- `AppInlineLoader.vue`: ready-to-use inline/local loader
- `LoadingOverlay.vue`: project wrapper connected to `globalLoading`

## Basic usage

```vue
<script setup>
import { ref } from 'vue'
import AppLoader from '../components/loader/AppLoader.vue'

const loading = ref(true)
</script>

<template>
  <AppLoader :visible="loading" />
</template>
```

## Inline usage

```vue
<script setup>
import { ref } from 'vue'
import AppInlineLoader from '../components/loader/AppInlineLoader.vue'

const loading = ref(true)
</script>

<template>
  <AppInlineLoader
    :visible="loading"
    text="Chargement des transactions..."
  />
</template>
```

## Custom branding

```vue
<AppLoader
  :visible="loading"
  text="Synchronisation..."
  :show-overlay="true"
  overlay-color="rgba(13, 34, 81, 0.55)"
  spinner-color="#02ed77"
  track-color="rgba(255,255,255,0.15)"
>
  <template #before>
    <img src="/logo.svg" alt="Logo" style="width:48px;height:48px">
  </template>
</AppLoader>
```

## Current app integration

`src/components/loader/LoadingOverlay.vue` is the adapter for the current project.
It reads `globalLoading` from `src/stores/loading.js`.

This lets you reuse `AppLoader.vue` elsewhere without carrying the store logic.

## Overlay behavior

- `showOverlay`: shows or hides the background overlay layer
- `overlayColor`: sets the overlay color
- `fullscreen`: controls whether the loader covers the full viewport

Example without overlay:

```vue
<AppLoader
  :visible="loading"
  :show-overlay="false"
  :fullscreen="false"
  text="Chargement local..."
/>
```
