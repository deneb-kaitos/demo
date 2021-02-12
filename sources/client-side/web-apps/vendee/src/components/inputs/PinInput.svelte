<script>
  import {
    createEventDispatcher,
  } from 'svelte';

  export let length = 8;

  const dispatch = createEventDispatcher();
  const EMPTY_CELL = '';
  const MoveDirection = Object.freeze({
    LEFT: 37,
    RIGHT: 39,
  });
  let currentCell = 0;

  $: pins = (new Array(length)).fill(null).map((value, id) => ({
    id,
    value: EMPTY_CELL,
    activeCell: false,
  }));

  $: {
    dispatch('pin', {
      payload: pins.map((pin) => pin.value).join(EMPTY_CELL),
    });
  }

  const move = (direction) => {
    switch (direction) {
      case MoveDirection.LEFT: {
        if (currentCell > 0) {
          currentCell = currentCell - 1;
        }

        break;
      }
      case MoveDirection.RIGHT: {
        if (currentCell < pins.length - 1) {
          currentCell = currentCell + 1;
        }

        break;
      }
      default: {
        break;
      }
    }
  };

  const PRINTABLE_CHARS = 'qwertyuiopasdfghjklzxcvbnm1234567890QWERTYUIOPASDFGHJKLZXCVBNM!@#$%^&*_-+=:;?>.<,';

  const isPrintableKey = (key) => {
    return PRINTABLE_CHARS.includes(key);
  };

  const printKeyToCell = (key) => {
    (pins[currentCell]).value = key;

    pins = pins;

    move(MoveDirection.RIGHT);
  };

  const removeKeyFromCurrentCell = () => {
    (pins[currentCell]).value = EMPTY_CELL;

    pins = pins;

    move(MoveDirection.LEFT);
  };

  const handleKeyDown = (event) => {
    if (event.metaKey === true) {
      return;
    }

    const { key, keyCode } = event;

    if ([MoveDirection.LEFT, MoveDirection.RIGHT].includes(keyCode)) {
      move(keyCode);
    } else if (isPrintableKey(key)) {
      printKeyToCell(event.key);
    } else if (key === 'Backspace') {
      removeKeyFromCurrentCell();
    }
  };
</script>

<style>
  .pins {
    display: flex;
    flex: 1 0 auto;
    flex-wrap: nowrap;
    min-height: 3rem;
  }

  .pin-cell {
    display: flex;
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: calc(100% / 6);
    border-bottom: 3px solid transparent;
    background-color: hsl(0deg 0% 100% / 50%);
    justify-content: center;
    align-items: center;
    font-size: 3.5rem;
    line-height: 0;
  }

  .pin-cell:not(:empty) {
    background-color: hsl(0deg 0% 100% / 25%);
  }

  .pin-cell:not(:first-of-type) {
    margin-left: 0.25vw;
  }

  .activeCell {
    border-bottom-color: hsl(16deg 100% 50%) !important;
  }
</style>

<svelte:window on:keydown|stopPropagation|passive={handleKeyDown} />

<div class='pins'>
  {#each pins as pin (pin.id)}
    <div
      class='pin-cell roundedCorners'
      class:activeCell={pin.id === currentCell}
    >{pin.value}</div>
  {/each}
</div>
