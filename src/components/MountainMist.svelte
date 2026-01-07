<!--
  MountainMist.svelte
  
  Animated misty mountain silhouette for 404 page.
  Snowdonia-inspired with gentle rising mist effect.
  
  CSS-only animation for performance.
  Respects prefers-reduced-motion automatically.
  
  CUSTOMIZATION:
  - Adjust --mist-duration for mist speed (default: 8s)
  - Adjust --mist-opacity for visibility (default: 0.3)
-->

<script lang="ts">
  // Mist particles configuration
  const mistParticles = Array.from({ length: 8 }, (_, i) => ({
    id: i,
    left: i * 12.5 + Math.random() * 5,
    delay: Math.random() * 6,
    duration: 6 + Math.random() * 4,
    size: 80 + Math.random() * 60,
  }));
</script>

<div class="mountain-scene" aria-hidden="true">
  <!-- Mountain silhouette -->
  <svg
    class="mountains"
    viewBox="0 0 400 120"
    preserveAspectRatio="xMidYMax slice"
    xmlns="http://www.w3.org/2000/svg"
  >
    <!-- Background mountain -->
    <path
      d="M0,120 L0,80 L60,45 L100,70 L140,30 L180,55 L220,20 L260,50 L300,35 L340,60 L380,40 L400,65 L400,120 Z"
      fill="#1a1a1a"
      class="mountain-back"
    />
    <!-- Foreground mountain -->
    <path
      d="M0,120 L0,90 L50,65 L90,85 L130,50 L170,75 L210,40 L250,70 L290,55 L330,80 L370,60 L400,85 L400,120 Z"
      fill="#252525"
      class="mountain-front"
    />
    <!-- Snow caps -->
    <path
      d="M130,50 L140,55 L145,48 L155,58 L160,52 L170,60 L170,75 L130,50 Z"
      fill="#3a3a3a"
      class="snow-cap"
    />
    <path
      d="M210,40 L220,45 L225,38 L235,48 L240,42 L250,50 L250,70 L210,40 Z"
      fill="#3a3a3a"
      class="snow-cap"
    />
  </svg>

  <!-- Rising mist particles -->
  <div class="mist-container">
    {#each mistParticles as particle (particle.id)}
      <div
        class="mist-particle"
        style="
          left: {particle.left}%;
          width: {particle.size}px;
          height: {particle.size * 0.4}px;
          animation-delay: {particle.delay}s;
          animation-duration: {particle.duration}s;
        "
      ></div>
    {/each}
  </div>
</div>

<style>
  .mountain-scene {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 200px;
    overflow: hidden;
    pointer-events: none;
  }

  .mountains {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 120px;
  }

  .mountain-back {
    opacity: 0.7;
  }

  .mountain-front {
    opacity: 0.9;
  }

  .snow-cap {
    opacity: 0.5;
  }

  .mist-container {
    position: absolute;
    bottom: 40px;
    left: 0;
    right: 0;
    height: 150px;
    overflow: hidden;
  }

  .mist-particle {
    position: absolute;
    bottom: 0;
    border-radius: 50%;
    background: radial-gradient(
      ellipse at center,
      rgba(255, 255, 255, 0.15) 0%,
      rgba(255, 255, 255, 0.05) 40%,
      transparent 70%
    );
    filter: blur(8px);
    animation: mistRise linear infinite;
    will-change: transform, opacity;
  }

  @keyframes mistRise {
    0% {
      transform: translateY(0) scale(0.8);
      opacity: 0;
    }
    10% {
      opacity: 0.3;
    }
    50% {
      opacity: 0.2;
    }
    90% {
      opacity: 0.1;
    }
    100% {
      transform: translateY(-120px) scale(1.2);
      opacity: 0;
    }
  }

  /* Reduced motion */
  @media (prefers-reduced-motion: reduce) {
    .mist-particle {
      animation: none;
      opacity: 0.1;
      transform: translateY(-60px);
    }
  }
</style>
