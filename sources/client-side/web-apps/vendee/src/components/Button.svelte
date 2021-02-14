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
    font-variant: small-caps;
    text-transform: lowercase;
  }
</style>

<button
  {type}
  {disabled}
  class='roundedCorners'
>
  <slot></slot>
</button>
