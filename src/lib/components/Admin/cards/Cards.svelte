<script>
	import { collection, doc, getDocs } from 'firebase/firestore';
	import { db } from '$lib/firebaseConfig/firebase';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import Card from './EditCard.svelte';
	import AddButton from '$lib/components/Admin/utils/AddButton.svelte';
	import Block from '$lib/components/Admin/utils/block/Block.svelte';
	import Content from '$lib/components/Admin/utils/block/BlockContent.svelte';
	import StaticLoader from '$lib/components/Admin/environments/StaticLoader.svelte';

	export let addButtonContent = '';
	export let selectedEnvironment = null;
	export /**
	 * @type {null}
	 */
	let selectedCard = null;

	let cards = [];
	let currentCard = {
		id: 'null',
		title: '',
		description: '',
		img: { name: '', url: '' },
		activity: null,
		topics: [],
		loc: { longitude: 4.39, latitude: 50.82 }
	};
	let isLightBoxOpen = false;

	/**
	 * Function used to get all the cards of the selectedEnvironment from Firebase
	 */
	async function getCards() {
		let snapRef = await getDocs(collection(db, 'card-envs', selectedEnvironment, 'cards'));
		cards = snapRef.docs.map((doc) => {
			return doc.data();
		});
	}

	/**
	 * Each times the selectedEnvironment or the lightBox are updated we check for new environments
	 */
	$: if (selectedEnvironment || isLightBoxOpen) {
		getCards();
	}

	/**
	 * When we change the environment we reset the selectedCard
	 */
	$: if (selectedEnvironment) {
		selectedCard = null;
	}

	let lbStaticLoader = false;
</script>

<div
	class="flex flex-wrap gap-2 
        h-[35rem] overflow-auto"
>
	{#each cards as card}
		<Block
			onClick={() => {
				cards.splice(cards.indexOf(card), 1);
				cards = [...cards];
			}}
			ref={doc(db, 'card-envs', selectedEnvironment, 'cards', card.id)}
			isCard={true}
		>
			<Content
				element={card}
				bind:selectedElement={selectedCard}
				bind:currentElement={currentCard}
				bind:isLightBoxOpen
				isCard={true}
			/>
		</Block>
	{:else}
		<p
			class="w-full mt-5 text-center
				font-medium uppercase"
		>
			Empty
		</p>
	{/each}
</div>

<div>
	<button
		on:click={() => {
			isLightBoxOpen = true;
			currentCard = {
				id: 'null',
				title: '',
				description: '',
				img: { name: '', url: '' },
				activity: null,
				topics: [],
				loc: { longitude: 4.39, latitude: 50.82 }
			};
		}}
	>
		Add new Card
	</button>
	<button on:click={() => (lbStaticLoader = true)}>Load static file</button>
</div>

<LightBox isOpen={isLightBoxOpen} close={() => (isLightBoxOpen = false)}>
	<Card {selectedEnvironment} {currentCard} />
</LightBox>

<LightBox isOpen={lbStaticLoader} close={() => (lbStaticLoader = false)}>
	<StaticLoader {selectedEnvironment} />
</LightBox>
