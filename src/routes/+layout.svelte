<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main>
	<a
		href="/"
		class="btn absolute left-4 top-4 rounded px-4 py-2 text-white opacity-0 transition-all hover:opacity-100"
	>
		Home
	</a>
	{@render children()}
</main>
