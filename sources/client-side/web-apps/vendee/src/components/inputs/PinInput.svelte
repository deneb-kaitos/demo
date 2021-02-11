<script>
  import {
    onMount,
    onDestroy,
  } from 'svelte';
  import {
    nanoid,
  } from 'nanoid';

  export let length = 8;

  let body;
  let result = '';
  let currentCell = 0;
  const idPrefix = nanoid(3);
  const MoveDirection = Object.freeze({
    LEFT: 37,
    RIGHT: 39,
  });

  $: values = (new Array(length)).fill('');

  $: console.log({ currentCell });

  $: if (currentCell >=0 && currentCell < values.length) {
    const cellID = genID(currentCell);

    if ('window' in globalThis) {
      const element = document.querySelector(`#${cellID}`);

      if (element !== null) {
        element.classList.toggle('activeCell');
      }
    }
  }

  const genID = (id) => `c_${idPrefix}_${id}`;

  const removeActiveClass = (cellId) => {
    const element = document.querySelector(`#${genID(cellId)}`) ?? null;

    if (element !== null) {
      element.classList.remove('activeCell');
    }
  };

  const move = (direction) => {
    switch (direction) {
      case MoveDirection.LEFT: {
        if (currentCell > 0) {
          removeActiveClass(currentCell);

          currentCell = currentCell - 1;
        }

        break;
      }
      case MoveDirection.RIGHT: {
        if (currentCell < values.length - 1) {
          removeActiveClass(currentCell);

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
    console.log('printKeyToCell', key);

    const element = document.querySelector(`#${genID(currentCell)}`) ?? null;

    if (element !== null) {
      element.innerHTML = key;

      move(MoveDirection.RIGHT);
    }
  };

  const removeKeyFromCurrentCell = () => {
    const element = document.querySelector(`#${genID(currentCell)}`) ?? null;

    if (element !== null) {
      element.innerHTML = '';

      move(MoveDirection.LEFT);
    }
  };

  const handleKeyDown = (event) => {
    const { key, keyCode } = event;

    console.log('handleKey', key, keyCode);

    if ([MoveDirection.LEFT, MoveDirection.RIGHT].includes(keyCode)) {
      move(keyCode);
    } else if (isPrintableKey(key)) {
      printKeyToCell(event.key);
    } else if (key === 'Backspace') {
      removeKeyFromCurrentCell();
    }
  };

  onMount(() => {
    console.log({ length }, { values });
  });

  onDestroy(() => {});
</script>

<style>
  .pins {
    display: flex;
    flex: 1 0 auto;
    min-height: 3rem;
  }

  .pin-cell {
    display: flex;
    flex: 1 0 auto;
    border-bottom: 1px solid hsl(0deg 0% 57%);
    background-color: white;
    justify-content: center;
    align-items: center;
    font-size: 2.5rem;
    line-height: 2rem;
  }

  .pin-cell:not(:first-of-type) {
    margin-left: 0.25vw;
  }

  .activeCell {
    border-bottom-color: orangered !important;
  }
</style>

<svelte:window on:keydown|stopPropagation|passive={handleKeyDown} />

<div class='pins'>
  {#each values as value, index}
    <div
      id={genID(index)}
      class='pin-cell roundedCorners'
      class:activeCell={index === 0}
    >{value}</div>
  {/each}
</div>
