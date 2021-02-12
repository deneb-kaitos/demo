<script>
  export let type;
  export let disabled = true;
</script>

<style>
  button {
    align-self: center;
    height: 3rem;
    padding: 0.5vh 1vw;
    font-size: 1.25rem;
    border-width: 1px;
    border-color: var(--drop-shadow-color);
    font-variant: small-caps;
    text-transform: lowercase;
  }

  button:not(:disabled):hover {
    filter: drop-shadow(0px 0px 4px var(--drop-shadow-color));
  }

  button:active {
    filter: none;
    box-shadow: inset 3px 3px 1rem var(--drop-shadow-color);
  }
  button:focus {
    outline: none;
  }
</style>

<button
  {type}
  {disabled}
  class='roundedCorners'
>
  <slot></slot>
</button>
