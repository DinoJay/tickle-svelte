<script>
	// @ts-nocheck

	import PreviewCard from '$lib/components/cardView/utils/slider/PreviewCard.svelte';
	import * as d3 from 'd3';
	import { v4 as uuidv4 } from 'uuid';
	import LightBox from '$lib/components/utils/LightBox.svelte';

	import { collection, doc } from 'firebase/firestore';
	import { db } from '$lib/firebaseConfig/firebase';
	// import xmlJs from 'xml-js';

	import EditCard from '$lib/components/Admin/cards/EditCard.svelte';
	import xml2json from './xml2json';
	import Card from '$lib/components/Card/Card.svelte';

	// console.log('d3 xml', xml);
	let cardData = [];
	/**
	 * @type {{ id: string; title: any; description: string; img: { name: string; url: string; }; activity: null; topics: never[]; loc: { longitude: number; latitude: number; }; } | null}
	 */
	let currentCard = undefined;
	let selectedEnvironment = 'toxipedia';

	// onMount(() => {});

	const loadFile = () =>
		d3.xml('./opentoxipedia2018_modified.xml').then((xml) => {
			cardData = xml2json(xml).mediawiki.page;
			// console.log('xml', data);
		});

	// $: docRef = doc(db, 'card-envs', selectedEnvironment, 'cards', currentCard.id);
	// addDoc(collectionRef, currentElement).then((newDocRef) => {
	// 	updateDoc(newDocRef, {
	// 		id: newDocRef.id
	// 	});
	// 	currentElement.id = newDocRef.id;
	// });
	const createCards = () => {
		console.log('null');
		const collectionRef = collection(db, 'card-envs', selectedEnvironment, 'cards');
		console.log('data create', cardData);
		// console.log('data', data);
		cardData.map((d) => {
			console.log('ahe');
			const e = {
				id: d.title['#text'], //uuidv4(),
				title: d.title['#text'],
				description: d.definition['#text'],
				links: d.reference['#text'],
				img: { name: '', url: '' },
				activity: null,
				topics: [],
				loc: { longitude: 4.39, latitude: 50.82 }
			};
			return e;
			// addDoc(collectionRef, e);
		});
	};
</script>

<LightBox isOpen={!!currentCard} close={() => (currentCard = undefined)}>
	<EditCard {selectedEnvironment} {currentCard} />
</LightBox>

<div class="bg-white w-full  flex flex-col p-3" style="height:500px;width:450px">
	<h1 class="w-full text-xl">File Loader</h1>
	{#if cardData.length === 0}
		<div class="flex-grow w-full flex">
			<div class="m-auto">
				<input class="border-2 p-1" disabled bind:value={selectedEnvironment} />
				<button class="border-2  p-1 " on:click={loadFile}>Load File</button>
			</div>
		</div>
	{/if}
	<div class="flex flex-wrap overflow-y-auto">
		{#each cardData as d}
			<div
				class="flex-shrink-0 m-1"
				role="button"
				on:keypress={() => null}
				on:click={() => {
					currentCard = {
						id: 'null',
						title: d.title['#text'],
						description: d.definition['#text'],
						img: { name: '', url: '' },
						activity: null,
						topics: [],
						loc: { longitude: 4.39, latitude: 50.82 }
					};
				}}
			>
				<PreviewCard title={d.title['#text']} />
			</div>
		{/each}
	</div>
	{#if cardData.length > 0}
		<div class="flex mt-2">
			<button class="flex-grow p-1 text-lg border-2 z-50" on:click={createCards}
				>Create Cards</button
			>
		</div>
	{/if}
</div>
