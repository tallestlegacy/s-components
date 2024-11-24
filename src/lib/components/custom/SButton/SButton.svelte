<script lang="ts">
	import Button from '$lib/components/ui/button/button.svelte';
	import type { Snippet } from 'svelte';

	interface ButtonProps {
		startIcon?: string;
		endIcon?: string;
		children?: Snippet;
		class?: string;
		loading?: boolean;
		disabled?: boolean;
	}

	let props: ButtonProps = $props();
</script>

{#snippet buttonIcon(iconName?: string)}
	{#if iconName}
		<iconify-icon icon={iconName}> </iconify-icon>
	{/if}
{/snippet}

<Button
	{...{
		disabled: props.disabled,
		class: props.class,
	}}
>
	{@render buttonIcon(props.startIcon)}
	{@render props.children?.()}
	{@render buttonIcon(props.endIcon)}

	<!-- render loading spinner if loading -->
	{@render buttonIcon(props.loading ? 'svg-spinners:ring-resize' : undefined)}
</Button>
