<script lang="ts">
	import '../app.scss';
	import barefoot from '$lib/assets/PPPAAA.wav';
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { dev } from '$app/environment';
	import { goto } from '$app/navigation';

	const PLAY_INTRO_DELAY = 1300;
	let audio: HTMLAudioElement = $state(null);
	let { children } = $props();
	let paused = $state(true);
	let playingIntro = $state(dev);

	onMount(() => {
		audio.volume = 0.3;
		if (!dev) {
			goto('/');
		}
	});
</script>

<div class="app relative flex flex-col overflow-hidden">
	{#if !dev}
		<audio
			bind:this={audio}
			src={barefoot}
			bind:paused={paused}
		></audio>
	{/if}

	{#if paused && !dev}
		<button
			out:fade={{ duration: PLAY_INTRO_DELAY }}
			class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white rounded-full px-6 py-3 tracking-wide font-bold w-fit uppercase text-xs"
			onclick={() => {
				paused = false;
				setTimeout(() => {
					playingIntro = true;
				}, PLAY_INTRO_DELAY);
			}}
		>
			start
		</button>
	{:else if playingIntro}
		{@render children()}
	{/if}
</div>

<style>
    .app {
        height: 100dvh;
        width: 100dvw;
    }
</style>
