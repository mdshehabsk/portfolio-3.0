<script lang="ts">
  import { onMount } from 'svelte';

  let container: HTMLDivElement;
  let spot = $state({ x: 50, y: 30 });

  onMount(() => {
    const onMove = (e: PointerEvent) => {
      const rect = container.getBoundingClientRect();
      spot = {
        x: ((e.clientX - rect.left) / rect.width) * 100,
        y: ((e.clientY - rect.top) / rect.height) * 100
      };
    };

    const section = container.parentElement;
    section?.addEventListener('pointermove', onMove, { passive: true });
    return () => section?.removeEventListener('pointermove', onMove);
  });
</script>

<div
  bind:this={container}
  class="pointer-events-none absolute inset-0 -z-10 overflow-hidden"
  aria-hidden="true"
>
  <!-- Drifting aurora blobs -->
  <div class="aurora aurora-a absolute -top-40 -left-40 size-[32rem] rounded-full bg-brand/15 blur-[120px]"></div>
  <div class="aurora aurora-b absolute top-1/3 -right-40 size-[28rem] rounded-full bg-brand/10 blur-[120px]"></div>

  <!-- Dot pattern, masked so it fades out toward the edges -->
  <div
    class="absolute inset-0 bg-[radial-gradient(rgba(255,255,255,0.08)_1px,transparent_1px)] [background-size:24px_24px] [mask-image:radial-gradient(ellipse_at_center,black_30%,transparent_75%)]"
  ></div>

  <!-- Cursor spotlight -->
  <div
    class="absolute inset-0 transition-opacity duration-500"
    style="background: radial-gradient(600px circle at {spot.x}% {spot.y}%, rgba(151, 255, 86, 0.08), transparent 60%);"
  ></div>
</div>

<style>
  .aurora-a {
    animation: drift-a 18s ease-in-out infinite alternate;
  }
  .aurora-b {
    animation: drift-b 22s ease-in-out infinite alternate;
  }

  @keyframes drift-a {
    from { transform: translate3d(0, 0, 0) scale(1); }
    to { transform: translate3d(12vw, 8vh, 0) scale(1.15); }
  }
  @keyframes drift-b {
    from { transform: translate3d(0, 0, 0) scale(1); }
    to { transform: translate3d(-10vw, -6vh, 0) scale(0.9); }
  }

  @media (prefers-reduced-motion: reduce) {
    .aurora-a,
    .aurora-b {
      animation: none;
    }
  }
</style>