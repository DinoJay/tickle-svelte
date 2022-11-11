<script>
	import Logo from '$lib/components/navigationBar/Logo.svelte';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import { db } from '$lib/firebaseConfig/firebase';
	import { collection, doc } from 'firebase/firestore';
	import TopicsCard from './edit/EditCardTopics/index.svelte';

	export let selectedEnvId;
	export let currentCard = {
		id: 'null',
		title: '',
		description: '',
		img: { name: '', url: '' },
		activity: null,
		topics: [],
		loc: { longitude: 4.39, latitude: 50.82 }
	};

	$: console.log('currentCard', currentCard.id);
	$: docRef = doc(db, 'card-envs', selectedEnvId, 'cards', currentCard.id);
	$: collectionRef = collection(db, 'card-envs', selectedEnvId, 'cards');

	const TITLE = 'title';
	const TOPICS = 'topic';
	const LINKS = 'links';
	const DESCR = 'descr';
	const ACTIVITY = 'activity';

	let selectedField;

	let openField;
</script>

<div class="flex flex-wrap">
	<button
		class="h-[36px] w-[40%] mt-[5%] mx-auto 
			border border-c-black 
			text-white bg-c-black hover:bg-c-dark-gray"
		on:click={() => (openTopics = true)}
	>
		Topics
	</button>
	<button
		class="h-[36px] w-[40%] mt-[3%] mx-auto 
			border border-c-black 
			text-white bg-c-black hover:bg-c-dark-gray"
		on:click={() => (openLinks = true)}
	>
		Links
	</button>

	<button
		class="h-[36px] w-[40%] mt-[3%] mx-auto 
			border border-c-black 
			text-white bg-c-black hover:bg-c-dark-gray"
		on:click={() => (openActivity = true)}
	>
		Activity
	</button>

	<LightBox isOpen={selectedField === TOPICS} close={() => (openTopics = false)}>
		<EditCardTopics {selectedEnvId} selectedCardId={currentCard.id} />
	</LightBox>

	<!-- <LightBox isOpen={openLinks} close={() => (openLinks = false)}>
		<EditLinks {selectedEnvironment} bind:currentCard {docRef} />
	</LightBox>
	<LightBox isOpen={openActivity} close={() => (openActivity = false)}>
		<ActivityCard {selectedEnvironment} bind:currentCard {docRef} />
	</LightBox> -->
</div>
