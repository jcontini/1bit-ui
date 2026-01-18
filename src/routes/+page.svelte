<script lang="ts">
  import { Button, Modal, Checkbox, Input, Dropdown, Radio, List, Tabs, TabPanel, ProgressBar, Toggle, Slider } from '$lib';
  
  let showModal = $state(false);
  let panelTab = $state('info');
  let checked = $state(true);
  let text = $state('');
  let dropdown = $state('all');
  let radio = $state('day');
  let selected = $state('item2');
  let tab = $state('stats');
  let darkMode = $state(true);
  let borderWidth = $state(2);
</script>

<div class="page" class:dark={darkMode} style="--1bit-border-width: {borderWidth}px;">
  <header class="header">
    <h1>1bit UI</h1>
    <p>A retro 1-bit component library for Svelte</p>
    <Toggle bind:checked={darkMode} labelOff="Light" labelOn="Dark" />
  </header>

  <div class="grid">
    <!-- Column 1: Simple controls -->
    <section class="col-1">
      <h2>Buttons</h2>
      <div class="row">
        <Button>OK</Button>
        <Button>Cancel</Button>
        <Button>New</Button>
      </div>
    </section>
    
    <section class="col-1">
      <h2>Inputs</h2>
      <Input bind:value={text} placeholder="Box input" />
      <div style="height: 4px;"></div>
      <Input variant="underline" placeholder="Underline input" />
    </section>
    
    <section class="col-1">
      <h2>Dropdown</h2>
      <Dropdown 
        options={[
          { value: 'all', label: 'All' },
          { value: 'business', label: 'Business' },
          { value: 'personal', label: 'Personal' }
        ]} 
        bind:value={dropdown} 
      />
    </section>
    
    <section class="col-1">
      <h2>Checkboxes</h2>
      <Checkbox label="Show completed" bind:checked />
      <Checkbox label="Show priorities" />
      <Checkbox label="Auto-sync" />
    </section>
    
    <section class="col-1">
      <h2>Radio Buttons</h2>
      <Radio name="view" value="day" label="Day view" bind:group={radio} />
      <Radio name="view" value="week" label="Week view" bind:group={radio} />
      <Radio name="view" value="month" label="Month view" bind:group={radio} />
    </section>
    
    <section class="col-1">
      <h2>Slider</h2>
      <Slider bind:value={borderWidth} min={1} max={5} step={1} label="Border:" />
    </section>
    
    <section class="col-1">
      <h2>Progress</h2>
      <ProgressBar value={42} label="Memory:" />
      <ProgressBar value={78} label="Storage:" />
    </section>
    
    <!-- Column 2: Complex/multi components -->
    <section class="col-2 row-start">
      <h2>Tabs</h2>
      <Tabs 
        tabs={[
          { id: 'stats', label: 'Stats' },
          { id: 'equip', label: 'Equip' },
          { id: 'spells', label: 'Spells' },
          { id: 'misc', label: 'Misc' }
        ]} 
        bind:activeTab={tab} 
      />
    </section>
    
    <section class="col-2">
      <h2>List</h2>
      <List 
        items={[
          { id: 'item1', content: 'Wakem & McLaughlin' },
          { id: 'item2', content: '4 cs printers blank' },
          { id: 'item3', content: 'Spaulding & Bros' }
        ]} 
        bind:selectedId={selected} 
      />
    </section>
    
    <section class="col-2">
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
    
    <section class="col-2">
      <h2>Modal</h2>
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
    </section>
    
    <!-- Column 3: Typography & Dither Patterns -->
    <section class="type-specimen">
      <h2>Typography</h2>
      <h1>Heading 1</h1>
      <h2>Heading 2</h2>
      <h3>Heading 3</h3>
      <p>Regular text</p>
      <p><strong>Bold</strong> <em>Italic</em></p>
      <p>0123456789</p>
      
      <h2 style="margin-top: 16px;">Dither Patterns</h2>
      <p class="dither-note">Classic 1-bit grayscale simulation</p>
      <div class="dither-grid">
        <div class="dither-sample">
          <div class="dither-box dither-25"></div>
          <span>25%</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-50"></div>
          <span>50%</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-75"></div>
          <span>75%</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-bayer"></div>
          <span>Bayer</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-hlines"></div>
          <span>H-Lines</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-vlines"></div>
          <span>V-Lines</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-diag"></div>
          <span>Diag</span>
        </div>
        <div class="dither-sample">
          <div class="dither-box dither-cross"></div>
          <span>Cross</span>
        </div>
      </div>
    </section>
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
    padding: 40px;
    background: var(--1bit-bg);
    color: var(--1bit-fg);
    font-family: 'Tamzen', monospace;
    font-size: 16px;
    line-height: 1.2;
    transition: background-color 0.2s, color 0.2s;
  }
  
  .header {
    margin-bottom: 32px;
  }
  
  .header h1 {
    margin: 0 0 4px 0;
    font-size: 2em;
  }
  
  .header p {
    margin: 0 0 12px 0;
    opacity: 0.7;
  }
  
  .page {
    max-width: 1100px;
  }
  
  h2 {
    font-size: 1.1em;
    margin: 0 0 0.3em 0;
  }
  
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-auto-flow: dense;
    gap: 20px 32px;
  }
  
  section {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
  }
  
  .col-1 { grid-column: 1; }
  .col-2 { grid-column: 2; }
  .row-start { grid-row-start: 1; }
  
  .row {
    display: flex;
    flex-wrap: wrap;
    gap: 4px;
  }
  
  .type-specimen {
    grid-column: 3;
    grid-row: 1 / span 6;
    border-left: 2px solid var(--1bit-fg);
    padding-left: 24px;
  }
  
  .type-specimen h1 { font-size: 2em; margin: 0 0 0.2em 0; }
  .type-specimen h2 { font-size: 1.5em; margin: 0 0 0.2em 0; }
  .type-specimen h3 { font-size: 1.25em; margin: 0 0 0.2em 0; }
  .type-specimen p { margin: 0 0 0.3em 0; }
  
  .dither-note {
    font-size: 0.75em;
    opacity: 0.7;
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
  }
  
  .dither-sample span {
    font-size: 10px;
  }
  
  .dither-box {
    width: 32px;
    height: 32px;
    image-rendering: pixelated;
    image-rendering: crisp-edges;
  }
  
  .bit-modal-inline {
    background-color: var(--1bit-bg);
    border: 3px solid var(--1bit-fg);
    border-radius: 3px;
    min-width: 200px;
    max-width: 300px;
  }
  
  .bit-modal-inline__titlebar {
    background-color: var(--1bit-fg);
    color: var(--1bit-bg);
    padding: 2px 6px;
    font-weight: bold;
  }
  
  .bit-modal-inline__content {
    padding: 6px;
    border: 2px solid var(--1bit-fg);
    margin: 4px;
  }
  
  .bit-modal-inline__content p {
    margin: 0;
  }
  
  .bit-modal-inline__footer {
    padding: 4px 6px;
    display: flex;
    gap: 4px;
    justify-content: flex-end;
  }
</style>
