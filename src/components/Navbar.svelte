<script lang="ts">
  export let currentPath: string = "/";
  
  const navItems = [
    { href: "/", label: "Home" },
    { href: "/menu", label: "Menu" },
    { href: "/about", label: "About Us" },
    { href: "/press", label: "Press" }
  ];
</script>

<nav>
  <ul>
    {#each navItems as item}
      <li>
        <a 
          href={item.href} 
          class:active={currentPath === item.href}
        >
          {item.label}
        </a>
      </li>
    {/each}
  </ul>
</nav>
