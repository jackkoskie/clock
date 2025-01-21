<script lang="ts">
	let { pageName = '', siteName, description = '', image = '', url } = $props();

	let title = $derived(`${pageName}${pageName === '' ? '' : ' | '}${siteName}`);
</script>

<svelte:head>
	<title>{title}</title>
	<meta charset="utf-8" />
	<meta name="description" content={description} />
	<meta name="robots" content="index, follow" />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:site" content="@jackkoskie" />
	<meta name="twitter:creator" content="@jackkoskie" />
	<meta name="twitter:title" content={title} />
	<meta name="twitter:description" content={description} />

	<meta property="og:type" content="article" />
	<meta property="og:title" content={title} />
	<meta property="og:description" content={description} />
	<meta property="og:url" content={url} />

	{#if image !== ''}
		<meta name="twitter:image" content={image} />
		<meta property="og:image" content={image} />
	{/if}
</svelte:head>
