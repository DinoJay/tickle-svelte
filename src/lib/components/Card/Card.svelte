<script>
	import { onMount } from 'svelte';
	import { db } from '$lib/firebaseConfig/firebase';
	import { collection, doc, getDocs, setDoc } from 'firebase/firestore';
	import { store } from '/src/stores/index';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import Quiz from '$lib/components/Card/Challenge/Quiz/index.svelte';
	import Hangman from '$lib/components/Card/Challenge/Hangman/Hangman.svelte';
	import GeoCatching from './Challenge/geoCaching/geoCaching.svelte';
	import Activity from './Challenge/Activity.svelte';

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
	export let selectedEnvId;

	let activityOpen = false;

	$: uid = $store.currentUser.uid;
	$: activityInformation = {
		completed: false,
		date: new Date().getTime(),
		cardId: id,
		envId: envId,
		type: activity?.type,
		succeeded: false,
		uid,
		score: 0,
		maxScore: 0
	};

	$: console.log('activity', activity);
	$: console.log('links', links);
</script>

<LightBox {title} isOpen={open} close={onClose}>
	<img src={img?.url} alt={title} class="w-full mb-3 object-contain " style="height:300px" />
	<p class="max-h-32 mb-3 overflow-y-auto ">
		{description}
	</p>
	{#if links}
		<div>
			{#each links as l}
				<div><a target="_blank" class="text-blue-500" href={l}>{l}</a></div>
			{/each}
		</div>
	{/if}
	<button
		on:click={() => {
			activityOpen = true;
			console.log('click', activity, 'activityOpen', activityOpen);
		}}
		class="mt-auto w-full bg-c-black text-white text-xl p-2"
	>
		Challenge
	</button>
</LightBox>

<Activity
	open={activityOpen}
	onClose={() => {
		activityOpen = false;
	}}
	{activity}
	onSubmit={(response) => {
		const docRef = doc(db, 'card-envs', selectedEnvId, 'cards', id, 'activitySubmissions', uid);
		const ac = { ...activityInformation, response };
		console.log('submit', ac);
		setDoc(docRef, ac);
	}}
/>
