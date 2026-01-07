<!--
  BeerBubbles.svelte
  
  Ambient animated background with rising beer bubbles.
  Themed for Snowdonia Pub with golden/amber colors.
  
  CSS-only animation for performance (no JS runtime cost).
  Respects prefers-reduced-motion automatically via CSS.
-->

<script lang="ts">
  // Number of bubbles to render
  const bubbleCount = 15;
  
  // Generate random properties for each bubble
  const bubbles = Array.from({ length: bubbleCount }, (_, i) => ({
    id: i,
    left: Math.random() * 100,           // Random horizontal position (%)
    size: 4 + Math.random() * 8,          // Random size (4-12px)
    delay: Math.random() * 8,             // Random animation delay (0-8s)
    duration: 6 + Math.random() * 6,      // Random duration (6-12s)
    opacity: 0.3 + Math.random() * 0.4    // Random opacity (0.3-0.7)
  }));
</script>

<div class="bubbles-container" aria-hidden="true">
  {#each bubbles as bubble (bubble.id)}
    <div 
      class="bubble"
      style="
        left: {bubble.left}%;
        width: {bubble.size}px;
        height: {bubble.size}px;
        animation-delay: {bubble.delay}s;
        animation-duration: {bubble.duration}s;
        --bubble-opacity: {bubble.opacity};
      "
    ></div>
  {/each}
</div>

<style>
  .bubbles-container {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
    z-index: 1;
  }
  
  .bubble {
    position: absolute;
    bottom: -20px;
    border-radius: 50%;
    background: radial-gradient(
      circle at 30% 30%,
      rgba(255, 215, 130, var(--bubble-opacity, 0.5)),
      rgba(211, 133, 0, var(--bubble-opacity, 0.3)) 60%,
      rgba(180, 100, 0, 0.1)
    );
    box-shadow: 
      inset 0 0 3px rgba(255, 255, 255, 0.3),
      0 0 6px rgba(211, 133, 0, 0.2);
    animation: bubbleRise linear infinite;
    will-change: transform, opacity;
  }
  
  @keyframes bubbleRise {
    0% {
      transform: translateY(0) scale(0.8);
      opacity: 0;
    }
    5% {
      opacity: var(--bubble-opacity, 0.5);
    }
    50% {
      transform: translateY(-50vh) translateX(10px) scale(1);
    }
    95% {
      opacity: var(--bubble-opacity, 0.5);
    }
    100% {
      transform: translateY(-110vh) translateX(-5px) scale(0.9);
      opacity: 0;
    }
  }
  
  /* Respect reduced motion preference */
  @media (prefers-reduced-motion: reduce) {
    .bubble {
      animation: none;
      opacity: 0.2;
      transform: translateY(-50vh);
    }
  }
</style>
