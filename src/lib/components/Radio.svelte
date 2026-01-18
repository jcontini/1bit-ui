<script lang="ts">
  interface Props {
    checked?: boolean;
    disabled?: boolean;
    label?: string;
    name?: string;
    value?: string;
    group?: string;
    onchange?: (value: string) => void;
  }
  
  let { 
    checked = false, 
    disabled = false, 
    label, 
    name = '', 
    value = '',
    group = $bindable(''),
    onchange 
  }: Props = $props();
  
  function handleChange() {
    group = value;
    onchange?.(value);
  }
</script>

<label class="bit-radio" class:bit-radio--disabled={disabled}>
  <input
    type="radio"
    {name}
    {value}
    checked={group === value}
    {disabled}
    onchange={handleChange}
    class="bit-radio__input"
  />
  <span class="bit-radio__circle">
    {#if group === value}
      <span class="bit-radio__dot"></span>
    {/if}
  </span>
  {#if label}
    <span class="bit-radio__label">{label}</span>
  {/if}
</label>

<style>
  .bit-radio {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    cursor: pointer;
    font-family: inherit;
    font-size: inherit;
  }
  
  .bit-radio--disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .bit-radio__input {
    position: absolute;
    opacity: 0;
    width: 0;
    height: 0;
  }
  
  .bit-radio__circle {
    width: 1em;
    height: 1em;
    border: 1px solid var(--1bit-fg);
    border-radius: 50%;
    background-color: var(--1bit-bg);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }
  
  .bit-radio__dot {
    width: 0.5em;
    height: 0.5em;
    background-color: var(--1bit-fg);
    border-radius: 50%;
  }
  
  .bit-radio__label {
    user-select: none;
  }
</style>
