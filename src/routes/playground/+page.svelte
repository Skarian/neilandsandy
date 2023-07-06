<script lang="ts">
	import { onMount } from 'svelte';
	import { createFilter } from 'cc-gram';

	let image: any;
	let filter: any;

	onMount(() => {
		// Initialize the filter
		filter = createFilter({ init: false });

		// Apply filter manually to your image
		filter.applyFilter('#my-image');
	});

	// Function to download the filtered image
	async function downloadImage() {
		const blob = await filter.getBlob(image, {
			type: 'image/jpeg',
			quality: 0.8
		});

		// Convert blob to URL and download
		const url = URL.createObjectURL(blob);
		const link = document.createElement('a');
		link.href = url;
		link.download = 'my-image.jpg';
		link.click();
	}
</script>

<img id="my-image" bind:this={image} src="test.JPG" data-filter="moon" />

<button on:click={downloadImage}> Download Image </button>
