<template>
  <footer
    v-if="$nav.currentPage > 1"
    class="master-footer"
  >
    <span class="master-footer__title">CRA for ROS · prefix.dev · ROSCon 2026</span>
    <span class="master-footer__page">
      {{ $nav.currentPage }} / {{ $nav.total }}
    </span>
  </footer>
</template>

<style scoped>
.master-footer {
  position: absolute;
  bottom: 0.6rem;
  left: 2.6rem;
  right: 2.6rem;
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  font-family: var(--tufte-serif);
  font-size: 0.75rem;
  font-style: italic;
  color: var(--tufte-muted);
  pointer-events: none;
  z-index: 10;
}

.master-footer__title {
  letter-spacing: 0.02em;
}

.master-footer__page {
  font-variant-numeric: oldstyle-nums;
}
</style>
