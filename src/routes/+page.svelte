<script lang="ts">
	import Card from '../components/card.svelte';
	import { images } from '$lib/utils/images';
	let videoUrl =
		'https://ik.imagekit.io/neilandsandy/proposal_video_compressed.mp4?updatedAt=1688643253191';
</script>

<div class="container mx-auto space-y-8">
	<div class="flex items-center flex-col">
		<h3 class="text-xl h3 mb-4 text-center">Proposal Video</h3>
		<video class="w-72 aspect-9/16 rounded shadow-lg" controls>
			<source src={videoUrl} type="video/mp4" />
			Your browser does not support the video tag.
		</video>
	</div>
	<div>
		<h3 class="text-xl h3 mb-4">Proposal Pictures</h3>
		<section
			class="padding-4 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-6 xl:grid-cols-8 gap-4"
		>
			{#each images as image}
				<Card {image} />
			{/each}
			<section />
		</section>
	</div>
</div>
