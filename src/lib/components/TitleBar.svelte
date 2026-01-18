<script lang="ts">
  interface Props {
    title: string;
    showDropdown?: boolean;
    rightContent?: string;
  }
  
  let { title, showDropdown = false, rightContent = '' }: Props = $props();
</script>

<div class="bit-titlebar">
  <div class="bit-titlebar__left">
    <span class="bit-titlebar__title">{title}</span>
    {#if showDropdown}
      <span class="bit-titlebar__dropdown">▼</span>
    {/if}
  </div>
  {#if rightContent}
    <div class="bit-titlebar__right">
      {rightContent}
    </div>
  {/if}
</div>

<style>
  .bit-titlebar {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
    padding: var(--1bit-spacing-xs) var(--1bit-spacing-sm);
    font-family: inherit;
    font-size: inherit;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .bit-titlebar__left {
    display: flex;
    align-items: center;
    gap: var(--1bit-spacing-xs);
  }
  
  .bit-titlebar__dropdown {
    font-size: inherit;
  }
  
  .bit-titlebar__right {
    font-weight: normal;
  }
</style>
