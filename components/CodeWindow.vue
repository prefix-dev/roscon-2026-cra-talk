<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  /** File name / path shown in the header (e.g. .github/workflows/ci.yml) */
  title?: string
  /** When set, the filename links to the real file (opens in a new tab). */
  href?: string
}>()

/** Tool logos keyed by the first path segment of the filename. */
const LOGOS: Record<string, string> = {
  // add per-tool logos here if you drop SVGs into public/
}
const logo = computed(() => {
  const seg = props.title?.split('/')[0]?.toLowerCase()
  return seg ? LOGOS[seg] : undefined
})
</script>

<template>
  <div class="codewin">
    <div class="codewin-bar">
      <img v-if="logo" class="codewin-logo" :src="logo" alt="" />
      <svg v-else class="codewin-icon" viewBox="0 0 16 16" width="1em" height="1em"
        fill="none" stroke="currentColor" stroke-width="1.2" stroke-linejoin="round">
        <path d="M3.5 1.5h5.2L12.5 5.3v9.2a.5.5 0 0 1-.5.5H3.5a.5.5 0 0 1-.5-.5V2a.5.5 0 0 1 .5-.5z" />
        <path d="M8.6 1.6V5.4H12.4" />
      </svg>
      <a v-if="title && href" class="codewin-name codewin-name--link" :href="href"
        target="_blank" rel="noopener">{{ title }}</a>
      <span v-else-if="title" class="codewin-name">{{ title }}</span>
    </div>
    <div class="codewin-body">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.codewin {
  background: #f5f5e9;
  border-radius: 6px;
  overflow: hidden;
  font-family: var(--tufte-mono);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.08);
  border: 1px solid rgba(0, 0, 0, 0.08);
  margin-top: 0.5rem;
  /* Hug the content height — don't stretch to fill a grid/flex row, which
   * would leave empty background below the code. */
  align-self: start;
}
.codewin-bar {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  padding: 0.4rem 0.75rem;
  /* cooler, flatter header than the terminal's warm tan bar */
  background: #edeee8;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
}
.codewin-icon {
  flex-shrink: 0;
  color: #999;
}
.codewin-logo {
  flex-shrink: 0;
  height: 1.1em;
  width: auto;
  display: block;
}
.codewin-name {
  flex: 1;
  text-align: left;
  font-size: 0.82rem;
  color: #555;
  letter-spacing: 0.01em;
}
/* Clickable filename — subtle until hovered, so it doesn't shout "link". */
.codewin-name--link {
  text-decoration: underline;
  text-decoration-color: rgba(0, 0, 0, 0.18);
  text-underline-offset: 2px;
  cursor: pointer;
}
.codewin-name--link:hover {
  color: var(--tufte-accent);
  text-decoration-color: var(--tufte-accent);
}
.codewin-body {
  padding: 0.15rem 0.4rem;
  color: #222;
  font-size: 0.78rem;
  line-height: 1.6;
}
/* Neutralize the theme's block-code vertical padding so the code hugs
 * the window edges. */
.codewin-body :deep(pre) {
  padding: 0 !important;
}
</style>
