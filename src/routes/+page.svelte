<script lang="ts">
  import { Button, Modal, Checkbox, Input, Radio, List, TabPanel, Tabs, ProgressBar, Toggle, Slider, Dropdown, Panel, TitleBar, Screen } from '$lib';
  
  let showModal = $state(false);
  let panelTab = $state('info');
  let showDateTime = $state(true);
  let showDitherOptions = $state(true);
  let colorTheme = $state('amber');
  let themeMode = $state('dark');
  let showSidebar = $state(true);
  let borderWidth = $state(1);
  let brightness = $state(100);
  let ditherPattern = $state('checker-1');
  
  // Demo state for components
  let inputValue = $state('');
  let inputVariant = $state<'box' | 'underline'>('box');
  let dropdownValue = $state('option1');
  let activeTab = $state('tab1');
  
  const dropdownOptions = [
    { value: 'option1', label: 'Option 1' },
    { value: 'option2', label: 'Option 2' },
    { value: 'option3', label: 'Option 3' }
  ];
  
  // Custom color inputs
  let customBg = $state('#A8B858');
  let customFg = $state('#000000');
  
  const darkMode = $derived(themeMode === 'dark');
  
  // Built-in color themes
  const builtInThemes: Record<string, { name: string; light: { bg: string; fg: string }; dark: { bg: string; fg: string } }> = {
    classic: {
      name: 'Classic LCD',
      light: { bg: '#A8B858', fg: '#000000' },
      dark: { bg: '#000000', fg: '#A8B858' }
    },
    amber: {
      name: 'Amber CRT',
      light: { bg: '#FFB000', fg: '#2A1A00' },
      dark: { bg: '#2A1A00', fg: '#FFB000' }
    },
    green: {
      name: 'Green CRT',
      light: { bg: '#33FF33', fg: '#0A3309' },
      dark: { bg: '#0A3309', fg: '#33FF33' }
    },
    indigo: {
      name: 'Indigo',
      light: { bg: '#FFFFFF', fg: '#58AAE5' },
      dark: { bg: '#0A1A2A', fg: '#58AAE5' }
    },
    wizard: {
      name: 'Sharp Wizard',
      light: { bg: '#739380', fg: '#000000' },
      dark: { bg: '#000000', fg: '#739380' }
    }
  };
  
  // Custom themes from localStorage
  let customThemes = $state<Record<string, { name: string; light: { bg: string; fg: string }; dark: { bg: string; fg: string } }>>({});
  
  // Load custom themes on mount
  $effect(() => {
    if (typeof window !== 'undefined') {
      const saved = localStorage.getItem('1bit-custom-themes');
      if (saved) {
        try {
          customThemes = JSON.parse(saved);
        } catch (e) {
          console.error('Failed to load custom themes');
        }
      }
    }
  });
  
  // All themes combined
  const colorThemes = $derived({ ...builtInThemes, ...customThemes });
  
  // List items for theme picker (sorted alphabetically)
  const themeListItems = $derived(
    Object.entries(colorThemes)
      .map(([id, theme]) => ({ id, content: theme.name }))
      .sort((a, b) => a.content.localeCompare(b.content))
  );
  
  // Save custom theme
  function saveCustomTheme() {
    const id = `custom_${Date.now()}`;
    const name = `Custom ${Object.keys(customThemes).length + 1}`;
    customThemes = {
      ...customThemes,
      [id]: {
        name,
        light: { bg: customBg, fg: customFg },
        dark: { bg: customFg, fg: customBg }
      }
    };
    localStorage.setItem('1bit-custom-themes', JSON.stringify(customThemes));
    colorTheme = id;
  }
  
  const ditherPatterns = [
    { id: 'checker-1', label: 'Check 1' },
    { id: 'checker-2', label: 'Check 2' },
    { id: 'hlines', label: 'H-Lines' },
    { id: 'vlines', label: 'V-Lines' },
  ];
  
  // Helper to parse hex color
  function hexToRgb(hex: string) {
    const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
    return result ? {
      r: parseInt(result[1], 16),
      g: parseInt(result[2], 16),
      b: parseInt(result[3], 16)
    } : { r: 0, g: 0, b: 0 };
  }
  
  // Mix two colors
  function mixColors(fg: string, bg: string, amount: number) {
    const fgRgb = hexToRgb(fg);
    const bgRgb = hexToRgb(bg);
    const r = Math.round(fgRgb.r * amount + bgRgb.r * (1 - amount));
    const g = Math.round(fgRgb.g * amount + bgRgb.g * (1 - amount));
    const b = Math.round(fgRgb.b * amount + bgRgb.b * (1 - amount));
    return `rgb(${r}, ${g}, ${b})`;
  }
  
  // Get current colors based on theme and mode
  $effect(() => {
    const theme = colorThemes[colorTheme];
    if (!theme) return;
    const colors = darkMode ? theme.dark : theme.light;
    document.documentElement.style.setProperty('--1bit-bg', colors.bg);
    // Apply brightness by mixing fg with bg
    const mixedFg = mixColors(colors.fg, colors.bg, brightness / 100);
    document.documentElement.style.setProperty('--1bit-fg', mixedFg);
  });
  
  // Clock for Sharp Wizard display
  let now = $state(new Date());
  
  const timeZone = $derived(Intl.DateTimeFormat().resolvedOptions().timeZone);
  const tzParts = $derived(timeZone.split('/'));
  const cityName = $derived(tzParts[tzParts.length - 1]?.replace(/_/g, ' ')?.toUpperCase() ?? 'LOCAL');
  const regionName = $derived(tzParts.length > 1 ? tzParts[0].replace(/_/g, ' ').toUpperCase() : '');
  
  const hours = $derived(now.getHours() % 12 || 12);
  const minutes = $derived(now.getMinutes().toString().padStart(2, '0'));
  const ampm = $derived(now.getHours() >= 12 ? 'PM' : 'AM');
  
  const dateString = $derived(
    now.toLocaleDateString('en-US', { weekday: 'short' }).toUpperCase() + ' ' +
    now.toLocaleDateString('en-US', { month: 'short' }).toUpperCase() + ' ' +
    now.getDate() + ', ' +
    now.getFullYear()
  );
  
  const gmtOffset = $derived(() => {
    const offset = -now.getTimezoneOffset();
    const sign = offset >= 0 ? '+' : '-';
    const hrs = Math.floor(Math.abs(offset) / 60).toString().padStart(2, '0');
    const mins = (Math.abs(offset) % 60).toString().padStart(2, '0');
    return `(${sign}${hrs}:${mins})`;
  });
  
  $effect(() => {
    const interval = setInterval(() => {
      now = new Date();
    }, 1000);
    return () => clearInterval(interval);
  });
  
  // Screen size detection
  let screenWidth = $state(typeof window !== 'undefined' ? window.innerWidth : 1024);
  
  const screenSizes = [
    { name: 'XS', min: 0, max: 480 },
    { name: 'SM', min: 480, max: 768 },
    { name: 'MD', min: 768, max: 1024 },
    { name: 'LG', min: 1024, max: 1280 },
    { name: 'XL', min: 1280, max: Infinity }
  ];
  
  const currentSize = $derived(screenSizes.find(s => screenWidth >= s.min && screenWidth < s.max) ?? screenSizes[2]);
  const sizeIndex = $derived(screenSizes.indexOf(currentSize));
  const sizePercent = $derived((sizeIndex + 1) / screenSizes.length * 100);
  
  $effect(() => {
    if (typeof window === 'undefined') return;
    
    const handleResize = () => {
      screenWidth = window.innerWidth;
    };
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  });
  
  function getDitherClass(pattern: string) {
    if (pattern === 'none') return '';
    if (pattern === '25') return 'dither-25';
    if (pattern === '50') return 'dither-50';
    if (pattern === '75') return 'dither-75';
    return `dither-${pattern}`;
  }
