<script lang="ts">
  interface Props {
    value?: number;
    min?: number;
    max?: number;
    step?: number;
    label?: string;
    showValue?: boolean;
    disabled?: boolean;
    onchange?: (value: number) => void;
  }
  
  let { 
    value = $bindable(50), 
    min = 0, 
    max = 100, 
    step = 1,
    label,
    showValue = true,
    disabled = false,
    onchange 
  }: Props = $props();
  
  function handleInput(e: Event) {
    const target = e.target as HTMLInputElement;
    value = Number(target.value);
    onchange?.(value);
  }
  
  // Calculate fill percentage for the track
  const fillPercent = $derived(((value - min) / (max - min)) * 100);
</script>

<div class="bit-slider" class:bit-slider--disabled={disabled}>
  {#if label}
    <span class="bit-slider__label">{label}</span>
  {/if}
  <div class="bit-slider__track-container">
    <div class="bit-slider__track">
      <div class="bit-slider__fill" style="width: {fillPercent}%"></div>
    </div>
    <input
      type="range"
      class="bit-slider__input"
      {min}
      {max}
      {step}
      {disabled}
      bind:value
      oninput={handleInput}
    />
  </div>
  {#if showValue}
    <span class="bit-slider__value">{value}</span>
  {/if}
</div>

<style>
  .bit-slider {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: inherit;
    font-size: inherit;
  }
  
  .bit-slider--disabled {
    opacity: 0.5;
  }
  
  .bit-slider__label {
    white-space: nowrap;
  }
  
  .bit-slider__track-container {
    position: relative;
    flex: 1;
    min-width: 80px;
    height: 16px;
    display: flex;
    align-items: center;
  }
  
  .bit-slider__track {
    position: absolute;
    width: 100%;
    height: 8px;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    pointer-events: none;
  }
  
  .bit-slider__fill {
    height: 100%;
    background-color: var(--1bit-fg);
  }
  
  .bit-slider__input {
    position: relative;
    width: 100%;
    height: 16px;
    margin: 0;
    background: transparent;
    cursor: pointer;
    -webkit-appearance: none;
    appearance: none;
  }
  
  .bit-slider__input:focus {
    outline: none;
  }
  
  /* Webkit (Chrome, Safari, Edge) thumb */
  .bit-slider__input::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 12px;
    height: 16px;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    cursor: pointer;
  }
  
  .bit-slider__input::-webkit-slider-thumb:hover {
    background-color: var(--1bit-fg);
  }
  
  /* Firefox thumb */
  .bit-slider__input::-moz-range-thumb {
    width: 12px;
    height: 16px;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    border-radius: 0;
    cursor: pointer;
  }
  
  .bit-slider__input::-moz-range-thumb:hover {
    background-color: var(--1bit-fg);
  }
  
  /* Firefox track (hide default) */
  .bit-slider__input::-moz-range-track {
    background: transparent;
  }
  
  .bit-slider__value {
    min-width: 24px;
    text-align: right;
    font-variant-numeric: tabular-nums;
  }
</style>
