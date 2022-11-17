<script>
	import EditQuestion from './EditQuestion.svelte';
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import Panel from '$lib/components/Admin/Panel.svelte';

	export let questions = [];
	export let onChange = () => {};

	$: console.log('questions', questions);
	/**
	 * Add a new question to questions
	 */
	const addQuestion = () => {
		questions = [...questions, { answers: [], img: { url: '', name: '' }, text: 'empty question' }];
		onChange();
	};

	/**
	 * Remove a question from questions
	 * @param question
	 */
	const removeQuestion = (question) => {
		questions.splice(questions.indexOf(question), 1);
		questions = [...questions];
		onChange();
	};
</script>

<div
	class="flex flex-col h-[65%] w-full mt-auto overflow-y-auto
		border border-c-gray"
>
	{#each questions as q}
		<Panel title={q.text}>
			<div class="flex h-[3rem] w-full mb-1 bg-c-light-gray ">
				{q.text}
			</div>
		</Panel>
	{/each}
</div>
<button class="w-full bg-c-light-gray hover:bg-c-gray" on:click={() => addQuestion()}>
	Add a question
</button>
