<script lang="ts">
	import { onDestroy, onMount } from 'svelte';

	// shadcn
	import { Card } from '$lib/components/ui/card';
	import SEditorToolBar from './SEditorToolBar.svelte';

	// Extensions
	import { Editor } from '@tiptap/core';
	import { Markdown } from 'tiptap-markdown';
	import StarterKit from '@tiptap/starter-kit';
	import Underline from '@tiptap/extension-underline';
	import Placeholder from '@tiptap/extension-placeholder';

	let element: HTMLDivElement;
	let editor: Editor | null = $state(null);

	let transactionTracker = $state(Symbol());

	let {
		value = $bindable(''),
		placeholder = 'Write something interesting!',
	}: { value?: string; placeholder?: string } = $props();

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
				Markdown,
				Underline,
				Placeholder.configure({
					placeholder,
				}),
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
	{:else}
		<div class="h-[4rem] bg-muted"></div>
	{/if}

	<div bind:this={element}></div>
</Card>

<style>
	:global(.tiptap p.is-editor-empty:first-child::before) {
		color: #adb5bd;
		content: attr(data-placeholder);
		float: left;
		height: 0;
		pointer-events: none;
	}

	:global(.tiptap p.is-empty::before) {
		color: #adb5bd;
		content: attr(data-placeholder);
		float: left;
		height: 0;
		pointer-events: none;
	}
</style>
