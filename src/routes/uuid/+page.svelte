<script lang="ts">
	let generatedUuids = $state<string[]>([]);
	let count = $state(1);

	function generateUuid(): string {
		return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
			const r = (Math.random() * 16) | 0;
			const v = c === 'x' ? r : (r & 0x3) | 0x8;
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

<div class="mx-auto max-w-4xl">
	<h1 class="mb-8 text-3xl font-bold text-slate-900">UUID Generator</h1>

	<div class="mb-6 rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
		<div class="mb-6 flex items-center gap-4">
			<div class="flex items-center gap-2">
				<label for="count" class="text-sm font-medium text-slate-700">Generate</label>
				<input
					id="count"
					type="number"
					min="1"
					max="100"
					bind:value={count}
					oninput={handleCountInput}
					class="w-20 rounded border border-slate-300 px-2 py-1 focus:border-transparent focus:ring-2 focus:ring-blue-500 focus:outline-none"
				/>
				<span class="text-sm text-slate-600">UUIDs</span>
			</div>

			<button
				onclick={generateUuids}
				class="rounded-lg bg-blue-600 px-4 py-2 text-white transition-colors hover:bg-blue-700"
			>
				Generate
			</button>

			{#if generatedUuids.length > 1}
				<button
					onclick={copyAllUuids}
					class="rounded-lg bg-green-600 px-4 py-2 text-white transition-colors hover:bg-green-700"
				>
					Copy All
				</button>
			{/if}

			{#if generatedUuids.length > 0}
				<button
					onclick={clear}
					class="rounded-lg bg-slate-600 px-4 py-2 text-white transition-colors hover:bg-slate-700"
				>
					Clear
				</button>
			{/if}
		</div>

		{#if generatedUuids.length > 0}
			<div class="space-y-3">
				{#each generatedUuids as uuid, index}
					<div class="flex items-center justify-between rounded-lg bg-slate-50 p-3">
						<div class="flex items-center gap-3">
							<span class="w-8 text-sm text-slate-500">#{index + 1}</span>
							<code class="font-mono text-slate-900 select-all">{uuid}</code>
						</div>
						<button
							onclick={() => copyToClipboard(uuid)}
							class="rounded bg-slate-200 px-3 py-1 text-sm text-slate-700 transition-colors hover:bg-slate-300"
						>
							Copy
						</button>
					</div>
				{/each}
			</div>
		{/if}
	</div>

	<div class="grid grid-cols-1 gap-6 md:grid-cols-2">
		<div class="rounded-lg bg-blue-50 p-4">
			<h3 class="mb-2 text-sm font-medium text-blue-900">About UUIDs</h3>
			<p class="text-sm text-blue-800">
				UUID (Universally Unique Identifier) is a 128-bit identifier. Version 4 UUIDs are randomly
				generated and have a very low probability of collision, making them ideal for distributed
				systems.
			</p>
		</div>

		<div class="rounded-lg bg-green-50 p-4">
			<h3 class="mb-2 text-sm font-medium text-green-900">UUID Format</h3>
			<p class="text-sm text-green-800">
				Standard format: xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx<br />
				• 32 hexadecimal digits<br />
				• Displayed in 5 groups separated by hyphens<br />
				• The '4' indicates version 4 (random)
			</p>
		</div>
	</div>
</div>
