<script setup lang="ts">
import { ScalarIcon } from '@scalar/components'
import { useClipboard } from '@scalar/use-hooks/useClipboard'

import { formatExample } from './helpers/format-example'

defineProps<{
  examples?: unknown
  example?: unknown
}>()

const { copyToClipboard } = useClipboard()
</script>
<template>
  <!-- single example (deprecated) -->
  <template v-if="example">
    <div class="property-example property-example-visible">
      <div class="property-example-header">
        <span class="property-example-label">Example</span>
      </div>
      <div class="property-example-value-container">
        <button
          class="property-example-value group"
          type="button"
          @click="copyToClipboard(String(formatExample(example)))">
          <span>
            {{ formatExample(example) }}
          </span>
          <ScalarIcon
            class="group-hover:text-c-1 text-c-3 ml-auto min-h-3 min-w-3"
            icon="Clipboard"
            size="xs" />
        </button>
      </div>
    </div>
  </template>

  <!-- multiple examples -->
  <template
    v-if="
      examples &&
      typeof examples === 'object' &&
      Object.keys(examples).length > 0
    ">
    <div class="property-example property-example-visible">
      <div class="property-example-header">
        <span class="property-example-label">
          {{ Object.keys(examples).length === 1 ? 'Example' : 'Examples' }}
        </span>
      </div>
      <div class="property-example-value-container">
        <button
          v-for="(exampleValue, key) in examples"
          :key="key"
          class="property-example-value group"
          type="button"
          @click="copyToClipboard(String(formatExample(exampleValue)))">
          <span>{{ formatExample(exampleValue) }} </span>
          <ScalarIcon
            class="text-c-3 group-hover:text-c-1 ml-auto min-h-3 min-w-3"
            icon="Clipboard"
            size="xs" />
        </button>
      </div>
    </div>
  </template>
</template>

<style scoped>
.property-example {
  display: flex;
  flex-direction: column;
  font-size: var(--scalar-mini);
  position: relative;
}

/* Visible example styles */
.property-example-visible {
  border: var(--scalar-border-width) solid var(--scalar-border-color);
  border-radius: var(--scalar-radius);
  margin-top: 8px;
  margin-bottom: 8px;
  overflow: hidden;
}

.property-example-header {
  background-color: var(--scalar-background-2);
  padding: 6px 10px;
  border-bottom: var(--scalar-border-width) solid var(--scalar-border-color);
}

.property-example-label {
  color: var(--scalar-color-2);
  font-weight: var(--scalar-semibold);
  font-size: var(--scalar-small);
}

.property-example-value-container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  padding: 6px;
  width: 100%;
}

/* Original tooltip styles (keeping for backwards compatibility) */
.property-example:not(.property-example-visible):hover:before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 20px;
  border-radius: var(--scalar-radius);
}

.property-example:not(.property-example-visible):hover
  .property-example-label
  span {
  color: var(--scalar-color-1);
}

.property-example:not(.property-example-visible) .property-example-label span {
  color: var(--scalar-color-3);
  position: relative;
  border-bottom: var(--scalar-border-width) dotted currentColor;
}

.property-example-value {
  font-family: var(--scalar-font-code);
  display: flex;
  gap: 8px;
  align-items: center;
  width: 100%;
  padding: 6px;
  background: var(--scalar-background-2);
  border: var(--scalar-border-width) solid var(--scalar-border-color);
  border-radius: var(--scalar-radius);
  margin-bottom: 4px;
}

.property-example-value span {
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: normal; /* Changed from nowrap to normal to allow wrapping */
  width: 100%; /* Ensure the span takes full width */
  text-align: left; /* Left-align the text */
}

.property-example-value :deep(svg) {
  color: var(--scalar-color-3);
}

.property-example-value:hover :deep(svg) {
  color: var(--scalar-color-1);
}

/* Original tooltip value list styles */
.property-example:not(.property-example-visible) .property-example-value-list {
  position: absolute;
  top: 18px;
  left: 50%;
  transform: translate3d(-50%, 0, 0);
  overflow: auto;
  background-color: var(--scalar-background-1);
  box-shadow: var(--scalar-shadow-1);
  border-radius: var(--scalar-radius-lg);
  border: var(--scalar-border-width) solid var(--scalar-border-color);
  padding: 9px;
  min-width: 200px;
  max-width: 300px;
  flex-direction: column;
  gap: 3px;
  display: none;
  z-index: 10;
}

.property-example:not(.property-example-visible):hover
  .property-example-value-list,
.property-example:not(.property-example-visible):focus-within
  .property-example-value-list {
  display: flex;
}
</style>
