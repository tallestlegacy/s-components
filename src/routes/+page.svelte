<script>
	import SEditor from '$lib/components/custom/SEditor/SEditor.svelte';

	let myText = $state(` # Hello World
It's me, daniel `);
</script>

<div class="p-4">
	<SEditor bind:value={myText} />
</div>
