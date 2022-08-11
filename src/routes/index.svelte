<script lang="ts">
	import haversine from 'haversine-distance';
	let lat: number;
	let long: number;

	let latOld: number;
	let longOld: number;

	let total = 0;

	function getLocation() {
		if (navigator.geolocation) {
			console.log(
				navigator.geolocation.getCurrentPosition((position) => {
					latOld = lat;
					longOld = long;
					lat = position.coords.latitude;
					long = position.coords.longitude;
				})
			);
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
	}
</script>

{lat}
{long}

total: {total}

steps: {total / 0.8}

<button on:click={walk}>start</button>
<button on:click={stop}>stop</button>
