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
			href: "/timestamp",
			title: "Unix Timestamp",
			description: "Convert between Unix timestamps and human-readable dates with real-time updates",
			icon: "M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z",
			gradientFrom: "from-blue-500",
			gradientTo: "to-blue-600"
		},
		{
			href: "/base64",
			title: "Base64 Encode/Decode",
			description: "Encode and decode Base64 strings with instant conversion and error handling",
			icon: "M8 9l3 3-3 3m5 0h3M5 20h14a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z",
			gradientFrom: "from-emerald-500",
			gradientTo: "to-emerald-600"
		},
		{
			href: "/url",
			title: "URL Encode/Decode",
			description: "Encode and decode URL components with proper percent encoding",
			icon: "M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1",
			gradientFrom: "from-amber-500",
			gradientTo: "to-orange-500"
		},
		{
			href: "/json",
			title: "JSON Formatter",
			description: "Format, validate, and minify JSON data with syntax highlighting",
			icon: "M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4",
			gradientFrom: "from-purple-500",
			gradientTo: "to-purple-600"
		},
		{
			href: "/uuid",
			title: "UUID Generator",
			description: "Generate cryptographically secure UUIDs (v4) in bulk with one click",
			icon: "M7 7h.01M7 3h5c.512 0 1.024.195 1.414.586l7 7a2 2 0 010 2.828l-7 7a2 2 0 01-2.828 0l-7-7A1.994 1.994 0 013 12V7a4 4 0 014-4z",
			gradientFrom: "from-pink-500",
			gradientTo: "to-rose-500"
		},
		{
			href: "/hash",
			title: "Hash Generator",
			description: "Generate secure hashes using SHA-1, SHA-256, and SHA-512 algorithms",
			icon: "M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z",
			gradientFrom: "from-red-500",
			gradientTo: "to-red-600"
		}
	];

	let searchQuery = "";
	let filteredTools = tools;

	function updateFilter() {
		console.log('updateFilter called with searchQuery:', searchQuery);
		
		if (!searchQuery.trim()) {
			console.log('showing all tools');
			filteredTools = tools;
		} else {
			const query = searchQuery.toLowerCase();
			filteredTools = tools.filter(tool => 
				tool.title.toLowerCase().includes(query) ||
				tool.description.toLowerCase().includes(query)
			);
			console.log('filtered to:', filteredTools.length, 'tools');
		}
	}

	// Call updateFilter whenever searchQuery changes
	$: if (searchQuery !== undefined) updateFilter();
</script>

<div class="text-center">
	<div class="mb-16">
		<h1 class="text-5xl font-bold bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 bg-clip-text text-transparent mb-6">
			Developer Tools
		</h1>
		<p class="text-xl text-slate-600 max-w-2xl mx-auto leading-relaxed mb-8">
			A beautiful collection of essential utilities for modern developers. Fast, secure, and completely client-side.
		</p>
		
		<!-- Search Bar -->
		<div class="max-w-md mx-auto relative">
			<div class="relative">
				<svg class="absolute left-3 top-1/2 transform -translate-y-1/2 w-5 h-5 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-4.35-4.35M18 10.5a7.5 7.5 0 11-15 0 7.5 7.5 0 0115 0z"/>
				</svg>
				<input
					type="text"
					placeholder="Search tools..."
					bind:value={searchQuery}
					class="w-full pl-10 pr-4 py-3 bg-white/70 backdrop-blur-sm border border-indigo-200 rounded-xl focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-200 text-slate-700 placeholder-slate-400"
				/>
			</div>
		</div>
	</div>

	<!-- Debug info -->
	<div class="mb-4 text-sm text-gray-500">
		Search: "{searchQuery}" | Found: {filteredTools.length} tools
	</div>

	<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 max-w-6xl mx-auto">
		{#each filteredTools as tool}
			<a href={tool.href} class="group block p-8 bg-white/70 backdrop-blur-sm rounded-2xl shadow-xl border border-indigo-100 hover:shadow-2xl hover:border-indigo-200 transition-all duration-300 hover:-translate-y-1">
				<div class="w-12 h-12 bg-gradient-to-br {tool.gradientFrom} {tool.gradientTo} rounded-xl flex items-center justify-center mb-4 group-hover:scale-110 transition-transform duration-300">
					<svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={tool.icon}/>
					</svg>
				</div>
				<h3 class="text-xl font-semibold text-slate-900 mb-3 group-hover:text-indigo-600 transition-colors">{tool.title}</h3>
				<p class="text-slate-600 leading-relaxed">{tool.description}</p>
			</a>
		{/each}
	</div>

	{#if filteredTools.length === 0}
		<div class="text-center py-12">
			<div class="w-16 h-16 bg-slate-100 rounded-full flex items-center justify-center mx-auto mb-4">
				<svg class="w-8 h-8 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-4.35-4.35M18 10.5a7.5 7.5 0 11-15 0 7.5 7.5 0 0115 0z"/>
				</svg>
			</div>
			<h3 class="text-lg font-semibold text-slate-900 mb-2">No tools found</h3>
			<p class="text-slate-600">Try adjusting your search query</p>
		</div>
	{/if}

	<div class="mt-16 p-8 bg-gradient-to-r from-indigo-50 to-purple-50 rounded-2xl border border-indigo-100">
		<h3 class="text-lg font-semibold text-slate-900 mb-3">✨ Features</h3>
		<div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-sm text-slate-600">
			<div class="flex items-center space-x-2">
				<div class="w-2 h-2 bg-green-500 rounded-full"></div>
				<span>100% Client-side Processing</span>
			</div>
			<div class="flex items-center space-x-2">
				<div class="w-2 h-2 bg-blue-500 rounded-full"></div>
				<span>No Data Sent to Servers</span>
			</div>
			<div class="flex items-center space-x-2">
				<div class="w-2 h-2 bg-purple-500 rounded-full"></div>
				<span>Real-time Processing</span>
			</div>
		</div>
	</div>
</div>
