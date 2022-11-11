<script>
	import { collection, doc, getDocs } from 'firebase/firestore';
	import { db } from '$lib/firebaseConfig/firebase';
	import CardsPage from './CardsPage.svelte';

	export let selectedEnvId;

	let loading = false;
	let cards = [];
	// let currentCard = {
	// 	id: 'null',
	// 	title: '',
	// 	description: '',
	// 	img: { name: '', url: '' },
	// 	activity: null,
	// 	topics: [],
	// 	loc: { longitude: 4.39, latitude: 50.82 }
	// };

	async function loadCards() {
		loading = true;
		let snapRef = await getDocs(collection(db, 'card-envs', selectedEnvId, 'cards'));
		cards = snapRef.docs.map((doc) => {
			return doc.data();
		});
		loading = false;
	}

	$: loadCards();

	let lbStaticLoader = false;
</script>

<div class="flex flex-col flex-grow">
	{#if loading}
		<div>Loading...</div>
	{/if}
	{#if !loading}
		<CardsPage {cards} {selectedEnvId} onChange={(cs) => (cards = cs)} />
	{/if}
</div>
