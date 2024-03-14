<script>
	import '../app.css';
	import Spacer from '../components/spacer.svelte';

	const playlist = {
		name: 'Driving',
		description: 'Pop jams for the card',
		author: 'Ari Vaniderstine',
		totalDuration: 30000,
		followerCount: 200,
		tracks: [
			{
				name: 'Eiei',
				duration: 10000,
				album: 'A_1',
				artist: 'Art_1',
				releaseDate: new Date()
			},
			{
				name: 'OuOu',
				duration: 20000,
				album: 'A_2',
				artist: 'Art_2',
				releaseDate: new Date()
			}
		]
	};
</script>

<div class="w-screen h-screen bg pt-5 pl-6 pr-9 text-white font-montserrat">
	<div class="flex gap-8">
		<div class="flex w-[300px] h-[300px] flex-wrap">
			<div class="w-[150px] h-[150px] bg-lime-500"></div>
			<div class="w-[150px] h-[150px] bg-red-500"></div>
			<div class="w-[150px] h-[150px] bg-indigo-500"></div>
			<div class="w-[150px] h-[150px] bg-cyan-500"></div>
		</div>

		<div class="flex flex-1 h-[300px] flex-col justify-between">
			<Spacer gap={'1.5rem'} />
			<h2 class="">Playlist</h2>
			<h1 class="text-6xl">{playlist.name}</h1>
			<h1>{playlist.description}</h1>
			<h1>
				Created by: <span class="font-bold">
					{playlist.author}
				</span>
				{playlist.tracks.length} songs, Add time converter later
			</h1>
			<div class="flex w-full justify-between">
				<div class="flex items-center gap-[0.875rem]">
					<button class="px-[3.125rem] py-4 bg-[#55ba54] rounded-full">PLAY</button>
					<button class="border-2 border-white rounded-full w-12 h-12 font-bold">...</button>
				</div>
				<div class="text-right">
					<h1>FOLLOWERS</h1>
					<h1>{playlist.followerCount}</h1>
				</div>
			</div>
		</div>
	</div>
</div>

<style>
	.bg {
		background: linear-gradient(#373737, #181818);
	}
</style>
