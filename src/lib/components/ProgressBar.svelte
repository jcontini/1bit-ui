<script lang="ts">
  interface Props {
    value: number;  // 0-100
    label?: string;
  }
  
  let { value, label }: Props = $props();
  
  const clampedValue = $derived(Math.max(0, Math.min(100, value)));
</script>

<div class="bit-progress">
  {#if label}
    <span class="bit-progress__label">{label}</span>
  {/if}
  <div class="bit-progress__track">
    <div class="bit-progress__fill" style="width: {clampedValue}%"></div>
  </div>
</div>

<style>
  .bit-progress {
    display: flex;
    align-items: center;
    gap: 6px;
    font-family: inherit;
    font-size: inherit;
  }
  
  .bit-progress__label {
    white-space: nowrap;
  }
  
  .bit-progress__track {
    width: 150px;
    height: 20px;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
  }
  
  .bit-progress__fill {
    height: 100%;
    background-color: var(--1bit-fg);
  }
</style>
