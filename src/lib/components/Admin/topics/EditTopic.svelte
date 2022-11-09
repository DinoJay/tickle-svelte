<script>
	import UploadFile from './../utils/edit/UploadFile.svelte';
	import { db } from '$lib/firebaseConfig/firebase';
	import { collection, doc, setDoc, deleteDoc } from 'firebase/firestore';

	export let currentTopic = {
		title: '',
		description: '',
		id: null,
		img: { name: '', url: '' },
		color: ''
	};
	export let onChange;
	export let onRemove;
	export let selectedEnvId;

	$: docRef = doc(db, 'card-envs', selectedEnvId, 'topics', currentTopic.id);

	const updateDb = (data) => {
		setDoc(docRef, data)
			.then((docRef) => {
				console.log(
					'Entire Document has been updated successfully',
					'currentTopicId'
					// currentTopic.id,
					// 'selectedenvid',
					// selectedEnvId
				);
			})
			.catch((error) => {
				console.log(error);
			});
	};
</script>

<form class="flex-grow flex flex-col" on:submit={(e) => e.preventDefault()}>
	<div class="mb-3 ">
		<div><label for="title">Title:</label></div>
		<input
			value={currentTopic.title}
			on:input={(e) => {
				const newTopic = { ...currentTopic, title: e.target.value };
				updateDb(newTopic);
				onChange(newTopic);
			}}
			class="w-full"
			name="title"
			placeholder="title"
		/>
	</div>
	<div>
		<div><label for="description">Description:</label></div>
		<textarea
			value={currentTopic.description}
			on:input={(e) => {
				const newTopic = { ...currentTopic, description: e.target.value };
				updateDb(newTopic);
				onChange(newTopic);
			}}
			class="border w-full"
			name="description"
			placeholder="description"
		/>
	</div>
	<div>
		<p>Image</p>
		<UploadFile
			url={currentTopic.img?.url}
			onChange={(url, name) => {
				const newEnv = { ...currentTopic, img: { name, url } };
				onChange(newEnv);
				updateDb(newEnv);
			}}
		/>
	</div>
	<div class="mt-auto">
		<button
			class="del-btn w-full"
			on:click={() => {
				deleteDoc(docRef);
				onRemove(currentTopic.id);
			}}>Delete</button
		>
	</div>
</form>

<style>
	input {
		@apply border-2 p-1;
	}
</style>
