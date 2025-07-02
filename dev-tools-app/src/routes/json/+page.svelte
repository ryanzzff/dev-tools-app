<script lang="ts">
	let inputJson = $state('');
	let outputJson = $state('');
	let error = $state('');
	let isValid = $state(false);

	function validateAndFormat() {
		try {
			error = '';
			if (!inputJson.trim()) {
				outputJson = '';
				isValid = false;
				return;
			}

			const parsed = JSON.parse(inputJson);
			outputJson = JSON.stringify(parsed, null, 2);
			isValid = true;
		} catch (e) {
			error = e instanceof Error ? e.message : 'Invalid JSON';
			isValid = false;
			outputJson = '';
		}
	}

	function minify() {
		try {
			error = '';
			if (!inputJson.trim()) {
				outputJson = '';
				return;
			}

			const parsed = JSON.parse(inputJson);
			outputJson = JSON.stringify(parsed);
			isValid = true;
		} catch (e) {
			error = e instanceof Error ? e.message : 'Invalid JSON';
			isValid = false;
		}
	}

	function clear() {
		inputJson = '';
		outputJson = '';
		error = '';
		isValid = false;
	}

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
	}

	function loadSample() {
		inputJson = `{
  "name": "John Doe",
  "age": 30,
  "email": "john@example.com",
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zipCode": "10001"
  },
  "hobbies": ["reading", "coding", "hiking"],
  "isActive": true
}`;
		validateAndFormat();
	}
</script>

<div class="max-w-6xl mx-auto">
	<h1 class="text-3xl font-bold text-slate-900 mb-8">JSON Formatter & Validator</h1>

	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-4">
				<h2 class="text-lg font-semibold text-slate-900">Input JSON</h2>
				<div class="flex gap-2">
					<button
						onclick={loadSample}
						class="px-3 py-1 text-sm bg-blue-100 text-blue-700 rounded hover:bg-blue-200 transition-colors"
					>
						Sample
					</button>
					<button
						onclick={() => copyToClipboard(inputJson)}
						disabled={!inputJson}
						class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
					>
						Copy
					</button>
				</div>
			</div>
			
			<textarea
				bind:value={inputJson}
				oninput={validateAndFormat}
				placeholder="Paste your JSON here..."
				rows="15"
				class="w-full px-3 py-2 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none font-mono text-sm"
			></textarea>
			
			<div class="flex items-center justify-between mt-4">
				<div class="flex items-center gap-2">
					{#if isValid}
						<div class="flex items-center text-green-600">
							<svg class="w-4 h-4 mr-1" fill="currentColor" viewBox="0 0 20 20">
								<path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"/>
							</svg>
							Valid JSON
						</div>
					{:else if error}
						<div class="text-red-600 text-sm">
							<svg class="w-4 h-4 mr-1 inline" fill="currentColor" viewBox="0 0 20 20">
								<path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd"/>
							</svg>
							{error}
						</div>
					{/if}
				</div>
				
				<div class="flex gap-2">
					<button
						onclick={minify}
						disabled={!inputJson}
						class="px-3 py-1 text-sm bg-orange-100 text-orange-700 rounded hover:bg-orange-200 transition-colors disabled:opacity-50"
					>
						Minify
					</button>
					<button
						onclick={validateAndFormat}
						disabled={!inputJson}
						class="px-3 py-1 text-sm bg-green-100 text-green-700 rounded hover:bg-green-200 transition-colors disabled:opacity-50"
					>
						Format
					</button>
				</div>
			</div>
		</div>

		<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6">
			<div class="flex items-center justify-between mb-4">
				<h2 class="text-lg font-semibold text-slate-900">Formatted Output</h2>
				<button
					onclick={() => copyToClipboard(outputJson)}
					disabled={!outputJson}
					class="px-3 py-1 text-sm bg-slate-100 text-slate-700 rounded hover:bg-slate-200 transition-colors disabled:opacity-50"
				>
					Copy
				</button>
			</div>
			
			<textarea
				bind:value={outputJson}
				readonly
				placeholder="Formatted JSON will appear here..."
				rows="15"
				class="w-full px-3 py-2 border border-slate-300 rounded-lg bg-slate-50 resize-none font-mono text-sm"
			></textarea>
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
		<h3 class="text-sm font-medium text-blue-900 mb-2">About JSON</h3>
		<p class="text-sm text-blue-800">
			JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for humans to read and write.
			This tool validates your JSON syntax and formats it with proper indentation for better readability.
		</p>
	</div>
</div>