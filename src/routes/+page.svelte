<script lang="ts">
	interface Tool {
		href: string;
		title: string;
		description: string;
		icon: string;
		gradientFrom: string;
		gradientTo: string;
	}

	const tools: Tool[] = [
		{
			href: '/timestamp',
			title: 'Unix Timestamp',
			description:
				'Convert between Unix timestamps and human-readable dates with real-time updates',
			icon: 'M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z',
			gradientFrom: 'from-blue-500',
			gradientTo: 'to-blue-600'
		},
		{
			href: '/base64',
			title: 'Base64 Encode/Decode',
			description: 'Encode and decode Base64 strings with instant conversion and error handling',
			icon: 'M8 9l3 3-3 3m5 0h3M5 20h14a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z',
			gradientFrom: 'from-emerald-500',
			gradientTo: 'to-emerald-600'
		},
		{
			href: '/url',
			title: 'URL Encode/Decode',
			description: 'Encode and decode URL components with proper percent encoding',
			icon: 'M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1',
			gradientFrom: 'from-amber-500',
			gradientTo: 'to-orange-500'
		},
		{
			href: '/json',
			title: 'JSON Formatter',
			description: 'Format, validate, and minify JSON data with syntax highlighting',
			icon: 'M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4',
			gradientFrom: 'from-purple-500',
			gradientTo: 'to-purple-600'
		},
		{
			href: '/uuid',
			title: 'UUID Generator',
			description: 'Generate cryptographically secure UUIDs (v4) in bulk with one click',
			icon: 'M7 7h.01M7 3h5c.512 0 1.024.195 1.414.586l7 7a2 2 0 010 2.828l-7 7a2 2 0 01-2.828 0l-7-7A1.994 1.994 0 013 12V7a4 4 0 014-4z',
			gradientFrom: 'from-pink-500',
			gradientTo: 'to-rose-500'
		},
		{
			href: '/hash',
			title: 'Hash Generator',
			description: 'Generate secure hashes using SHA-1, SHA-256, and SHA-512 algorithms',
			icon: 'M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z',
			gradientFrom: 'from-red-500',
			gradientTo: 'to-red-600'
		},
		{
			href: '/jwt',
			title: 'JWT Encoder/Decoder',
			description: 'Encode and decode JSON Web Tokens with verification and customizable payloads',
			icon: 'M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z',
			gradientFrom: 'from-indigo-500',
			gradientTo: 'to-purple-600'
		}
	];

	let searchQuery = '';
	let filteredTools = tools;

	function updateFilter() {
		console.log('updateFilter called with searchQuery:', searchQuery);

		if (!searchQuery.trim()) {
			console.log('showing all tools');
			filteredTools = tools;
		} else {
			const query = searchQuery.toLowerCase();
			filteredTools = tools.filter(
				(tool) =>
					tool.title.toLowerCase().includes(query) || tool.description.toLowerCase().includes(query)
			);
			console.log('filtered to:', filteredTools.length, 'tools');
		}
	}

	// Call updateFilter whenever searchQuery changes
	$: if (searchQuery !== undefined) updateFilter();
</script>

<div class="text-center">
	<div class="mb-16">
		<h1
			class="mb-6 bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 bg-clip-text text-5xl font-bold text-transparent"
		>
			Developer Tools
		</h1>
		<p class="mx-auto mb-8 max-w-2xl text-xl leading-relaxed text-slate-600">
			A beautiful collection of essential utilities for modern developers. Fast, secure, and
			completely client-side.
		</p>

		<!-- Search Bar -->
		<div class="relative mx-auto max-w-md">
			<div class="relative">
				<svg
					class="absolute top-1/2 left-3 h-5 w-5 -translate-y-1/2 transform text-slate-400"
					fill="none"
					stroke="currentColor"
					viewBox="0 0 24 24"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M21 21l-4.35-4.35M18 10.5a7.5 7.5 0 11-15 0 7.5 7.5 0 0115 0z"
					/>
				</svg>
				<input
					type="text"
					placeholder="Search tools..."
					bind:value={searchQuery}
					class="w-full rounded-xl border border-indigo-200 bg-white/70 py-3 pr-4 pl-10 text-slate-700 placeholder-slate-400 backdrop-blur-sm transition-all duration-200 focus:border-transparent focus:ring-2 focus:ring-indigo-500"
				/>
			</div>
		</div>
	</div>

	<!-- Debug info -->
	<!-- <div class="mb-4 text-sm text-gray-500">
		Search: "{searchQuery}" | Found: {filteredTools.length} tools
	</div> -->

	<div class="mx-auto grid max-w-6xl grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-3">
		{#each filteredTools as tool}
			<a
				href={tool.href}
				class="group block rounded-2xl border border-indigo-100 bg-white/70 p-8 shadow-xl backdrop-blur-sm transition-all duration-300 hover:-translate-y-1 hover:border-indigo-200 hover:shadow-2xl"
			>
				<div
					class="h-12 w-12 bg-gradient-to-br {tool.gradientFrom} {tool.gradientTo} mb-4 flex items-center justify-center rounded-xl transition-transform duration-300 group-hover:scale-110"
				>
					<svg class="h-6 w-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={tool.icon} />
					</svg>
				</div>
				<h3
					class="mb-3 text-xl font-semibold text-slate-900 transition-colors group-hover:text-indigo-600"
				>
					{tool.title}
				</h3>
				<p class="leading-relaxed text-slate-600">{tool.description}</p>
			</a>
		{/each}
	</div>

	{#if filteredTools.length === 0}
		<div class="py-12 text-center">
			<div
				class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-slate-100"
			>
				<svg class="h-8 w-8 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M21 21l-4.35-4.35M18 10.5a7.5 7.5 0 11-15 0 7.5 7.5 0 0115 0z"
					/>
				</svg>
			</div>
			<h3 class="mb-2 text-lg font-semibold text-slate-900">No tools found</h3>
			<p class="text-slate-600">Try adjusting your search query</p>
		</div>
	{/if}

	<div
		class="mt-16 rounded-2xl border border-indigo-100 bg-gradient-to-r from-indigo-50 to-purple-50 p-8"
	>
		<h3 class="mb-3 text-lg font-semibold text-slate-900">✨ Features</h3>
		<div class="grid grid-cols-1 gap-4 text-sm text-slate-600 md:grid-cols-3">
			<div class="flex items-center space-x-2">
				<div class="h-2 w-2 rounded-full bg-green-500"></div>
				<span>100% Client-side Processing</span>
			</div>
			<div class="flex items-center space-x-2">
				<div class="h-2 w-2 rounded-full bg-blue-500"></div>
				<span>No Data Sent to Servers</span>
			</div>
			<div class="flex items-center space-x-2">
				<div class="h-2 w-2 rounded-full bg-purple-500"></div>
				<span>Real-time Processing</span>
			</div>
		</div>
	</div>
</div>
