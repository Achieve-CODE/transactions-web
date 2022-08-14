<script lang="ts">
	import '../app.css';
	import haversine from 'haversine-distance';

	let lat: number;
	let long: number;

	let latOld: number;
	let longOld: number;

	let total = 0;

	function getLocation() {
		if (navigator.geolocation) {
			navigator.geolocation.getCurrentPosition((position) => {
				latOld = lat;
				longOld = long;
				lat = position.coords.latitude;
				long = position.coords.longitude;
			});
		}
	}

	let timeout: any;

	function walk() {
		getLocation();
		total +=
			haversine({ latitude: latOld, longitude: longOld }, { latitude: lat, longitude: long }) || 0;
		timeout = setTimeout(() => {
			walk();
		}, 1000);
	}

	function stop() {
		clearTimeout(timeout);
		timeout = null;
	}

	function transaction() {
		if (total / 0.8 > 10) {
			fetch(
				`https://corsproxy.felixschaefer.access.ly/http://5.161.66.94:3000/transferToWallet?amount=${
					(total / 0.8) * 0.01
				}`,
				{
					headers: {
						method: 'GET'
					}
				}
			);
			total = 0;
		} else alert('Go run more.');
	}
</script>

<!-- Latitude: {lat}
Longitude: {long}
total: {total} -->

<div
	class="min-w-screen min-h-screen h-full w-full 
flex flex-col justify-center items-center gap-10
bg-[#F27F3D]"
>
	<div class="text-4xl text-white border-b-2 border-white pb-10">
		<span class="text-6xl font-bold">{Math.floor(total / 0.8)}</span> steps
	</div>

	<div class="flex flex-col gap-4">
		{#if timeout}
			<button
				on:click={stop}
				class="py-2 px-8 rounded-full text-xl
				border-2 border-white text-white font-bold uppercase">stop</button
			>
		{:else}
			<button
				on:click={walk}
				class="py-2 px-8 rounded-full text-xl
				border-2 border-white text-white font-bold uppercase">start</button
			>
		{/if}

		{#if total / 0.8 > 10}
			<button
				on:click={transaction}
				class="py-2 px-8 rounded-full text-xl
				border-2 border-white text-white font-bold uppercase">transaction</button
			>
		{/if}
	</div>
</div>
