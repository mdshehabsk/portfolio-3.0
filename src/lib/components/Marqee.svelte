<script lang="ts">
  type Skill = { label: string; highlight?: boolean };

  const skills: Skill[] = [
    { label: 'React.js', highlight: true },
    { label: 'Next.js' },
    { label: 'TypeScript', highlight: true },
    { label: 'Node.js' },
    { label: 'NestJS' },
    { label: 'PostgreSQL' },
    { label: 'Prisma' },
    { label: 'Docker', highlight: true },
    { label: 'GitHub Actions' },
    { label: 'CI/CD' },
    { label: 'AWS', highlight: true },
    { label: 'Nginx' },
    { label: 'Prometheus' },
    { label: 'Grafana' },
    { label: 'Socket.io' },
    { label: 'Linux' }
  ];

  let paused = $state(false);

  // Duplicate the list so the loop is seamless
  const items = [...skills, ...skills];
</script>

<section
  class="relative overflow-hidden border-y border-white/10 bg-zinc-950/80 py-6"
  aria-label="Skills marquee"
>
  <div class="flex items-center">
    <!-- Scrolling track -->
    <div
      class="marquee-track flex w-max shrink-0 items-center gap-6 whitespace-nowrap"
      class:paused
    >
      {#each items as skill, i (i)}
        <span
          class="flex items-center gap-6 font-display text-xl font-semibold uppercase tracking-wide sm:text-2xl {skill.highlight
            ? 'text-brand'
            : 'text-zinc-500'}"
        >
          {skill.label}
          <span class="text-zinc-700" aria-hidden="true">/</span>
        </span>
      {/each}
    </div>

    <!-- Pause / play control -->
    <button
      type="button"
      onclick={() => (paused = !paused)}
      aria-pressed={paused}
      class="absolute right-4 top-1/2 flex -translate-y-1/2 items-center gap-2 rounded-full border border-white/15 bg-base/90 px-4 py-2 font-body text-sm text-zinc-200 backdrop-blur-md transition-colors hover:border-brand hover:text-brand"
    >
      {#if paused}
        <svg class="size-3.5" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M8 5v14l11-7z" />
        </svg>
        Play motion
      {:else}
        <svg class="size-3.5" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
          <path d="M6 5h4v14H6zM14 5h4v14h-4z" />
        </svg>
        Pause motion
      {/if}
    </button>
  </div>

  <!-- Edge fades so items dissolve into the background -->
  <div class="pointer-events-none absolute inset-y-0 left-0 w-24 bg-linear-to-r from-zinc-950 to-transparent"></div>
  <div class="pointer-events-none absolute inset-y-0 right-0 w-24 bg-linear-to-l from-zinc-950 to-transparent"></div>
</section>

<style>
  .marquee-track {
    animation: marquee 30s linear infinite;
    will-change: transform;
    backface-visibility: hidden;
  }

  .marquee-track.paused {
    animation-play-state: paused;
  }

  @keyframes marquee {
    from {
      transform: translateX(0);
    }
    to {
      transform: translateX(-50%);
    }
  }

  @media (prefers-reduced-motion: reduce) {
    .marquee-track {
      animation: none;
    }
  }
</style>