<script lang="ts">
  import type { Snippet } from 'svelte';
  
  interface Props {
    title: string;
    open?: boolean;
    onclose?: () => void;
    showInfo?: boolean;
    children: Snippet;
    footer?: Snippet;
  }
  
  let { title, open = true, onclose, showInfo = false, children, footer }: Props = $props();
</script>

{#if open}
  <div class="bit-modal-backdrop" onclick={onclose} onkeydown={e => e.key === 'Escape' && onclose?.()} role="presentation">
    <div class="bit-modal" onclick={e => e.stopPropagation()} role="dialog" aria-modal="true" aria-labelledby="modal-title">
      <div class="bit-modal__titlebar">
        <span id="modal-title" class="bit-modal__title">{title}</span>
        {#if showInfo}
          <span class="bit-modal__info">ⓘ</span>
        {/if}
      </div>
      <div class="bit-modal__content">
        {@render children()}
      </div>
      {#if footer}
        <div class="bit-modal__footer">
          {@render footer()}
        </div>
      {/if}
    </div>
  </div>
{/if}

<style>
  .bit-modal-backdrop {
    position: fixed;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: rgba(0, 0, 0, 0.2);
    z-index: 1000;
  }
  
  .bit-modal {
    background-color: var(--1bit-bg);
    border: var(--1bit-border-thick) solid var(--1bit-fg);
    border-radius: 3px;
    min-width: 200px;
    max-width: 90vw;
    overflow: hidden;
  }
  
  .bit-modal__titlebar {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
    padding: var(--1bit-spacing-xs) var(--1bit-spacing-md);
    font-family: inherit;
    font-size: inherit;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: var(--1bit-spacing-sm);
  }
  
  .bit-modal__title {
    flex: 1;
  }
  
  .bit-modal__info {
    font-size: inherit;
  }
  
  .bit-modal__content {
    padding: var(--1bit-spacing-md);
    font-family: inherit;
    font-size: inherit;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    margin: var(--1bit-spacing-sm);
  }
  
  .bit-modal__footer {
    padding: var(--1bit-spacing-sm) var(--1bit-spacing-md);
    display: flex;
    gap: var(--1bit-spacing-sm);
    justify-content: flex-end;
  }
</style>
