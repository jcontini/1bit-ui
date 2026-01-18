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
    padding: 0 4px;
  }
  
  .bit-input--box {
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
  }
  
  .bit-input--underline {
    background-color: transparent;
    border: none;
    border-bottom: 1px dotted var(--1bit-fg);
    padding-left: 0;
  }
  
  .bit-input:focus {
    outline: none;
  }
  
  .bit-input--underline:focus {
    box-shadow: none;
    border-bottom-style: solid;
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
