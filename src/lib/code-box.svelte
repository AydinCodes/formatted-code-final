<script lang="ts">
	export let data: { isCode: boolean; text: string; language?: string };

	const copyCode = () => {
		navigator.clipboard.writeText(data.text);
	};
</script>

<div class="code">
	<div class="code-header flex flex-row justify-between items-center">
		{#if data.language}
			<p>{data.language}</p>
		{:else}
			<div />
		{/if}
		<button on:click={copyCode} class="btn flex btn-ghost items-center"
			><span class="ml-2">Copy code</span></button
		>
	</div>
	<div class="code-container">
		<code class="preformatted text-sm">{data.text}</code>
	</div>
</div>

<style>
	.code {
		width: 100%;
		margin: 1.5rem 0;
	}

	.code-header {
		background-color: var(--fallback-b3, oklch(var(--b3)));
		padding: 0.25rem 1rem;

		border-top-right-radius: 0.5rem;
		border-top-left-radius: 0.5rem;
	}

	.code-container {
		background-color: black;
		padding: 2rem 1.25rem;
		border-bottom-right-radius: 0.5rem;
		border-bottom-left-radius: 0.5rem;
	}

	.preformatted {
		white-space: pre-wrap;
		word-wrap: break-word;
		color: white;
	}
</style>
