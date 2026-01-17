<script lang="ts">
	import type Live from "../main";

	export let plugin: Live;
</script>

<div class="settings-footer">
	<div class="settings-footer-links">
		<a
			href="https://discord.system3.md"
			target="_blank"
			rel="noopener noreferrer"
		>
			Discord
		</a>
		<span class="settings-footer-separator">·</span>
		<a
			href="https://github.com/No-Instructions/Relay/issues"
			target="_blank"
			rel="noopener noreferrer"
		>
			Feedback
		</a>
		<span class="settings-footer-separator">·</span>
		<a
			href="https://relay.md"
			target="_blank"
			rel="noopener noreferrer"
		>
			relay.md
		</a>
	</div>
	{#if plugin.version}
		<div class="settings-footer-version">
			v{plugin.version}
		</div>
	{/if}
</div>

<style>
	.settings-footer {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 16px 20px;
		margin: 24px -100px -100px -100px;
		padding-left: 120px;
		padding-right: 120px;
		padding-bottom: 116px;
		background: var(--color-base-10);
	}

	.settings-footer-links {
		display: flex;
		align-items: center;
		gap: 8px;
		font-size: var(--font-ui-small);
	}

	.settings-footer-links a {
		color: var(--text-muted);
		text-decoration: none;
	}

	.settings-footer-links a:hover {
		color: var(--text-normal);
		text-decoration: underline;
	}

	.settings-footer-separator {
		color: var(--text-faint);
	}

	.settings-footer-version {
		font-size: var(--font-ui-smaller);
		color: var(--text-faint);
	}
</style>
