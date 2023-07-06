<script lang="ts">
	import Card from '../components/card.svelte';
	import { images } from '$lib/utils/images';
</script>

<div class="container mx-auto">
	<section
		class="padding-4 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-6 xl:grid-cols-8 gap-4"
	>
		{#each images as image}
			<Card {image} />
		{/each}
		<section />
	</section>
</div>
