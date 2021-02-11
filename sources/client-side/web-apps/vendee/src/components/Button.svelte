<script>
  import {
    onMount,
  } from 'svelte';

  export let type;
  export let disabled = true;
  export let width;

  let component;

  onMount(() => {
    component.style.setProperty('--min-width', width);
  });
</script>

<style>
  button {
    --min-width: 'auto';

    min-width: var(--width);
  }
</style>

<button
  {type}
  {disabled}
  width='auto'
  bind:this={component}
>
  <slot></slot>
</button>
