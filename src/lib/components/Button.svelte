<script lang="ts">
  import type { Snippet } from 'svelte';
  
  interface Props {
    onclick?: (e: MouseEvent) => void;
    disabled?: boolean;
    variant?: 'default' | 'small' | 'icon';
    children: Snippet;
  }
  
  let { onclick, disabled = false, variant = 'default', children }: Props = $props();
</script>

<button
  class="bit-button bit-button--{variant}"
  {onclick}
  {disabled}
>
  {@render children()}
</button>

<style>
  .bit-button {
    font-family: inherit;
    font-size: inherit;
    line-height: 1;
    color: var(--1bit-fg);
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    border-radius: 4px;
    padding: 0 6px;
    cursor: pointer;
    position: relative;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    vertical-align: middle;
  }
  
  .bit-button:hover:not(:disabled) {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-button:active:not(:disabled) {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .bit-button--small {
    padding: 0 4px;
    font-size: inherit;
  }
  
  .bit-button--icon {
    padding: 0 2px;
    min-width: 28px;
    min-height: 28px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }
</style>
