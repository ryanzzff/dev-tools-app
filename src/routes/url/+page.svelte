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

<div class="max-w-4xl mx-auto">
	<h1 class="text-3xl font-bold text-slate-900 mb-8">URL Encoder/Decoder</h1>

	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-4">
				<h2 class="text-lg font-semibold text-slate-900">Plain Text</h2>
				<button
					onclick={() => copyToClipboard(plainText)}
					disabled={!plainText}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			
			<textarea
				bind:value={plainText}
				oninput={encode}
				placeholder="Enter text to URL encode..."
				rows="10"
				class="w-full px-3 py-2 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none"
			></textarea>
			
			{#if encodeError}
				<p class="text-red-600 text-sm mt-2">{encodeError}</p>
			{/if}
		</div>

		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-4">
				<h2 class="text-lg font-semibold text-slate-900">URL Encoded</h2>
				<button
					onclick={() => copyToClipboard(encodedText)}
					disabled={!encodedText}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			
			<textarea
				bind:value={encodedText}
				oninput={decode}
				placeholder="Enter URL encoded text to decode..."
				rows="10"
				class="w-full px-3 py-2 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none font-mono text-sm"
			></textarea>
			
			{#if decodeError}
				<p class="text-red-600 text-sm mt-2">{decodeError}</p>
			{/if}
		</div>
	</div>

	<div class="flex justify-center mt-6">
		<button
			onclick={clear}
			class="px-6 py-2 bg-slate-600 text-white rounded-lg hover:bg-slate-700 transition-colors"
		>
			Clear All
		</button>
	</div>

	<div class="mt-8 p-4 bg-blue-50 rounded-lg">
		<h3 class="text-sm font-medium text-blue-900 mb-2">About URL Encoding</h3>
		<p class="text-sm text-blue-800">
			URL encoding (percent encoding) converts characters into a format that can be transmitted over the Internet.
			Special characters are replaced with a % followed by two hexadecimal digits representing the ASCII code.
		</p>
		<div class="mt-2 text-xs text-blue-700">
			<p>Common examples: space → %20, & → %26, # → %23, ? → %3F</p>
		</div>
	</div>
</div>