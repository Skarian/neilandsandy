<script lang="ts">
	// The ordering of these imports is critical to your app working properly
	import '@skeletonlabs/skeleton/themes/theme-vintage.css';
	// If you have source.organizeImports set to true in VSCode, then it will auto change this ordering
	import '@skeletonlabs/skeleton/styles/skeleton.css';
	// Most of your app wide CSS should be put in this file
	import '../app.postcss';
	import { AppBar, AppShell, Modal } from '@skeletonlabs/skeleton';
	import Lazy from '../components/lazy.svelte';
	import Head from '../components/head.svelte';
</script>

<Head />
<AppShell regionPage="relative" slotPageHeader="sticky top-0 z-10">
	<svelte:fragment slot="pageHeader">
		<AppBar gridColumns="grid-cols-1" slotDefault="place-self-center">
			<h3 class="h3">Neil + Sandy</h3>
		</AppBar>
	</svelte:fragment>
	<Modal />
	<div class="p-8">
		<Lazy />
		<slot />
	</div>
</AppShell>
