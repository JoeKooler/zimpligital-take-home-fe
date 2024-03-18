<script lang="ts">
	import '../../app.css';
	import {
		Table,
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell,
		Checkbox,
		TableSearch,
		Spinner
	} from 'flowbite-svelte';

	type Track = {
		_id: string;
		title: string;
		duration: number;
		album: string;
		artist: string;
		releaseDate: string;
		coverImgSrc?: string;
	};

	type Playlist = {
		_id: string;
		name: string;
		description?: string;
		author: string;
		totalDuration: number;
		followerCount: number;
		tracks: Track[];
	};

	import { onMount } from 'svelte';
	import Spacer from '../../components/spacer.svelte';

	let data: Playlist | null = null;
	let forceRerender = 0;

	onMount(async () => {
		// I don't know the playlist selection screen so nevermind lol
		const response = await fetch('http://localhost:8080/myPlaylist');
		if (!response.ok) {
			throw new Error(`HTTP error! status: ${response.status}`);
		}
		data = await response.json();
		console.log('returned ', data);
	});

	let selectedIds: Record<string | number, boolean> = {};

	let searchTerm = '';

	async function removeTrackFromPlaylist(playlistId: string, trackId: string) {
		const response = await fetch('http://localhost:8080/myPlaylist/removeTrackFromMyPlaylist', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ playlistId, trackId })
		});

		if (!response.ok) {
			throw new Error(`HTTP error! status: ${response.status}`);
		}

		data = await response.json();
	}

	function convertToYYYYMMDD(dateString: string) {
		const date = new Date(dateString);
		const year = date.getFullYear();
		const month = String(date.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed in JavaScript
		const day = String(date.getDate()).padStart(2, '0');

		return `${year}-${month}-${day}`;
	}

	$: filteredItems = data?.tracks.filter(
		(item) => item.title.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1
	);
</script>

<div class="w-screen h-screen bg pt-5 pl-6 pr-9 text-white font-montserrat">
	{#if !data}
		<div class="flex w-full h-[calc(100vh-20px)] justify-center items-center">
			<Spinner size="20" />
		</div>
	{:else}
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
				<h1 class="text-6xl">{data?.name}</h1>
				<h1>{data?.description}</h1>
				<h1>
					Created by: <span class="font-bold">
						{data?.author}
					</span>
					{data?.tracks.length} songs, Add time converter later
				</h1>
				<div class="flex w-full justify-between">
					<div class="flex items-center gap-[0.875rem]">
						<button class="px-[3.125rem] py-4 bg-[#55ba54] rounded-full">PLAY</button>
						<button class="border-2 border-white rounded-full w-12 h-12 font-bold">...</button>
					</div>
					<div class="text-right">
						<h1>FOLLOWERS</h1>
						<h1>{data?.followerCount}</h1>
					</div>
				</div>
			</div>
		</div>

		<TableSearch
			placeholder="Filter"
			hoverable={true}
			bind:inputValue={searchTerm}
			color="custom"
			inputClass="bg-transparent border-none text-wgite text-sm rounded-lg w-80 p-2.5 ps-10"
		>
			<TableHead>
				<TableHeadCell class="w-[10%]"></TableHeadCell>
				<TableHeadCell class="w-[20%]">TITLE</TableHeadCell>
				<TableHeadCell class="w-[20%]">ARTIST</TableHeadCell>
				<TableHeadCell class="w-[20%]">ALBUM</TableHeadCell>
				<TableHeadCell class="w-[20%]">RELEASE DATE</TableHeadCell>
				<TableHeadCell class="w-[10%] text-right pr-0">DURATION</TableHeadCell>
			</TableHead>
			<TableBody>
				{#each filteredItems ?? [] as track}
					<TableBodyRow>
						<TableBodyCell class="text-right">
							<button
								on:click={() => removeTrackFromPlaylist(data?._id ?? '', track._id)}
								class="text-xl">-</button
							>
						</TableBodyCell>
						<TableBodyCell>
							{track.title}
						</TableBodyCell>
						<TableBodyCell>{track.artist}</TableBodyCell>
						<TableBodyCell>{track.album}</TableBodyCell>
						<TableBodyCell>{convertToYYYYMMDD(track.releaseDate)}</TableBodyCell>
						<TableBodyCell class="w-[10%] text-right pr-0">{track.duration}</TableBodyCell>
					</TableBodyRow>
				{/each}
			</TableBody>
		</TableSearch>
	{/if}
</div>

<style>
	.bg {
		background: linear-gradient(#373737, #181818);
	}
</style>
