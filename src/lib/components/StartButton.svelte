<script lang="ts">
	import { isGlobeReady, isToneReady } from '$lib/stores/loadingStore';
	import ContinueButton from './ContinueButton.svelte';

	export let onClick: () => void;
</script>

<ContinueButton scrollTo="globe" canClick={$isToneReady} canScroll={$isGlobeReady} {onClick}>
	{#if $isToneReady}
		Start Experience
	{:else}
		Loading...
	{/if}
</ContinueButton>
