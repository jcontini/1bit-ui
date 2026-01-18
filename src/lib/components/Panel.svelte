<script lang="ts">
  import type { Snippet } from 'svelte';
  
  interface Props {
    title?: string;
    variant?: 'raised' | 'inset' | 'flat';
    children: Snippet;
  }
  
  let { title, variant = 'raised', children }: Props = $props();
</script>

<div class="bit-panel bit-panel--{variant}">
  {#if title}
    <div class="bit-panel__title">{title}</div>
  {/if}
  <div class="bit-panel__content">
    {@render children()}
  </div>
</div>

<style>
  .bit-panel {
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    font-family: inherit;
    font-size: inherit;
  }
  
  .bit-panel--raised {
    /* No shadow - 1-bit display */
  }
  
  .bit-panel--inset {
    background-color: var(--1bit-bg);
  }
  
  .bit-panel--flat {
    /* No shadow effects */
  }
  
  .bit-panel__title {
    padding: var(--1bit-spacing-xs) var(--1bit-spacing-sm);
    border-bottom: var(--1bit-border-width) solid var(--1bit-fg);
    font-weight: bold;
  }
  
  .bit-panel__content {
    padding: var(--1bit-spacing-sm);
  }
</style>
