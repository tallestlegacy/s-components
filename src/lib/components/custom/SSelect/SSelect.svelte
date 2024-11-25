<script lang="ts">
	import { Combobox } from 'bits-ui';
	import { flyAndScale } from '$lib/utils';
	import { Check, ChevronsUpDown, Menu } from 'lucide-svelte';
	import { twMerge } from 'tailwind-merge';
	import type { InputValue } from '$lib/types/components';

	interface Option {
		value: InputValue;
		label: string;
	}
	type Options = Option[];

	// value to be bound
	let inputValue: string = $state('');

	// custom classes
	interface Props {
		placeholder?: string;
		name?: string;
		options?: Options;
		disabled?: boolean;
		value?: InputValue;
		class?: string;
		leading?: import('svelte').Snippet;
		trailing?: import('svelte').Snippet;
	}

	let {
		placeholder = 'Select ...',
		name = placeholder,
		options = [],
		disabled = false,
		value: selected = $bindable(''),
		class: className = '',
		leading,
		trailing,
	}: Props = $props();

	let touchedInput = $state(false);

	let filteredOptions = $derived(
		inputValue && touchedInput
			? options.filter((option) =>
					option.label.toLowerCase().includes(inputValue?.toString().toLowerCase() || ''),
				)
			: options,
	);
</script>

<Combobox.Root
	items={filteredOptions}
	bind:inputValue
	bind:touchedInput
	{disabled}
	selected={options.find((e) => e.value === selected)}
	onSelectedChange={(opt) => {
		selected = opt?.value || '';
	}}
>
	<div class={twMerge('relative w-full', className)}>
		<!-- leading -->
		<div class="absolute start-3 top-1/2 -translate-y-1/2 text-border">
			{#if leading}{@render leading()}{:else}
				<Menu class="size-4" />
			{/if}
		</div>

		<Combobox.Input
			class="border-border-input placeholder:text-foreground-alt/50 inline-flex h-[2.5rem] w-full truncate rounded-[9px] border bg-background px-11 text-sm transition-colors focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 focus:ring-offset-background
            {disabled ? 'bg-muted' : ''}"
			{placeholder}
			aria-label={placeholder}
		/>
		<!-- trailing -->
		<div class="absolute end-3 top-1/2 -translate-y-1/2 text-border">
			{#if trailing}{@render trailing()}{:else}
				<ChevronsUpDown class="size-4" />
			{/if}
		</div>
	</div>

	<Combobox.Content
		class="z-50 max-h-[320px] w-full overflow-auto rounded-xl border border-border bg-background px-1 py-3 shadow-popover outline-none"
		transition={flyAndScale}
		avoidCollisions
		sideOffset={8}
	>
		{#each filteredOptions as option (option.value)}
			<Combobox.Item
				class="flex h-10 w-full select-none items-center rounded-sm py-3 pl-5 pr-1.5 text-sm capitalize outline-none transition-all duration-75 data-[highlighted]:bg-muted"
				value={option.value}
				label={option.label}
			>
				{option.label}
				<Combobox.ItemIndicator class="ml-auto" asChild={false}>
					<Check class="size-4" />
				</Combobox.ItemIndicator>
			</Combobox.Item>
		{:else}
			<span class="block px-5 py-2 text-sm text-muted-foreground">No results found</span>
		{/each}
	</Combobox.Content>
	<Combobox.HiddenInput {name} />
</Combobox.Root>
