<script>
	import SortIcon from '../../ui/SortIcon.svelte';
	import { safeExtractColumn } from './datatable.js';
	import { getContext } from 'svelte';
	import { propKey } from '@evidence-dev/component-utilities/chartContext';
	const props = getContext(propKey);

	export let rowNumbers = undefined;
	export let headerColor = undefined;
	export let headerFontColor = undefined;
	export let finalColumnOrder = undefined;
	export let columnSummary = undefined;
	export let sortable = undefined;
	export let sort = undefined;
	export let formatColumnTitles = undefined;
	export let sortBy = undefined;
	export let wrapTitles = undefined;
</script>

<thead>
	{#if $props.columns.length > 0}
		{@const columnsWithGroupSpan = $props.columns.map((column, index, array) => {
			// Determine if this column starts a new group or continues an existing one
			let isNewGroup = index === 0 || column.colGroup !== array[index - 1].colGroup;
			let span = 1;
			if (column.colGroup) {
				// Count how many contiguous columns have the same group
				for (let i = index + 1; i < array.length && array[i].colGroup === column.colGroup; i++) {
					span++;
				}
			}
			return { ...column, isNewGroup, span: isNewGroup ? span : 0 }; // Only assign span to the first column in a group
		})}
		{#if columnsWithGroupSpan.length > 0}
			<tr class="border-0" style:background-color={headerColor}>
				{#if rowNumbers}
					<th class="index w-[2%] px-[8px] py-[2px]" style:background-color={headerColor} />
				{/if}
				{#each columnsWithGroupSpan as column}
					{#if column.colGroup && column.isNewGroup}
						<th colspan={column.span} class="px-[2px] pt-1 align-bottom text-gray-900">
							<!-- Group header with dynamic colspan -->
							<div class=" border-b-[1px] border-b-gray-600 whitespace-normal pb-[2px]">
								{column.colGroup}
							</div>
						</th>
					{:else if column.colGroup}
						<!-- Not new group, th covered by previous column span-->
					{:else}
						<!-- Not part of a group - empty header cell -->
						<th></th>
					{/if}
				{/each}
			</tr>
		{/if}
	{/if}

	<tr class="border-b border-gray-600">
		{#if rowNumbers}
			<th class="index w-[2%] px-[8px] py-[2px]" style:background-color={headerColor} />
		{/if}
		{#if $props.columns.length > 0}
			{#each $props.columns.sort((a, b) => finalColumnOrder.indexOf(a.id) - finalColumnOrder.indexOf(b.id)) as column}
				<th
					class="{safeExtractColumn(column, columnSummary).type} px-[8px] py-[2px]"
					style:text-align={column.align}
					style:color={headerFontColor}
					style:background-color={headerColor}
					style:cursor={sortable ? 'pointer' : 'auto'}
					style:white-space={column.wrapTitle || wrapTitles ? 'normal' : 'nowrap'}
					on:click={sortable ? sort(column.id) : ''}
					style:vertical-align="bottom"
				>
					{column.title
						? column.title
						: formatColumnTitles
							? safeExtractColumn(column, columnSummary).title
							: safeExtractColumn(column, columnSummary).id}
					{#if sortBy.col === column.id}
						<SortIcon ascending={sortBy.ascending} />
					{/if}
				</th>
			{/each}
		{:else}
			{#each columnSummary
				.filter((d) => d.show === true)
				.sort((a, b) => finalColumnOrder.indexOf(a.id) - finalColumnOrder.indexOf(b.id)) as column}
				<th
					class="{column.type} px-[8px] py-[2px]"
					style:color={headerFontColor}
					style:background-color={headerColor}
					style:cursor={sortable ? 'pointer' : 'auto'}
					style:white-space={wrapTitles ? 'normal' : 'nowrap'}
					style:vertical-align="bottom"
					on:click={sortable ? sort(column.id) : ''}
				>
					<span class="col-header">
						{formatColumnTitles ? column.title : column.id}
					</span>
					{#if sortBy.col === column.id}
						<SortIcon ascending={sortBy.ascending} />
					{/if}
				</th>
			{/each}
		{/if}
	</tr>
</thead>

<style>
	th {
		white-space: nowrap;
		overflow: hidden;
	}

	th:first-child {
		padding-left: 3px;
	}

	.index {
		color: var(--grey-300);
		text-align: left;
		max-width: -moz-min-content;
		max-width: min-content;
	}

	.string {
		text-align: left;
	}

	.date {
		text-align: left;
	}

	.number {
		text-align: right;
	}

	.boolean {
		text-align: left;
	}
</style>
