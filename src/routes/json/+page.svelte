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

<div class="mx-auto max-w-6xl">
	<h1 class="mb-8 text-3xl font-bold text-slate-900">JSON Formatter & Validator</h1>

	<div class="grid grid-cols-1 gap-6 lg:grid-cols-2">
		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold text-slate-900">Input JSON</h2>
				<div class="flex gap-2">
					<button
						onclick={loadSample}
						class="rounded bg-blue-100 px-3 py-1 text-sm text-blue-700 transition-colors hover:bg-blue-200"
					>
						Sample
					</button>
					<button
						onclick={() => copyToClipboard(inputJson)}
						disabled={!inputJson}
						class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
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
				class="w-full resize-none rounded-lg border border-slate-300 px-3 py-2 font-mono text-sm focus:border-transparent focus:ring-2 focus:ring-blue-500 focus:outline-none"
			></textarea>

			<div class="mt-4 flex items-center justify-between">
				<div class="flex items-center gap-2">
					{#if isValid}
						<div class="flex items-center text-green-600">
							<svg class="mr-1 h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
								<path
									fill-rule="evenodd"
									d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
									clip-rule="evenodd"
								/>
							</svg>
							Valid JSON
						</div>
					{:else if error}
						<div class="text-sm text-red-600">
							<svg class="mr-1 inline h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
								<path
									fill-rule="evenodd"
									d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z"
									clip-rule="evenodd"
								/>
							</svg>
							{error}
						</div>
					{/if}
				</div>

				<div class="flex gap-2">
					<button
						onclick={minify}
						disabled={!inputJson}
						class="rounded bg-orange-100 px-3 py-1 text-sm text-orange-700 transition-colors hover:bg-orange-200 disabled:opacity-50"
					>
						Minify
					</button>
					<button
						onclick={validateAndFormat}
						disabled={!inputJson}
						class="rounded bg-green-100 px-3 py-1 text-sm text-green-700 transition-colors hover:bg-green-200 disabled:opacity-50"
					>
						Format
					</button>
				</div>
			</div>
		</div>

		<div class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
			<div class="mb-4 flex items-center justify-between">
				<h2 class="text-lg font-semibold text-slate-900">Formatted Output</h2>
				<button
					onclick={() => copyToClipboard(outputJson)}
					disabled={!outputJson}
					class="rounded bg-slate-100 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-200 disabled:opacity-50"
				>
					Copy
				</button>
			</div>

			<textarea
				bind:value={outputJson}
				readonly
				placeholder="Formatted JSON will appear here..."
				rows="15"
				class="w-full resize-none rounded-lg border border-slate-300 bg-slate-50 px-3 py-2 font-mono text-sm"
			></textarea>
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
		<h3 class="mb-2 text-sm font-medium text-blue-900">About JSON</h3>
		<p class="text-sm text-blue-800">
			JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for
			humans to read and write. This tool validates your JSON syntax and formats it with proper
			indentation for better readability.
		</p>
	</div>
</div>
