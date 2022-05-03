<script>
	import { scrollRef } from 'svelte-scrolling';
	import Globe from '../components/Globe.svelte';

	export let hasStarted = false;
</script>

<section use:scrollRef={'globe'}>
	<div class="grid">
		<Globe visible={hasStarted} />
		<div>
			<h2>The Great Lakes</h2>
			<p>
				Spanning more than 750 miles, the Great Lakes support an unfathomable number of people and
				ecosystems. The Great Lakes are one of the world’s largest surface freshwater ecosystems,
				containing 84% of North America's surface fresh water and about 21% of the world's supply of
				surface fresh water.
			</p>
			<p>The Great Lakes Basin supports more than 30 million people and</p>
			<p>
				<a class="source" href="https://www.epa.gov/greatlakes/facts-and-figures-about-great-lakes"
					>www.epa.gov</a
				>
			</p>
		</div>
	</div>
</section>

<style>
	.grid {
		display: grid;
		grid-template-columns: 1fr 1fr;
	}

	p {
		margin-right: 4rem;
	}
</style>
