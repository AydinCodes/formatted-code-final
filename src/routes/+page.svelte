<script lang="ts">
	import OpenAI from 'openai';
	import { PUBLIC_OPENAI_API_KEY } from '$env/static/public';
	import type { ChatCompletionMessage } from 'openai/resources/index.mjs';
	import { splitCodeFromText } from '$lib';
	import CodeBox from '$lib/code-box.svelte';

	let message: string = '';
	let result: ChatCompletionMessage;
	let loading: boolean = false;
	let processedChat: Array<{ isCode: boolean; text: string; language?: string }>;

	const openai = new OpenAI({ apiKey: PUBLIC_OPENAI_API_KEY, dangerouslyAllowBrowser: true });

	async function getResult() {
		loading = true;
		const completion = await openai.chat.completions.create({
			messages: [
				{ role: 'system', content: 'You are a helpful assistant.' },
				{ role: 'user', content: message }
			],
			model: 'gpt-3.5-turbo'
		});

		result = completion.choices[0].message;
		if (result.content) {
			processedChat = splitCodeFromText(result.content);
		}
		console.log(processedChat);
		loading = false;
		message = '';
	}
</script>

<div class="flex flex-col items-center p-[5rem] justify-between h-[100vh]">
	<div class="px-[2rem] w-[40%]">
		{#if loading}
			<p>Loading...</p>
		{/if}
		{#if processedChat}
			{#each processedChat as part}
				{#if part.isCode === true}
					<CodeBox data={part} />
				{:else}
					<p>{part.text}</p>
				{/if}
			{/each}
		{/if}
	</div>
	<div class="flex items-center space-x-2 w-[40%]">
		<input
			bind:value={message}
			type="text"
			placeholder="Type here"
			class="input input-bordered w-full"
		/>
		<button on:click={getResult} class="btn btn-active btn-accent">Send</button>
	</div>
</div>

<style>
	p {
		white-space: pre-wrap;
		word-wrap: break-word;
	}
</style>
