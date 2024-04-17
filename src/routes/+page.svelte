<script lang="ts">
	import Scene from '$lib/scene.svelte';
	const splats = ['sedia', 'cactino'];
	let selector: HTMLSelectElement;

	let unique = {};

	function restart() {
		unique = {};
	}
</script>

<div class="absolute right-5 top-5 z-50">
	<select
		class="rounded-lg border p-2 font-semibold capitalize"
		on:change={restart}
		bind:this={selector}
		name="splat"
		id="splat"
	>
		{#each splats as splat}
			<option class="capitalize" value={splat}>{splat}</option>
		{/each}
	</select>
</div>

{#if selector}
	{#key unique}
		<Scene name={selector.value} />
	{/key}
{/if}
