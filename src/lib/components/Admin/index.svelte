<script>
	import { db } from '$lib/firebaseConfig/firebase';

	import { collection, doc, getDocs } from 'firebase/firestore';
	import Panel from '$lib/components/Admin/Panel.svelte';
	import Environments from '$lib/components/Admin/environments/Environments.svelte';
	import Topics from '$lib/components/Admin/Topics/index.svelte';
	import Cards from '$lib/components/Admin/cards/index.svelte';
	import Map from '$lib/components/Admin/Map/index.svelte';
	import GeoCatchingValidation from '$lib/components/Admin/geoCaching/GeoCachingValidation.svelte';
	// @ts-ignore
	import { store, loadCardEnvironments, setEnvs } from '/src/stores/index';

	//	import { updateCurrentUser } from 'firebase/auth';
	//uploads images : https://svelte.dev/repl/b17c13d4f1bb40799ccf09e0841ddd90?version=3.49.0

	/**
	 * selectedEnvironment - The current environment selected in the admin page
	 * selectedCard - The current card selected from an envrionment
	 */
	// toxin default environment change later
	loadCardEnvironments();
	let selectedEnvId = 'bnW56f62WWEJ0bwJwQ0m';
	$: envs = $store.envs;
	const changeSelectedEnv = (/** @type {string} */ id) => {
		selectedEnvId = id;
	};
	$: selectedEnv = envs.find((d) => d.id === selectedEnvId);
	$: console.log('envs', envs);
	let cards;

	let selectedCardId = null;

	$: {
		getDocs(collection(db, 'card-envs', selectedEnvId, 'cards')).then((snapRef) => {
			const cs = snapRef.docs.map((doc) => {
				return doc.data();
			});
			cards = cs;
		});
	}
	const setCards = (cs) => {
		console.log('setCards', cards);

		cards = cs;
	};
</script>

{#if $store?.currentUser?.admin}
	<div class="grid grid-cols-1 gap-3 m-2 lg:grid-cols-2 lg:m-5">
		<div>
			<Panel title="Environments">
				<Environments
					{envs}
					{selectedEnv}
					onSelectEnv={(id) => (selectedEnvId = id)}
					onChange={(envs) => setEnvs(envs)}
				/>
			</Panel>
		</div>

		<div>
			<Panel title={'Topics'}>
				<Topics {selectedEnvId} />
			</Panel>
		</div>

		<div>
			<Panel title={'Cards'}>
				<Cards {cards} {selectedEnvId} onChange={setCards} />
			</Panel>
		</div>

		<div>
			<Panel title={`Map`} height={'40rem'}>
				<Map {cards} {selectedEnvId} onChange={setCards} />
			</Panel>
		</div>
	</div>
{/if}
{#if !$store?.currentUser?.admin === null}
	<div class="absolute top-[50%] left-[calc(50%-8rem)] h-12 w-64 text-center">
		You do not have the permission to acces this page.
	</div>
{/if}

{#if $store?.currentUser?.admin === undefined}
	<div class="absolute top-[50%] left-[calc(50%-8rem)] h-12 w-64 text-center">Loading...</div>
{/if}
