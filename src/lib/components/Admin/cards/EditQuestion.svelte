<script>
	import LightBox from '$lib/components/utils/LightBox.svelte';
	import UploadFile from '../utils/edit/UploadFile.svelte';
	export let q;
	export let onChange;
	export let onCreate;
	export let onClose;

	$: textQ = q ? q.text : null;
	$: console.log('q', q);
	$: answers = q ? q.answers : [];
	let nw = '';
	$: console.log('q', q);
</script>

<LightBox title={textQ} isOpen={!!q} close={onClose}>
	<div class="flex flex-col flex-grow overflow-y-auto">
		<div class="mb-3">
			<UploadFile url={q.img?.url} />
		</div>
		<div>
			<h2 class="text-gray-500 form-text">Question:</h2>
			<textarea
				class="border-2 p-2 w-full"
				placeholder="please add question!"
				rows="4"
				value={textQ}
				on:input={(e) => onChange({ ...q, text: e.target.value })}
			/>
		</div>
		<div class="flex-grow flex flex-col">
			<h2 class="form-text">Answers:</h2>
			<ul>
				{#each answers as a}
					<li><span>{a.text}</span></li>
				{/each}
			</ul>
			<div class="flex">
				<input class="border-2 p-1 w-full" bind:value={nw} placeholder="Please add Answer" />
				<button
					class="create-btn"
					on:click={() => {
						onChange({ ...q, answers: [...q.answers, { text: nw, correct: false }] });
						nw = '';
					}}>Add</button
				>
			</div>
		</div>
		{#if onCreate !== null}
			<button class="create-btn mt-3" on:click={onCreate}>Add new Question</button>
		{/if}
	</div>
</LightBox>
