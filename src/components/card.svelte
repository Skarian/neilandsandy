<script lang="ts">
	import Image from './image.svelte';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import { modalStore, type ModalSettings, type ModalComponent } from '@skeletonlabs/skeleton';
	import Modal from './modal.svelte';
	export let image: string;

	const scale = tweened(1, {
		duration: 300,
		easing: cubicOut
	});

	const modalComponent: ModalComponent = {
		ref: Modal,
		props: { image: image },
		slot: '<p>Skeleton</p>'
	};

	const modal: ModalSettings = {
		type: 'component',
		component: modalComponent
	};

	const handleClick = () => {
		scale.set(0.9);
		modalStore.trigger(modal);
	};
</script>

<button
	on:mouseenter={() => scale.set(1.05)}
	on:mouseleave={() => scale.set(1)}
	on:click={handleClick}
	on:mouseup={() => scale.set(1)}
	style={`scale: ${$scale}`}
>
	<div class="w-full aspect-square">
		<Image imgSrc={image} imgClass="w-full rounded-lg" />
	</div>
</button>
