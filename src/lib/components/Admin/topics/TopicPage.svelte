<script>
	import PreviewTopic from './PreviewT.svelte';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import EditTopic from './EditTopic.svelte';
	export let topics;
	export let onClick;

	export let lbOpen = false;
	export let selectedTopic;
	export let onChange;
	export let selectedEnvId;
</script>

{#each topics as t}
	<PreviewTopic
		{...t}
		onClick={() => {
			onClick(t.id);
			lbOpen = true;
		}}
	/>
{/each}

<LightBox isOpen={lbOpen} close={() => (lbOpen = false)}>
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
