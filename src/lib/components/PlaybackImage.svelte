<script lang="ts">
	export let src: string;
	export let alt: string;

	export let onClick: () => void;
</script>

<button on:click={onClick}>
	<img {src} {alt} />
</button>

<style>
	button {
		border: 5px solid transparent;
		background: none;
		padding: 0;
	}

	button:hover {
		border: 5px solid whitesmoke;
	}

	img {
		height: 100%;
		width: 100%;
		aspect-ratio: 1;
		object-fit: cover;
	}
</style>
