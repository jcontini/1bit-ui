<script lang="ts">
  interface ListItem {
    id: string;
    content: string;
    selected?: boolean;
  }
  
  interface Props {
    items: ListItem[];
    selectedId?: string;
    onselect?: (id: string) => void;
    showDividers?: boolean;
  }
  
  let { items, selectedId = $bindable(''), onselect, showDividers = true }: Props = $props();
  
  function handleSelect(id: string) {
    selectedId = id;
    onselect?.(id);
  }
</script>

<div class="bit-list">
  {#each items as item, i}
    <button
      class="bit-list__item"
      class:bit-list__item--selected={item.id === selectedId}
      class:bit-list__item--divider={showDividers && i < items.length - 1}
      onclick={() => handleSelect(item.id)}
    >
      {item.content}
    </button>
  {/each}
</div>

<style>
  .bit-list {
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    display: flex;
    flex-direction: column;
    font-family: inherit;
    font-size: inherit;
  }
  
  .bit-list__item {
    padding: 4px 8px;
    text-align: left;
    background: none;
    border: none;
    cursor: pointer;
    font-family: inherit;
    font-size: inherit;
    line-height: 1;
    color: var(--1bit-fg);
  }
  
  .bit-list__item--divider {
    position: relative;
  }
  
  .bit-list__item--divider::after {
    content: '';
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    height: 1px;
    background: repeating-linear-gradient(
      to right,
      var(--1bit-fg) 0px,
      var(--1bit-fg) 2px,
      transparent 2px,
      transparent 4px
    );
  }
  
  .bit-list__item:hover:not(.bit-list__item--selected) {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-list__item--selected {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
</style>
