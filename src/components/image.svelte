<script lang="ts">
	import { onMount } from 'svelte';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import type { Filters } from '$lib/utils/images';
	import { createFilter } from 'cc-gram';
	import pkg from 'file-saver';
	import { toastStore, type ToastSettings } from '@skeletonlabs/skeleton';
	const { saveAs } = pkg;

	export let imgSrc: string;
	// export let widths: number[] = [480, 800, 1200];
	export let widths: number[] = [100, 200, 300, 400, 500, 600];
	export let alt = '';
	export let imgClass: string = '';
	export let imgFilter: Filters = '';
	export let lazyLoad: boolean = false;

	let loaded = false;
	let failed = false;
	let loading = false;
	let image: any;
	const filter = createFilter({ init: false });

	function generateURL(width: number): string {
		let new_url = `https://ik.imagekit.io/neilandsandy/${imgSrc}?tr=w-${width},h-${width}`;
		return new_url;
	}

	function generateDownloadURL(): string {
		let new_url = `https://ik.imagekit.io/neilandsandy/${imgSrc}`;
		return new_url;
	}

	function replaceExtensionWithPNG(filename: string): string {
		var extensionIndex = filename.lastIndexOf('.');
		if (extensionIndex !== -1) {
			var nameWithoutExtension = filename.substring(0, extensionIndex);
			return nameWithoutExtension + '.png';
		}
		return filename + '.png';
	}

	function sanitizeId(str: string) {
		const cleanedStr = str.replace(/[._]/g, '');

		// If the first character is a number, prepend a letter
		if (cleanedStr[0] >= '0' && cleanedStr[0] <= '9') {
			return 'id' + cleanedStr;
		}

		return cleanedStr;
	}

	export async function downloadImageWithFilter() {
		const img = document.createElement('img');
		img.src = generateDownloadURL();
		img.id = 'temp-image';
		img.setAttribute('data-filter', imgFilter);
		img.crossOrigin = 'anonymous';

		const hiddenDiv = document.createElement('div');
		hiddenDiv.style.display = 'none';
		document.body.appendChild(hiddenDiv);
		hiddenDiv.appendChild(img);

		await new Promise((resolve, reject) => {
			img.onload = resolve;
			img.onerror = reject;
		});

		filter.applyFilter('#temp-image');
		const blob = await filter.getBlob(img, {
			type: 'image/jpeg',
			quality: 0.8
		});

		// Check if blob is not null before saving
		if (blob) {
			saveAs(blob, imgSrc);
		} else {
			console.error('Failed to create blob from image');
		}

		document.body.removeChild(hiddenDiv);
	}

	export function openOriginalImage() {
		const originalImageURL = generateDownloadURL();
		window.open(originalImageURL, '_blank');
	}

	let main = generateURL(widths[widths.length - 1]);
	let placeholder = replaceExtensionWithPNG(imgSrc);
	let id = sanitizeId(imgSrc);
	const opacity = tweened(1, {
		duration: 400,
		easing: cubicOut
	});

	onMount(() => {
		loading = true;

		// Create a promise that resolves when the image loads
		let loadImage = new Promise((resolve, reject) => {
			const img = new Image();
			img.src = main;

			img.onload = () => resolve(img);
			img.onerror = reject;
		});

		loadImage
			.then(() => {
				loading = false;
				loaded = true;
			})
			.catch(() => {
				loading = false;
				failed = true;
			});
	});

	$: if (imgFilter !== '') {
		filter.applyFilter(`#${id}-image`);
	}

	export function updateFilter(newFilter: Filters) {
		imgFilter = newFilter;
		// Now that the data-filter attribute is updated, apply the filter
		filter.applyFilter(`#${id}-image`);
	}
</script>

<div class="parent">
	{#if loaded}
		<img
			id={`${id}-image`}
			bind:this={image}
			data-filter={imgFilter}
			on:load={() => {
				// Apply the filter after image is loaded
				if (imgFilter != '') {
					// Apply filter manually to your image
					filter.applyFilter(`#${id}-image`);
				}

				// Make placeholder transparent
				opacity.set(0);
			}}
			src={main}
			{alt}
			class={`${imgClass} image1 ${lazyLoad ? 'lazyload' : ''}`}
			srcset={widths.map((width) => `${generateURL(width)} ${width}w`).join(', ')}
			sizes={widths.map((width) => `(max-width: ${width * 1.25}px) ${width}px`).join(', ') +
				`, ${widths[widths.length - 1]}px`}
			decoding="async"
		/>
	{/if}
	{#if loading || loaded}
		<img
			id={`${id}-placeholder`}
			data-filter={imgFilter}
			src={`/blurred/${placeholder}`}
			{alt}
			class={`${imgClass} image2 ${lazyLoad ? 'lazyload' : ''}`}
			style={`opacity: ${$opacity}`}
			on:load={() => {
				// Apply the filter after image is loaded
				if (imgFilter != '') {
					// Apply filter manually to your image
					filter.applyFilter(`#${id}-placeholder`);
				}
			}}
		/>
	{/if}
</div>

<style>
	.parent {
		position: relative;
		top: 0;
		left: 0;
	}

	.image1 {
		position: relative;
		top: 0;
		left: 0;
	}

	.image2 {
		position: absolute;
		top: 0;
		left: 0;
	}
</style>
