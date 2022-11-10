<script>
	import { db } from '$lib/firebaseConfig/firebase';
	import { doc, setDoc } from 'firebase/firestore';
	import { v4 as uuidv4 } from 'uuid';

	import UploadFile from './../../admin/utils/edit/UploadFile.svelte';
	$: id = uuidv4();
	let topic = { id: null, title: '', description: '', img: {} };
	/**
	 * @type {(arg0: { title: string; description: string; img: {}; }) => void}
	 */
	export let onChange;
	/**
	 * @type {string}
	 */
	export let selectedEnvId;

	$: docRef = doc(db, 'card-envs', selectedEnvId, 'topics', id);
	const updateDb = (/** @type {{ title: string; description: string; img: {}; }} */ data) => {
		setDoc(docRef, data)
			.then((docRef) => {
				console.log('Entire Document has been updated successfully');
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
			value={topic.title}
			on:input={(e) => {
				topic = { ...topic, id, title: e.target?.value };
			}}
			class="w-full"
			name="title"
			placeholder="title"
		/>
	</div>
	<div>
		<div><label for="description">Description:</label></div>
		<textarea
			value={topic.description}
			on:input={(e) => {
				topic = { ...topic, id, description: e.target?.value };
			}}
			class="border w-full"
			name="description"
			placeholder="description"
		/>
	</div>
	<div>
		<p>Image</p>
		<UploadFile
			url={topic.img?.url}
			onChange={(url, name) => {
				topic = { ...topic, id, img: { name, url } };
			}}
		/>
	</div>
	<div class="mt-auto ">
		<button
			class="create-btn w-full"
			on:click={() => {
				updateDb(topic);
				onChange(topic);
			}}>Create</button
		>
	</div>
</form>
