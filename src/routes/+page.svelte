<script lang="ts">
	import { emojis, type Emoji } from '../lib/data/emojis';

	let selectedEmoji: Emoji = emojis[0];
	let message = '';

	function handleKeydown(index: number) {
		selectedEmoji = emojis[index];
		message = '';
	}

	async function copyToClipboard(text: string) {
		try {
			await navigator.clipboard.writeText(text);
			message = 'copied to clipboard! ✅';
		} catch (err) {
			console.error('Failed to copy: ', err);
		}
	}

	function downloadImage(imageUrl: string) {
		const link = document.createElement('a');
		const fileExtension = imageUrl.split('.').pop()?.split('?')[0] || 'png';

		link.href = imageUrl;
		link.download = `${selectedEmoji.name}.${fileExtension}`;

		document.body.appendChild(link);
		link.click();
		document.body.removeChild(link);
	}

	const ACTION_BTN_CLASSES =
		'rounded-md px-2 py-1 text-xs text-violet-50/70 bg-violet-50/5 hover:bg-violet-50/10 focus:outline-none focus:ring-1 focus:ring-violet-50';
</script>

<svelte:head>
	<title>slack cats 🐈</title>
	<meta name="description" content="a variety of silly cat emojis for Slack 🐱" />
</svelte:head>

<div class="h-screen bg-gray-800 flex flex-col items-center pb-12">
	<h1 class="text-2xl font-semibold mt-10 text-violet-400">slack cats</h1>
	<p class="text-violet-50/80 max-w-[40ch] text-center text-xs mt-2">
		✨ the emoji directory you didn't know you needed ✨
	</p>

	{#if selectedEmoji}
		<img
			src={selectedEmoji.src}
			alt={selectedEmoji.alt}
			class="h-32 w-32 md:h-52 md:w-52 object-cover mt-8"
		/>
		<div class="flex flex-col gap-2 items-center mt-1">
			<p class="text-violet-50 text-lg">{`${selectedEmoji.name}`}</p>
			<div class="flex gap-1">
				<button class={ACTION_BTN_CLASSES} on:click={() => copyToClipboard(selectedEmoji.name)}
					>copy name</button
				>
				<button class={ACTION_BTN_CLASSES} on:click={() => downloadImage(selectedEmoji.src)}
					>download image</button
				>
				<!-- Added Download Button -->
			</div>
			<p class="text-xs text-gray-400">{message}</p>
		</div>
	{/if}
	<div
		class="flex w-5/6 justify-center flex-wrap gap-3 mt-12 max-h-[400px] md:max-h-fit overflow-y-scroll rounded-md bg-violet-400/5 py-4"
	>
		{#each emojis as emoji, index}
			<button
				on:click={() => handleKeydown(index)}
				on:keydown={() => handleKeydown(index)}
				class="border border-violet-400/60 flex gap-1 items-center p-2 rounded-md w-fit bg-violet-950/20 focus:outline-none focus:ring-1 focus:ring-violet-300"
			>
				<img src={emoji.src} alt={emoji.alt} class="object-cover h-[32px] w-[32px]" />
				<p class="text-sm text-violet-50">{`:${emoji.name}:`}</p>
			</button>
		{/each}
	</div>
</div>
