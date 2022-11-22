<script>
	import EditQuestion from './EditQuestion.svelte';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import Panel from '$lib/components/Admin/Panel.svelte';
	import { v4 as uuidv4 } from 'uuid';

	export let questions = [];
	export let onChange;

	const nq = { id: null, answers: [], img: { url: '', name: '' }, text: '' };
	let lb = false;
	let sq = null;

	const onQuestionChange = (q) => onChange([...questions, q]);
	console.log('questions', questions);
</script>

<div class="flex flex-col w-full  flex-grow overflow-y-auto  ">
	{#if questions.length === 0}
		<div class="m-auto text-xl text-gray-500">No Questions added!</div>
	{/if}
	{#each questions as q, i}
		<button class="crop p-2 border-2" on:click={() => (sq = q)}>
			<span class="font-bold">Q{i + 1}:</span><span class="ml-1">{q.text}</span>
		</button>
	{/each}
</div>
<button onClick={() => (lb = true)} class="w-full border-2 mt-auto p-2" on:click={() => (sq = nq)}>
	Add a question
</button>

<EditQuestion
	q={sq}
	onCreate={sq?.id === null ? () => onQuestionChange({ ...sq, id: uuidv4() }) : null}
	onChange={sq?.id !== null ? () => onQuestionChange : (q) => (sq = q)}
	onClose={() => (sq = null)}
/>
