<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf';
	import SButton from './SButton.svelte';

	const { Story } = defineMeta({
		title: 'Primitives/Button',
		component: SButton,
		tags: ['autodocs'],
	});
</script>

<Story name="Default">Click me</Story>
