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
        <span class="bit-tabpanel__tab-inner">{tab.label}</span>
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
    --corner-cut: 3px;
    font-family: inherit;
    font-size: inherit;
    line-height: 1;
    padding: 0;
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
    border: none;
    cursor: pointer;
    position: relative;
    top: var(--1bit-border-width);
    display: inline-flex;
    align-items: center;
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% 100%,
      0 100%,
      0 var(--corner-cut)
    );
  }
  
  .bit-tabpanel__tab-inner {
    padding: 4px 8px;
    display: flex;
    align-items: center;
  }
  
  .bit-tabpanel__tab:hover:not(.bit-tabpanel__tab--active) {
    opacity: 0.8;
  }
  
  .bit-tabpanel__tab--active {
    z-index: 1;
  }
  
  .bit-tabpanel__tab--active .bit-tabpanel__tab-inner {
    --inner-cut: 2px;
    background-color: var(--1bit-bg);
    color: var(--1bit-fg);
    margin: 1px 1px 0 1px;
    clip-path: polygon(
      var(--inner-cut) 0,
      calc(100% - var(--inner-cut)) 0,
      100% var(--inner-cut),
      100% 100%,
      0 100%,
      0 var(--inner-cut)
    );
  }
  
  .bit-tabpanel__content {
    border: var(--1bit-border-width) solid var(--1bit-fg);
    padding: 6px;
    background-color: var(--1bit-bg);
    font-size: inherit;
  }
</style>
