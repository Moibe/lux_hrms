<script lang="ts">
  // Barra lateral "de vidrio", sin tilt 3D (igual que la superior). Incluye el handle
  // para replegar/mostrar. Publica su ancho real a la variable CSS --sidebar-width
  // para que el panel de contenido se ajuste solo. Items de ejemplo: reemplázalos.
  let {
    collapsed = false,
    toggleCollapsed
  }: {
    collapsed?: boolean;
    toggleCollapsed: () => void;
  } = $props();

  let sidebarWidth = $state(240);

  $effect(() => {
    if (typeof document !== 'undefined' && !collapsed) {
      document.documentElement.style.setProperty('--sidebar-width', `${sidebarWidth}px`);
    }
  });

  function handleCollapseClick(e: MouseEvent) {
    e.stopPropagation();
    toggleCollapsed();
  }
</script>

{#if !collapsed}
  <aside class="sidebar" bind:clientWidth={sidebarWidth}>
    <nav>
      <a href="/" class="nav-item" aria-current="page">
        <span class="nav-ico" aria-hidden="true"></span>
        <span>Sección uno</span>
      </a>
      <a href="/seccion-dos" class="nav-item">
        <span class="nav-ico" aria-hidden="true"></span>
        <span>Sección dos</span>
      </a>
      <a href="/seccion-tres" class="nav-item">
        <span class="nav-ico" aria-hidden="true"></span>
        <span>Sección tres</span>
      </a>
    </nav>

    <div class="sidebar-footer">
      <button type="button" class="collapse-btn" onclick={handleCollapseClick} aria-label="Replegar barra">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6" /></svg>
      </button>
    </div>
  </aside>
{:else}
  <button type="button" class="reveal-handle" onclick={toggleCollapsed} aria-label="Mostrar barra">
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6" /></svg>
  </button>
{/if}

<style>
  .sidebar {
    position: fixed;
    top: calc(2rem + var(--topnav-height, 64px));
    left: 1rem;
    bottom: 1rem;
    box-sizing: border-box;
    width: max-content;
    min-width: 240px;
    max-width: 380px;
    padding: 1.5rem 1rem;
    display: flex;
    flex-direction: column;
    background: rgba(255, 255, 255, 0.012);
    backdrop-filter: blur(8px) saturate(110%);
    -webkit-backdrop-filter: blur(8px) saturate(110%);
    border: 1px solid #fff;
    border-radius: 16px;
    box-shadow:
      inset 0 1px 0 rgba(255, 255, 255, 0.08),
      0 4px 16px rgba(0, 0, 0, 0.12);
    user-select: none;
  }
  /* Móvil: flota encima del panel de contenido (el layout ya no le deja hueco), con el color
     oscuro del gradiente casi sólido para que sus items se lean sobre el contenido. */
  @media (max-width: 680px) {
    .sidebar {
      z-index: 8;
      background: color-mix(in srgb, var(--grad-b, #1e3a8a) 92%, transparent);
    }
  }
  nav {
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
    flex: 1;
    min-height: 0;
    overflow-y: auto;
    scrollbar-width: none;
    -ms-overflow-style: none;
  }
  nav::-webkit-scrollbar {
    display: none;
  }
  .nav-item {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.7rem 0.95rem;
    color: rgba(255, 255, 255, 0.92);
    text-decoration: none;
    font-size: 0.95rem;
    letter-spacing: 0.01em;
    border-radius: 8px;
    border: 1px solid transparent;
    transition: background 0.18s ease, border-color 0.18s ease;
  }
  .nav-ico {
    width: 16px;
    height: 16px;
    border-radius: 5px;
    flex-shrink: 0;
    background: rgba(255, 255, 255, 0.55);
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.3);
  }
  .nav-item:hover {
    background: rgba(255, 255, 255, 0.09);
    border-color: rgba(255, 255, 255, 0.16);
  }
  /* Mismo item activo que la TopNav: blanco translúcido (el azul de nexus ya es el fondo). */
  .nav-item[aria-current='page'] {
    color: #fff;
    background: rgba(255, 255, 255, 0.16);
    border-color: rgba(255, 255, 255, 0.45);
    box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1) inset;
  }
  .nav-item[aria-current='page'] .nav-ico {
    background: #fff;
  }
  .sidebar-footer {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: auto;
    padding-top: 1rem;
    border-top: 1px solid rgba(255, 255, 255, 0.08);
  }
  .collapse-btn,
  .reveal-handle {
    background: rgba(255, 255, 255, 0.04);
    border: 1px solid rgba(255, 255, 255, 0.14);
    border-radius: 8px;
    padding: 0.4rem 0.5rem;
    color: rgba(255, 255, 255, 0.85);
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font: inherit;
    transition: background 0.18s ease, border-color 0.18s ease, color 0.18s ease;
  }
  .collapse-btn:hover,
  .reveal-handle:hover {
    background: rgba(255, 255, 255, 0.1);
    border-color: rgba(255, 255, 255, 0.24);
    color: #fff;
  }
  .reveal-handle {
    position: fixed;
    left: 0.75rem;
    top: 50%;
    transform: translateY(-50%);
    padding: 0.55rem 0.45rem;
    border-radius: 12px;
    border: 1px solid #fff;
    background: rgba(255, 255, 255, 0.012);
    backdrop-filter: blur(8px) saturate(110%);
    -webkit-backdrop-filter: blur(8px) saturate(110%);
    box-shadow:
      inset 0 1px 0 rgba(255, 255, 255, 0.08),
      0 4px 16px rgba(0, 0, 0, 0.12);
    z-index: 10;
  }
</style>
