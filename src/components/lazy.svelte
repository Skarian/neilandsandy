<script lang="ts">
	import { onMount, onDestroy } from 'svelte';

	let images: NodeListOf<HTMLImageElement>;
	let observer: IntersectionObserver;

	function handleIntersection(
		entries: IntersectionObserverEntry[],
		observer: IntersectionObserver
	) {
		entries.forEach((entry: IntersectionObserverEntry) => {
			if (entry.isIntersecting) {
				entry.target.setAttribute('loading', 'eager');
			} else {
				entry.target.setAttribute('loading', 'lazy');
			}
		});
	}

	onMount(() => {
		images = document.querySelectorAll('img');
		observer = new IntersectionObserver(handleIntersection);

		images.forEach((img: HTMLImageElement) => {
			// Set loading to eager if image is in viewport on initial load, lazy if not
			if (
				img.getBoundingClientRect().top <= window.innerHeight &&
				img.getBoundingClientRect().bottom >= 0
			) {
				img.setAttribute('loading', 'eager');
			} else {
				img.setAttribute('loading', 'lazy');
			}

			// Observe the image for changes in intersection with viewport
			observer.observe(img);
		});
	});

	onDestroy(() => {
		if (observer) {
			images.forEach((img: HTMLImageElement) => observer.unobserve(img));
		}
	});
</script>
