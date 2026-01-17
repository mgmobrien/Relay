<script lang="ts">
	import { ChevronRight, Satellite, Folder, FolderLock } from "lucide-svelte";
	import type { Relay, RemoteSharedFolder, RelayUser } from "../Relay";
	import type { SharedFolder } from "../SharedFolder";
	import { createEventDispatcher } from "svelte";
	import RelayText from "./RelayText.svelte";
	import Avatar from "./Avatar.svelte";

	export let relay: Relay | undefined = undefined;
	export let remoteFolder: RemoteSharedFolder | undefined = undefined;
	export let sharedFolder: SharedFolder | undefined = undefined;
	export let user: RelayUser | undefined = undefined;

	const dispatch = createEventDispatcher();

	function goHome() {
		dispatch("goBack", { clear: true });
	}

	function goToRelay() {
		dispatch("goToRelay", { relay: relay || remoteFolder?.relay });
	}

	function goToAccount() {
		dispatch("goToAccount");
	}
</script>

<div class="settings-header">
	<div class="settings-header-left">
		<button
			class="settings-header-home"
			on:click={goHome}
			aria-label="Go to Relay home"
		>
			<RelayText size={48} />
		</button>

		{#if relay || remoteFolder || sharedFolder}
			<ChevronRight size={16} class="settings-header-separator" />
		{/if}

		{#if relay && !remoteFolder && !sharedFolder}
			<!-- On relay page -->
			<span class="settings-header-current">
				<Satellite size={16} class="settings-header-icon" />
				{relay.name || "(Untitled relay)"}
			</span>
		{:else if relay || remoteFolder?.relay}
			<!-- On folder page, show relay as clickable -->
			<button class="settings-header-crumb" on:click={goToRelay}>
				<Satellite size={16} class="settings-header-icon" />
				{(relay || remoteFolder?.relay)?.name || "(Untitled relay)"}
			</button>
			<ChevronRight size={16} class="settings-header-separator" />
			<span class="settings-header-current">
				{#if remoteFolder?.private}
					<FolderLock size={16} class="settings-header-icon" />
				{:else}
					<Folder size={16} class="settings-header-icon" />
				{/if}
				{#if remoteFolder}
					{remoteFolder.name || "(Untitled channel)"}
				{:else if sharedFolder}
					{sharedFolder.name}
				{/if}
			</span>
		{:else if sharedFolder}
			<!-- Orphan shared folder (no relay) -->
			<span class="settings-header-current">
				<Folder size={16} class="settings-header-icon" />
				{sharedFolder.name}
			</span>
		{/if}
	</div>

	{#if user}
		<button
			class="settings-header-account"
			on:click={goToAccount}
			aria-label="Account settings"
		>
			<Avatar size="28px" {user} />
		</button>
	{/if}
</div>

<style>
	.settings-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 12px 20px 6px 20px;
		margin-bottom: 8px;
		position: sticky;
		top: 0;
		background: var(--background-primary);
		z-index: 10;
		box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
	}

	.settings-header :global(.settings-header-icon) {
		flex-shrink: 0;
		margin-right: 4px;
	}

	.settings-header-left {
		display: flex;
		align-items: center;
		gap: 8px;
		min-width: 0;
		flex: 1;
	}

	.settings-header-home {
		display: flex;
		align-items: center;
		justify-content: center;
		background: none;
		border: none;
		padding: 4px;
		border-radius: var(--radius-s);
		cursor: pointer;
		box-shadow: none;
		flex-shrink: 0;
	}

	.settings-header-home:hover {
		background: var(--background-modifier-hover);
	}

	.settings-header :global(.settings-header-separator) {
		color: var(--text-faint);
		flex-shrink: 0;
	}

	.settings-header-crumb {
		display: flex;
		align-items: center;
		background: none;
		border: none;
		padding: 4px 8px;
		border-radius: var(--radius-s);
		cursor: pointer;
		color: var(--text-muted);
		font-size: inherit;
		box-shadow: none;
		white-space: nowrap;
	}

	.settings-header-crumb:hover {
		background: var(--background-modifier-hover);
		color: var(--text-normal);
	}

	.settings-header-current {
		display: flex;
		align-items: center;
		color: var(--text-normal);
		font-weight: var(--font-medium);
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.settings-header-account {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 4px;
		border-radius: var(--radius-s);
		background: none;
		border: none;
		box-shadow: none;
	}

	.settings-header-account:hover {
		background: var(--background-modifier-hover);
	}
</style>
