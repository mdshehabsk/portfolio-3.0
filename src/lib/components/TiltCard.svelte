<script lang="ts">
  import hero from '../../images/hero.png'
  let el: HTMLDivElement;
  let rotateX = $state(0);
  let rotateY = $state(0);
  let hovering = $state(false);

  const MAX_TILT = 12;

  function handleMove(e: PointerEvent) {
    const rect = el.getBoundingClientRect();
    const px = (e.clientX - rect.left) / rect.width;
    const py = (e.clientY - rect.top) / rect.height;

    rotateY = (px - 0.5) * 2 * MAX_TILT;
    rotateX = -(py - 0.5) * 2 * MAX_TILT;
  }

  function handleLeave() {
    hovering = false;
    rotateX = 0;
    rotateY = 0;
  }
</script>

<div class="relative w-full max-w-md perspective-distant">
  <div
    bind:this={el}
    role="group"
    aria-label="Interactive tilt card"
    onpointerenter={() => (hovering = true)}
    onpointermove={handleMove}
    onpointerleave={handleLeave}
    class="group relative aspect-4/5 w-full rounded-3xl border border-white/10 bg-zinc-900/60 shadow-2xl shadow-black/60 transition-transform duration-300 ease-out transform-3d z-50"
    style="transform: rotateX({rotateX}deg) rotateY({rotateY}deg) scale({hovering ? 1.03 : 1});"
  >


    <img src={hero} alt="Portfolio illustration">

  </div>
</div>
