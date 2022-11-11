<script>
	import { db } from '$lib/firebaseConfig/firebase';

	import { collection, doc } from 'firebase/firestore';
	import Panel from '$lib/components/Admin/Panel.svelte';
	import Environments from '$lib/components/Admin/environments/Environments.svelte';
	import Topics from '$lib/components/Admin/Topics/index.svelte';
	import Cards from '$lib/components/Admin/cards/index.svelte';
	import Map from '$lib/components/Admin/map/Map.svelte';
	import GeoCatchingValidation from '$lib/components/Admin/geoCaching/GeoCachingValidation.svelte';
	// @ts-ignore
	import { store, loadCardEnvironments, setEnvs } from '/src/stores/index';

	// uploads images : https://svelte.dev/repl/b17c13d4f1bb40799ccf09e0841ddd90?version=3.49.0

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

	let selectedCardId = null;
</script>

{#if $store?.currentUser?.admin}
	<div
		class="grid grid-cols-1 gap-3 m-2
        lg:grid-cols-2 lg:m-5"
	>
		<Panel title="Environments">
			<Environments
				{envs}
				{selectedEnv}
				onSelectEnv={(id) => (selectedEnvId = id)}
				onChange={(envs) => setEnvs(envs)}
			/>
		</Panel>

		<Panel title={'Topics'}>
			<Topics {selectedEnvId} />
		</Panel>

		<Panel title={'Cards'}>
			<Cards {selectedEnvId} />
		</Panel>

		<!-- <Panel
			title={`Map ${!selectedCard ? '- Please select a card!' : ''}`}
			selectedEnvironment={selectedCard}
		>
			<Map {selectedEnvironment} {selectedCard} />
		</Panel>

		<Panel title={'GeoCaching'}>
			<GeoCatchingValidation />
		</Panel> -->
	</div>
{:else}
	<div class="absolute top-[50%] left-[calc(50%-8rem)] h-12 w-64 text-center">
		You do not have the permission to acces this page.
	</div>
{/if}
