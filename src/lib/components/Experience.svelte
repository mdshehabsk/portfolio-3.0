<script lang="ts">
	import { onMount } from 'svelte';
	import { gsap } from 'gsap';
	import { ScrollTrigger } from 'gsap/ScrollTrigger';
	import ExperienceBackdrop from './ExperienceBackdrop.svelte';

	type Product = { name: string; url: string };
	type Job = {
		company: string;
		role: string;
		period: string;
		location: string;
		summary: string;
		highlights: string[];
		products: Product[];
	};

	const jobs: Job[] = [
		{
			company: 'Qubegrid',
			role: 'Full-Stack Lead Developer',
			period: 'Jan 2026 — Present',
			location: 'Remote · Dhaka, Bangladesh',
			summary: 'Led technical decisions for a multi-vendor e-commerce platform.',
			highlights: [
				'Led backend architecture, database design, ORM, theme rendering, and deployment strategy.',
				'Built payments, delivery, SMS, webhook, and order workflow integrations.',
				'Implemented custom-domain routing and automated SSL provisioning for vendor storefronts.',
				'Built CI/CD and VPS deployment workflows and handled production troubleshooting.',
        "Implemented monitoring tools and logging for performance and error tracking."
			],
			products: [{ name: 'Multi-vendor e-commerce - Qubegrid', url: 'https://qubegrid.com/' }]
		},
		{
			company: 'Themespell',
			role: 'WordPress Developer',
			period: 'Feb 2025 — Oct 2026',
			location: 'Remote · Dhaka, Bangladesh',
			summary: 'Built WordPress themes, plugins, and WooCommerce product features.',
			highlights: [
				'Developed classic themes with Elementor integration and add-ons.',
				'Built and maintained the TS Team plugin for showcasing team members.',
				'Developed and improved TS Product Showcase, a WooCommerce plugin for managing products.',
				'Delivered client-focused WordPress, Elementor, and WooCommerce solutions.'
			],
			products: [
				{ name: 'TS Team Plugin', url: 'https://themespell.com/ts-team-member/' },
				{ name: 'TS Product Showcase', url: 'https://themespell.com/ts-product-showcase/' }
			]
		},
		{
			company: 'Kodezen',
			role: 'React.js Developer',
			period: 'Feb 2024 — Nov 2024',
			location: 'Remote · Jhenaidah, Bangladesh',
			summary: 'Built Gutenberg blocks and scalable frontend architecture.',
			highlights: [
				'Developed Gutenberg-based WordPress products with 10+ custom blocks.',
				'Solved complex frontend challenges and designed scalable, performant architecture.',
				'Built reusable UI components across the product ecosystem.',
				'Mentored junior developers through code reviews and technical guidance.'
			],
			products: [{ name: 'Ablocks', url: 'https://ablocks.pro/' }]
		}
	];

	let section: HTMLElement;
	let timelineContainer: HTMLElement;
	let progress: HTMLElement;

	onMount(() => {
		gsap.registerPlugin(ScrollTrigger);

		const ctx = gsap.context(() => {
			// Heading animation
			gsap.from('[data-exp-heading]', {
				opacity: 0,
				y: 30,
				duration: 0.8,
				ease: 'power3.out',
				scrollTrigger: { trigger: '[data-exp-heading]', start: 'top 85%' }
			});

			// Job cards animation
			gsap.from('[data-exp-job]', {
				opacity: 0,
				x: 30,
				duration: 0.7,
				ease: 'power3.out',
				stagger: 0.15,
				scrollTrigger: { trigger: timelineContainer, start: 'top 80%' }
			});

			// Timeline progress line
			gsap.fromTo(
				progress,
				{ height: 0 },
				{
					height: '100%',
					ease: 'none',
					scrollTrigger: {
						trigger: timelineContainer,
						start: 'top 40%',
						end: 'bottom 50%',
						scrub: 0.5
					}
				}
			);

			// Timeline dots pop in
			gsap.utils.toArray<HTMLElement>('[data-exp-dot]').forEach((dot) => {
				gsap.from(dot, {
					scale: 0,
					opacity: 0,
					duration: 0.5,
					ease: 'back.out(1.5)',
					scrollTrigger: { trigger: dot, start: 'top 85%' }
				});
			});
		}, section);

		return () => ctx.revert();
	});
</script>

