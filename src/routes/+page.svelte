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
		<div>
			<h1 class="text-4xl font-bold text-white">Welcome to Clock</h1>
			<h3 class="mb-4">Please select a clock option</h3>
			<div class="join">
				<a href="/12h" class="btn join-item">12h</a>
				<a href="/12h/sec" class="btn join-item">12h Seconds</a>
				<a href="/24h" class="btn join-item">24h</a>
				<a href="/24h/sec" class="btn join-item">24h Seconds</a>
			</div>
		</div>
	</div>
</main>
