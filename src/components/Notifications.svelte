<style>
  .default-position-style-top-left,
  .default-position-style-top-center,
  .default-position-style-top-right,
  .default-position-style-bottom-left,
  .default-position-style-bottom-center,
  .default-position-style-bottom-right {
    position: fixed;
    width: 270px;
  }

  .default-position-style-top-left {
    top: 0;
    left: 0;
  }

  .default-position-style-top-center {
    top: 0;
    left: 50%;
    transform: translateX(-50%);
  }

  .default-position-style-top-right {
    top: 0;
    right: 0;
  }

  .default-position-style-bottom-left {
    bottom: 0;
    left: 0;
  }

  .default-position-style-bottom-center {
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
  }

  .default-position-style-bottom-right {
    bottom: 0;
    right: 0;
  }
</style>
<script>
  import { setContext } from 'svelte';

  import Notification from './Notification.svelte';

  import context from '../context';
  import store from '../store';
  import positions from '../positions';

  export let item = null;
  export let withoutStyles = false;

  const getClass = (position = '') => {
    const defaultPositionClass = ` default-position-style-${position}`;

    return `position-${position}${withoutStyles ? '' : defaultPositionClass}`;
  };

  setContext(context, store);
</script>

<slot></slot>
<div class="notifications">
  {#each positions as position}
    <div class={getClass(position)}>
      <div>
        {#each $store as notification (notification.id)}
          {#if notification.position === position}
            {#if item}
              <svelte:component this={item} {notification}/>
            {:else}
              <Notification {notification} {withoutStyles}/>
            {/if}
          {/if}
        {/each}
      </div>
    </div>
  {/each}
</div>
