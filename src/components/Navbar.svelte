<script lang="ts">
  import { fly } from "svelte/transition";
  import { onMount } from "svelte";

  interface Props {
    currentPath?: string;
  }

  let { currentPath = "/" }: Props = $props();

  const navItems = [
    { href: "/", label: "Home" },
    { href: "/menu", label: "Menu" },
    { href: "/about", label: "About Us" },
    { href: "/press", label: "Press" },
  ];

  let mounted = $state(false);
  let prefersReducedMotion = $state(false);

  onMount(() => {
    prefersReducedMotion = window.matchMedia(
      "(prefers-reduced-motion: reduce)"
    ).matches;
    mounted = true;
  });
</script>

<nav class="navbar">
  {#if mounted}
    <ul
      in:fly={{
        y: prefersReducedMotion ? 0 : -10,
        duration: prefersReducedMotion ? 0 : 400,
        delay: 200,
      }}
    >
      {#each navItems as item, i}
        <li
          in:fly={{
            y: prefersReducedMotion ? 0 : -10,
            duration: prefersReducedMotion ? 0 : 300,
            delay: prefersReducedMotion ? 0 : 300 + i * 80,
          }}
        >
          <a
            href={item.href}
            class:active={currentPath === item.href}
            class="nav-link"
          >
            {item.label}
            <span class="nav-underline"></span>
          </a>
        </li>
      {/each}
    </ul>
  {:else}
    <ul>
      {#each navItems as item}
        <li>
          <a
            href={item.href}
            class:active={currentPath === item.href}
            class="nav-link"
          >
            {item.label}
            <span class="nav-underline"></span>
          </a>
        </li>
      {/each}
    </ul>
  {/if}
</nav>

<style>
  .navbar {
    border-top: 1px solid #333;
    border-bottom: 1px solid #333;
    padding: 1.2rem 0;
    background-color: #0f0f0f;
    position: sticky;
    top: 0;
    z-index: 100;
  }

  ul {
    list-style: none;
    display: flex;
    justify-content: center;
    gap: 2.5rem;
    flex-wrap: wrap;
    margin: 0;
    padding: 0;
  }

  li {
    position: relative;
  }

  .nav-link {
    text-decoration: none;
    color: #ddd;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-size: 0.9rem;
    transition:
      color 0.3s ease,
      letter-spacing 0.3s ease;
    position: relative;
    display: inline-block;
    padding: 0.25rem 0;
  }

  .nav-underline {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, #d35400, #e67e22);
    transform: scaleX(0);
    transform-origin: center;
    transition: transform 0.3s cubic-bezier(0.22, 1, 0.36, 1);
  }

  .nav-link:hover,
  .nav-link.active {
    color: #d35400;
  }

  .nav-link:hover .nav-underline,
  .nav-link.active .nav-underline {
    transform: scaleX(1);
  }

  .nav-link.active .nav-underline {
    background: linear-gradient(90deg, #e67e22, #f39c12);
    box-shadow: 0 0 8px rgba(211, 84, 0, 0.5);
  }

  /* Subtle hover glow on active link */
  .nav-link.active {
    text-shadow: 0 0 10px rgba(211, 84, 0, 0.3);
  }

  /* Reduced motion */
  @media (prefers-reduced-motion: reduce) {
    .nav-underline {
      transition: none;
    }

    .nav-link {
      transition: none;
    }
  }

  /* Mobile adjustments */
  @media (max-width: 768px) {
    ul {
      gap: 1rem;
    }

    .nav-link {
      font-size: 0.8rem;
      letter-spacing: 1px;
    }
  }
</style>
