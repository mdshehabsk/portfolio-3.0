<script lang="ts">
  import { onMount } from 'svelte';
  let { paused = false }: { paused?: boolean } = $props();  
  let container: HTMLDivElement;
  let canvas: HTMLCanvasElement;

  onMount(() => {
    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    const CELL = 56;        // grid cell size in px
    const RADIUS = 240;     // influence radius around cursor
    const PULL = 28;        // how far lines bend toward the cursor
    const STEP = 6;         // sampling step for smooth curves
    const BRAND = '151, 255, 86';
    const FAR = -10000;

    let width = 0;
    let height = 0;
    let target = { x: FAR, y: FAR };
    let current = { x: FAR, y: FAR };
    let frame = 0;

    const resize = () => {
      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      const rect = container.getBoundingClientRect();
      width = rect.width;
      height = rect.height;
      canvas.width = width * dpr;
      canvas.height = height * dpr;
      canvas.style.width = `${width}px`;
      canvas.style.height = `${height}px`;
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    };

    const onPointerMove = (e: PointerEvent) => {
       if (paused) {
        target = { x: FAR, y: FAR };
        return;
      }
      const rect = container.getBoundingClientRect();
      const inside =
        e.clientX >= rect.left &&
        e.clientX <= rect.right &&
        e.clientY >= rect.top &&
        e.clientY <= rect.bottom;

      target = inside
        ? { x: e.clientX - rect.left, y: e.clientY - rect.top }
        : { x: FAR, y: FAR };
    };

    // Pull a point toward the cursor with a smooth falloff
    const warp = (x: number, y: number): [number, number] => {
      const dx = current.x - x;
      const dy = current.y - y;
      const dist = Math.hypot(dx, dy);
      if (dist >= RADIUS || dist === 0) return [x, y];
      const force = Math.pow(1 - dist / RADIUS, 2) * PULL;
      return [x + (dx / dist) * force, y + (dy / dist) * force];
    };

    const traceGrid = () => {
      ctx.beginPath();
      for (let x = 0; x <= width; x += CELL) {
        for (let y = 0; y <= height; y += STEP) {
          const [px, py] = warp(x, y);
          if (y === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
      }
      for (let y = 0; y <= height; y += CELL) {
        for (let x = 0; x <= width; x += STEP) {
          const [px, py] = warp(x, y);
          if (x === 0) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
      }
      ctx.stroke();
    };

    const draw = () => {
      // Smooth follow for a fluid feel
      current.x += (target.x - current.x) * 0.12;
      current.y += (target.y - current.y) * 0.12;

      ctx.clearRect(0, 0, width, height);

      // Base grid
      ctx.lineWidth = 1;
      ctx.strokeStyle = 'rgba(255, 255, 255, 0.06)';
      traceGrid();

      // Soft green light halo under the cursor
      const halo = ctx.createRadialGradient(current.x, current.y, 0, current.x, current.y, RADIUS);
      halo.addColorStop(0, `rgba(${BRAND}, 0.14)`);
      halo.addColorStop(1, `rgba(${BRAND}, 0)`);
      ctx.fillStyle = halo;
      ctx.fillRect(0, 0, width, height);

      // Glowing lines that fade out with distance
      const lineGlow = ctx.createRadialGradient(current.x, current.y, 0, current.x, current.y, RADIUS);
      lineGlow.addColorStop(0, `rgba(${BRAND}, 0.9)`);
      lineGlow.addColorStop(1, `rgba(${BRAND}, 0)`);
      ctx.lineWidth = 1.2;
      ctx.strokeStyle = lineGlow;
      traceGrid();

      frame = requestAnimationFrame(draw);
    };

    resize();
    window.addEventListener('resize', resize);
    window.addEventListener('pointermove', onPointerMove, { passive: true });
    frame = requestAnimationFrame(draw);

    return () => {
      cancelAnimationFrame(frame);
      window.removeEventListener('resize', resize);
      window.removeEventListener('pointermove', onPointerMove);
    };
  });
</script>

<div bind:this={container} class="pointer-events-none absolute inset-0 overflow-hidden" aria-hidden="true">
  <canvas bind:this={canvas} ></canvas>
</div>
