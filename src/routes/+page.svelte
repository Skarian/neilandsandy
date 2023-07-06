<script lang="ts">
	import Sections from '../components/sections.svelte';
	let videoUrl =
		'https://ik.imagekit.io/neilandsandy/proposal_video_compressed.mp4?updatedAt=1688643253191';
</script>

<div class="container mx-auto space-y-8">
	<div class="flex items-center flex-col">
		<h3 class="text-xl h3 mb-4 text-center">Proposal Video</h3>
		<video class="w-72 aspect-9/16 rounded shadow-lg rounded-lg" controls>
			<source src={videoUrl} type="video/mp4" />
			Your browser does not support the video tag.
		</video>
	</div>
	<Sections />
</div>
