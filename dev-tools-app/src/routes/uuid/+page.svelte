<script lang="ts">
	let generatedUuids = $state<string[]>([]);
	let count = $state(1);

	function generateUuid(): string {
		return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
			const r = Math.random() * 16 | 0;
			const v = c === 'x' ? r : (r & 0x3 | 0x8);
			return v.toString(16);
		});
	}

	function generateUuids() {
		const newUuids = [];
		for (let i = 0; i < count; i++) {
			newUuids.push(generateUuid());
		}
		generatedUuids = newUuids;
	}

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
	}

	function copyAllUuids() {
		const allUuids = generatedUuids.join('\n');
		navigator.clipboard.writeText(allUuids);
	}

	function clear() {
		generatedUuids = [];
	}

	function handleCountInput(event: Event) {
		const target = event.target as HTMLInputElement;
		const value = parseInt(target.value);
		if (!isNaN(value) && value > 0 && value <= 100) {
			count = value;
		}
	}

	// Generate one UUID on initial load
	$effect(() => {
		if (generatedUuids.length === 0) {
			generateUuids();
		}
	});
</script>

<div class="max-w-4xl mx-auto">
	<h1 class="text-3xl font-bold text-slate-900 mb-8">UUID Generator</h1>

	<div class="bg-white rounded-lg shadow-sm border border-slate-200 p-6 mb-6">
		<div class="flex items-center gap-4 mb-6">
			<div class="flex items-center gap-2">
				<label for="count" class="text-sm font-medium text-slate-700">Generate</label>
				<input
					id="count"
					type="number"
					min="1"
					max="100"
					bind:value={count}
					oninput={handleCountInput}
					class="w-20 px-2 py-1 border border-slate-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
				/>
				<span class="text-sm text-slate-600">UUIDs</span>
			</div>
			
			<button
				onclick={generateUuids}
				class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors"
			>
				Generate
			</button>

			{#if generatedUuids.length > 1}
				<button
					onclick={copyAllUuids}
					class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors"
				>
					Copy All
				</button>
			{/if}

			{#if generatedUuids.length > 0}
				<button
					onclick={clear}
					class="px-4 py-2 bg-slate-600 text-white rounded-lg hover:bg-slate-700 transition-colors"
				>
					Clear
				</button>
			{/if}
		</div>

		{#if generatedUuids.length > 0}
			<div class="space-y-3">
				{#each generatedUuids as uuid, index}
					<div class="flex items-center justify-between p-3 bg-slate-50 rounded-lg">
						<div class="flex items-center gap-3">
							<span class="text-sm text-slate-500 w-8">#{index + 1}</span>
							<code class="font-mono text-slate-900 select-all">{uuid}</code>
						</div>
						<button
							onclick={() => copyToClipboard(uuid)}
							class="px-3 py-1 text-sm bg-slate-200 text-slate-700 rounded hover:bg-slate-300 transition-colors"
						>
							Copy
						</button>
					</div>
				{/each}
			</div>
		{/if}
	</div>

	<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
		<div class="p-4 bg-blue-50 rounded-lg">
			<h3 class="text-sm font-medium text-blue-900 mb-2">About UUIDs</h3>
			<p class="text-sm text-blue-800">
				UUID (Universally Unique Identifier) is a 128-bit identifier. Version 4 UUIDs are randomly generated
				and have a very low probability of collision, making them ideal for distributed systems.
			</p>
		</div>

		<div class="p-4 bg-green-50 rounded-lg">
			<h3 class="text-sm font-medium text-green-900 mb-2">UUID Format</h3>
			<p class="text-sm text-green-800">
				Standard format: xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx<br>
				• 32 hexadecimal digits<br>
				• Displayed in 5 groups separated by hyphens<br>
				• The '4' indicates version 4 (random)
			</p>
		</div>
	</div>
</div>