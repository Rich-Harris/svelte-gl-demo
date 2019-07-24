<script>
	import * as yootils from 'yootils';
	import { onMount } from 'svelte';
	import { Box, Material } from '@sveltejs/gl';
	import { logo } from './images.js';

	const material = {
		'u-color': [1, 1, 1]
	};

	const img = new Image();
	img.src = logo;

	let canvas;

	img.onload = () => {
		const aspect = img.naturalWidth / img.naturalHeight;
		const s = 512;

		const bitmap = yootils.createSprite(s, s, ctx => {
			ctx.fillStyle = 'white';
			ctx.fillRect(0, 0, s, s);

			ctx.strokeStyle = '#eee';
			ctx.lineWidth = 4;
			ctx.strokeRect(0, 0, s, s);

			const width = s * 0.7;
			const height = width / aspect;

			ctx.drawImage(
				img,
				(s - width) / 2,
				(s - height) / 2,
				width,
				height
			);
		});

		canvas = document.createElement('canvas');
		canvas.width = canvas.height = s * 4;

		// use this layout:
		//   T
		// L F R B
		//   B

		const ctx = canvas.getContext('2d');

		ctx.fillStyle = 'white';
		ctx.fillRect(0, 0, canvas.width, canvas.height);

		ctx.drawImage(bitmap, 1 * s, 0 * s); // top
		ctx.drawImage(bitmap, 0 * s, 1 * s); // left
		ctx.drawImage(bitmap, 1 * s, 1 * s); // front
		ctx.drawImage(bitmap, 2 * s, 1 * s); // right
		ctx.drawImage(bitmap, 3 * s, 1 * s); // back
		ctx.drawImage(bitmap, 1 * s, 2 * s); // bottom

		// ctx.fillStyle = 'white';
		// ctx.font = `bold ${s / 5}px sans-serif`;
		// ctx.textBaseline = 'middle';
		// ctx.textAlign = 'center';

		// ctx.fillText('top',    3/2 * s, 1/2 * s);
		// ctx.fillText('left',   1/2 * s, 3/2 * s);
		// ctx.fillText('front',  3/2 * s, 3/2 * s);
		// ctx.fillText('right',  5/2 * s, 3/2 * s);
		// ctx.fillText('back',   7/2 * s, 3/2 * s);
		// ctx.fillText('bottom', 3/2 * s, 5/2 * s);

		// material.set_image('map', canvas);

		// document.body.appendChild(canvas);
	};

	img.onerror = (event) => {
		console.error(event);
	};
</script>

<Box {...$$props} {...material} u-colormap={canvas}/>