<script lang="ts">
	import "../app.css"


	let coins = 0
</script>


<div class="min-w-screen min-h-screen h-full w-full 
flex flex-col justify-center items-center gap-10
bg-[#F27F3D]">

	<div class="text-4xl text-white border-b-2 border-white pb-10">
		<span class="text-6xl font-bold">{coins}</span> coins
	</div>


		<button
		class="py-2 px-8 rounded-full text-xl
				border-2 border-white text-white font-bold uppercase"
		>update coins</button>

</div>

