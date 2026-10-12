<script lang="ts">
  // Tailwind v4 + tokens de shadcn. El gradiente glass de :global(body) de abajo GANA:
  // los estilos :global de Svelte van sin @layer, así que pisan el @layer base de Tailwind.
  import '../app.css';
  import type { Snippet } from 'svelte';
  import favicon from '$lib/assets/favicon.svg';
  import { afterNavigate } from '$app/navigation';
  import TopNav from '$lib/TopNav.svelte';
  import Sidebar from '$lib/Sidebar.svelte';

  let { children }: { children: Snippet } = $props();
  let collapsed = $state(false);

  // En móvil el sidebar flota encima del contenido (ver @media abajo y en Sidebar.svelte):
  // arranca replegado y se vuelve a replegar en cada navegación para no tapar la página.
  // afterNavigate también corre al montar, así que cubre la carga inicial.
  afterNavigate(() => {
    if (window.matchMedia('(max-width: 680px)').matches) collapsed = true;
  });

  // View Transitions cuando el browser las soporta para animar el repliegue.
  function withTransition(fn: () => void) {
    if (typeof document !== 'undefined' && 'startViewTransition' in document) {
      (document as unknown as { startViewTransition: (cb: () => void) => void }).startViewTransition(fn);
    } else {
      fn();
    }
  }
  function toggleCollapsed() {
    withTransition(() => {
      collapsed = !collapsed;
    });
  }
</script>

<svelte:head>
  <link rel="icon" href={favicon} />
</svelte:head>

<TopNav />
<Sidebar {collapsed} {toggleCollapsed} />
<main class={collapsed ? 'collapsed' : ''}>
  <div class="work-scroll">
    {@render children()}
  </div>
</main>

<style>
  :global(:root) {
    --topnav-height: 64px;
    /* Color oscuro del gradiente: el Sidebar lo usa de fondo casi sólido cuando flota en móvil. */
    --grad-b: #1e293b;
  }

  :global(html, body) {
    margin: 0;
    padding: 0;
    height: 100%;
  }
  /* Gradiente con los colores de nexus: primary #4268fb → slate oscuro #1e293b. */
  :global(body) {
    min-height: 100vh;
    background: linear-gradient(135deg, #4268fb 0%, #1e293b 100%);
    background-attachment: fixed;
    color: rgba(255, 255, 255, 0.95);
    font-family: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
  }

  :global(*) {
    scrollbar-width: auto;
    scrollbar-color: rgba(255, 255, 255, 0.55) rgba(255, 255, 255, 0.1);
  }
  :global(::-webkit-scrollbar) {
    width: 14px;
    height: 14px;
  }
  :global(::-webkit-scrollbar-track) {
    background: rgba(255, 255, 255, 0.07);
    border-radius: 999px;
  }
  :global(::-webkit-scrollbar-thumb) {
    background: rgba(255, 255, 255, 0.55);
    border-radius: 999px;
    border: 3px solid transparent;
    background-clip: padding-box;
  }
  :global(::-webkit-scrollbar-thumb:hover) {
    background: rgba(255, 255, 255, 0.78);
    background-clip: padding-box;
  }

  main {
    position: fixed;
    top: calc(2rem + var(--topnav-height));
    right: 1rem;
    bottom: 1rem;
    box-sizing: border-box;
    background: rgba(255, 255, 255, 0.012);
    backdrop-filter: blur(8px) saturate(110%);
    -webkit-backdrop-filter: blur(8px) saturate(110%);
    border: 1px solid #fff;
    border-radius: 16px;
    box-shadow:
      inset 0 1px 0 rgba(255, 255, 255, 0.08),
      0 4px 16px rgba(0, 0, 0, 0.12);
    overflow: hidden;
    transition: left 0.22s ease-out;
    left: calc(var(--sidebar-width, 240px) + 2rem);
  }
  main.collapsed {
    left: 2rem;
  }
  /* Móvil (mismo corte que la TopNav): el panel usa todo el ancho y el sidebar flota encima
     (su z-index vive en Sidebar.svelte). */
  @media (max-width: 680px) {
    main,
    main.collapsed {
      left: 1rem;
    }
  }

  .work-scroll {
    position: absolute;
    top: 16px;
    bottom: 16px;
    left: 0;
    right: 0;
    overflow-y: auto;
    overflow-x: hidden;
    padding: 0 16px;
  }
</style>
