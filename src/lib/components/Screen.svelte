<script lang="ts">
  import type { Snippet } from 'svelte';
  
  interface Props {
    width?: number;
    height?: number;
    scale?: number;
    dark?: boolean;
    children: Snippet;
  }
  
  let { width = 160, height = 160, scale = 2, dark = false, children }: Props = $props();
</script>

<div class="bit-device" style="--scale: {scale}; --width: {width}px; --height: {height}px;">
  <div class="bit-device__bezel">
    <div class="bit-device__screen-wrapper">
      <div class="bit-device__screen" class:dark={dark} style="width: {width}px; height: {height}px">
        <div class="onebit">
          {@render children()}
        </div>
      </div>
    </div>
    <div class="bit-device__graffiti">
      <div class="bit-device__graffiti-area">
        <span class="bit-device__graffiti-label">abc</span>
      </div>
      <div class="bit-device__graffiti-divider"></div>
      <div class="bit-device__graffiti-area">
        <span class="bit-device__graffiti-label">123</span>
      </div>
    </div>
  </div>
</div>

<style>
  .bit-device {
    --scale: 2;
    display: inline-block;
    /* Reserve space for scaled content */
    width: calc((var(--width) + 32px) * var(--scale));
    height: calc((var(--height) + 80px) * var(--scale));
  }
  
  .bit-device__bezel {
    background: linear-gradient(145deg, #4a4a4a 0%, #2a2a2a 100%);
    border-radius: 12px;
    padding: 16px;
    box-shadow: 
      0 8px 32px rgba(0,0,0,0.4),
      inset 0 1px 0 rgba(255,255,255,0.1);
    transform: scale(var(--scale));
    transform-origin: top left;
  }
  
  .bit-device__screen-wrapper {
    /* This wrapper enables the pixelated scaling */
    image-rendering: pixelated;
    image-rendering: crisp-edges;
    -ms-interpolation-mode: nearest-neighbor;
  }
  
  .bit-device__screen {
    background-color: var(--1bit-bg);
    border: 2px solid var(--1bit-fg);
    border-radius: 2px;
    overflow: hidden;
    /* Crisp pixel rendering */
    image-rendering: pixelated;
    image-rendering: crisp-edges;
  }
  
  .bit-device__screen .onebit {
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
  
  .bit-device__graffiti {
    display: flex;
    margin-top: 12px;
    gap: 4px;
  }
  
  .bit-device__graffiti-area {
    flex: 1;
    height: 40px;
    background: #1a1a1a;
    border-radius: 2px;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    padding-bottom: 4px;
  }
  
  .bit-device__graffiti-label {
    font-family: inherit;
    font-size: 6px;
    color: #666;
    text-transform: lowercase;
  }
  
  .bit-device__graffiti-divider {
    width: 2px;
    background: #3a3a3a;
  }
</style>
