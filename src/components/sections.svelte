<script>
	import Card from '../components/card.svelte';
	import { images } from '$lib/utils/images';
	import { imageSections } from '$lib/utils/images';
</script>

{#each imageSections as section}
	<div>
		<h3 class="text-xl h3 mb-4">{section.name}</h3>
		<section
			class="padding-4 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-6 xl:grid-cols-8 gap-4"
		>
			{#each section.images as image}
				<Card {image} />
			{/each}
			<section />
		</section>
	</div>
{/each}
