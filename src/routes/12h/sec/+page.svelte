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

<div class="flex h-screen w-screen items-center justify-center text-center">
	<h1 class="text-9xl font-bold text-white">
		{hours > 12 ? hours - 12 : hours == 0 ? 12 : hours}:{minutes < 10
			? '0' + minutes
			: minutes}:{seconds < 10 ? '0' + seconds : seconds}
		{ampm}
	</h1>
</div>
