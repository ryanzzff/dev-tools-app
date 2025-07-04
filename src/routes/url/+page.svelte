<script lang="ts">
	let plainText = $state('');
	let encodedText = $state('');
	let encodeError = $state('');
	let decodeError = $state('');

	function encode() {
		try {
			encodeError = '';
			if (!plainText) {
				encodedText = '';
				return;
			}
			encodedText = encodeURIComponent(plainText);
		} catch (e) {
			encodeError = 'Error encoding text';
		}
	}

	function decode() {
		try {
			decodeError = '';
			if (!encodedText) {
				plainText = '';
				return;
			}
			plainText = decodeURIComponent(encodedText);
		} catch (e) {
			decodeError = 'Invalid URL encoded string';
		}
	}

	function clear() {
		plainText = '';
		encodedText = '';
		encodeError = '';
		decodeError = '';
	}

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
	}
</script>

<div class="mx-auto max-w-4xl">
	<h1 class="mb-8 text-3xl font-bold text-slate-900">URL Encoder/Decoder</h1>

	<div class="grid grid-cols-1 gap-6 lg:grid-cols-2">
		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold text-slate-900">Plain Text</h2>
				<button
					onclick={() => copyToClipboard(plainText)}
					disabled={!plainText}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>

			<textarea
				bind:value={plainText}
				oninput={encode}
				placeholder="Enter text to URL encode..."
				rows="10"
				class="w-full resize-none rounded-lg border border-slate-300 px-3 py-2 focus:border-transparent focus:ring-2 focus:ring-blue-500 focus:outline-none"
			></textarea>

			{#if encodeError}
				<p class="mt-2 text-sm text-red-600">{encodeError}</p>
			{/if}
		</div>

		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold text-slate-900">URL Encoded</h2>
				<button
					onclick={() => copyToClipboard(encodedText)}
					disabled={!encodedText}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>

			<textarea
				bind:value={encodedText}
				oninput={decode}
				placeholder="Enter URL encoded text to decode..."
				rows="10"
				class="w-full resize-none rounded-lg border border-slate-300 px-3 py-2 font-mono text-sm focus:border-transparent focus:ring-2 focus:ring-blue-500 focus:outline-none"
			></textarea>

			{#if decodeError}
				<p class="mt-2 text-sm text-red-600">{decodeError}</p>
			{/if}
		</div>
	</div>

	<div class="mt-6 flex justify-center">
		<button
			onclick={clear}
			class="rounded-lg bg-slate-600 px-6 py-2 text-white transition-colors hover:bg-slate-700"
		>
			Clear All
		</button>
	</div>

	<div class="mt-8 rounded-lg bg-blue-50 p-4">
		<h3 class="mb-2 text-sm font-medium text-blue-900">About URL Encoding</h3>
		<p class="text-sm text-blue-800">
			URL encoding (percent encoding) converts characters into a format that can be transmitted over
			the Internet. Special characters are replaced with a % followed by two hexadecimal digits
			representing the ASCII code.
		</p>
		<div class="mt-2 text-xs text-blue-700">
			<p>Common examples: space → %20, & → %26, # → %23, ? → %3F</p>
		</div>
	</div>
</div>
