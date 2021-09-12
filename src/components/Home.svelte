<script>
	import Connect from './Connect.svelte';
	import Gallery from './Gallery.svelte';

	//import GalleryItem from './components/GalleryItem.svelte';

	let marinaIsEnabled = false;

	const handleMarinaStatus = (event) => {
		marinaIsEnabled = event.detail.enabled;
	}

</script>

  <section class="section main">
    <h1 class="title">Brera</h1>
    <h2 class="subtitle">Your personal digital art gallery</h2>
  
    {#if marinaIsEnabled}
      <Gallery />
    {:else}
      <Connect on:status={handleMarinaStatus} />
    {/if}
  </section>


<style>
  .main {
			text-align: center;
			padding: 1em;
			max-width: 240px;
		}

  @media (min-width: 640px) {
    .main {
				max-width: none;
			}
		}
</style>