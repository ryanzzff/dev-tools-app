<script lang="ts">
	import { SignJWT, jwtVerify, decodeJwt } from 'jose';

	let payload = $state('{\n  "sub": "1234567890",\n  "name": "John Doe",\n  "iat": 1516239022\n}');
	let secret = $state('your-256-bit-secret');
	let algorithm = $state('HS256');
	let jwtToken = $state('');
	let decodedHeader = $state('');
	let decodedPayload = $state('');
	let encodeError = $state('');
	let decodeError = $state('');
	let verifyError = $state('');
	let isValid = $state(false);

	const algorithms = [
		'HS256', 'HS384', 'HS512'
	];

	async function encodeJWT() {
		try {
			encodeError = '';
			
			if (!payload.trim() || !secret.trim()) {
				jwtToken = '';
				return;
			}

			const parsedPayload = JSON.parse(payload);
			const secretKey = new TextEncoder().encode(secret);
			
			const jwt = await new SignJWT(parsedPayload)
				.setProtectedHeader({ alg: algorithm })
				.setIssuedAt()
				.setExpirationTime('2h')
				.sign(secretKey);
			
			jwtToken = jwt;
		} catch (e) {
			encodeError = e instanceof Error ? e.message : 'Error encoding JWT';
			jwtToken = '';
		}
	}

	async function decodeJWT() {
		try {
			decodeError = '';
			verifyError = '';
			isValid = false;
			
			if (!jwtToken.trim()) {
				decodedHeader = '';
				decodedPayload = '';
				return;
			}

			// Decode without verification first
			const decoded = decodeJwt(jwtToken);
			decodedPayload = JSON.stringify(decoded, null, 2);
			
			// Get header
			const parts = jwtToken.split('.');
			if (parts.length !== 3) {
				throw new Error('Invalid JWT format');
			}
			
			const header = JSON.parse(atob(parts[0]));
			decodedHeader = JSON.stringify(header, null, 2);
			
			// Try to verify if secret is provided
			if (secret.trim()) {
				try {
					const secretKey = new TextEncoder().encode(secret);
					await jwtVerify(jwtToken, secretKey);
					isValid = true;
				} catch (e) {
					verifyError = e instanceof Error ? e.message : 'JWT verification failed';
				}
			}
		} catch (e) {
			decodeError = e instanceof Error ? e.message : 'Error decoding JWT';
			decodedHeader = '';
			decodedPayload = '';
		}
	}

	function clear() {
		payload = '{\n  "sub": "1234567890",\n  "name": "John Doe",\n  "iat": 1516239022\n}';
		secret = 'your-256-bit-secret';
		algorithm = 'HS256';
		jwtToken = '';
		decodedHeader = '';
		decodedPayload = '';
		encodeError = '';
		decodeError = '';
		verifyError = '';
		isValid = false;
	}

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
	}

	function generateRandomSecret() {
		const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
		let result = '';
		for (let i = 0; i < 32; i++) {
			result += chars.charAt(Math.floor(Math.random() * chars.length));
		}
		secret = result;
	}
</script>

