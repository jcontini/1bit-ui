<script lang="ts">
  interface Option {
    value: string;
    label: string;
  }

  interface Props {
    options: Option[];
    value?: string;
    disabled?: boolean;
    onchange?: (value: string) => void;
  }

  let { options, value = $bindable(''), disabled = false, onchange }: Props = $props();
  
  let isOpen = $state(false);
  let triggerEl = $state<HTMLButtonElement | null>(null);
  let listEl = $state<HTMLUListElement | null>(null);
  let focusedIndex = $state(-1);
  
  const selectedLabel = $derived(
    options.find(o => o.value === value)?.label ?? options[0]?.label ?? ''
  );

  function toggle() {
    if (disabled) return;
    isOpen = !isOpen;
    if (isOpen) {
      focusedIndex = options.findIndex(o => o.value === value);
      if (focusedIndex === -1) focusedIndex = 0;
    }
  }

  function select(option: Option) {
    value = option.value;
    onchange?.(value);
    isOpen = false;
    triggerEl?.focus();
  }

  function handleKeydown(e: KeyboardEvent) {
    if (!isOpen) {
      if (e.key === 'Enter' || e.key === ' ' || e.key === 'ArrowDown') {
        e.preventDefault();
        toggle();
      }
      return;
    }

    switch (e.key) {
      case 'Escape':
        e.preventDefault();
        isOpen = false;
        triggerEl?.focus();
        break;
      case 'ArrowDown':
        e.preventDefault();
        focusedIndex = (focusedIndex + 1) % options.length;
        break;
      case 'ArrowUp':
        e.preventDefault();
        focusedIndex = (focusedIndex - 1 + options.length) % options.length;
        break;
      case 'Enter':
      case ' ':
        e.preventDefault();
        if (focusedIndex >= 0) {
          select(options[focusedIndex]);
        }
        break;
    }
  }

  function handleClickOutside(e: MouseEvent) {
    const target = e.target as Node;
    if (!triggerEl?.contains(target) && !listEl?.contains(target)) {
      isOpen = false;
    }
  }

  $effect(() => {
    if (isOpen) {
      document.addEventListener('click', handleClickOutside);
      return () => document.removeEventListener('click', handleClickOutside);
    }
  });
</script>

<div class="bit-dropdown">
  <button
    bind:this={triggerEl}
    type="button"
    class="bit-dropdown__trigger"
    class:bit-dropdown__trigger--open={isOpen}
    {disabled}
    onclick={toggle}
    onkeydown={handleKeydown}
    aria-haspopup="listbox"
    aria-expanded={isOpen}
  >
    <span class="bit-dropdown__value">{selectedLabel}</span>
    <span class="bit-dropdown__arrow">▼</span>
  </button>
  
  {#if isOpen}
    <ul
      bind:this={listEl}
      class="bit-dropdown__list"
      role="listbox"
    >
      {#each options as option, i}
        <!-- svelte-ignore a11y_click_events_have_key_events -->
        <li
          class="bit-dropdown__option"
          class:bit-dropdown__option--selected={option.value === value}
          class:bit-dropdown__option--focused={i === focusedIndex}
          role="option"
          aria-selected={option.value === value}
          onclick={() => select(option)}
          onmouseenter={() => focusedIndex = i}
        >
          <span class="bit-dropdown__check">
            {option.value === value ? '✓' : ''}
          </span>
          {option.label}
        </li>
      {/each}
    </ul>
  {/if}
</div>

<style>
  .bit-dropdown {
    position: relative;
    display: inline-block;
  }
  
  .bit-dropdown__trigger {
    display: inline-flex;
    align-items: center;
    gap: var(--1bit-spacing-sm);
    font-family: inherit;
    font-size: inherit;
    color: var(--1bit-fg);
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    border-radius: 4px;
    padding: 2px 6px;
    cursor: pointer;
    min-width: 80px;
  }
  
  .bit-dropdown__trigger:focus {
    outline: none;
    box-shadow: 0 0 0 1px var(--1bit-fg);
  }
  
  .bit-dropdown__trigger:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  .bit-dropdown__value {
    flex: 1;
    text-align: left;
  }
  
  .bit-dropdown__arrow {
    font-size: 10px;
    color: var(--1bit-fg);
  }
  
  .bit-dropdown__list {
    position: absolute;
    top: 100%;
    left: 0;
    z-index: 100;
    min-width: 100%;
    margin: 2px 0 0 0;
    padding: 2px 0;
    list-style: none;
    background-color: var(--1bit-bg);
    border: var(--1bit-border-width) solid var(--1bit-fg);
    border-radius: 6px;
  }
  
  .bit-dropdown__option {
    display: flex;
    align-items: center;
    padding: 4px 8px;
    cursor: pointer;
    white-space: nowrap;
  }
  
  .bit-dropdown__option--focused {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .bit-dropdown__option--focused .bit-dropdown__check {
    color: var(--1bit-bg);
  }
  
  .bit-dropdown__check {
    width: 16px;
    margin-right: 4px;
    color: var(--1bit-fg);
  }
</style>
