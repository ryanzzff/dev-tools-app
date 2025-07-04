<script lang="ts">
	let timestamp = $state('');
	let dateTime = $state('');
	let currentTimestamp = $state(Math.floor(Date.now() / 1000));

	function updateCurrentTime() {
		currentTimestamp = Math.floor(Date.now() / 1000);
	}

	function timestampToDate() {
		if (!timestamp) return;
		const ts = parseInt(timestamp);
		if (isNaN(ts)) return;

		const date = new Date(ts * 1000);
		dateTime = date.toISOString().slice(0, 19).replace('T', ' ');
	}

	function dateToTimestamp() {
		if (!dateTime) return;
		const date = new Date(dateTime);
		if (isNaN(date.getTime())) return;

		timestamp = Math.floor(date.getTime() / 1000).toString();
	}

	function useCurrentTime() {
		timestamp = currentTimestamp.toString();
		timestampToDate();
	}

	setInterval(updateCurrentTime, 1000);
</script>

<div class="mx-auto max-w-4xl">
	<div class="mb-12 text-center">
		<div
			class="mb-4 inline-flex h-16 w-16 items-center justify-center rounded-2xl bg-gradient-to-br from-blue-500 to-blue-600"
		>
			<svg class="h-8 w-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
				/>
			</svg>
		</div>
		<h1
			class="mb-4 bg-gradient-to-r from-blue-600 to-indigo-600 bg-clip-text text-4xl font-bold text-transparent"
		>
			Unix Timestamp Converter
		</h1>
		<p class="mx-auto max-w-2xl text-lg text-slate-600">
			Convert between Unix timestamps and human-readable dates with real-time updates
		</p>
	</div>

	<div class="mb-8 rounded-2xl border border-blue-100 bg-white/70 p-8 shadow-xl backdrop-blur-sm">
		<div class="mb-6 flex items-center space-x-3">
			<div
				class="flex h-8 w-8 items-center justify-center rounded-lg bg-gradient-to-br from-green-500 to-emerald-500"
			>
				<svg class="h-4 w-4 text-white" fill="currentColor" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
						clip-rule="evenodd"
					/>
				</svg>
			</div>
			<h2 class="text-xl font-semibold text-slate-900">Current Time</h2>
		</div>
		<div
			class="flex items-center justify-between rounded-xl border border-slate-200/50 bg-gradient-to-br from-slate-50 to-slate-100/50 p-6"
		>
			<div class="space-y-2">
				<div class="font-mono text-3xl font-bold text-slate-900">{currentTimestamp}</div>
				<div class="text-slate-600">{new Date(currentTimestamp * 1000).toLocaleString()}</div>
			</div>
			<button
				onclick={useCurrentTime}
				class="rounded-xl bg-gradient-to-r from-blue-600 to-indigo-600 px-6 py-3 font-medium text-white shadow-lg transition-all duration-300 hover:from-blue-700 hover:to-indigo-700 hover:shadow-xl"
			>
				Use Current
			</button>
		</div>
	</div>

	<div class="grid grid-cols-1 gap-8 lg:grid-cols-2">
		<div class="rounded-2xl border border-blue-100 bg-white/70 p-8 shadow-xl backdrop-blur-sm">
			<div class="mb-6 flex items-center space-x-3">
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
				<label for="timestamp" class="text-lg font-semibold text-slate-900"> Unix Timestamp </label>
			</div>
			<input
				id="timestamp"
				type="text"
				bind:value={timestamp}
				oninput={timestampToDate}
				placeholder="1640995200"
				class="w-full rounded-xl border border-slate-200 bg-white/80 px-4 py-4 font-mono text-lg transition-all duration-300 focus:border-blue-500 focus:ring-4 focus:ring-blue-500/20 focus:outline-none"
			/>
			<p class="mt-3 flex items-center space-x-2 text-sm text-slate-500">
				<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z"
						clip-rule="evenodd"
					/>
				</svg>
				<span>Enter a Unix timestamp (seconds since epoch)</span>
			</p>
		</div>

		<div class="rounded-2xl border border-blue-100 bg-white/70 p-8 shadow-xl backdrop-blur-sm">
			<div class="mb-6 flex items-center space-x-3">
				<div
					class="flex h-8 w-8 items-center justify-center rounded-lg bg-gradient-to-br from-purple-500 to-purple-600"
				>
					<svg class="h-4 w-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
						/>
					</svg>
				</div>
				<label for="datetime" class="text-lg font-semibold text-slate-900"> Date & Time </label>
			</div>
			<input
				id="datetime"
				type="datetime-local"
				bind:value={dateTime}
				oninput={dateToTimestamp}
				class="w-full rounded-xl border border-slate-200 bg-white/80 px-4 py-4 text-lg transition-all duration-300 focus:border-purple-500 focus:ring-4 focus:ring-purple-500/20 focus:outline-none"
			/>
			<p class="mt-3 flex items-center space-x-2 text-sm text-slate-500">
				<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z"
						clip-rule="evenodd"
					/>
				</svg>
				<span>Select or enter a date and time</span>
			</p>
		</div>
	</div>

	<div
		class="mt-12 rounded-2xl border border-blue-100 bg-gradient-to-r from-blue-50 to-indigo-50 p-8"
	>
		<div class="flex items-start space-x-4">
			<div
				class="mt-1 flex h-8 w-8 flex-shrink-0 items-center justify-center rounded-lg bg-gradient-to-br from-blue-500 to-indigo-500"
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
				<h3 class="mb-3 text-lg font-semibold text-blue-900">About Unix Timestamps</h3>
				<p class="leading-relaxed text-blue-800">
					Unix timestamps represent the number of seconds that have elapsed since January 1, 1970,
					00:00:00 UTC (the Unix epoch). They're commonly used in programming and databases for
					storing dates and times in a universal, timezone-independent format.
				</p>
			</div>
		</div>
	</div>
</div>