</script>

<div class="page" style="--1bit-border-width: {borderWidth}px;">
  <div class="page__content">
    <header class="header">
      <div class="header__text">
        <h1>1bit UI</h1>
        <p>A retro 1-bit component library for Svelte</p>
      </div>
      <Toggle labelOff="Light" labelOn="Dark" checked={themeMode === 'dark'} onchange={(checked) => themeMode = checked ? 'dark' : 'light'} />
    </header>

  <div class="grid">
    <!-- Column 1: Controls -->
    <div class="column">
      <section>
        <h2>Colors</h2>
        <List 
          items={themeListItems} 
          bind:selectedId={colorTheme}
        />
      </section>
      
      <section>
        <h2>Options</h2>
        <Checkbox label="Show Date/Time" bind:checked={showDateTime} />
        <Checkbox label="Show Sidebar" bind:checked={showSidebar} />
      </section>
      
      <section>
        <h2>Input Type</h2>
        <Radio name="inputType" value="box" label="Box" bind:group={inputVariant} />
        <Radio name="inputType" value="underline" label="Underline" bind:group={inputVariant} />
      </section>
      
      <section>
        <h2>Input</h2>
        <Input bind:value={inputValue} placeholder="Type here..." variant={inputVariant} />
      </section>
      
      <section>
        <h2>Dropdown</h2>
        <Dropdown options={dropdownOptions} bind:value={dropdownValue} />
      </section>
    </div>
    
    <!-- Column 2: Complex/multi components -->
    <div class="column">
      <section>
        <h2>Tab Panel</h2>
        <TabPanel 
          tabs={[
            { id: 'info', label: 'Info' },
            { id: 'notes', label: 'Notes' },
            { id: 'settings', label: 'Settings' }
          ]}
          bind:activeTab={panelTab}
        >
          {#snippet children(active)}
            {#if active === 'info'}
              <div>Welcome to 1bit UI!</div>
            {:else if active === 'notes'}
              <div>Your notes here.</div>
            {:else}
              <div>Settings panel.</div>
            {/if}
          {/snippet}
        </TabPanel>
      </section>
      
      <section>
        <h2>Modal</h2>
        <div class="bit-modal-inline__border">
          <div class="bit-modal-inline">
            <div class="bit-modal-inline__titlebar">
              <span>Sample Dialog</span>
            </div>
            <div class="bit-modal-inline__content">
              <p>Delete this item?</p>
            </div>
            <div class="bit-modal-inline__footer">
              <Button>Cancel</Button>
              <Button>OK</Button>
            </div>
          </div>
        </div>
      </section>
      
      <section>
        <h2>Tabs</h2>
        <Tabs 
          tabs={[
            { id: 'tab1', label: 'File' },
            { id: 'tab2', label: 'Edit' },
            { id: 'tab3', label: 'View' }
          ]}
          bind:activeTab={activeTab}
        />
      </section>
      
      <section>
        <h2>Panel</h2>
        <Panel title="System Info">
          <p style="margin: 0;">Memory: 2MB</p>
        </Panel>
      </section>
      
    </div>

    <!-- Column 3: Sharp Wizard Clock -->
    {#if showSidebar}
    <section class="wizard-clock">
      {#if showDateTime}
      <div class="wizard-panel__border">
        <div class="wizard-panel {getDitherClass(ditherPattern) || 'dither-50'}">
          <div class="wizard-panel__inner-shadow">
          <div class="wizard-panel__inner-border">
          <div class="wizard-panel__inner">
          <div class="wizard-panel__header">
            <svg class="wizard-panel__globe" viewBox="0 0 24 24" fill="currentColor">
              <!-- Pixel art globe icon -->
              <rect x="8" y="2" width="8" height="2"/>
              <rect x="6" y="4" width="2" height="2"/>
              <rect x="16" y="4" width="2" height="2"/>
              <rect x="4" y="6" width="2" height="2"/>
              <rect x="18" y="6" width="2" height="2"/>
              <rect x="2" y="8" width="2" height="8"/>
              <rect x="20" y="8" width="2" height="8"/>
              <rect x="4" y="16" width="2" height="2"/>
              <rect x="18" y="16" width="2" height="2"/>
              <rect x="6" y="18" width="2" height="2"/>
              <rect x="16" y="18" width="2" height="2"/>
              <rect x="8" y="20" width="8" height="2"/>
              <!-- Horizontal line -->
              <rect x="4" y="11" width="16" height="2"/>
              <!-- Vertical line -->
              <rect x="11" y="4" width="2" height="16"/>
              <!-- Curved lines -->
              <rect x="7" y="6" width="2" height="2"/>
              <rect x="6" y="8" width="2" height="2"/>
              <rect x="6" y="14" width="2" height="2"/>
              <rect x="7" y="16" width="2" height="2"/>
              <rect x="15" y="6" width="2" height="2"/>
              <rect x="16" y="8" width="2" height="2"/>
              <rect x="16" y="14" width="2" height="2"/>
              <rect x="15" y="16" width="2" height="2"/>
            </svg>
            <div class="wizard-panel__location">
              <span>{cityName}</span>
              {#if regionName}<span class="wizard-panel__region">{regionName}</span>{/if}
            </div>
          </div>
          <div class="wizard-panel__bar"></div>
          <div class="wizard-panel__date">{dateString}</div>
          <div class="wizard-panel__time">
            <span class="wizard-panel__digits">{hours}:{minutes}</span>
            <span class="wizard-panel__ampm">{ampm}</span>
          </div>
          <div class="wizard-panel__bar"></div>
          <div class="wizard-panel__gmt">{gmtOffset()}</div>
          </div>
          </div>
          </div>
        </div>
      </div>
      {/if}
      
      <div class="brightness-section">
        <h2>Brightness</h2>
        <Slider bind:value={brightness} min={20} max={100} step={10} />
      </div>
      
      <div class="screen-size-section">
        <h2>Screen Size</h2>
        <ProgressBar value={sizePercent} label={currentSize.name} />
      </div>
    </section>
    {/if}
  </div>
  </div>
  
  <Modal title="Hello" open={showModal} onclose={() => showModal = false}>
    {#snippet children()}
      <p>This is a 1bit UI modal!</p>
    {/snippet}
    {#snippet footer()}
      <Button onclick={() => showModal = false}>OK</Button>
    {/snippet}
  </Modal>
</div>

<style>
  :global(body) {
    margin: 0;
    font-family: 'Tamzen', monospace;
    font-size: 16px;
    line-height: 1.2;
  }
  
  .page {
    min-height: 100vh;
    background: var(--1bit-bg);
    color: var(--1bit-fg);
    transition: background-color 0.2s, color 0.2s;
  }
  
  .page__content {
    padding: 40px;
    margin: 0 auto;
    max-width: 1200px;
  }
  
  .header {
    margin-bottom: 32px;
    padding-bottom: 16px;
    border-bottom: var(--1bit-border-width) solid var(--1bit-fg);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .header__text h1 {
    margin: 0 0 4px 0;
    font-size: 2em;
  }
  
  .header__text p {
    margin: 0;
    opacity: 0.7;
  }
  
  .stepper {
    display: inline-flex;
    align-items: stretch;
    border: var(--1bit-border-width) solid var(--1bit-fg);
    line-height: 1;
  }
  
  .stepper__btn {
    background: var(--1bit-bg);
    color: var(--1bit-fg);
    border: none;
    margin: 0;
    padding: 2px 8px;
    cursor: pointer;
    font-family: inherit;
    font-size: inherit;
    line-height: 1;
    display: flex;
    align-items: center;
  }
  
  .stepper__btn:hover {
    background: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .stepper__value {
    padding: 2px 8px;
    min-width: 40px;
    text-align: center;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    border-left: var(--1bit-border-width) solid var(--1bit-fg);
    border-right: var(--1bit-border-width) solid var(--1bit-fg);
  }
  
  h2 {
    font-size: 1.1em;
    margin: 0 0 0.3em 0;
  }
  
  .grid {
    display: flex;
    gap: 32px;
  }
  
  .column {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 12px;
  }
  
  section {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
  }
  
  .row {
    display: flex;
    flex-wrap: wrap;
    gap: 4px;
  }
  
  .wizard-clock {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 16px;
    align-items: stretch;
  }
  
  .wizard-panel__border {
    --corner-cut: 12px;
    background-color: var(--1bit-fg);
    padding: 1px 1px 2px 1px;
    width: 100%;
    image-rendering: pixelated;
    image-rendering: crisp-edges;
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }
  
  .wizard-panel {
    --corner-cut: 11px;
    padding: 12px;
    background-color: var(--1bit-bg);
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }
  
  .wizard-panel__inner-shadow {
    --corner-cut: 10px;
    background-color: var(--1bit-fg);
    padding: 0 2px 2px 0;
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }

  .wizard-panel__inner-border {
    --corner-cut: 9px;
    background-color: var(--1bit-fg);
    padding: 1px;
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }

  .wizard-panel__inner {
    --corner-cut: 8px;
    background-color: var(--1bit-bg);
    padding: 6px 6px 4px 6px;
    display: flex;
    flex-direction: column;
    gap: 2px;
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }
  
  .wizard-panel__header {
    display: flex;
    align-items: center;
    gap: 8px;
    font-weight: bold;
    line-height: 1.2;
  }

  .wizard-panel__globe {
    width: 38px;
    height: 38px;
    flex-shrink: 0;
  }

  .wizard-panel__location {
    display: flex;
    flex-direction: column;
    line-height: 1.1;
  }
  
  .wizard-panel__region {
    opacity: 0.8;
  }

  .wizard-panel__bar {
    height: 3px;
    background-color: var(--1bit-fg);
    margin: 2px 0;
  }
  
  .wizard-panel__date {
    font-size: 0.9em;
  }
  
  .wizard-panel__time {
    display: flex;
    align-items: flex-end;
    justify-content: flex-end;
    gap: 4px;
    margin: 2px 0;
    padding-right: 8px;
  }
  
  .wizard-panel__digits {
    font-size: 3em;
    font-weight: bold;
    line-height: 0.85;
    letter-spacing: 1px;
  }
  
  .wizard-panel__ampm {
    font-size: 1em;
    font-weight: bold;
    padding-bottom: 4px;
  }
  
  .wizard-panel__gmt {
    font-size: 0.85em;
    text-align: right;
    padding-right: 8px;
    opacity: 0.8;
  }
  
  .dither-picker {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  
  .dither-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    margin-top: 8px;
  }
  
  .dither-sample {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2px;
    background: none;
    border: var(--1bit-border-width) solid transparent;
    padding: 4px;
    cursor: pointer;
    font-family: inherit;
    color: var(--1bit-fg);
  }
  
  .dither-sample:hover {
    border-color: var(--1bit-fg);
  }
  
  .dither-sample--active {
    border-color: var(--1bit-fg);
    background: var(--1bit-fg);
    color: var(--1bit-bg);
  }
  
  .dither-sample span {
    font-size: 10px;
  }
  
  .dither-box {
    width: 32px;
    height: 32px;
    image-rendering: pixelated;
    image-rendering: crisp-edges;
    border: var(--1bit-border-width) solid var(--1bit-fg);
    background-color: var(--1bit-bg);
  }
  
  /* Dynamic dither patterns using CSS variables */
  .dither-checker-1 {
    background: repeating-conic-gradient(var(--1bit-bg) 0deg 90deg, var(--1bit-fg) 90deg 180deg) 0 0/6px 6px;
  }
  
  .dither-checker-2 {
    background: repeating-conic-gradient(var(--1bit-bg) 0deg 90deg, var(--1bit-fg) 90deg 180deg) 0 0/4px 4px;
  }
  
  .dither-hlines {
    background-image: repeating-linear-gradient(
      to bottom,
      var(--1bit-fg) 0px,
      var(--1bit-fg) 1px,
      transparent 1px,
      transparent 2px
    );
  }
  
  .dither-vlines {
    background-image: repeating-linear-gradient(
      to right,
      var(--1bit-fg) 0px,
      var(--1bit-fg) 1px,
      transparent 1px,
      transparent 2px
    );
  }
  
  .bit-modal-inline__border {
    --corner-cut: 4px;
    background-color: var(--1bit-fg);
    padding: var(--1bit-border-width);
    min-width: 200px;
    max-width: 300px;
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }
  
  .bit-modal-inline {
    --corner-cut: 3px;
    background-color: var(--1bit-bg);
    clip-path: polygon(
      var(--corner-cut) 0,
      calc(100% - var(--corner-cut)) 0,
      100% var(--corner-cut),
      100% calc(100% - var(--corner-cut)),
      calc(100% - var(--corner-cut)) 100%,
      var(--corner-cut) 100%,
      0 calc(100% - var(--corner-cut)),
      0 var(--corner-cut)
    );
  }
  
  .bit-modal-inline__titlebar {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
    padding: 4px 8px;
    font-weight: bold;
  }
  
  .bit-modal-inline__content {
    padding: 8px;
  }
  
  .bit-modal-inline__content p {
    margin: 0;
  }
  
  .bit-modal-inline__footer {
    padding: 4px 8px 8px;
    display: flex;
    gap: 4px;
    justify-content: flex-end;
  }
</style>
