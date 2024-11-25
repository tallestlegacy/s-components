<script>
	import SEditor from '$lib/components/custom/SEditor/SEditor.svelte';
	import SSelect from '$lib/components/custom/SSelect/SSelect.svelte';

	let myText = $state('');
</script>

<div class="mx-auto max-w-screen-lg p-4">
	<SEditor bind:value={myText} />
</div>
