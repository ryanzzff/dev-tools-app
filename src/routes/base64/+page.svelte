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

<div class="max-w-4xl mx-auto">
	<div class="text-center mb-12">
		<div class="inline-flex items-center justify-center w-16 h-16 bg-gradient-to-br from-emerald-500 to-emerald-600 rounded-2xl mb-4">
			<svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 9l3 3-3 3m5 0h3M5 20h14a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
			</svg>
		</div>
		<h1 class="text-4xl font-bold bg-gradient-to-r from-emerald-600 to-green-600 bg-clip-text text-transparent mb-4">
			Base64 Encoder/Decoder
		</h1>
		<p class="text-lg text-slate-600 max-w-2xl mx-auto">
			Encode and decode Base64 strings with instant conversion and error handling
		</p>
	</div>

	<div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
		<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-emerald-100 p-8">
			<div class="flex items-center justify-between mb-6">
				<div class="flex items-center space-x-3">
					<div class="w-8 h-8 bg-gradient-to-br from-emerald-500 to-emerald-600 rounded-lg flex items-center justify-center">
						<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"/>
						</svg>
					</div>
					<h2 class="text-lg font-semibold text-slate-900">Plain Text</h2>
				</div>
				<button
					onclick={() => copyToClipboard(plainText)}
					disabled={!plainText}
					class="px-4 py-2 bg-gradient-to-r from-emerald-500 to-emerald-600 text-white rounded-lg hover:from-emerald-600 hover:to-emerald-700 transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed font-medium"
				>
					Copy
				</button>
			</div>
			
			<textarea
				bind:value={plainText}
				oninput={encode}
				placeholder="Enter text to encode..."
				rows="12"
				class="w-full px-4 py-4 bg-white/80 border border-slate-200 rounded-xl focus:outline-none focus:ring-4 focus:ring-emerald-500/20 focus:border-emerald-500 transition-all duration-300 resize-none"
			></textarea>
			
			{#if encodeError}
				<p class="text-red-600 text-sm mt-3 flex items-center space-x-2">
					<svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
						<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd"/>
					</svg>
					<span>{encodeError}</span>
				</p>
			{/if}
		</div>

		<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-emerald-100 p-8">
			<div class="flex items-center justify-between mb-6">
				<div class="flex items-center space-x-3">
					<div class="w-8 h-8 bg-gradient-to-br from-blue-500 to-blue-600 rounded-lg flex items-center justify-center">
						<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 20l4-16m6 16l-4-16"/>
						</svg>
					</div>
					<h2 class="text-lg font-semibold text-slate-900">Base64 Encoded</h2>
				</div>
				<button
					onclick={() => copyToClipboard(encodedText)}
					disabled={!encodedText}
					class="px-4 py-2 bg-gradient-to-r from-blue-500 to-blue-600 text-white rounded-lg hover:from-blue-600 hover:to-blue-700 transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed font-medium"
				>
					Copy
				</button>
			</div>
			
			<textarea
				bind:value={encodedText}
				oninput={decode}
				placeholder="Enter Base64 to decode..."
				rows="12"
				class="w-full px-4 py-4 bg-white/80 border border-slate-200 rounded-xl focus:outline-none focus:ring-4 focus:ring-blue-500/20 focus:border-blue-500 transition-all duration-300 resize-none font-mono text-sm"
			></textarea>
			
			{#if decodeError}
				<p class="text-red-600 text-sm mt-3 flex items-center space-x-2">
					<svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
						<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd"/>
					</svg>
					<span>{decodeError}</span>
				</p>
			{/if}
		</div>
	</div>

	<div class="flex justify-center mt-8">
		<button
			onclick={clear}
			class="px-8 py-3 bg-gradient-to-r from-slate-600 to-slate-700 text-white rounded-xl hover:from-slate-700 hover:to-slate-800 transition-all duration-300 shadow-lg hover:shadow-xl font-medium"
		>
			Clear All
		</button>
	</div>

	<div class="mt-12 p-8 bg-gradient-to-r from-emerald-50 to-green-50 rounded-2xl border border-emerald-100">
		<div class="flex items-start space-x-4">
			<div class="w-8 h-8 bg-gradient-to-br from-emerald-500 to-green-500 rounded-lg flex items-center justify-center flex-shrink-0 mt-1">
				<svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 20 20">
					<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"/>
				</svg>
			</div>
			<div>
				<h3 class="text-lg font-semibold text-emerald-900 mb-3">About Base64 Encoding</h3>
				<p class="text-emerald-800 leading-relaxed">
					Base64 is a binary-to-text encoding scheme that represents binary data in an ASCII string format using a radix-64 representation.
					It's commonly used for encoding data in emails, JSON, data URLs, and web APIs where binary data needs to be transmitted over text-based protocols.
				</p>
			</div>
		</div>
	</div>
</div>