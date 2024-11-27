<script lang="ts">
	import { onMount } from 'svelte';
	import type { PageData } from './$types';

	let { data }: { data: PageData } = $props();

	let time = $state(new Date());
	let hours = $derived(time.getHours());
	let minutes = $derived(time.getMinutes());
	let seconds = $derived(time.getSeconds());

	onMount(() => {
		const interval = setInterval(() => {
			time = new Date();
		}, 1000);

		return () => clearInterval(interval);
	});
</script>

<div class="flex h-screen w-screen items-center justify-center text-center">
	<h1 class="text-9xl font-bold text-white">
		{hours < 10 ? '0' + hours : hours}:{minutes < 10 ? '0' + minutes : minutes}
	</h1>
</div>
