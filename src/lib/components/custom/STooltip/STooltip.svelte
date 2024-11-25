<script lang="ts">
	import type { Snippet } from 'svelte';

	import { Info } from 'lucide-svelte';

	import { buttonVariants } from '$lib/components/ui/button/index.js';
	import * as Tooltip from '$lib/components/ui/tooltip/index.js';

	interface TooltipProps {
		children?: Snippet;
		content?: string | Snippet;
	}

	const { children, content }: TooltipProps = $props();
</script>

<Tooltip.Provider>
	<Tooltip.Root>
		{#if children}
			<Tooltip.Trigger>
				{@render children?.()}
			</Tooltip.Trigger>
		{:else}
			<Tooltip.Trigger>
				<Info />
			</Tooltip.Trigger>
		{/if}
		<Tooltip.Content>
			{#if typeof content === 'string'}
				{content}
			{:else}
				{@render content?.()}
			{/if}
		</Tooltip.Content>
	</Tooltip.Root>
</Tooltip.Provider>
