<script lang="ts">
	import { isActive, type Editor } from '@tiptap/core';
	import 'iconify-icon';

	import { Toggle } from '$lib/components/ui/toggle';

	const { editor }: { editor: Editor } = $props();

	interface ToolBarOption {
		icon: string;
		label: string;
		isActive?: boolean;
		onclick: () => void;
	}

	const fontOptions = $derived([
		{
			icon: 'ph:text-b',
			label: 'Bold',
			isActive: editor.isActive('bold'),
			onclick: () => editor.chain().focus().toggleBold().run(),
		},
		{
			icon: 'ph:text-italic',
			label: 'italic',
			isActive: editor.isActive('italic'),
			onclick: () => editor.chain().focus().toggleItalic().run(),
		},
	]);

	const blockOptions = $derived([
		{
			icon: 'ph:list-dashes',
			label: 'Bullet List',
			isActive: editor.isActive('bulletList'),
			onclick: () => editor.chain().focus().toggleBulletList().run(),
		},
		{
			icon: 'ph:list-numbers',
			label: 'Ordered List',
			isActive: editor.isActive('orderedList'),
			onclick: () => editor.chain().focus().toggleBulletList().run(),
		},
	]);
</script>

{#snippet toolbarOption({ label, onclick, icon, isActive }: ToolBarOption)}
	<Toggle aria-label={label} {onclick} pressed={isActive} variant="outline" size="sm">
		<iconify-icon {icon}> </iconify-icon>
	</Toggle>
{/snippet}

<div class="flex items-center gap-2 border-b p-2">
	<!-- font style -->
	{#each fontOptions as option}
		{@render toolbarOption(option)}
	{/each}

	...

	{#each blockOptions as option}
		{@render toolbarOption(option)}
	{/each}
</div>
