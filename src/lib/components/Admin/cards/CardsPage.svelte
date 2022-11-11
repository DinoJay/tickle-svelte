<script>
	import EditCard from '$lib/components/Admin/cards/EditCard.svelte';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import PreviewCard from '$lib/components/PreviewCard.svelte';
	export let cards = [];
	export let selectedEnvId;
	let selectedCardId = null;

	$: selectedCard = cards.find((c) => c.id === selectedCardId);
	$: console.log('selectedCard', selectedCard);
</script>

<div
	class="flex flex-wrap gap-2 
        h-[35rem] overflow-auto"
>
	{#each cards as c}
		<PreviewCard {...c} onClick={() => (selectedCardId = c.id)} />
	{/each}
</div>

<LightBox
	isOpen={!!selectedCardId}
	title={selectedCard?.title}
	close={() => (selectedCardId = null)}
>
	<EditCard currentCard={selectedCard} {selectedEnvId} />
</LightBox>
