<script lang="ts">
  interface Props {
    checked?: boolean;
    disabled?: boolean;
    labelOn?: string;
    labelOff?: string;
    onchange?: (checked: boolean) => void;
  }
  
  let { 
    checked = $bindable(false), 
    disabled = false, 
    labelOn = 'On',
    labelOff = 'Off',
    onchange 
  }: Props = $props();
  
  function toggle() {
    if (disabled) return;
    checked = !checked;
    onchange?.(checked);
  }
</script>

<button
  type="button"
  class="bit-toggle"
  class:bit-toggle--checked={checked}
  class:bit-toggle--disabled={disabled}
  {disabled}
  onclick={toggle}
  role="switch"
  aria-checked={checked}
>
  <span class="bit-toggle__label bit-toggle__label--off" class:bit-toggle__label--active={!checked}>
    {labelOff}
  </span>
  <span class="bit-toggle__label bit-toggle__label--on" class:bit-toggle__label--active={checked}>
    {labelOn}
  </span>
</button>

<style>
  .bit-toggle {
    display: inline-flex;
    font-family: inherit;
    font-size: inherit;
    border: var(--1bit-border-width) solid var(--1bit-fg);
    background: var(--1bit-bg);
    padding: 0;
    cursor: pointer;
    overflow: hidden;
  }
  
  .bit-toggle--disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .bit-toggle__label {
    padding: 2px 8px;
    line-height: 1;
    color: var(--1bit-fg);
    background: var(--1bit-bg);
    transition: background-color 0.1s, color 0.1s;
  }
  
  .bit-toggle__label--active {
    background: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-toggle__label--off {
    border-right: 1px solid var(--1bit-fg);
  }
</style>
