<script lang="ts">
	export let src: string;
	export let alt: string;
</script>

<img {src} {alt} />
<div />

<style>
	div {
		background: rgba(0, 0, 0, 0.2);
	}

	div,
	img {
		height: 100vh;
		width: 100vw;

		position: absolute;
		inset: 0;
		object-fit: cover;

		z-index: 0;

		pointer-events: none;
		-webkit-user-drag: none;
		user-drag: none;
		-moz-user-select: none;
		-webkit-user-select: none;
		-ms-user-select: none;
		user-select: none;
	}
</style>
