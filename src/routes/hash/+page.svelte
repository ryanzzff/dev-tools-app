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
		return hashArray.map((b) => b.toString(16).padStart(2, '0')).join('');
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

<div class="mx-auto max-w-4xl">
	<h1 class="mb-8 text-3xl font-bold text-slate-900">Hash Generator</h1>

	<div class="mb-6 rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
		<div class="mb-4 flex items-center justify-between">
			<h2 class="text-lg font-semibold text-slate-900">Input Text</h2>
			<div class="flex gap-2">
				<button
					onclick={loadSample}
					class="rounded bg-blue-100 px-3 py-1 text-sm text-blue-700 transition-colors hover:bg-blue-200"
				>
					Sample
				</button>
				<button
					onclick={clear}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200"
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
			class="w-full resize-none rounded-lg border border-slate-300 px-3 py-2 focus:border-transparent focus:ring-2 focus:ring-blue-500 focus:outline-none"
		></textarea>
	</div>

	<div class="space-y-4">
		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-3 flex items-center justify-between">
				<h3 class="text-lg font-semibold text-slate-900">MD5</h3>
				<button
					onclick={() => copyToClipboard(md5Hash)}
					disabled={!md5Hash || md5Hash.includes('requires')}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block rounded bg-slate-50 p-3 font-mono text-sm break-all text-slate-700">
				{md5Hash || 'Hash will appear here...'}
			</code>
			<p class="mt-2 text-xs text-orange-600">
				Note: MD5 is cryptographically broken and should not be used for security purposes
			</p>
		</div>

		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-3 flex items-center justify-between">
				<h3 class="text-lg font-semibold text-slate-900">SHA-1</h3>
				<button
					onclick={() => copyToClipboard(sha1Hash)}
					disabled={!sha1Hash}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block rounded bg-slate-50 p-3 font-mono text-sm break-all text-slate-700">
				{sha1Hash || 'Hash will appear here...'}
			</code>
			<p class="mt-2 text-xs text-orange-600">Note: SHA-1 is deprecated for cryptographic use</p>
		</div>

		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-3 flex items-center justify-between">
				<h3 class="text-lg font-semibold text-slate-900">SHA-256</h3>
				<button
					onclick={() => copyToClipboard(sha256Hash)}
					disabled={!sha256Hash}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block rounded bg-slate-50 p-3 font-mono text-sm break-all text-slate-700">
				{sha256Hash || 'Hash will appear here...'}
			</code>
			<p class="mt-2 text-xs text-green-600">Recommended for most cryptographic applications</p>
		</div>

		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-3 flex items-center justify-between">
				<h3 class="text-lg font-semibold text-slate-900">SHA-512</h3>
				<button
					onclick={() => copyToClipboard(sha512Hash)}
					disabled={!sha512Hash}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			<code class="block rounded bg-slate-50 p-3 font-mono text-sm break-all text-slate-700">
				{sha512Hash || 'Hash will appear here...'}
			</code>
			<p class="mt-2 text-xs text-green-600">More secure variant with longer hash length</p>
		</div>
	</div>

	<div class="mt-8 rounded-lg bg-blue-50 p-4">
		<h3 class="mb-2 text-sm font-medium text-blue-900">About Hash Functions</h3>
		<p class="text-sm text-blue-800">
			Hash functions are mathematical algorithms that transform input data into fixed-size strings.
			They're commonly used for data integrity verification, password storage, and digital
			signatures.
		</p>
	</div>
</div>
