<script>
	import { onMount } from 'svelte';
	import { Scene, Material, AmbientLight, PointLight, DirectionalLight, OrbitControls, PerspectiveCamera, Group, Plane, Cone, Cube, Sphere, Overlay } from '@sveltejs/gl';
	import SvelteBox from './SvelteBox.svelte';
	import { logotype } from './images.js';

	const prime = 0xff3e00;
	const second = 0x676778;
	const flash = 0x40b3ff;

	let light = {};

	onMount(() => {
		let frame;

		const loop = () => {
			frame = requestAnimationFrame(loop);

			light.x = 3 * Math.sin(Date.now() * 0.001);
			light.y = 2.5 + 2 * Math.sin(Date.now() * 0.0004);
			light.z = 3 * Math.cos(Date.now() * 0.002);
		};

		loop();

		return () => cancelAnimationFrame(frame);
	});
</script>

<style>
	.info {
		position: absolute;
		top: 1em;
		left: 1em;
		background-color: rgba(255,255,255,0.7);
		padding: 1em;
		border-radius: 2px;
	}
</style>

<Scene>
	<OrbitControls maxPolarAngle={Math.PI / 2} let:location let:target>
		<PerspectiveCamera {location} {target} near={0.01}/>
	</OrbitControls>

	<AmbientLight intensity={0.3}/>
	<DirectionalLight direction={[-1,-1,-1]} intensity={0.5}/>

	<!-- moving light -->
	<Sphere location={[light.x,light.y + 0.2,light.z]} color={second} subdivisions={3} scale={0.1} />
	<PointLight location={[light.x,light.y,light.z]} color={second} intensity={0.6} />

	<!-- floor -->
	<Plane color={0xffffff} location={[0,-0.01,0]} rotation={[-90,0,0]} scale={10} />

	<!-- transparent balls -->
	<Sphere color={prime} alpha={0.7} subdivisions={4} scale={0.3} location={[0,0.3,0]} />
	<Sphere color={flash} alpha={0.7} subdivisions={4} scale={0.7} location={[-1,0.7,-1]} />
	<Sphere color={second} alpha={0.7} subdivisions={4} scale={0.2} location={[-0.8,0.2,0]} />

	<!-- logo boxes -->
	<SvelteBox location={[-2,0.5,0]} rotation={[0,-20,0]}/>
	<SvelteBox location={[1,0.75,-1]} rotation={[0,30,0]} scale={1.5}/>
</Scene>

<div class="info">
	<h1><a target="_blank" rel="noopener" href="https://github.com/sveltejs/gl">@sveltejs/gl</a></h1>
	<p>The code for this demo lives <a target="_blank" rel="noopener" href="https://github.com/rich-harris/svelte-gl-demo">here</a></p>
</div>