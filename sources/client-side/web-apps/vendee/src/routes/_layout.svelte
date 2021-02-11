<script context='module'>
  export async function preload(page, session) {
    return {
      token: session.token ?? null,
    }
  };
</script>

<script>
  import {
    isTokenValid,
  } from '../helpers/isTokenValid.mjs';
  import Nav from '../components/Nav.svelte';

  export let segment;
  export let token;
</script>

{#if isTokenValid(token) === true}
  <Nav {segment} />
{/if}

<slot></slot>
