<script lang="ts">
	// Only import what you need!
	import { Icon, ArrowDown } from 'svelte-hero-icons';
	import { scrollElement } from 'svelte-scrolling';

	export let onClick: () => void;
	export let scrollTo: string;
	export let canClick: boolean = true;
	export let canScroll: boolean = true;

	const handleClick = () => {
		if (!canClick) return;

		onClick();
		handleScroll();
	};

	let hasDelayed = false;

	const handleScroll = () => {
		if (canScroll && hasDelayed) {
			scrollElement(scrollTo);
		} else {
			hasDelayed = true;
			setTimeout(handleScroll, 100);
		}
	};
</script>

<button disabled={!canClick} on:click={handleClick}>
	<slot />
	{#if canClick}
		<Icon src={ArrowDown} size="16" />
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
