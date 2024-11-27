<script lang="ts">
	import { onMount } from 'svelte';
	import type { PageData } from './$types';

	let { data }: { data: PageData } = $props();

	let time = $state(new Date());
	let hours = $derived(time.getHours());
	let minutes = $derived(time.getMinutes());
	let seconds = $derived(time.getSeconds());
	let ampm = $derived(hours >= 12 ? 'PM' : 'AM');

	onMount(() => {
		const interval = setInterval(() => {
			time = new Date();
		}, 1000);

		return () => clearInterval(interval);
	});
</script>

<main class="container">
	<div class="flex h-screen w-screen items-center justify-center text-center">
		<h1 class="text-[15rem] font-bold text-white">
			{hours}:{minutes < 10 ? '0' + minutes : minutes}
			{ampm}
		</h1>
	</div>
</main>