<div class="max-w-6xl mx-auto">
	<div class="text-center mb-12">
		<div class="inline-flex items-center justify-center w-16 h-16 bg-gradient-to-br from-indigo-500 to-purple-600 rounded-2xl mb-4">
			<svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/>
			</svg>
		</div>
		<h1 class="text-4xl font-bold bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent mb-4">
			JWT Encoder/Decoder
		</h1>
		<p class="text-lg text-slate-600 max-w-2xl mx-auto">
			Encode and decode JSON Web Tokens with verification and customizable payloads
		</p>
	</div>

	<!-- Configuration Section -->
	<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 p-8 mb-8">
		<h2 class="text-xl font-semibold text-slate-900 mb-6">Configuration</h2>
		
		<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
			<div>
				<label class="block text-sm font-medium text-slate-700 mb-2">Algorithm</label>
				<select
					bind:value={algorithm}
					onchange={encodeJWT}
					class="w-full px-4 py-3 bg-white/80 border border-slate-200 rounded-xl focus:outline-none focus:ring-4 focus:ring-indigo-500/20 focus:border-indigo-500 transition-all duration-300"
				>
					{#each algorithms as alg}
						<option value={alg}>{alg}</option>
					{/each}
				</select>
			</div>
			
			<div>
				<label class="block text-sm font-medium text-slate-700 mb-2">Secret Key</label>
				<div class="flex space-x-2">
					<input
						type="text"
						bind:value={secret}
						oninput={encodeJWT}
						placeholder="Enter secret key..."
						class="flex-1 px-4 py-3 bg-white/80 border border-slate-200 rounded-xl focus:outline-none focus:ring-4 focus:ring-indigo-500/20 focus:border-indigo-500 transition-all duration-300"
					/>
					<button
						onclick={generateRandomSecret}
						class="px-4 py-3 bg-gradient-to-r from-indigo-500 to-purple-600 text-white rounded-xl hover:from-indigo-600 hover:to-purple-700 transition-all duration-300 font-medium whitespace-nowrap"
					>
						Generate
					</button>
				</div>
			</div>
		</div>
	</div>

	<div class="grid grid-cols-1 xl:grid-cols-2 gap-8">
		<!-- Encode Section -->
		<div class="space-y-6">
			<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 p-8">
				<div class="flex items-center justify-between mb-6">
					<div class="flex items-center space-x-3">
						<div class="w-8 h-8 bg-gradient-to-br from-indigo-500 to-purple-600 rounded-lg flex items-center justify-center">
							<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"/>
							</svg>
						</div>
						<h2 class="text-lg font-semibold text-slate-900">Payload (JSON)</h2>
					</div>
				</div>
				
				<textarea
					bind:value={payload}
					oninput={encodeJWT}
					placeholder="Enter JSON payload..."
					rows="8"
					class="w-full px-4 py-4 bg-white/80 border border-slate-200 rounded-xl focus:outline-none focus:ring-4 focus:ring-indigo-500/20 focus:border-indigo-500 transition-all duration-300 resize-none font-mono text-sm"
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

			<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 p-8">
				<div class="flex items-center justify-between mb-6">
					<div class="flex items-center space-x-3">
						<div class="w-8 h-8 bg-gradient-to-br from-green-500 to-emerald-600 rounded-lg flex items-center justify-center">
							<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>
							</svg>
						</div>
						<h2 class="text-lg font-semibold text-slate-900">Generated JWT</h2>
					</div>
					<button
						onclick={() => copyToClipboard(jwtToken)}
						disabled={!jwtToken}
						class="px-4 py-2 bg-gradient-to-r from-green-500 to-emerald-600 text-white rounded-lg hover:from-green-600 hover:to-emerald-700 transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed font-medium"
					>
						Copy
					</button>
				</div>
				
				<textarea
					bind:value={jwtToken}
					oninput={decodeJWT}
					placeholder="JWT token will appear here..."
					rows="6"
					class="w-full px-4 py-4 bg-white/80 border border-slate-200 rounded-xl focus:outline-none focus:ring-4 focus:ring-green-500/20 focus:border-green-500 transition-all duration-300 resize-none font-mono text-sm"
				></textarea>
			</div>
		</div>

		<!-- Decode Section -->
		<div class="space-y-6">
			<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 p-8">
				<div class="flex items-center justify-between mb-6">
					<div class="flex items-center space-x-3">
						<div class="w-8 h-8 bg-gradient-to-br from-blue-500 to-blue-600 rounded-lg flex items-center justify-center">
							<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"/>
							</svg>
						</div>
						<h2 class="text-lg font-semibold text-slate-900">Decoded Header</h2>
					</div>
					<button
						onclick={() => copyToClipboard(decodedHeader)}
						disabled={!decodedHeader}
						class="px-4 py-2 bg-gradient-to-r from-blue-500 to-blue-600 text-white rounded-lg hover:from-blue-600 hover:to-blue-700 transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed font-medium"
					>
						Copy
					</button>
				</div>
				
				<textarea
					bind:value={decodedHeader}
					readonly
					placeholder="Header will appear here..."
					rows="4"
					class="w-full px-4 py-4 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none resize-none font-mono text-sm"
				></textarea>
			</div>

			<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 p-8">
				<div class="flex items-center justify-between mb-6">
					<div class="flex items-center space-x-3">
						<div class="w-8 h-8 bg-gradient-to-br from-purple-500 to-purple-600 rounded-lg flex items-center justify-center">
							<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 20l4-16m6 16l-4-16"/>
							</svg>
						</div>
						<h2 class="text-lg font-semibold text-slate-900">Decoded Payload</h2>
					</div>
					<button
						onclick={() => copyToClipboard(decodedPayload)}
						disabled={!decodedPayload}
						class="px-4 py-2 bg-gradient-to-r from-purple-500 to-purple-600 text-white rounded-lg hover:from-purple-600 hover:to-purple-700 transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed font-medium"
					>
						Copy
					</button>
				</div>
				
				<textarea
					bind:value={decodedPayload}
					readonly
					placeholder="Payload will appear here..."
					rows="8"
					class="w-full px-4 py-4 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none resize-none font-mono text-sm"
				></textarea>
			</div>

			<!-- Verification Status -->
			{#if jwtToken && secret}
				<div class="bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 p-6">
					<div class="flex items-center space-x-3">
						{#if isValid}
							<div class="w-8 h-8 bg-gradient-to-br from-green-500 to-emerald-600 rounded-lg flex items-center justify-center">
								<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
								</svg>
							</div>
							<div>
								<h3 class="text-lg font-semibold text-green-700">JWT Valid</h3>
								<p class="text-sm text-green-600">Token signature verified successfully</p>
							</div>
						{:else}
							<div class="w-8 h-8 bg-gradient-to-br from-red-500 to-red-600 rounded-lg flex items-center justify-center">
								<svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
								</svg>
							</div>
							<div>
								<h3 class="text-lg font-semibold text-red-700">JWT Invalid</h3>
								<p class="text-sm text-red-600">{verifyError || 'Token signature verification failed'}</p>
							</div>
						{/if}
					</div>
				</div>
			{/if}
		</div>
	</div>

	{#if decodeError}
		<div class="mt-6 p-4 bg-red-50 border border-red-200 rounded-xl">
			<p class="text-red-600 text-sm flex items-center space-x-2">
				<svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
					<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd"/>
				</svg>
				<span>{decodeError}</span>
			</p>
		</div>
	{/if}

	<div class="flex justify-center mt-8">
		<button
			onclick={clear}
			class="px-8 py-3 bg-gradient-to-r from-slate-600 to-slate-700 text-white rounded-xl hover:from-slate-700 hover:to-slate-800 transition-all duration-300 shadow-lg hover:shadow-xl font-medium"
		>
			Clear All
		</button>
	</div>

	<div class="mt-12 p-8 bg-gradient-to-r from-indigo-50 to-purple-50 rounded-2xl border border-indigo-100">
		<div class="flex items-start space-x-4">
			<div class="w-8 h-8 bg-gradient-to-br from-indigo-500 to-purple-500 rounded-lg flex items-center justify-center flex-shrink-0 mt-1">
				<svg class="w-4 h-4 text-white" fill="currentColor" viewBox="0 0 20 20">
					<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"/>
				</svg>
			</div>
			<div>
				<h3 class="text-lg font-semibold text-indigo-900 mb-3">About JSON Web Tokens (JWT)</h3>
				<p class="text-indigo-800 leading-relaxed">
					JSON Web Tokens are a compact, URL-safe means of representing claims to be transferred between two parties. 
					They consist of three parts: header, payload, and signature, separated by dots. JWTs are commonly used for 
					authentication and information exchange in web applications and APIs.
				</p>
			</div>
		</div>
	</div>
</div>