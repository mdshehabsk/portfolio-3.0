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
      role: 'Full-Stack Lead Developer ',
      period: 'Jan 2026 — Present',
      location: "Remmote - Dhaka, Bangladesh",
      summary: 'Led technical decisions for a multi-vendor e-commerce platform.',
      highlights: [
        'Led backend architecture, database design, ORM, theme rendering, and deployment strategy.',
        'Built payments, delivery, SMS, webhook, and order workflow integrations.',
        'Implemented custom-domain routing and automated SSL provisioning for vendor storefronts.',
        'Built CI/CD and VPS deployment workflows and handled production troubleshooting.'
      ],
      products: [{ name: 'Multi-vendor e-commerce platform', url: 'https://qubegrid.com/' }]
    },
    {
      company: 'Themespell',
      role: 'WordPress Developer',
      period: 'Feb 2025 — Oct 2026',
      location: 'Remmote - Dhaka, Bangladesh',
      summary: 'Built WordPress themes, plugins, and WooCommerce product features.',
      highlights: [
        'Developed classic themes with Elementor integration and add-ons.',
        'Built and maintained the TS Team plugin for showcasing team members.',
        'Developed and improved TS Product Showcase, a WooCommerce plugin for managing products.',
        'Delivered client-focused WordPress, Elementor, and WooCommerce solutions.'
      ],
      products: [
        { name: 'TS Team Plugin', url: 'https://themespell.com/ts-team-plugin/' },
        { name: 'TS Product Showcase', url: 'https://themespell.com/ts-product-showcase/' }
      ]
    },
    {
      company: 'Kodezen',
      role: 'React.js Developer',
      period: 'Feb 2024 — Nov 2024',

      location: 'Remmote - Jhenaidaha, Bangladesh',
      summary: 'Built Gutenberg blocks and scalable frontend architecture.',
      highlights: [
        'Developed Gutenberg-based WordPress products with 10+ custom blocks.',
        'Solved complex frontend challenges and designed scalable, performant architecture.',
        'Built reusable UI components across the product ecosystem.',
        'Mentored junior developers through code reviews and technical guidance.'
      ],
      products: [{ name: 'Gutenberg Blocks for WordPress', url: 'https://ablocks.pro/' }]
    }
  ];
let section: HTMLElement;
  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);

    const ctx = gsap.context(() => {
      gsap.from('[data-exp-heading]', {
        opacity: 0,
        y: 30,
        duration: 0.9,
        ease: 'power3.out',
        scrollTrigger: { trigger: '[data-exp-heading]', start: 'top 85%' }
      });

      gsap.from('[data-exp-item]', {
        opacity: 0,
        x: -40,
        duration: 0.8,
        ease: 'power3.out',
        stagger: 0.2,
        scrollTrigger: { trigger: '[data-exp-list]', start: 'top 75%' }
      });

      gsap.fromTo(
        '[data-exp-line]',
        { scaleY: 0 },
        {
          scaleY: 1,
          ease: 'none',
          transformOrigin: 'top center',
          scrollTrigger: {
            trigger: '[data-exp-list]',
            start: 'top 70%',
            end: 'bottom 60%',
            scrub: true
          }
        }
      );
    }, '#experience');

    return () => ctx.revert();
  });
</script>



<section
  bind:this={section}
  id="experience"
  class="relative isolate overflow-hidden py-28"
>
  <ExperienceBackdrop />

  <div class="relative mx-auto max-w-7xl px-6 lg:px-10">
    <!-- Heading -->
    <div class="mb-20">
      <p class="font-mono text-xs tracking-[0.25em] text-brand uppercase">03 — Experience</p>
      <h2 data-exp-heading class="mt-6 font-display text-5xl font-semibold tracking-tight text-white sm:text-6xl">
        Where I've <span class="text-brand">shipped.</span>
      </h2>
    </div>

    <!-- Jobs -->
    <div data-exp-list>
      {#each jobs as job, i (i)}
        <article
          data-exp-job
          class="group relative  gap-6 py-12 flex justify-between md:gap-12"
        >
          <!-- Animated top rule -->
          <span
            data-exp-rule
            class="absolute inset-x-0 top-0 h-px bg-linear-to-r from-brand/60 via-white/10 to-transparent filter drop-shadow-lg"
          ></span>




          <!-- Left: duration + location -->
          <div class="md:sticky md:top-32 md:self-start basis-3/12">
            <p class="font-mono text-xs tracking-wider text-brand uppercase">{job.period}</p>
            <p class="mt-2 font-body text-sm text-zinc-500">{job?.location}</p>
          </div>
git remote get-url origin >/dev/null 2>&1 \
  && git remote set-url origin git@github.com:mdshehabsk/portfolio-3.0.git \
  || git remote add origin git@github.com:mdshehabsk/portfolio-3.0.git

git status
git add .
git commit -m "Update portfolio"
git push -u origin HEAD
          <!-- Right: role, company, details -->
          <div class="">
            <h3
              class="font-display text-3xl font-semibold text-white transition-colors duration-300 group-hover:text-brand sm:text-4xl"
            >
              {job.role}
            </h3>
            <p class="mt-2 font-display text-xl text-zinc-300">{job.company}</p>

            <p class="mt-5 max-w-2xl font-body text-zinc-400">{job.summary}</p>

            <ul class="mt-6 space-y-3">
              {#each job.highlights as point,i (i)}
                <li class="flex gap-3 font-body text-sm leading-relaxed text-zinc-400">
                  <span class="mt-2 size-1 shrink-0 rounded-full bg-brand/60"></span>
                  {point}
                </li>
              {/each}
            </ul>

            {#if job.products.length}
              <div class="mt-8 flex flex-wrap gap-3">
                {#each job.products as product,i (i)}
                  <a
                    href={product.url}
                    target="_blank"
                    rel="noopener noreferrer"
                    class="group/link inline-flex items-center gap-2 rounded-full border border-white/10 bg-white/[0.03] px-4 py-2 font-body text-sm text-zinc-200 transition-all duration-300 hover:-translate-y-0.5 hover:border-brand hover:text-brand"
                  >
                    {product.name}
                    <svg
                      class="size-3.5 transition-transform duration-300 group-hover/link:translate-x-0.5 group-hover/link:-translate-y-0.5"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2.5"
                    >
                      <path d="M7 17 17 7M9 7h8v8" stroke-linecap="round" stroke-linejoin="round" />
                    </svg>
                  </a>
                {/each}
              </div>
            {/if}
          </div>
        </article>
      {/each}
    </div>
  </div>
</section>