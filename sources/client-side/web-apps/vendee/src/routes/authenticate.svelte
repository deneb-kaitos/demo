<!-- FIXME: remove this when ready -->

<script context='module'>
  import {
    isTokenValid,
  } from '../helpers/isTokenValid.mjs';
  import Logo from '../components/icons/Logo.svelte';
  import CircleAlert from '../components/icons/CircleAlert.svelte';
  import Check from '../components/icons/Check.svelte';
  import Button from '../components/Button.svelte';
  import PinInput from '../components/inputs/PinInput.svelte';

  export async function preload(page, session) {
    if (isTokenValid(session.token) === true) {
      return this.redirect(200, '/');
    }
  };
</script>

<script>
  import {
    nanoid,
  } from 'nanoid';

  let pushNotificationAllowed = false;

  const handleSubmit = () => {
    console.log('handleSubmit');
  };
</script>

<style>
  article {
    --height: 40%;
    --width: 40%;
    --center: calc(50% - var(--height) / 2);
    --background-color: hsl(0, 0%, 100%);

    display: grid;
    grid-gap: min(0.25vh, 0.25vw);
    grid-template-columns: 1fr;
    grid-template-rows: 3fr 2fr;
    grid-template-areas:
      'logo-container'
      'submit-container'
    ;

    justify-self: center;
    align-self: center;
    /* width: var(--width); */
    height: var(--height);
    position: absolute;
    top: calc(var(--center) / 1.5);

    padding: max(1vh, 1vw);
  }

  .logo-container {
    grid-area: logo-container;

    display: flex;
    justify-content: center;
    align-items: center;

    filter: var(--drop-shadow);
  }

  .submit-container {
    grid-area: submit-container;
    display: flex;
    flex-direction: column;
    justify-content: stretch;
    align-items: stretch;
  }

  form {
    display: flex;
    flex-direction: column;
    flex: 1 0 auto;
  }

  .form-row {
    display: flex;
    flex: 1 0 auto;
    justify-content: center;
    align-items: stretch;
  }

  .form-row:not(:first-of-type) {
    margin-top: 0.5vh;
  }

  .push-notification-consent {
    display: grid;
    grid-template-columns: 1fr 3fr;
  }

  .push-notification-consent > div {
    display: flex;
    flex: 1 0 auto;
    justify-content: center;
    align-items: center;
  }

  #allow-push-notifications {
    display: none;
  }

  .push-notification-consent > div > label {
    display: flex;
    flex: 1 0 auto;
    height: 100%;
    justify-content: center;
    align-items: center;
  }
</style>

<article class="roundedCorners">
  <div class='logo-container'>
    <Logo />
  </div>
  <form class='submit-container' on:submit|preventDefault|stopPropagation={handleSubmit}>
    <input type="text" autocomplete="username" class='hidden' value={nanoid(15)}>
    <div class='form-row'>
      <PinInput length={6} />
    </div>
    <div class='form-row push-notification-consent'>
      <div>
        <input type='checkbox' id='allow-push-notifications' name='allow-push-notifications' bind:checked={pushNotificationAllowed} />
        {#if pushNotificationAllowed === true}
          <Check />
        {:else}
          <CircleAlert />
        {/if}
      </div>
      <div>
        <label for='allow-push-notifications'>I hereby consent to receive required notifications</label>
      </div>
    </div>
    <div class='form-row'>
      <Button
        type='submit'
        width='25%'
        disabled={pushNotificationAllowed === false}
      >
        log in
      </Button>
    </div>
  </form>
</article>
