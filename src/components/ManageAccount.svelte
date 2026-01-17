<script lang="ts">
	import { createEventDispatcher } from "svelte";
	import { debounce } from "obsidian";
	import type { RelayUser } from "../Relay";
	import type Live from "../main";
	import Avatar from "./Avatar.svelte";
	import SettingItemHeading from "./SettingItemHeading.svelte";
	import SettingItem from "./SettingItem.svelte";
	import SettingGroup from "./SettingGroup.svelte";

	export let plugin: Live;
	export let user: RelayUser;

	const dispatch = createEventDispatcher();

	async function logout() {
		await plugin.loginManager.logout();
		dispatch("goBack", { clear: true });
	}
</script>

<div class="account-header">
	<Avatar size="64px" {user} />
	<div class="account-info">
		<div class="account-name">{user.name || "Unknown"}</div>
		<div class="account-email">{user.email || ""}</div>
	</div>
</div>

<SettingItemHeading name="Account"></SettingItemHeading>
<SettingGroup>
	<SettingItem
		name="Sign out"
		description="Sign out of your Relay account on this device."
	>
		<button
			on:click={debounce(() => {
				logout();
			})}
		>
			Sign out
		</button>
	</SettingItem>
</SettingGroup>

<style>
	.account-header {
		display: flex;
		align-items: center;
		gap: 16px;
		padding: 24px;
		margin-bottom: 16px;
	}

	.account-info {
		display: flex;
		flex-direction: column;
		gap: 4px;
		min-width: 0;
	}

	.account-name {
		font-size: 1.25em;
		font-weight: var(--font-semibold);
		color: var(--text-normal);
	}

	.account-email {
		color: var(--text-muted);
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
</style>
