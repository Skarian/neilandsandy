<script lang="ts">
	export let image: string;
	import { modalStore } from '@skeletonlabs/skeleton';
	import Image from './image.svelte';
	import { canvasFilters } from '$lib/utils/images';
	import { writable, type Writable } from 'svelte/store';
	import type { Filters } from '$lib/utils/images';

	interface ImageComponent {
		downloadImageWithFilter: () => Promise<void>;
		updateFilter: (newFilter: Filters) => void;
		openOriginalImage: () => void;
	}

	const handleClose = () => {
		modalStore.close();
	};

	const selectedFilter: Writable<string> = writable('');

	let child: ImageComponent;
	let filter: Filters = '';

	function applyFilter(newFilter: Filters) {
		filter = newFilter;

		setTimeout(() => {
			child.updateFilter(filter);
		}, 0);
	}
</script>

<div class="bg-surface-100-800-token rounded-lg flex flex-col">
	<!-- First Section: Title and Close Button -->
	<div class="p-4 flex flex-row justify-between">
		<h2 class="text-2xl font-semibold">{image}</h2>
		<button id="closeButton" class="focus:outline-none" on:click={handleClose}>
			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke="currentColor"
				class="w-6 h-6 h2"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M6 18L18 6M6 6l12 12"
				/>
			</svg>
		</button>
	</div>

	<!-- Second Section: Image and Filter Button -->
	<div class="p-4 flex flex-grow flex-col">
		<div class="flex justify-center mb-4">
			<Image bind:this={child} imgSrc={image} imgClass={`max-h-80 rounded-lg`} imgFilter={filter} />
		</div>
		<!-- Filters Section-->
		<div
			class="overflow-y-auto h-40 max-w-2xl flex flex-wrap items-center justify-center p-2 bg-surface-200-700-token rounded-lg"
		>
			{#each canvasFilters as filter}
				<!-- content here -->
				<div class="flex flex-col items-center w-24">
					<button
						class="p-2"
						on:click={() => {
							selectedFilter.set(filter);
							applyFilter(filter);
						}}
					>
						<div
							class={`rounded-md ${
								filter === $selectedFilter ? 'border-2 border-primary-500' : ''
							}`}
						>
							<Image imgSrc={image} imgClass={`w-full rounded-md`} imgFilter={filter} />
						</div>
						<div class="text-xs">{filter === '' ? 'normal' : filter.toLowerCase()}</div>
					</button>
				</div>
			{/each}
		</div>
	</div>
	<!-- Third Section: Download and Share Button -->
	<div class="flex flex-col items-center mb-4">
		<div class="mb-2 space-x-2">
			<button
				type="button"
				class="btn rounded-lg variant-ghost-primary"
				on:click={() => child.downloadImageWithFilter()}
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="1.5"
					stroke="currentColor"
					class="w-3 h-3"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M3 16.5v2.25A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75V16.5M16.5 12L12 16.5m0 0L7.5 12m4.5 4.5V3"
					/>
				</svg>

				<span class="text-xs">Download with Filter</span>
			</button>
			<button
				type="button"
				class="btn rounded-lg variant-ghost-secondary"
				on:click={() => child.openOriginalImage()}
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="1.5"
					stroke="currentColor"
					class="w-3 h-3"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M13.5 6H5.25A2.25 2.25 0 003 8.25v10.5A2.25 2.25 0 005.25 21h10.5A2.25 2.25 0 0018 18.75V10.5m-10.5 6L21 3m0 0h-5.25M21 3v5.25"
					/>
				</svg>

				<span class="text-xs">View Original</span>
			</button>
		</div>
		<div class="text-xs italic">Filter download may not work on iOS mobile / iPhone</div>
	</div>
</div>
