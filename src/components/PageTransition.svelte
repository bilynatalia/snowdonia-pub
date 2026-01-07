<!--
  PageTransition.svelte
  
  Wrapper component for smooth page content transitions.
  Uses Svelte fly transition for fade + translate effect.
  Respects prefers-reduced-motion preference.
-->

<script lang="ts">
  import { fly } from "svelte/transition";
  import { onMount } from "svelte";
  import type { Snippet } from "svelte";

  interface Props {
    children: Snippet;
  }

  let { children }: Props = $props();

  let visible = $state(false);
  let prefersReducedMotion = $state(false);

  onMount(() => {
    // Check reduced motion preference
    prefersReducedMotion = window.matchMedia(
      "(prefers-reduced-motion: reduce)"
    ).matches;
    visible = true;
  });
</script>

{#if visible}
  <div
    class="page-content"
    in:fly={{
      y: prefersReducedMotion ? 0 : 20,
      duration: prefersReducedMotion ? 0 : 500,
      delay: 100,
    }}
  >
    {@render children()}
  </div>
{/if}

<style>
  .page-content {
    width: 100%;
  }
</style>
