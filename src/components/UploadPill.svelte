<script lang="ts">
	export let text: string;
	export let label: string = "";
	export let color: string | undefined = undefined;
	export let onclick: (() => void) | undefined = undefined;

	function handleClick(e: MouseEvent) {
		if (onclick) {
			e.stopPropagation();
			onclick();
		}
	}

	function handleKeypress(e: KeyboardEvent) {
		if ((e.key === "Enter" || e.key === " ") && onclick) {
			e.preventDefault();
			e.stopPropagation();
			onclick();
		}
	}
</script>

<div
	class="nav-file-tag system3-uploadpill"
	class:clickable={onclick}
	aria-label={label || undefined}
	role={onclick ? "button" : undefined}
	tabindex={onclick ? 0 : undefined}
	style={color ? `color:${color}` : ""}
	on:click={handleClick}
	on:keypress={handleKeypress}
>
	<span>{text}</span>
</div>

<style>
	.system3-uploadpill {
		text-wrap: nowrap;
	}
	.system3-uploadpill.clickable {
		cursor: pointer;
	}
	.system3-uploadpill.clickable:hover {
		background-color: var(--background-modifier-hover);
	}
</style>
