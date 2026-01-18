<script lang="ts">
  interface Tab {
    id: string;
    label: string;
  }
  
  interface Props {
    tabs: Tab[];
    activeTab?: string;
    onchange?: (id: string) => void;
  }
  
  let { tabs, activeTab = $bindable(''), onchange }: Props = $props();
  
  // Set first tab as active if none specified
  $effect(() => {
    if (!activeTab && tabs.length > 0) {
      activeTab = tabs[0].id;
    }
  });
  
  function handleTabClick(id: string) {
    activeTab = id;
    onchange?.(id);
  }
</script>

<div class="bit-tabs">
  {#each tabs as tab, i}
    <button
      class="bit-tabs__tab"
      class:bit-tabs__tab--active={tab.id === activeTab}
      class:bit-tabs__tab--first={i === 0}
      class:bit-tabs__tab--last={i === tabs.length - 1}
      onclick={() => handleTabClick(tab.id)}
    >
      {tab.label}
    </button>
  {/each}
</div>

<style>
  .bit-tabs {
    display: inline-flex;
    font-family: inherit;
    font-size: inherit;
    border: var(--1bit-border-width) solid var(--1bit-fg);
  }
  
  .bit-tabs__tab {
    padding: 0 6px;
    background-color: var(--1bit-bg);
    border: none;
    border-right: var(--1bit-border-width) solid var(--1bit-fg);
    cursor: pointer;
    font-family: inherit;
    font-size: inherit;
    line-height: 1;
    color: var(--1bit-fg);
    display: inline-flex;
    align-items: center;
  }
  
  .bit-tabs__tab--last {
    border-right: none;
  }
  
  .bit-tabs__tab:hover:not(.bit-tabs__tab--active) {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-tabs__tab--active {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
</style>
