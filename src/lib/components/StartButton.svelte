<script lang="ts">
	import { isGlobeReady, isToneReady } from '$lib/stores/loadingStore';

	// Only import what you need!
	import { Icon, ArrowDown } from 'svelte-hero-icons';
	import { scrollElement } from 'svelte-scrolling';

	export let onClick: () => void;

	const handleClick = () => {
		if (!$isToneReady) return;

		onClick();
		handleScroll();
	};

	const handleScroll = () => {
		if (!$isGlobeReady) {
			setTimeout(handleScroll, 100);
			return;
		} else {
			scrollElement('globe');
		}
	};
</script>

<button disabled={!$isToneReady} on:click={handleClick}>
	{#if $isToneReady}
		Start Experience <Icon src={ArrowDown} size="16" />
	{:else}
		Loading...
	{/if}
</button>

<style>
	button {
		padding: 1rem 2rem;
		width: 16rem;
		border-radius: 8px;
		border: 2px solid whitesmoke;
		background: transparent;

		z-index: 1;
		color: whitesmoke;

		margin-top: 2rem;

		transition: 0.2s;
		cursor: pointer;

		display: flex;
		align-items: center;
		justify-content: center;

		gap: 0.5rem;
	}

	button:hover {
		color: #194f16;
		background: whitesmoke;
	}

	button:disabled {
		color: whitesmoke;
		background: transparent;

		cursor: progress;
	}
</style>
