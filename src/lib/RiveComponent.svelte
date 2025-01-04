<script lang="ts">
	import * as pkg from '@rive-app/webgl';
	const { Rive, RuntimeLoader } = pkg;
	import riveWASMResource from '@rive-app/webgl/rive.wasm?url';
	import { onMount } from 'svelte';
	import path from '$lib/game.riv?url';

	let canvas: HTMLCanvasElement;
	let riveInstance: any = $state();

	function onCleanup() {
		riveInstance.cleanup();
	}

	onMount(() => {
		RuntimeLoader.setWasmUrl(riveWASMResource);
		riveInstance = new Rive({
			src: path,
			canvas: canvas,
			autoplay: true,
			stateMachines: 'Game Machine',
			useOffscreenRenderer: true
		});
	});
</script>

<div class="canvas-wrapper">
	<canvas width={2000} height={2000} bind:this={canvas}></canvas>
</div>

<button onclick={onCleanup} class="clean-up-button">CALL CLEANUP</button>

<style>
	.canvas-wrapper {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		position: fixed;
		bottom: 210px;
		left: 0;
	}
	canvas {
		max-width: 800px;
		aspect-ratio: 1 / 1;
		display: flex;
		justify-content: center;
	}

	.clean-up-button {
		position: fixed;
		top: 100px;
		right: 20px;
		padding: 10px 20px;
		background-color: red;
		color: white;
		border: none;
		border-radius: 5px;
		cursor: pointer;
		z-index: 9999;
	}
</style>
