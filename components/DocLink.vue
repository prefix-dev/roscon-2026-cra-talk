<script setup lang="ts">
withDefaults(
  defineProps<{
    /** URL this slide's claim is sourced from */
    href: string
    /** Short label, e.g. "Art. 14" — rendered as "source: Art. 14" */
    label?: string
  }>(),
  { label: '' }
)
</script>

<template>
  <a class="doc-link" :href="href" target="_blank" rel="noopener"
     :title="`Open the source${label ? `: ${label}` : ''}`">
    <carbon-book class="doc-link__icon" />
    <span>source<template v-if="label">: {{ label }}</template></span>
  </a>
</template>

<style scoped>
.doc-link {
  position: absolute;
  top: 1.55rem;
  right: 2.6rem;
  display: inline-flex;
  align-items: center;
  gap: 0.4em;
  font-family: var(--tufte-mono);
  font-size: 0.62rem;
  letter-spacing: 0.02em;
  color: var(--tufte-muted) !important;
  text-decoration: none !important;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 999px;
  padding: 0.2rem 0.65rem;
  background: var(--tufte-code-bg);
  z-index: 20;
  transition: color 0.15s ease, border-color 0.15s ease;
}
.doc-link:hover {
  color: var(--tufte-accent) !important;
  border-color: var(--tufte-accent);
}
.doc-link__icon { width: 1.1em; height: 1.1em; flex-shrink: 0; }
</style>
