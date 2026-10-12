<script lang="ts">
  // Barra superior "de vidrio" + responsive (en móvil colapsa a solo-íconos).
  // Sin tilt 3D: queda quieta al pasar el mouse. Los items son de ejemplo: reemplázalos
  // por los de tu app.
  import { page } from '$app/state';

  // Edita estos items por las secciones reales de tu app.
  const items = [
    { href: '/', label: 'Inicio' },
    { href: '/seccion-dos', label: 'Sección dos' },
    { href: '/seccion-tres', label: 'Sección tres' }
  ];
</script>

<header class="topnav">
  <a href="/" class="brand" aria-label="Inicio">
    <span class="brand-ico" aria-hidden="true"></span>
    <span class="brand-title">Lux HRMS</span>
  </a>

  <nav class="topnav-nav">
    {#each items as it (it.href)}
      <a href={it.href} class="nav-item" aria-current={page.url.pathname === it.href ? 'page' : undefined}>
        <span class="nav-ico" aria-hidden="true"></span>
        <span class="nav-label">{it.label}</span>
      </a>
    {/each}
  </nav>
</header>

<style>
  .topnav {
    position: fixed;
    top: 1rem;
    left: 1rem;
    right: 1rem;
    height: var(--topnav-height, 64px);
    padding: 0 1.25rem;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    background: rgba(255, 255, 255, 0.012);
    backdrop-filter: blur(8px) saturate(110%);
    -webkit-backdrop-filter: blur(8px) saturate(110%);
    border: 1px solid #fff;
    border-radius: 16px;
    box-shadow:
      inset 0 1px 0 rgba(255, 255, 255, 0.08),
      0 4px 16px rgba(0, 0, 0, 0.12);
    user-select: none;
    z-index: 9;
  }

  .brand {
    display: flex;
    align-items: center;
    gap: 0.65rem;
    color: rgba(255, 255, 255, 0.98);
    text-decoration: none;
    border-radius: 8px;
    padding: 0.25rem 0.4rem;
    transition: background 0.18s ease;
  }
  .brand:hover {
    background: rgba(255, 255, 255, 0.05);
  }
  .brand-ico {
    width: 22px;
    height: 22px;
    border-radius: 6px;
    flex-shrink: 0;
    background: rgba(255, 255, 255, 0.85);
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.4);
  }
  .brand-title {
    font-size: 1.2rem;
    letter-spacing: 0.005em;
    text-shadow:
      0 0 10px rgba(255, 255, 255, 0.28),
      0 0 24px rgba(255, 255, 255, 0.14);
  }

  .topnav-nav {
    display: flex;
    align-items: center;
    gap: 0.3rem;
    margin-left: 1.25rem;
    padding-left: 1.25rem;
    border-left: 1px solid rgba(255, 255, 255, 0.1);
  }

  .nav-item {
    display: inline-flex;
    align-items: center;
    gap: 0.45rem;
    padding: 0.45rem 0.8rem;
    color: rgba(255, 255, 255, 0.88);
    text-decoration: none;
    font-size: 0.9rem;
    border-radius: 8px;
    border: 1px solid transparent;
    transition: background 0.18s ease, border-color 0.18s ease, color 0.18s ease;
    white-space: nowrap;
  }
  .nav-item:hover {
    background: rgba(255, 255, 255, 0.09);
    border-color: rgba(255, 255, 255, 0.16);
    color: #fff;
  }
  /* El extremo claro del gradiente ya es el azul de nexus (#4268fb), así que el item activo
     va en blanco translúcido: un tinte azul se perdería contra el fondo. */
  .nav-item[aria-current='page'] {
    color: #fff;
    background: rgba(255, 255, 255, 0.16);
    border-color: rgba(255, 255, 255, 0.45);
    box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1) inset;
  }
  .nav-ico {
    width: 16px;
    height: 16px;
    border-radius: 5px;
    flex-shrink: 0;
    background: rgba(255, 255, 255, 0.55);
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.3);
  }

  /* En pantallas chicas: solo íconos (oculta texto y título) para que no se desborde. */
  @media (max-width: 680px) {
    .topnav {
      padding: 0 0.6rem;
    }
    .brand {
      gap: 0;
      padding: 0.25rem;
    }
    .brand-title {
      display: none;
    }
    .topnav-nav {
      margin-left: 0.5rem;
      padding-left: 0.5rem;
      gap: 0.1rem;
    }
    .nav-item {
      padding: 0.45rem 0.5rem;
    }
    .nav-label {
      display: none;
    }
  }
  @media (max-width: 360px) {
    .topnav {
      padding: 0 0.4rem;
    }
    .topnav-nav {
      margin-left: 0.35rem;
      padding-left: 0.35rem;
    }
    .nav-item {
      padding: 0.45rem 0.35rem;
    }
  }
</style>