<section bind:this={section} id="experience" class="relative overflow-hidden py-24">
	<div class="relative mx-auto max-w-6xl px-6 lg:px-8">
		<!-- Heading -->
		<div class="mb-16 md:mb-24">
			<p class="font-mono text-xs font-semibold tracking-[0.2em] text-brand uppercase">
				03 — Experience
			</p>
			<h2 data-exp-heading class="mt-4 text-4xl font-bold tracking-tight text-white md:text-5xl">
				Where I've <span class="text-brand">Shipped</span>
			</h2>
		</div>

		<!-- Timeline Container -->
		<div bind:this={timelineContainer} class=" relative pl-8 md:pl-0">
			<!-- Timeline Track (background line) -->
			<div class="absolute top-0 bottom-0 left-0 w-px bg-zinc-700/60"></div>

			<!-- Timeline Progress (animated line) -->
			<div
				bind:this={progress}
				class="absolute top-0 left-0 w-px bg-linear-to-b from-brand to-[#97ff5640]"
				style="box-shadow: 0 0 20px rgba(151, 255, 86, 0.6);"
			></div>

			<!-- Jobs List -->
			<div class="space-y-8 md:space-y-12 md:pl-0">
				{#each jobs as job, i (i)}
					<article data-exp-job class="relative">
						<!-- Desktop Layout: Grid -->
						<div class="hidden md:grid md:grid-cols-12 md:gap-8">
							<!-- Left Section: Timeline Dot + Duration/Location -->
							<div class="relative md:col-span-3">
								<!-- Timeline Dot – now perfectly centered on the track -->
								<div
									data-exp-dot
									class="absolute top-2 left-0 z-10 h-4 w-4 -translate-x-1/2 rounded-full border-2 border-brand bg-zinc-900"
								></div>

								<!-- Duration and Location – pushed right so it never overlaps the line -->
								<div class="sticky top-24 pl-6">
									<p class="font-mono text-xs font-semibold tracking-wide text-brand uppercase">
										{job.period}
									</p>
									<p class="mt-2 text-sm text-gray-400">{job.location}</p>
								</div>
							</div>

							<!-- Right Section: Job Card -->
							<div class="md:col-span-9">
								<div
									class="rounded-lg border border-zinc-700/50 bg-linear-to-br from-zinc-900/30 to-zinc-950/50 p-6 backdrop-blur-sm transition-all duration-300 hover:border-zinc-600/80 md:p-8"
								>
									<h3 class="text-2xl font-bold text-white md:text-3xl">
										{job.role}
									</h3>
									<p class="mt-2 text-lg font-semibold text-zinc-300">{job.company}</p>
									<p class="mt-4 max-w-2xl text-sm leading-relaxed text-gray-400">
										{job.summary}
									</p>

									<ul class="mt-6 space-y-3">
										{#each job.highlights as highlight, j (j)}
											<li class="flex gap-3 text-sm leading-relaxed text-gray-300">
												<span class="mt-1.5 h-1.5 w-1.5 shrink-0 rounded-full bg-brand"></span>
												<span>{highlight}</span>
											</li>
										{/each}
									</ul>

									{#if job.products.length}
										<div class="mt-6 flex flex-wrap gap-3">
											{#each job.products as product, i (i)}
												<a
													href={product.url}
													target="_blank"
													rel="noopener noreferrer"
													class="inline-flex items-center gap-2 rounded-md border border-zinc-600 bg-zinc-800/40 px-3 py-2 text-xs font-medium text-zinc-200 transition-all duration-300 hover:border-brand hover:bg-zinc-800/60 hover:text-brand md:text-sm"
												>
													{product.name}
													<svg
														class="h-3 w-3"
														fill="none"
														stroke="currentColor"
														stroke-width="2"
														viewBox="0 0 24 24"
													>
														<path
															d="M7 17L17 7M9 7h8v8"
															stroke-linecap="round"
															stroke-linejoin="round"
														/>
													</svg>
												</a>
											{/each}
										</div>
									{/if}
								</div>
							</div>
						</div>

						<!-- Mobile Layout -->
						<div class="md:hidden">
							<div
								class="rounded-lg border border-zinc-700/50 bg-linear-to-br from-zinc-900/30 to-zinc-950/50 p-4 backdrop-blur-sm"
							>
								<div class="flex items-start gap-3">
									<div data-exp-dot class="mt-1 h-3 w-3 shrink-0 rounded-full bg-brand"></div>
									<div class="flex-1">
										<h3 class="text-lg font-bold text-white">
											{job.role}
										</h3>
										<p class="mt-1 text-sm font-semibold text-zinc-300">{job.company}</p>
										<p class="mt-1 text-xs font-semibold tracking-wide text-brand uppercase">
											{job.period}
										</p>
										<p class="mt-1 text-xs text-gray-400">{job.location}</p>
										<p class="mt-3 text-xs leading-relaxed text-gray-400">
											{job.summary}
										</p>

										<ul class="mt-3 space-y-2">
											{#each job.highlights as highlight, j (j)}
												<li class="flex gap-2 text-xs leading-relaxed text-gray-300">
													<span class="mt-1 h-1 w-1 shrink-0 rounded-full bg-brand"></span>
													<span>{highlight}</span>
												</li>
											{/each}
										</ul>

										{#if job.products.length}
											<div class="mt-4 flex flex-wrap gap-2">
												{#each job.products as product, i (i)}
													<a
														href={product.url}
														target="_blank"
														rel="noopener noreferrer"
														class="inline-flex items-center gap-1 rounded-md border border-zinc-600 bg-zinc-800/40 px-2 py-1 text-xs font-medium text-zinc-200 transition-all duration-300 hover:border-brand hover:bg-zinc-800/60 hover:text-brand"
													>
														{product.name}
														<svg
															class="h-2.5 w-2.5"
															fill="none"
															stroke="currentColor"
															stroke-width="2"
															viewBox="0 0 24 24"
														>
															<path
																d="M7 17L17 7M9 7h8v8"
																stroke-linecap="round"
																stroke-linejoin="round"
															/>
														</svg>
													</a>
												{/each}
											</div>
										{/if}
									</div>
								</div>
							</div>
						</div>
					</article>
				{/each}
			</div>
		</div>
	</div>

  <ExperienceBackdrop/>
</section>
