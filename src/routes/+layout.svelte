<script lang="ts">
	import '../app.css';

	let { children } = $props();
</script>

<div class="min-h-screen bg-gradient-to-br from-indigo-50 via-white to-purple-50">
	<header class="border-b border-indigo-100 bg-white/80 shadow-lg backdrop-blur-md">
		<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
			<div class="flex h-16 items-center justify-between">
				<div class="flex items-center space-x-3">
					<div
						class="flex h-8 w-8 items-center justify-center rounded-lg bg-gradient-to-br from-indigo-600 to-purple-600"
					>
						<svg class="h-5 w-5 text-white" fill="currentColor" viewBox="0 0 20 20">
							<path
								d="M13 6a3 3 0 11-6 0 3 3 0 016 0zM18 8a2 2 0 11-4 0 2 2 0 014 0zM14 15a4 4 0 00-8 0v3h8v-3z"
							/>
						</svg>
					</div>
					<h1
						class="bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-xl font-bold text-transparent"
					>
						DevTools
					</h1>
				</div>
				<nav class="hidden space-x-6 md:flex">
					<a
						href="/"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>Home</a
					>
					<a
						href="/timestamp"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>Timestamp</a
					>
					<a
						href="/base64"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>Base64</a
					>
					<a
						href="/url"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>URL</a
					>
					<a
						href="/json"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>JSON</a
					>
					<a
						href="/uuid"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>UUID</a
					>
					<a
						href="/hash"
						class="rounded-lg px-3 py-2 text-slate-600 transition-all duration-200 hover:bg-indigo-50 hover:text-indigo-600"
						>Hash</a
					>
				</nav>
			</div>
		</div>
	</header>

	<main class="mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8">
		{@render children()}
	</main>
</div>
