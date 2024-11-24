<script lang="ts">
	import { onDestroy, onMount } from 'svelte';

	// shadcn
	import { Card } from '$lib/components/ui/card';

	// Extensions
	import { Editor } from '@tiptap/core';
	import BulletList from '@tiptap/extension-bullet-list';
	import ListItem from '@tiptap/extension-list-item';
	import OrderedList from '@tiptap/extension-ordered-list';
	import SEditorToolBar from './SEditorToolBar.svelte';
	import StarterKit from '@tiptap/starter-kit';
	import { Markdown } from 'tiptap-markdown';

	let element: HTMLDivElement;
	let editor: Editor | null = $state(null);

	let transactionTracker = $state(Symbol());

	let { value = $bindable('') }: { value?: string } = $props();

	onMount(() => {
		editor = new Editor({
			element: element,
			editorProps: {
				attributes: {
					class: 'prose prose-sm p-4 max-w-none focus:outline-none',
				},
			},
			content: value,
			onTransaction: () => {
				// force re-render so `editor.isActive` works as expected
				editor = editor;
				transactionTracker = Symbol();
			},
			extensions: [
				StarterKit.configure({
					heading: {
						levels: [1, 2, 3, 4, 5, 6],
					},
				}),
				// Paragraph, Text,
				ListItem,
				BulletList,
				OrderedList,
				Markdown,
			],
		});

		editor.on('update', ({ editor }) => {
			value = editor.storage.markdown.getMarkdown();
		});
	});

	onDestroy(() => {
		if (editor) {
			editor.destroy();
		}
	});
</script>

<Card>
	{#if editor !== null}
		{#key transactionTracker}
			<SEditorToolBar {editor} />
		{/key}
	{/if}

	<div bind:this={element}></div>
</Card>
