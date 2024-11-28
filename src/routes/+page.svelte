<script lang="ts">
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import type { PageData } from './$types';
	import { goto } from '$app/navigation';
	import ColorPicker from 'svelte-awesome-color-picker';

	let { data }: { data: PageData } = $props();

	let twentyfour = $state(false);
	let sec = $state(false);
	let hex = $state('#1d232a');
	let showMenu = $state(true);

	if ($page.url.searchParams.get('24') === 'true') twentyfour = true;
	if ($page.url.searchParams.get('sec') === 'true') sec = true;
	if ($page.url.searchParams.get('hex')) hex = $page.url.searchParams.get('hex') as string;
	if ($page.url.searchParams.get('hide') === 'true') showMenu = false;

	const toggleHidden = () => {
		showMenu = !showMenu;

		let query = new URLSearchParams($page.url.searchParams.toString());
		query.set('hide', (!showMenu).toString());
		goto(`?${query.toString()}`);
	};

	const toggleTwentyFour = () => {
		twentyfour = !twentyfour;

		let query = new URLSearchParams($page.url.searchParams.toString());
		query.set('24', twentyfour.toString());
		goto(`?${query.toString()}`);
	};

	const toggleSec = () => {
		sec = !sec;
		$page.url.searchParams.set('sec', sec.toString());
		let query = new URLSearchParams($page.url.searchParams.toString());
		query.set('sec', sec.toString());
		goto(`?${query.toString()}`);
	};

	$effect(() => {
		if (hex === '#1d232a') return;
		let query = new URLSearchParams($page.url.searchParams.toString());
		query.set('hex', hex.toString());
		goto(`?${query.toString()}`);
	});

	let time = $state(new Date());

	let hours = $derived(time.getHours());
	let minutes = $derived(time.getMinutes());
	let seconds = $derived(time.getSeconds());
	let ampm = $derived(hours >= 12 ? 'PM' : 'AM');

	let text = $derived(
		`${twentyfour ? (hours < 10 ? '0' + hours : hours) : hours > 12 ? hours - 12 : hours}:${minutes < 10 ? '0' + minutes : minutes}${sec ? ':' + (seconds < 10 ? '0' + seconds : seconds) : ''}${twentyfour ? '' : ' ' + ampm}`
	);

	const reset = () => {
		twentyfour = false;
		sec = false;
		hex = '#1d232a';
		showMenu = true;
		goto('/');
	};

	const colorIsDark = (bgColor: string) => {
		let color = bgColor.charAt(0) === '#' ? bgColor.substring(1, 7) : bgColor;
		let r = parseInt(color.substring(0, 2), 16); // hexToR
		let g = parseInt(color.substring(2, 4), 16); // hexToG
		let b = parseInt(color.substring(4, 6), 16); // hexToB
		let uicolors = [r / 255, g / 255, b / 255];
		let c = uicolors.map((col) => {
			if (col <= 0.03928) {
				return col / 12.92;
			}
			return Math.pow((col + 0.055) / 1.055, 2.4);
		});
		let L = 0.2126 * c[0] + 0.7152 * c[1] + 0.0722 * c[2];
		return L <= 0.179;
	};

	onMount(() => {
		const interval = setInterval(() => {
			time = new Date();
		}, 1000);

		console.info('Welcome to the Clock!\n\nMade by Jack Koskie');

		return () => clearInterval(interval);
	});
</script>

<div
	class="join absolute left-4 top-4 rounded px-4 py-2 transition-opacity {showMenu
		? ''
		: 'opacity-0'} hover:opacity-100"
>
	<button onclick={toggleTwentyFour} class="btn join-item">{twentyfour ? '12h' : '24h'}</button>
	<button onclick={toggleSec} class="btn join-item">{sec ? 'Hide Seconds' : 'Show Seconds'}</button>
	<div class="dark btn join-item"><ColorPicker bind:hex position="responsive" /></div>
	<button onclick={reset} class="btn join-item">Reset</button>
	<a href="https://github.com/jackkoskie/clock" target="_blank" class="btn join-item">GitHub</a>
	<button onclick={toggleHidden} class="btn join-item">
		{showMenu ? 'Hide' : 'Show'} Menu
	</button>
</div>
<div
	class="flex h-screen w-screen items-center justify-center text-center"
	style="background-color: {hex};"
>
	<h1 class="text- text-9xl font-bold {colorIsDark(hex) ? 'text-white' : 'text-black'}">
		{text}
	</h1>
</div>

<style>
	.dark {
		--cp-bg-color: #333;
		/* --cp-border-color: white; */
		/* --cp-text-color: white; */
		--cp-input-color: #555;
		/* --cp-button-hover-color: #777; */
	}
</style>
