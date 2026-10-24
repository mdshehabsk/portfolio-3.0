<script lang="ts">
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import GridBackground from './GridBackground.svelte';
  import TiltCard from './TiltCard.svelte';
  let badge: HTMLElement;
  let firstName: HTMLElement;
  let lastName: HTMLElement;
  let role: HTMLElement;
  let desc: HTMLElement;
  let ctas: HTMLElement;
  let card: HTMLElement;

  onMount(() => {
    const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });

    tl.from(badge, { opacity: 0, y: 16, duration: 0.6 })
      .from([firstName, lastName], { yPercent: 110, opacity: 0, duration: 1, stagger: 0.12 }, '-=0.3')
      .from([role, desc], { opacity: 0, y: 20, duration: 0.7, stagger: 0.15 }, '-=0.6')
      .from(ctas, { opacity: 0, y: 20, duration: 0.6 }, '-=0.4')
      .from(card, { opacity: 0, x: 60, rotate: 4, duration: 1.2 }, '-=1');

    return () => tl.kill();
  });
</script>

<section class="relative min-h-screen overflow-hidden pt-32 pb-20">
  <GridBackground  />

  <div class="relative z-10 mx-auto grid max-w-7xl items-center gap-12 px-6 lg:grid-cols-2 lg:px-10">
    <!-- Left: information -->
    <div>
      <span
        bind:this={badge}
        class="inline-flex items-center gap-2 rounded-full border border-brand/30 bg-brand/5 px-4 py-1.5 font-body text-xs font-medium uppercase tracking-[0.2em] text-brand"
      >
        <span class="size-1.5 animate-pulse rounded-full bg-brand"></span>
        Available for work
      </span>

      <h1 class="mt-6 font-display text-6xl leading-[0.9] font-bold tracking-tight text-white sm:text-7xl lg:text-8xl">
        <span class="block overflow-hidden pb-2">
          <span bind:this={firstName} class="block">Shehab</span>
        </span>
        <span class="block overflow-hidden pb-2">
          <span bind:this={lastName} class="block text-brand">Uddin.</span>
        </span>
      </h1>

      <p bind:this={role} class="mt-6 font-display text-2xl font-medium text-zinc-200 sm:text-3xl">
        Full Stack Developer & DevOps Engineer
      </p>

      <p bind:this={desc} class="mt-4 max-w-xl font-body  leading-relaxed text-zinc-400 sm:text-lg">
        I build fast, scalable web applications, from pixel-perfect interfaces to robust APIs, and ship them
        with automated pipelines, load balancing, and cloud infrastructure built to stay up.
      </p>

      <div bind:this={ctas} class="mt-10 flex flex-wrap gap-4">
        <a
          href="https://drive.google.com/file/d/1ACta0TV1ym4VnF5QJtc6P3mXQa6onPjh/view?usp=sharing"
          target="_blank"
          class="inline-flex items-center gap-3 rounded-full bg-brand py-3 pr-3 pl-6 font-body font-semibold text-base transition-transform hover:scale-105"
        >
          View my resume
          <span class="flex size-8 items-center justify-center rounded-full bg-base text-brand">
            <svg class="size-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
              <path d="M7 17 17 7M9 7h8v8" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </span>
        </a>
        <a
          href="https://cal.com/mdshehabsk"
          class="  inline-flex items-center rounded-full border border-white/15 px-6 py-3 font-body font-medium text-white transition-colors hover:border-brand hover:text-brand"
        >
          Book a meeting
        </a>
      </div>
    </div>

    <!-- Right: 3D tilt card -->
    <div bind:this={card} class="flex justify-center lg:justify-end">
      <TiltCard  />
    </div>
  </div>
</section>
