<script lang="ts">
	import { resolve } from '$app/paths';
  import { onMount } from 'svelte';

  let open = $state(false);
  let scrolled = $state(false);

  const links = [
    { href: '#work', label: 'Work' },
    { href: '#about', label: 'About' },
    { href: '#experience', label: 'Experience' },
    { href: '#contact', label: 'Contact' }
  ];

  onMount(() => {
    const onScroll = () => (scrolled = window.scrollY > 20);
    onScroll();
    window.addEventListener('scroll', onScroll, { passive: true });
    return () => window.removeEventListener('scroll', onScroll);
  });
</script>

<header
  class="fixed inset-x-0 top-0 z-50 transition-all duration-300 {scrolled
    ? 'border-b border-white/5 bg-base/80 backdrop-blur-md'
    : 'border-b border-transparent'}"
>
  <nav class="mx-auto flex max-w-7xl items-center justify-between px-6 py-5 lg:px-10">
    <a href={resolve("/")} class="font-display text-2xl font-bold tracking-tight text-white">
      SU<span class="text-brand">/</span>
    </a>

    <ul class="hidden items-center gap-8 md:flex">
      {#each links as link (link.href)}
        <li>
          <a href={link.href} class="font-body text-sm text-zinc-400 transition-colors hover:text-brand">
            {link.label}
          </a>
        </li>
      {/each}
    </ul>

    <a
      href="https://cal.com/mdshehabsk"
      class="hidden items-center gap-2 rounded-full bg-brand px-5 py-2.5 font-body text-sm font-semibold text-base transition-transform hover:scale-105 md:inline-flex"
    >
      Let's talk
      <svg class="size-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
        <path d="M7 17 17 7M9 7h8v8" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </a>

    <button
      class="inline-flex size-10 items-center justify-center rounded-full border border-white/10 text-white md:hidden"
      aria-label="Toggle menu"
      aria-expanded={open}
      onclick={() => (open = !open)}
    >
      {#if open}
        <svg class="size-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 6l12 12M18 6L6 18" stroke-linecap="round" />
        </svg>
      {:else}
        <svg class="size-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M4 7h16M4 12h16M4 17h10" stroke-linecap="round" />
        </svg>
      {/if}
    </button>
  </nav>

  {#if open}
    <div class="border-t border-white/5 bg-base/95 backdrop-blur-md md:hidden">
      <ul class="flex flex-col px-6 py-4">
        {#each links as link (link.href)}
          <li>
            <a
              href={link.href}
              onclick={() => (open = false)}
              class="block border-b border-white/5 py-4 font-body text-lg text-zinc-300 hover:text-brand"
            >
              {link.label}
            </a>
          </li>
        {/each}
      </ul>
    </div>
  {/if}
</header>
