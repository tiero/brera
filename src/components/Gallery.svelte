<script>
  import Mint from './Mint.svelte'
  let showModal = false;
</script>

<section class="section is-medium">
  <!-- svelte-ignore a11y-missing-attribute -->
  <p> Your NFTs you own in <strong>Marina</strong> will appear here. 
    Start <a on:click={() => {showModal = !showModal}}>minting your own</a></p>
  {#if showModal}
    <Mint bind:active={showModal} />
  {/if}
</section>


