<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf';
	import STooltip from './STooltip.svelte';

	const { Story } = defineMeta({
		title: 'Primitives/Tooltip',
		component: STooltip,
		tags: ['autodocs'],
	});
</script>

<Story name="Default" args={{ content: 'Hey you pretty little thing !' }} />
