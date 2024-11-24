<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf';
	import SEditor from './SEditor.svelte';

	const { Story } = defineMeta({
		title: 'Primitives/Editor',
		component: SEditor,
		tags: ['autodocs'],
	});
</script>

<Story name="Default" />
