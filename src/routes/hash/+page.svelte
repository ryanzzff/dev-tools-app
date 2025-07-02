<script lang="ts">
	let inputText = $state('');
	let md5Hash = $state('');
	let sha1Hash = $state('');
	let sha256Hash = $state('');
	let sha512Hash = $state('');

	async function generateHashes() {
		if (!inputText) {
			md5Hash = '';
			sha1Hash = '';
			sha256Hash = '';
			sha512Hash = '';
			return;
		}

		const encoder = new TextEncoder();
		const data = encoder.encode(inputText);

		try {
			// MD5 is not available in Web Crypto API, so we'll use a simple implementation
			md5Hash = await simpleHash(inputText, 'MD5');
			
			// SHA-1
			const sha1Buffer = await crypto.subtle.digest('SHA-1', data);
			sha1Hash = bufferToHex(sha1Buffer);

			// SHA-256
			const sha256Buffer = await crypto.subtle.digest('SHA-256', data);
			sha256Hash = bufferToHex(sha256Buffer);

			// SHA-512
			const sha512Buffer = await crypto.subtle.digest('SHA-512', data);
			sha512Hash = bufferToHex(sha512Buffer);
		} catch (error) {
			console.error('Error generating hashes:', error);
		}
	}

	function bufferToHex(buffer: ArrayBuffer): string {
		const hashArray = Array.from(new Uint8Array(buffer));
		return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
	}

	// Simple MD5 implementation (note: this is a simplified version for demo purposes)
	async function simpleHash(str: string, algorithm: string): Promise<string> {
		// For MD5, we'll show a placeholder since it's not natively supported
		// In a real app, you'd use a library like crypto-js
		return 'MD5 requires external library (crypto-js)';
	}

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
	}

	function clear() {
		inputText = '';
		md5Hash = '';
		sha1Hash = '';
		sha256Hash = '';
		sha512Hash = '';
	}

	function loadSample() {
		inputText = 'Hello, World!';
		generateHashes();
	}
</script>

<div class="max-w-4xl mx-auto">
	<h1 class="text-3xl font-bold text-slate-900 mb-8">Hash Generator</h1>

	<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6 mb-6">
		<div class="flex items-center justify-between mb-4">
			<h2 class="text-lg font-semibold text-slate-900">Input Text</h2>
			<div class="flex gap-2">
				<button
					onclick={loadSample}
					class="px-3 py-1 text-sm bg-blue-100 text-blue-700 rounded hover:bg-blue-200 transition-colors"
				>
					Sample
				</button>
				<button
					onclick={clear}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors"
				>
					Clear
				</button>
			</div>
		</div>
		
		<textarea
			bind:value={inputText}
			oninput={generateHashes}
			placeholder="Enter text to hash..."
			rows="4"
			class="w-full px-3 py-2 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none"
		></textarea>
	</div>

	<div class="space-y-4">
		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-3">
				<h3 class="text-lg font-semibold text-slate-900">MD5</h3>
				<button
					onclick={() => copyToClipboard(md5Hash)}
					disabled={!md5Hash || md5Hash.includes('requires')}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block p-3 bg-slate-50 rounded font-mono text-sm break-all text-slate-700">
				{md5Hash || 'Hash will appear here...'}
			</code>
			<p class="text-xs text-orange-600 mt-2">Note: MD5 is cryptographically broken and should not be used for security purposes</p>
		</div>

		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-3">
				<h3 class="text-lg font-semibold text-slate-900">SHA-1</h3>
				<button
					onclick={() => copyToClipboard(sha1Hash)}
					disabled={!sha1Hash}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block p-3 bg-slate-50 rounded font-mono text-sm break-all text-slate-700">
				{sha1Hash || 'Hash will appear here...'}
			</code>
			<p class="text-xs text-orange-600 mt-2">Note: SHA-1 is deprecated for cryptographic use</p>
		</div>

		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-3">
				<h3 class="text-lg font-semibold text-slate-900">SHA-256</h3>
				<button
					onclick={() => copyToClipboard(sha256Hash)}
					disabled={!sha256Hash}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block p-3 bg-slate-50 rounded font-mono text-sm break-all text-slate-700">
				{sha256Hash || 'Hash will appear here...'}
			</code>
			<p class="text-xs text-green-600 mt-2">Recommended for most cryptographic applications</p>
		</div>

		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-3">
				<h3 class="text-lg font-semibold text-slate-900">SHA-512</h3>
				<button
					onclick={() => copyToClipboard(sha512Hash)}
					disabled={!sha512Hash}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block p-3 bg-slate-50 rounded font-mono text-sm break-all text-slate-700">
				{sha512Hash || 'Hash will appear here...'}
			</code>
			<p class="text-xs text-green-600 mt-2">More secure variant with longer hash length</p>
		</div>
	</div>

	<div class="mt-8 p-4 bg-blue-50 rounded-lg">
		<h3 class="text-sm font-medium text-blue-900 mb-2">About Hash Functions</h3>
		<p class="text-sm text-blue-800">
			Hash functions are mathematical algorithms that transform input data into fixed-size strings.
			They're commonly used for data integrity verification, password storage, and digital signatures.
		</p>
	</div>
</div>