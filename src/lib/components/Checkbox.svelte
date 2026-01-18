<script lang="ts">
  interface Props {
    checked?: boolean;
    disabled?: boolean;
    label?: string;
    onchange?: (checked: boolean) => void;
  }
  
  let { checked = $bindable(false), disabled = false, label, onchange }: Props = $props();
  
  function handleChange(e: Event) {
    const target = e.target as HTMLInputElement;
    checked = target.checked;
    onchange?.(checked);
  }
</script>

<label class="bit-checkbox" class:bit-checkbox--disabled={disabled}>
  <input
    type="checkbox"
    bind:checked
    {disabled}
    onchange={handleChange}
    class="bit-checkbox__input"
  />
  <span class="bit-checkbox__box">
    {#if checked}
      <span class="bit-checkbox__check"></span>
    {/if}
  </span>
  {#if label}
    <span class="bit-checkbox__label">{label}</span>
  {/if}
</label>

<style>
  .bit-checkbox {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    cursor: pointer;
    font-family: inherit;
    font-size: inherit;
  }
  
  .bit-checkbox--disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .bit-checkbox__input {
    position: absolute;
    opacity: 0;
    width: 0;
    height: 0;
  }
  
  .bit-checkbox__box {
    width: 1em;
    height: 1em;
    border: 1px solid var(--1bit-fg);
    background-color: var(--1bit-bg);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }
  
  .bit-checkbox__check {
    position: relative;
    width: 65%;
    height: 65%;
  }
  
  /* Draw a checkmark with two lines */
  .bit-checkbox__check::before,
  .bit-checkbox__check::after {
    content: '';
    position: absolute;
    background-color: var(--1bit-fg);
  }
  
  /* Short leg of check (going down-left) */
  .bit-checkbox__check::before {
    width: 2px;
    height: 40%;
    bottom: 25%;
    left: 20%;
    transform: rotate(-45deg);
  }
  
  /* Long leg of check (going up-right) */
  .bit-checkbox__check::after {
    width: 2px;
    height: 80%;
    bottom: 20%;
    left: 45%;
    transform: rotate(35deg);
  }
  
  .bit-checkbox__label {
    user-select: none;
  }
</style>
