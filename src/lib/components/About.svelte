<script lang="ts">
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';

  let section: HTMLElement;

  const pills = [
    { group: 'AI & integrations', tags: ['AI Integration', 'Payment Integration', 'AI Automation', 'OpenAI'] },
    { group: 'Mobile & interface', tags: ['Flutter', 'React Native', 'React', 'UI/UX design', 'Custom widgets', 'Animations'] },
    { group: 'Architecture & data', tags: ['Provider · BLoC · GetX', 'GetIt', 'REST APIs', 'Firebase', 'Node.js', 'Freezed · json_serializable'] },
    { group: 'Quality & delivery', tags: ['Git', 'CI/CD', 'DevTools', 'Performance optimization', 'Testing & debugging', 'Localization'] }
  ];

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);

    const ctx = gsap.context(() => {
      gsap.from('[data-about-reveal]', {
        opacity: 0,
        y: 30,
        duration: 0.9,
        ease: 'power3.out',
        stagger: 0.12,
        scrollTrigger: { trigger: section, start: 'top 75%' }
      });

      gsap.from('[data-about-row]', {
        opacity: 0,
        x: -20,
        duration: 0.6,
        ease: 'power2.out',
        stagger: 0.08,
        scrollTrigger: { trigger: '[data-about-rows]', start: 'top 80%' }
      });
    }, section);

    return () => ctx.revert();
  });
</script>

<section
  bind:this={section}
  id="about"
  class="relative mx-auto max-w-7xl px-6 py-28 lg:px-10"
>
  <div class="grid gap-16 lg:grid-cols-[1fr_1.4fr] lg:gap-20">
    <!-- Left: heading -->
    <div>
      <p data-about-reveal class="font-mono text-xs tracking-[0.25em] text-brand uppercase">
        02 — Behind the code
      </p>
      <h2 data-about-reveal class="mt-6 font-display text-6xl leading-[0.95] font-semibold tracking-tight text-white sm:text-7xl">
        A little
        <br />
        about <span class="text-brand">me.</span>
      </h2>
      <a
        data-about-reveal
        href="https://github.com/your-username"
        target="_blank"
        rel="noopener noreferrer"
        class="group mt-10 inline-flex items-center gap-3 border-b border-brand/40 pb-1 font-body text-sm text-zinc-300 transition-colors hover:border-brand hover:text-brand"
      >
        Find me on GitHub
        <svg class="size-4 transition-transform duration-300 group-hover:translate-x-1 group-hover:-translate-y-1" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M7 17 17 7M9 7h8v8" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </a>
    </div>

    <!-- Right: intro + skill rows -->
    <div>
      <h3 data-about-reveal class="font-display text-3xl leading-snug font-medium text-zinc-100 sm:text-4xl">
        Good apps start with people.
        <br />
        I build with both the <span class="text-brand">user and the code</span> in mind.
      </h3>

      <div class="mt-8 grid gap-6 font-body text-base leading-relaxed text-zinc-400 md:grid-cols-2">
        <p data-about-reveal>
          I'm a Full Stack Engineer with 5 years of experience building and shipping production React websites,
          Node.js backends and mobile apps with Flutter and React Native.
        </p>
        <p data-about-reveal>
          From custom interfaces and clean architecture to APIs, native features, and store releases, I enjoy
          bringing the whole product together. My work combines frontend and backend expertise with a practical
          understanding of deployment.
        </p>
      </div>

      <div data-about-rows class="mt-12">
        {#each pills as row, i (i)}
          <div
            data-about-row
            class="group relative grid gap-4 border-t border-white/10 py-6 transition-colors duration-300 last:border-b hover:bg-white/[0.02] md:grid-cols-[220px_1fr] md:gap-8 {i === 0 ? 'border-l-2 border-l-brand pl-5' : 'pl-0'}"
          >
            <div class="flex items-center gap-3">
              <span class="font-mono text-xs text-zinc-600">0{i + 1}</span>
              <h4 class="font-display text-base font-medium text-zinc-200 transition-colors group-hover:text-brand">
                {row.group}
              </h4>
            </div>
            <div class="flex flex-wrap gap-2">
              {#each row.tags as tag (tag)}
                <span
                  class="rounded-md border px-3 py-1.5 font-body text-sm transition-all duration-300 hover:-translate-y-0.5 {i === 0
                    ? 'border-brand/40 bg-brand/10 text-brand hover:bg-brand/20'
                    : 'border-white/10 bg-white/[0.03] text-zinc-400 hover:border-brand/40 hover:text-zinc-100'}"
                >
                  {tag}
                </span>
              {/each}
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>