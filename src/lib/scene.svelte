<script lang="ts">
	import { browser, dev } from '$app/environment';
	import { base } from '$app/paths';
	import * as SPLAT from 'gsplat';
	import { onDestroy, onMount } from 'svelte';
	export let name = 'sedia';

	let renderer: SPLAT.WebGLRenderer;
	let scene: SPLAT.Scene;
	let camera: SPLAT.Camera;
	let controls: SPLAT.OrbitControls;
	const url = dev ? `/splats/${name}.splat` : `${base}/splats/${name}.splat`;

	onMount(async () => {
		init_gsplat();
		const frame = () => {
			controls.update();
			renderer.render(scene, camera);
			requestAnimationFrame(frame);
		};
		requestAnimationFrame(frame);
	});

	const init_gsplat = async () => {
		if (browser) {
			scene = new SPLAT.Scene();
			camera = new SPLAT.Camera();
			renderer = new SPLAT.WebGLRenderer();
			controls = new SPLAT.OrbitControls(camera, renderer.canvas);

			handleResize();

			const splat = await SPLAT.Loader.LoadAsync(url, scene, () => {});

			window.addEventListener('resize', handleResize);
		}
	};

	const handleResize = () => {
		renderer.setSize(window.innerWidth, window.innerHeight);
	};

	onDestroy(() => {
		window.removeEventListener('resize', handleResize);
		scene.reset();
		renderer.canvas.remove();
	});
</script>
