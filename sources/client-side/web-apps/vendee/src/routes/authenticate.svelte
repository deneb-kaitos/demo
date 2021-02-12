<!--
  FIXME: remove this when ready
-->

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
  let pinLength = 6;
  let pin = null;

  const handleSubmit = () => {
    console.log('handleSubmit');
  };

  const isPinValid = (pin = null) => {
    if (pin === null) {
      return false;
    }

    if (pin.length === pinLength) {
      return true;
    }

    return false;
  };

  const handlePin = ({ detail: { payload } }) => {
    pin = payload;
  };
</script>

<style>
  article {
    --height: 40%;
    --center: calc(50% - var(--height) / 2);
    --background-color: hsl(0, 0%, 100%);

    display: grid;
    grid-gap: max(0.25vh, 0.25vw);
    grid-template-columns: 1fr;
    grid-template-rows: 3fr 3fr;
    grid-template-areas:
      'logo-container'
      'form-container'
    ;

    justify-self: center;
    align-self: center;
    min-height: var(--height);
    position: absolute;
    top: calc(var(--center) / 1.5);
  }

  .logo-container {
    grid-area: logo-container;

    display: flex;
    justify-content: center;
    align-items: center;

    filter: var(--drop-shadow);
  }

  .form-container {
    grid-area: form-container;
    display: grid;
    grid-gap: max(0.25vh, 0.25vw);
    grid-template-columns: 1fr;
    grid-template-rows: repeat(3, 1fr);
    grid-template-areas:
      'pin-container'
      'push-consent-container'
      'submit-container'
    ;
    width: 12vw;
  }

  .form-row {
    display: flex;
    flex: 1 0 auto;
    justify-content: center;
    align-items: stretch;
  }

  .pin-container {
    grid-area: pin-container;
  }

  .push-consent-container {
    grid-area: push-consent-container;
    display: flex;
    justify-content: space-between;
  }

  .push-consent-container > div {
    display: flex;
    flex: 1 0 auto;
    justify-content: center;
    align-items: center;
  }

  .push-consent-container > div > label {
    display: flex;
    flex: 1 0 auto;
    height: 100%;
    justify-content: flex-end;
    align-items: center;
  }

  .submit-container {
    grid-area: submit-container;
  }

  #allow-push-notifications {
    display: none;
  }
</style>

<article class="roundedCorners">
  <div class='logo-container'>
    <Logo />
  </div>
  <form on:submit|preventDefault|stopPropagation={handleSubmit} class='form-container'>
    <input type="text" autocomplete="username" class='hidden' value={nanoid(15)}>
    <div class='form-row pin-container'>
      <PinInput length={pinLength} on:pin={handlePin} />
    </div>
    <div class='form-row push-consent-container'>
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
    <div class='form-row submit-container'>
      <Button
        type='submit'
        width='25%'
        disabled={pushNotificationAllowed === false || isPinValid(pin) === false}
      >
        log in
      </Button>
    </div>
  </form>
</article>
