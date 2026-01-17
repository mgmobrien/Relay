<script lang="ts">
	import { createEventDispatcher } from "svelte";
	export let name: HTMLSpanElement | string = "";
	export let description: string = "";
	export let clickable: boolean = false;

	const dispatch = createEventDispatcher();

	function handleClick(e: MouseEvent) {
		if (clickable) {
			dispatch("click", e);
		}
	}

	function handleKeypress(e: KeyboardEvent) {
		if (clickable && (e.key === "Enter" || e.key === " ")) {
			dispatch("click", e);
		}
	}
</script>

<div
	class="setting-item mod-list-item"
	class:clickable
	role={clickable ? "button" : undefined}
	tabindex={clickable ? 0 : undefined}
	on:click={handleClick}
	on:keypress={handleKeypress}
>
	<div class="setting-item-info">
		<div class="setting-item-name truncate">
			<slot name="name">{name}</slot>
		</div>
		<slot name="description">
			{#if description}
				<div class="setting-item-description">{description}</div>
			{/if}
		</slot>
	</div>
	<div class="setting-item-control">
		<slot></slot>
	</div>
</div>

<style>
	.setting-item-control {
		width: unset !important;
		margin-top: unset !important;
	}
	.setting-item {
		flex-direction: unset !important;
	}
	.setting-item.clickable {
		margin-left: -12px;
		margin-right: -12px;
		padding-left: 12px;
		padding-right: 12px;
		border-radius: var(--radius-s);
	}
	.setting-item.clickable:hover {
		background-color: var(--background-modifier-hover);
	}
	.setting-item-info {
		min-width: 0;
		margin-top: auto;
		margin-bottom: auto;
	}
	.setting-item-name {
		min-width: 0;
	}
	.truncate :global(*) {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>
