<script lang="ts">
  import type { Snippet } from 'svelte';
  
  interface Tab {
    id: string;
    label: string;
  }
  
  interface Props {
    tabs: Tab[];
    activeTab?: string;
    children: Snippet<[string]>;
  }
  
  let { tabs, activeTab = $bindable(''), children }: Props = $props();
  
  $effect(() => {
    if (!activeTab && tabs.length > 0) {
      activeTab = tabs[0].id;
    }
  });
</script>

<div class="bit-tabpanel">
  <div class="bit-tabpanel__tabs">
    {#each tabs as tab}
      <button
        class="bit-tabpanel__tab"
        class:bit-tabpanel__tab--active={tab.id === activeTab}
        onclick={() => activeTab = tab.id}
      >
        {tab.label}
      </button>
    {/each}
  </div>
  <div class="bit-tabpanel__content">
    {@render children(activeTab)}
  </div>
</div>

<style>
  .bit-tabpanel {
    font-family: inherit;
  }
  
  .bit-tabpanel__tabs {
    display: flex;
    gap: 2px;
  }
  
  .bit-tabpanel__tab {
    font-family: inherit;
    font-size: inherit;
    line-height: 1;
    padding: 0 8px;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    border-bottom: none;
    border-radius: 6px 6px 0 0;
    cursor: pointer;
    color: var(--1bit-fg);
    position: relative;
    top: var(--1bit-border-width);
    display: inline-flex;
    align-items: center;
  }
  
  .bit-tabpanel__tab:hover:not(.bit-tabpanel__tab--active) {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-tabpanel__tab--active {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
    border-bottom: var(--1bit-border-width) solid var(--1bit-fg);
    z-index: 1;
  }
  
  .bit-tabpanel__content {
    border: var(--1bit-border-width) solid var(--1bit-fg);
    padding: 6px;
    background-color: var(--1bit-bg);
    font-size: inherit;
  }
</style>
