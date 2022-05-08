<script lang="ts">
	// Only import what you need!
	import { Icon, ExternalLink } from 'svelte-hero-icons';

	export let href: string;
</script>

<a {href}>
	<slot />
	<Icon src={ExternalLink} size="16" />
</a>

<style>
	a {
		padding: 1rem 2rem;
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

		text-decoration: none;

		gap: 0.5rem;
	}

	a:hover {
		color: #194f16;
		background: whitesmoke;
	}

	a:disabled {
		color: whitesmoke;
		background: transparent;

		cursor: progress;
	}
</style>
