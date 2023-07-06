<script lang="ts">
	import type { Filters } from '$lib/utils/images';

	interface ImageComponent {
		downloadImage: () => Promise<void>;
		updateFilter: (newFilter: Filters) => void;
	}

	import Image from '../../components/image.svelte';
	let child: ImageComponent;
	let filter: Filters = '';

	function applyFilter(newFilter: Filters) {
		filter = newFilter;

		setTimeout(() => {
			child.updateFilter(filter);
		}, 0);
	}
</script>

<div>
	<Image bind:this={child} imgSrc="DSC_5753.JPG" imgClass="w-full rounded-lg" imgFilter={filter} />
</div>
<button on:click={() => child.downloadImage()}>Download</button>
<button
	on:click={() => {
		applyFilter('moon');
	}}>moon</button
>
<button
	on:click={() => {
		applyFilter('xpro2');
	}}>xpro2</button
>
