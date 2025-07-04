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
			encodedText = btoa(plainText);
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
			plainText = atob(encodedText);
		} catch (e) {
			decodeError = 'Invalid Base64 string';
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
	<div class="mb-12 text-center">
		<div
			class="mb-4 inline-flex h-16 w-16 items-center justify-center rounded-2xl bg-gradient-to-br from-emerald-500 to-emerald-600"
		>
			<svg class="h-8 w-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M8 9l3 3-3 3m5 0h3M5 20h14a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
				/>
			</svg>
		</div>
		<h1
			class="mb-4 bg-gradient-to-r from-emerald-600 to-green-600 bg-clip-text text-4xl font-bold text-transparent"
		>
			Base64 Encoder/Decoder
		</h1>
		<p class="mx-auto max-w-2xl text-lg text-slate-600">
			Encode and decode Base64 strings with instant conversion and error handling
		</p>
	</div>

	<div class="grid grid-cols-1 gap-8 lg:grid-cols-2">
		<div class="rounded-2xl border border-emerald-100 bg-white/70 p-8 shadow-xl backdrop-blur-sm">
			<div class="mb-6 flex items-center justify-between">
				<div class="flex items-center space-x-3">
					<div
						class="flex h-8 w-8 items-center justify-center rounded-lg bg-gradient-to-br from-emerald-500 to-emerald-600"
					>
						<svg class="h-4 w-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M4 6h16M4 12h16m-7 6h7"
							/>
						</svg>
					</div>
					<h2 class="text-lg font-semibold text-slate-900">Plain Text</h2>
				</div>
				<button
					onclick={() => copyToClipboard(plainText)}
					disabled={!plainText}
					class="rounded-lg bg-gradient-to-r from-emerald-500 to-emerald-600 px-4 py-2 font-medium text-white transition-all duration-300 hover:from-emerald-600 hover:to-emerald-700 disabled:cursor-not-allowed disabled:opacity-50"
				>
					Copy
				</button>
			</div>

			<textarea
				bind:value={plainText}
				oninput={encode}
				placeholder="Enter text to encode..."
				rows="12"
				class="w-full resize-none rounded-xl border border-slate-200 bg-white/80 px-4 py-4 transition-all duration-300 focus:border-emerald-500 focus:ring-4 focus:ring-emerald-500/20 focus:outline-none"
			></textarea>

			{#if encodeError}
				<p class="mt-3 flex items-center space-x-2 text-sm text-red-600">
					<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
						<path
							fill-rule="evenodd"
							d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z"
							clip-rule="evenodd"
						/>
					</svg>
					<span>{encodeError}</span>
				</p>
			{/if}
		</div>

		<div class="rounded-2xl border border-emerald-100 bg-white/70 p-8 shadow-xl backdrop-blur-sm">
			<div class="mb-6 flex items-center justify-between">
				<div class="flex items-center space-x-3">
					<div
						class="flex h-8 w-8 items-center justify-center rounded-lg bg-gradient-to-br from-blue-500 to-blue-600"
					>
						<svg class="h-4 w-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M7 20l4-16m6 16l-4-16"
							/>
						</svg>
					</div>
					<h2 class="text-lg font-semibold text-slate-900">Base64 Encoded</h2>
				</div>
				<button
					onclick={() => copyToClipboard(encodedText)}
					disabled={!encodedText}
					class="rounded-lg bg-gradient-to-r from-blue-500 to-blue-600 px-4 py-2 font-medium text-white transition-all duration-300 hover:from-blue-600 hover:to-blue-700 disabled:cursor-not-allowed disabled:opacity-50"
				>
					Copy
				</button>
			</div>

			<textarea
				bind:value={encodedText}
				oninput={decode}
				placeholder="Enter Base64 to decode..."
				rows="12"
				class="w-full resize-none rounded-xl border border-slate-200 bg-white/80 px-4 py-4 font-mono text-sm transition-all duration-300 focus:border-blue-500 focus:ring-4 focus:ring-blue-500/20 focus:outline-none"
			></textarea>

			{#if decodeError}
				<p class="mt-3 flex items-center space-x-2 text-sm text-red-600">
					<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
						<path
							fill-rule="evenodd"
							d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z"
							clip-rule="evenodd"
						/>
					</svg>
					<span>{decodeError}</span>
				</p>
			{/if}
		</div>
	</div>

	<div class="mt-8 flex justify-center">
		<button
			onclick={clear}
			class="rounded-xl bg-gradient-to-r from-slate-600 to-slate-700 px-8 py-3 font-medium text-white shadow-lg transition-all duration-300 hover:from-slate-700 hover:to-slate-800 hover:shadow-xl"
		>
			Clear All
		</button>
	</div>

	<div
		class="mt-12 rounded-2xl border border-emerald-100 bg-gradient-to-r from-emerald-50 to-green-50 p-8"
	>
		<div class="flex items-start space-x-4">
			<div
				class="mt-1 flex h-8 w-8 flex-shrink-0 items-center justify-center rounded-lg bg-gradient-to-br from-emerald-500 to-green-500"
			>
				<svg class="h-4 w-4 text-white" fill="currentColor" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z"
						clip-rule="evenodd"
					/>
				</svg>
			</div>
			<div>
				<h3 class="mb-3 text-lg font-semibold text-emerald-900">About Base64 Encoding</h3>
				<p class="leading-relaxed text-emerald-800">
					Base64 is a binary-to-text encoding scheme that represents binary data in an ASCII string
					format using a radix-64 representation. It's commonly used for encoding data in emails,
					JSON, data URLs, and web APIs where binary data needs to be transmitted over text-based
					protocols.
				</p>
			</div>
		</div>
	</div>
</div>
