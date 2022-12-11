<script>
	import { onMount } from 'svelte';
	import { db } from '$lib/firebaseConfig/firebase';
	import { collection, getDocs } from 'firebase/firestore';
	import { store } from '/src/stores/index';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import Quiz from '$lib/components/Card/Challenge/Quiz/index.svelte';
	import Hangman from '$lib/components/Card/Challenge/Hangman/Hangman.svelte';
	import GeoCatching from './Challenge/geoCaching/geoCaching.svelte';

	export let title = '';
	export let description = '';
	export let img = '';
	export let activity;
	export let links = [];
	export let id = '';
	export let envId = '';
	export let open = false;
	export let onClose;
	export let onChange;

	let activityOpen = false;

	const activityInformation = {
		completed: false,
		date: new Date().getTime(),
		cardId: id,
		envId: envId,
		type: activity?.type,
		succeeded: false,
		uid: $store.currentUser.uid,
		score: 0,
		maxScore: 0
	};
</script>

<LightBox {title} isOpen={open} close={onClose}>
	<img src={img?.url} alt={title} class="w-full mb-3 object-contain " style="height:300px" />
	<p class="max-h-32 mb-3 overflow-y-auto ">
		{description}
	</p>
	<div>
		{#each links as l}
			<div><a target="_blank" class="text-blue-500" href={l}>{l}</a></div>
		{/each}
	</div>
	<button
		on:click={() => {
			activityOpen = true;
		}}
		class="mt-auto w-full bg-c-black text-white text-xl p-2"
	>
		Challenge
	</button>
</LightBox>

<LightBox
	title={activity?.type}
	isOpen={activityOpen}
	close={() => {
		activityOpen = false;
	}}
>
	{#if activity?.type == 'Quiz'}
		<Quiz
			{activity}
			onSubmit={(resps) => {
				console.log('responses', resps);
			}}
		/>
	{:else if activity?.type == 'Hangman'}
		<Hangman {activity} {activityInformation} />
	{:else if activity?.type == 'GeoCatching'}
		<GeoCatching {activity} {activityInformation} />
	{/if}
</LightBox>
