<script>
	import PreviewTopic from './PreviewT.svelte';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import EditTopic from './EditTopic.svelte';
	import CreateTopic from './CreateTopic.svelte';
	/**
	 * @type {any[]}
	 */
	export let topics;
	/**
	 * @type {(arg0: any) => void}
	 */
	export let onClick;

	/**
	 * @type {{ title: any; id: any; }}
	 */
	export let selectedTopic;
	/**
	 * @type {(arg0: any) => void}
	 */
	export let onChange;
	/**
	 * @type {any}
	 */
	export let selectedEnvId;
	let lbOpen = false;
	let clbOpen = false;
</script>

<div class="flex flex-wrap gap-3">
	{#each topics as t}
		<PreviewTopic
			{...t}
			onClick={() => {
				onClick(t.id);
				lbOpen = true;
			}}
		/>
	{/each}
</div>
<button class="create-btn mt-auto mb-1" on:click={() => (clbOpen = true)}>Create Topic</button>

<LightBox title={selectedTopic?.title} isOpen={lbOpen} close={() => (lbOpen = false)}>
	<EditTopic
		{selectedEnvId}
		onChange={(t) => {
			const ts = topics.map((d) => {
				if (d.id === t.id) {
					return t;
				}
				return d;
			});
			onChange(ts);
		}}
		onRemove={(id) => {
			onChange(topics.filter((t) => t.id !== id));
			lbOpen = false;
		}}
		currentTopic={selectedTopic}
	/>
</LightBox>

<LightBox isOpen={clbOpen} close={() => (clbOpen = false)}
	><CreateTopic
		onChange={(nt) => {
			onChange([...topics, nt]);
			clbOpen = false;
		}}
		{selectedEnvId}
	/></LightBox
>
