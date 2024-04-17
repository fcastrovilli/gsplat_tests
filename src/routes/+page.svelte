<script lang="ts">
	import { dev } from '$app/environment';
	import * as SPLAT from 'gsplat';
	import { onMount } from 'svelte';

	onMount(async () => {
		const scene = new SPLAT.Scene();
		const camera = new SPLAT.Camera();
		const renderer = new SPLAT.WebGLRenderer();
		const controls = new SPLAT.OrbitControls(camera, renderer.canvas);
		renderer.setSize(window.innerWidth, window.innerHeight);
		const url = dev ? '/splats/sedia.splat' : process.env.BASE_URL + '/splats/sedia.splat';
		await SPLAT.Loader.LoadAsync(url, scene, () => {});
		const frame = () => {
			controls.update();
			renderer.render(scene, camera);
			requestAnimationFrame(frame);
		};
		requestAnimationFrame(frame);
	});
</script>
