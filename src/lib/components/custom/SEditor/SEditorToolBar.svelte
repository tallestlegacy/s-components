<script lang="ts">
	import type { Editor } from '@tiptap/core';
	import {
		Bold,
		Check,
		Italic,
		List,
		ListOrdered,
		Strikethrough,
		TextQuote,
		Underline,
	} from 'lucide-svelte';

	import { Toggle } from '$lib/components/ui/toggle';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu';
	import STooltip from '../STooltip/STooltip.svelte';
	import Button from '$lib/components/ui/button/button.svelte';

	const { editor }: { editor: Editor } = $props();

	interface ToolBarOption {
		icon: any;
		label: string;
		isActive?: boolean;
		onclick: () => void;
	}

	const fontOptions = $derived([
		{
			icon: Bold,
			label: 'Bold',
			isActive: editor.isActive('bold'),
			onclick: () => editor.chain().focus().toggleBold().run(),
		},
		{
			icon: Italic,
			label: 'italic',
			isActive: editor.isActive('italic'),
			onclick: () => editor.chain().focus().toggleItalic().run(),
		},
		{
			icon: Underline,
			label: 'Underline',
			isActive: editor.isActive('underline'),
			onclick: () => editor.chain().focus().toggleUnderline().run(),
		},
		{
			icon: Strikethrough,
			label: 'Strike-Through',
			isActive: editor.isActive('strike'),
			onclick: () => editor.chain().focus().toggleStrike().run(),
		},
	]);

	const blockOptions = $derived([
		{
			icon: List,
			label: 'Bullet List',
			isActive: editor.isActive('bulletList'),
			onclick: () => editor.chain().focus().toggleBulletList().run(),
		},
		{
			icon: ListOrdered,
			label: 'Ordered List',
			isActive: editor.isActive('orderedList'),
			onclick: () => editor.chain().focus().toggleBulletList().run(),
		},
		{
			icon: TextQuote,
			label: 'Block Quote',
			isActive: editor.isActive('blockquote'),
			onclick: () => editor.chain().focus().toggleBlockquote().run(),
		},
	]);
</script>

<!-- generic toggle button with tooltip -->
{#snippet toolbarOption({ label, onclick, icon: Icon, isActive }: ToolBarOption)}
	<STooltip content={label}>
		<Toggle aria-label={label} {onclick} pressed={isActive} variant="outline" size="sm">
			<Icon />
		</Toggle>
	</STooltip>
{/snippet}

<!-- generic dropdown menu item -->
{#snippet dropdownOption(opts: { isActive: boolean; onclick: () => void; label: string })}
	<DropdownMenu.Item class="flex items-center justify-between gap-4" onclick={opts.onclick}>
		<span>
			{opts.label}
		</span>
		{#if opts.isActive}
			<Check />
		{/if}
	</DropdownMenu.Item>
{/snippet}

<!-- toggles for h1 to h6 -->
{#snippet headingOption(level: number)}
	{@render dropdownOption({
		isActive: editor.isActive('heading', { level }),
		onclick: () => editor.chain().focus().toggleHeading({ level }).run(),
		label: `Heading ${level}`,
	})}
{/snippet}

<div class="flex flex-wrap items-center justify-between gap-x-6 gap-y-2 border-b p-2">
	<DropdownMenu.Root>
		<DropdownMenu.Trigger>
			<Button variant="outline" size="sm">Heading ...</Button>
		</DropdownMenu.Trigger>
		<DropdownMenu.Content align="start">
			{#each Array(6) as _, index}
				{@render headingOption(index + 1)}
			{/each}
		</DropdownMenu.Content>
	</DropdownMenu.Root>

	<!-- font style -->
	<div class="flex gap-2">
		{#each fontOptions as option}
			{@render toolbarOption(option)}
		{/each}
	</div>

	<!-- lists -->
	<div class="flex gap-2">
		{#each blockOptions as option}
			{@render toolbarOption(option)}
		{/each}
	</div>
</div>
