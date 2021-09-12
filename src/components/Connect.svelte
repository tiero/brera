<script lang="ts">
  import { onMount, createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  $: installed = false;
  $: enabled = false;
  let skipInterval = false;

  const marina = window.marina;

  const connect = async () => {
    await marina.enable();
  };

  const notifyEnabled = () => {
    dispatch('status', { enabled: true });
  };

  const notifyDisabled = () => {
    dispatch('status', { enabled: false });
  };

  onMount(async () => {
    setInterval(async () => {
      if (skipInterval) return;

      // check if is installed
      const isInstalled = typeof marina !== 'undefined';
      if (!isInstalled) return;

      installed = true;

      // check if the website is enabled already
      const isEnabled = await marina.isEnabled();
      if (!isEnabled) return;

      enabled = true;
      notifyEnabled();

      // Start listening to ENABLED and DISABLED events and binds to the state
      await marina.on('ENABLED', () => {
        enabled = true;
        notifyEnabled();
      });
      await marina.on('DISABLED', () => {
        enabled = false;
        notifyDisabled();
      });

      // prevent next interval to happens
      skipInterval = true;
    }, 1000);
  });
</script>

{#if !installed}
  <button
    class="button"
    on:click={() => window.open('https://vulpem.com/marina')}
  >
    Install Marina
  </button>
{:else if !enabled}
  <button class="button" on:click={connect}> Connect with Marina </button>
{/if}
