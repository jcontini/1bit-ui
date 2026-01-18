<script lang="ts">
  interface Props {
    value?: string;
    placeholder?: string;
    disabled?: boolean;
    variant?: 'box' | 'underline';
    type?: 'text' | 'password' | 'number';
    oninput?: (value: string) => void;
  }
  
  let { 
    value = $bindable(''), 
    placeholder = '', 
    disabled = false, 
    variant = 'box',
    type = 'text',
    oninput 
  }: Props = $props();
  
  function handleInput(e: Event) {
    const target = e.target as HTMLInputElement;
    value = target.value;
    oninput?.(value);
  }
</script>

<input
  class="bit-input bit-input--{variant}"
  {type}
  bind:value
  {placeholder}
  {disabled}
  oninput={handleInput}
/>

<style>
  .bit-input {
    font-family: inherit;
    font-size: inherit;
    color: var(--1bit-fg);
    padding: 4px 8px;
    line-height: 1;
  }
  
  .bit-input--box {
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
  }
  
  .bit-input--underline {
    background-color: transparent;
    border: none;
    padding: 4px 0;
    background-image: repeating-linear-gradient(
      to right,
      var(--1bit-fg) 0px,
      var(--1bit-fg) 2px,
      transparent 2px,
      transparent 4px
    );
    background-repeat: repeat-x;
    background-position: bottom left;
    background-size: 100% 1px;
  }
  
  .bit-input:focus {
    outline: none;
  }
  
  .bit-input--underline:focus {
    box-shadow: none;
    background-image: linear-gradient(var(--1bit-fg), var(--1bit-fg));
  }
  
  .bit-input:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .bit-input::placeholder {
    color: var(--1bit-fg);
    opacity: 0.5;
  }
</style>
