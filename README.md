[index.html](https://github.com/user-attachments/files/33585094/index.html)
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#FAF9F6">
  <title>Tus carpetas en un solo lugar</title>
  <style>
    :root {
      color-scheme: light;
      --ink: #17171A;
      --muted: #6B6B72;
      --line: #E7E5DF;
      --paper: #FFFFFF;
      --canvas: #FAF9F6;
      --section-bg: #F1EFEA;
      --sidebar-bg: #FAF9F6;
      --input-bg: #FFFFFF;
      --primary: #111111;
      --primary-contrast: #FFFFFF;
      --brand-bg: #111111;
      --brand-mark-ink: #FAF9F6;
      --accent: #2F4BFF;
      --hover-bg: #F0EEE8;
      --card-ink: #111111;
      --cover-ink: #FFFFFF;
      --danger: #B42318;
      --danger-hover: #8F1D14;
      --disabled-ink: #6B6B72;
      --brand-subtitle: #6B6B72;
      --topbar-bg: rgb(250 249 246 / 93%);
      --cover-overlay: rgb(0 0 0 / 28%);
      --cover-hover-overlay: rgb(0 0 0 / 58%);
      --card-hover-overlay: rgb(0 0 0 / 24%);
      --cover-text-shadow: 0 1px 8px rgb(0 0 0 / 85%);
      --card-text-shadow: 0 1px 4px rgb(0 0 0 / 85%);
      --card-hover-shadow: 0 12px 28px rgb(0 0 0 / 50%);
      --float-shadow: 0 8px 22px rgb(0 0 0 / 19%);
      --soft-shadow: 0 5px 18px rgb(0 0 0 / 13%);
      --dark-control-bg: rgb(17 17 17 / 78%);
      --dark-control-border: rgb(255 255 255 / 70%);
      --page-placeholder: #6B6B72;
      --selection-ink: #FFFFFF;
      --sidebar: 252px;
      --sidebar-width: 252px;
      --fondo-recorrido: 200px;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    * { box-sizing: border-box; }
    body { margin: 0; min-width: 320px; min-height: 100vh; color: var(--ink); background: var(--canvas); }
    button, input { font: inherit; }
    button { color: inherit; }
    .app { position: relative; z-index: 1; min-height: 100vh; background: transparent; }
    ::selection { background: var(--accent); color: var(--selection-ink); }
    :focus-visible { outline-color: var(--accent); }

    .sidebar {
      position: fixed; inset: 0 auto 0 0; z-index: 5; display: flex; width: var(--sidebar-width);
      flex-direction: column; padding: 25px 17px 18px; border-right: 1px solid var(--line); background: var(--sidebar-bg);
      transform: translateX(0); transition: transform .25s ease, box-shadow .25s ease, visibility 0s linear .25s;
    }
    .menu-visible .sidebar { transition-delay: 0s; }
    .brand { display: flex; align-items: center; gap: 11px; padding: 0 9px; margin-bottom: 39px; }
    .brand-mark { display: grid; width: 34px; height: 34px; flex: 0 0 auto; place-items: center; border-radius: 11px; background: var(--brand-bg); color: var(--brand-mark-ink); }
    .brand-mark svg { width: 20px; height: 20px; }
    .brand-copy { display: flex; min-width: 0; flex-direction: column; gap: 2px; }
    .brand-name { font-size: 18px; font-weight: 720; letter-spacing: -.6px; white-space: nowrap; }
    .brand-subtitle { color: var(--brand-subtitle); font-size: 10px; line-height: 1.2; white-space: nowrap; }
    .workspace-label { padding: 0 10px; margin: 0 0 9px; color: var(--muted); font-size: 10px; font-weight: 700; letter-spacing: 1.2px; text-transform: uppercase; }
    .nav-item, .folder-nav {
      display: flex; width: 100%; align-items: center; gap: 10px; border: 0; border-radius: 8px;
      background: transparent; text-align: left; cursor: pointer;
    }
    .nav-item { padding: 10px 11px; margin-bottom: 4px; color: var(--ink); font-size: 13px; }
    .nav-item:hover, .folder-nav:hover { background: var(--hover-bg); }
    .nav-item.active { background: var(--primary); color: var(--primary-contrast); font-weight: 650; }
    .nav-item svg { width: 17px; height: 17px; flex: 0 0 auto; }
    .folder-section { margin-top: 29px; }
    .section-heading { display: flex; align-items: center; justify-content: space-between; padding: 0 8px; margin-bottom: 7px; }
    .section-heading .workspace-label { padding: 0; margin: 0; }
    .icon-button { display: inline-grid; width: 30px; height: 30px; place-items: center; border: 0; border-radius: 7px; background: transparent; color: var(--muted); cursor: pointer; }
    .icon-button:hover { background: var(--hover-bg); color: var(--ink); }
    .icon-button svg { width: 16px; height: 16px; }
    .folder-list { display: grid; gap: 2px; }
    .folder-nav { padding: 8px 9px; color: var(--ink); font-size: 12.5px; }
    .folder-nav.active { background: var(--primary); color: var(--primary-contrast); font-weight: 650; }
    .folder-nav .folder-dot { width: 9px; height: 9px; flex: 0 0 9px; border-radius: 3px; background: var(--folder-color, var(--accent)); }
    .folder-nav .folder-label { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
    .sidebar .section-nav { margin-top: 8px; }
    .sidebar-spacer { flex: 1; }
    .profile { display: flex; align-items: center; gap: 10px; padding: 14px 8px 0; border-top: 1px solid var(--line); }
    .avatar { display: grid; width: 32px; height: 32px; place-items: center; border-radius: 50%; background: var(--primary); color: var(--primary-contrast); font-size: 12px; font-weight: 700; }
    .profile-name { font-size: 12px; font-weight: 650; }
    .profile-caption { margin-top: 2px; color: var(--muted); font-size: 10px; }

    .main { min-height: 100vh; margin-left: var(--sidebar); transition: margin-left .25s ease; }
    .topbar { position: sticky; top: 0; z-index: 3; display: flex; height: 62px; align-items: center; justify-content: space-between; padding: 0 46px; border-bottom: 1px solid var(--line); background: var(--topbar-bg); backdrop-filter: blur(12px); }
    .app.fondo-imagen-activa .topbar { border-bottom-color: color-mix(in srgb, var(--line) 55%, transparent); backdrop-filter: blur(12px); }
    .app.fondo-imagen-activa.lectura-tono-claro .topbar { background: color-mix(in srgb, #FFFFFF 92%, transparent); }
    .app.fondo-imagen-activa.lectura-tono-oscuro .topbar { background: color-mix(in srgb, #111114 92%, transparent); }
    .app.fondo-imagen-activa.lectura-tono-claro .breadcrumbs,
    .app.fondo-imagen-activa.lectura-tono-claro .breadcrumbs button,
    .app.fondo-imagen-activa.lectura-tono-claro .breadcrumbs .crumb-current,
    .app.fondo-imagen-activa.lectura-tono-claro .breadcrumbs button:hover { color: #17171A; }
    .app.fondo-imagen-activa.lectura-tono-oscuro .breadcrumbs,
    .app.fondo-imagen-activa.lectura-tono-oscuro .breadcrumbs button,
    .app.fondo-imagen-activa.lectura-tono-oscuro .breadcrumbs .crumb-current,
    .app.fondo-imagen-activa.lectura-tono-oscuro .breadcrumbs button:hover { color: #F2F2F0; }
    .breadcrumbs { display: flex; min-width: 0; align-items: center; gap: 9px; color: var(--muted); font-size: 12px; }
    .breadcrumbs button { padding: 4px 0; border: 0; background: transparent; color: inherit; cursor: pointer; }
    .breadcrumbs button:hover { color: var(--ink); }
    .crumb-current { color: var(--ink); font-weight: 600; }
    .breadcrumbs .menu-toggle { display: inline-grid; width: 35px; height: 35px; flex: 0 0 auto; place-items: center; padding: 0; border: 1px solid var(--line); border-radius: 8px; background: var(--input-bg); color: var(--ink); cursor: pointer; font-size: 18px; }
    .breadcrumbs .menu-toggle:hover { background: var(--hover-bg); }
    .menu-toggle:focus-visible, .menu-borde:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }
    .menu-overlay { display: none; }
    .menu-borde { position: fixed; inset: 0 auto 0 0; z-index: 10; width: 12px; padding: 0; border: 0; background: transparent; opacity: 0; cursor: pointer; }
    .menu-borde:hover, .menu-borde:focus-visible { opacity: 1; background: color-mix(in srgb, var(--accent) 12%, transparent); }
    .menu-cerrado { --sidebar: 0px; }
    .menu-cerrado .sidebar { visibility: hidden; transform: translateX(-100%); }
    .menu-cerrado.menu-flotante .sidebar { visibility: visible; transform: translateX(0); box-shadow: var(--float-shadow); }
    .menu-cerrado.menu-flotante { --sidebar: 0px; }
    .top-actions { display: flex; align-items: center; gap: 11px; }
    .save-state { color: var(--muted); font-size: 11px; }
    .search-wrap { position: relative; }
    .search-wrap svg { position: absolute; top: 10px; left: 11px; width: 15px; height: 15px; color: var(--muted); pointer-events: none; }
    .search-input { width: 205px; height: 35px; padding: 0 12px 0 34px; border: 1px solid var(--line); border-radius: 8px; outline: none; background: var(--input-bg); color: var(--ink); font-size: 12px; }
    .search-input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px color-mix(in srgb, var(--accent) 18%, transparent); }
    .theme-toggle { width: 35px; height: 35px; flex: 0 0 auto; border: 1px solid var(--line); border-radius: 8px; background: var(--input-bg); color: var(--muted); }
    .theme-toggle:hover { background: var(--hover-bg); color: var(--ink); }
    .theme-toggle svg { width: 17px; height: 17px; }

    :root[data-theme="dark"] {
      color-scheme: dark;
      --ink: #F2F2F0;
      --muted: #A1A1A8;
      --line: #2A2A2F;
      --paper: #17171A;
      --canvas: #0E0E10;
      --section-bg: #17171A;
      --sidebar-bg: #17171A;
      --input-bg: #17171A;
      --primary: #F2F2F0;
      --primary-contrast: #111111;
      --accent: #7C8CFF;
      --hover-bg: #242429;
      --danger: #FF8A80;
      --danger-hover: #FFB4AB;
      --disabled-ink: #85858D;
      --brand-subtitle: #A1A1A8;
      --topbar-bg: rgb(14 14 16 / 93%);
      --cover-overlay: rgb(0 0 0 / 28%);
      --cover-hover-overlay: rgb(0 0 0 / 58%);
      --page-placeholder: #A1A1A8;
      --selection-ink: #111111;
    }
    :root[data-theme="dark"] .pagina-vista-organizada {
      --ink: #F2F2F2;
      --muted: #8A8A8A;
      --line: #242424;
      --paper: #1C1C1C;
      --canvas: #141414;
      --section-bg: #141414;
      --input-bg: #1C1C1C;
      --primary-contrast: #F2F2F2;
      --hover-bg: #242424;
    }

    .content { width: min(100%, 1040px); padding: 55px 50px 75px; margin: 0 auto; }
    .eyebrow { margin: 0 0 12px; color: var(--accent); font-size: 11px; font-weight: 700; letter-spacing: 1.2px; text-transform: uppercase; }
    h1 { margin: 0; font-size: clamp(31px, 4vw, 43px); font-weight: 650; letter-spacing: -1.8px; line-height: 1.15; }
    .intro { max-width: 520px; margin: 13px 0 0; color: var(--muted); font-size: 14px; line-height: 1.7; }
    .section-title { display: flex; align-items: center; justify-content: space-between; margin-bottom: 17px; }
    .section-title h2 { margin: 0; font-size: 16px; font-weight: 650; letter-spacing: -.3px; }
    .section-title p { margin: 5px 0 0; color: var(--muted); font-size: 11px; }
    .primary-button { display: inline-flex; min-height: 36px; align-items: center; justify-content: center; gap: 7px; padding: 0 12px; border: 1px solid var(--primary); border-radius: 8px; background: var(--primary); color: var(--primary-contrast); font-size: 11px; font-weight: 600; cursor: pointer; transition: background .15s, transform .15s; }
    .primary-button:hover { transform: translateY(-1px); background: var(--ink); }
    .primary-button svg { width: 14px; height: 14px; }
    .empty-state { padding: 37px 20px; border: 1px dashed var(--line); border-radius: 11px; color: var(--muted); text-align: center; font-size: 13px; }

    .carpetas-seccion { width: min(100%, 1140px); padding: 38px clamp(18px, 4vw, 50px) 34px; margin: 52px auto 0; background: var(--section-bg); color: var(--ink); }
    .carpetas-encabezado { margin: 0 0 21px; text-align: center; font-size: clamp(28px, 4vw, 38px); font-weight: 750; letter-spacing: -1.2px; }
    .carpetas-filtro-contenedor { display: flex; flex-direction: column; align-items: center; gap: 12px; margin-bottom: 28px; }
    .carpetas-solo-lector { position: absolute; width: 1px; height: 1px; padding: 0; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; clip-path: inset(50%); }
    .carpetas-filtro-boton { display: inline-flex; min-height: 39px; align-items: center; justify-content: center; gap: 9px; padding: 0 15px; border: 1px solid var(--line); border-radius: 0; background: var(--paper); color: var(--ink); font-size: 12px; font-weight: 600; cursor: pointer; transition: background .2s, color .2s; }
    .carpetas-filtro-boton:hover { border-color: var(--primary); background: var(--primary); color: var(--primary-contrast); }
    .carpetas-filtro-boton svg { width: 15px; height: 15px; }
    .carpetas-filtro[hidden] { display: none; }
    .carpetas-filtro { width: min(100%, 330px); }
    .carpetas-filtro input { width: 100%; min-height: 38px; padding: 0 11px; border: 1px solid var(--line); border-radius: 0; background: var(--input-bg); color: var(--ink); font: inherit; font-size: 13px; }
    .carpetas-grilla { display: grid; grid-template-columns: repeat(auto-fit, minmax(min(100%, 230px), 1fr)); gap: 18px; }
    .carpetas-grilla.galeria-grilla { grid-template-columns: repeat(auto-fill, minmax(min(100%, var(--gallery-card-size, 240px)), var(--gallery-card-size, 240px))); justify-content: start; }
    .galeria-tamano { display: flex; align-items: center; gap: 8px; }
    .galeria-tamano-boton { min-height: 38px; padding: 0 12px; border: 1px solid var(--line); border-radius: 8px; background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; cursor: pointer; }
    .galeria-tamano-boton:hover { border-color: var(--accent); color: var(--accent); }
    .galeria-tamano-control { display: flex; align-items: center; gap: 8px; padding: 5px 10px; border: 1px solid var(--line); border-radius: 8px; background: var(--paper); color: var(--ink); font-size: 11px; }
    .galeria-tamano-control input { width: 120px; accent-color: var(--accent); }
    .carpetas-tarjeta { position: relative; display: block; width: 100%; overflow: hidden; padding: 0; border: 1px solid var(--line); border-radius: 12px; aspect-ratio: 4 / 3; background: var(--hover-bg); color: var(--card-ink); font: inherit; text-align: inherit; text-decoration: none; cursor: pointer; transition: transform .2s ease, box-shadow .2s ease, border-color .2s ease; }
    .carpetas-item-con-opciones { position: relative; min-width: 0; }
    .carpetas-item-con-opciones > .carpetas-tarjeta { height: 100%; }
    .carpetas-item-con-opciones:focus-within { z-index: 2; }
    .carpetas-nuevo-elemento { display: grid; place-items: center; border: 1px dashed var(--muted); background: transparent; color: var(--ink); font-size: 15px; font-weight: 600; }
    .carpetas-nuevo-elemento:hover { border-color: var(--accent); color: var(--accent); }
    .carpetas-nuevo-elemento span { font-size: 38px; font-weight: 400; line-height: 1; }
    .carpetas-portada { display: block; width: 100%; height: 100%; object-fit: cover; transition: transform .3s, filter .3s; }
    .carpetas-superposicion { position: absolute; inset: 0; display: grid; place-items: center; padding: 20px; background: transparent; transition: background .2s; }
    .carpetas-detalles { display: grid; justify-items: center; gap: 7px; }
    .carpetas-subtitulo { color: var(--muted); font-size: 11px; font-weight: 550; }
    .materia-tarjeta-con-portada .carpetas-subtitulo { color: var(--cover-ink); text-shadow: var(--cover-text-shadow); }
    .carpetas-titulo { color: var(--card-ink); font-size: clamp(17px, 2vw, 23px); font-weight: 700; text-align: center; text-shadow: none; opacity: 1; transform: translateY(0); transition: opacity .2s, transform .2s; }
    .carpetas-tarjeta-con-portada .carpetas-superposicion { background: var(--cover-overlay); }
    .carpetas-titulo-con-portada { color: var(--cover-ink); font-size: clamp(17px, 2vw, 23px); font-weight: 700; text-shadow: var(--cover-text-shadow); }
    .carpetas-titulo-sin-portada { color: var(--card-ink); }
    .carpetas-accion { position: absolute; right: 0; bottom: 0; left: 0; padding: 12px; color: var(--card-ink); font-size: 12px; font-weight: 600; text-align: center; text-shadow: none; opacity: 0; transform: translateY(6px); transition: opacity .3s, transform .3s; }
    .carpetas-tarjeta-con-portada .carpetas-accion { color: var(--cover-ink); text-shadow: var(--cover-text-shadow); }
    .carpetas-tarjeta:hover, .carpetas-tarjeta:focus-visible { z-index: 1; border-color: var(--accent); box-shadow: var(--card-hover-shadow); transform: translateY(-3px); }
    .carpetas-tarjeta:hover .carpetas-portada, .carpetas-tarjeta:focus-visible .carpetas-portada { filter: brightness(.58); transform: scale(1.04); }
    .carpetas-tarjeta:hover .carpetas-superposicion, .carpetas-tarjeta:focus-visible .carpetas-superposicion { background: var(--card-hover-overlay); }
    .carpetas-tarjeta-con-portada:hover .carpetas-superposicion, .carpetas-tarjeta-con-portada:focus-visible .carpetas-superposicion { background: var(--cover-hover-overlay); }
    .carpetas-tarjeta:hover .carpetas-accion, .carpetas-tarjeta:focus-visible .carpetas-accion { opacity: 1; transform: translateY(0); }
    .carpetas-tarjeta:focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }
    .carpetas-vacio { grid-column: 1 / -1; padding: 28px 16px; color: var(--muted); font-size: 14px; text-align: center; }
    .carpetas-paginacion { display: flex; flex-wrap: wrap; align-items: center; justify-content: center; gap: 8px; margin-top: 27px; }
    .carpetas-pagina { display: inline-grid; min-width: 35px; height: 35px; place-items: center; padding: 0 9px; border: 0; border-radius: 50%; background: transparent; color: var(--ink); font: inherit; font-size: 12px; cursor: pointer; }
    .carpetas-pagina:hover:not(:disabled):not([aria-current="page"]) { background: var(--hover-bg); }
    .carpetas-pagina[aria-current="page"] { background: var(--primary); color: var(--primary-contrast); font-weight: 700; }
    .carpetas-pagina:disabled { color: var(--disabled-ink); cursor: default; }
    .carpetas-pagina-texto { min-width: 72px; border-radius: 0; }

    .paginas-vista { width: min(100%, 1220px); padding: 34px clamp(18px, 4vw, 48px) 42px; margin: 30px auto 0; background: var(--section-bg); color: var(--ink); }
    .paginas-encabezado { position: relative; display: flex; min-height: 64px; flex-direction: column; align-items: flex-start; justify-content: center; gap: 8px; margin-bottom: 26px; }
    .paginas-encabezado .carpetas-encabezado { margin: 0; font-size: clamp(27px, 4vw, 36px); letter-spacing: -1px; }
    .paginas-volver { padding: 0; border: 0; background: transparent; color: var(--muted); font: inherit; font-size: 12px; cursor: pointer; }
    .paginas-volver:hover { color: var(--accent); }
    .paginas-filtro-contenedor { display: flex; flex-direction: row; align-items: center; justify-content: space-between; gap: 14px; margin-bottom: 18px; }
    .paginas-grilla { align-items: start; }
    .materias-herramientas { display: flex; align-items: center; justify-content: space-between; gap: 14px; margin: 18px 0 22px; }
    .materias-filtro { display: block; flex: 1; max-width: 360px; }
    .materias-filtro input { min-height: 40px; padding: 0 12px; border: 1px solid var(--line); border-radius: 8px; background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; }
    .materias-grilla { align-items: start; }
    .materias-grilla.materias-grilla-filtradas:not(.vista-lista) { grid-template-columns: repeat(auto-fill, minmax(min(100%, 170px), 190px)); justify-content: start; gap: 12px; }
    .materias-grilla.materias-grilla-filtradas .materia-tarjeta { aspect-ratio: 16 / 10; border-radius: 9px; }
    .materias-grilla.materias-grilla-filtradas .carpetas-superposicion { padding: 12px; }
    .materias-grilla.materias-grilla-filtradas .carpetas-titulo, .materias-grilla.materias-grilla-filtradas .carpetas-titulo-con-portada { font-size: 16px; }
    .materias-grilla.materias-grilla-filtradas .carpetas-accion { padding: 8px; font-size: 11px; }
    .materia-tarjeta { background-color: color-mix(in srgb, var(--materia-color) 18%, var(--paper)) !important; color: var(--ink); }
    .materia-tarjeta:not(.carpetas-tarjeta-con-portada) .carpetas-titulo, .materia-tarjeta:not(.carpetas-tarjeta-con-portada) .carpetas-subtitulo { color: var(--ink); }
    .materia-tarjeta-con-portada .carpetas-subtitulo { color: var(--cover-ink); text-shadow: var(--cover-text-shadow); }
    .materia-item .paginas-boton-opciones { border-color: var(--line); border-radius: 8px; background: var(--paper); color: var(--ink); }
    .materia-lista-item { grid-column: 1 / -1; }
    .materia-nueva-tarjeta { aspect-ratio: 4 / 3 !important; }
    .carpetas-tarjeta-elemento { cursor: default; }
    .carpetas-tarjeta-elemento:hover, .carpetas-tarjeta-elemento:focus-visible { transform: none; }
    .materia-nueva-lista { min-height: 44px; }
    .paginas-lista-controles { display: flex; width: auto; flex: 1; flex-wrap: wrap; align-items: center; justify-content: flex-end; gap: 12px; }
    .paginas-vista-selector { display: inline-flex; gap: 4px; padding: 4px; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); }
    .paginas-vista-boton { display: inline-flex; min-height: 34px; align-items: center; justify-content: center; gap: 7px; padding: 0 11px; border: 0; border-radius: 6px; background: transparent; color: var(--muted); font: inherit; font-size: 12px; cursor: pointer; }
    .paginas-vista-boton[aria-pressed="true"] { background: var(--hover-bg); color: var(--ink); font-weight: 650; }
    .pagina-cuatrimestre { grid-column: 1 / -1; min-width: 0; padding: 0; }
    .pagina-cuatrimestre { margin-bottom: 12px; }
    .pagina-cuatrimestre > summary, .pagina-unidad > summary { display: flex; min-height: 51px; align-items: center; justify-content: space-between; gap: 10px; padding: 0 16px; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); color: var(--ink); font-size: 14px; font-weight: 700; cursor: pointer; list-style: none; }
    .pagina-cuatrimestre > summary::-webkit-details-marker, .pagina-unidad > summary::-webkit-details-marker { display: none; }
    .pagina-cuatrimestre > summary::before, .pagina-unidad > summary::before { color: var(--accent); content: "›"; font-size: 20px; line-height: 1; transition: transform .15s ease; }
    .pagina-cuatrimestre[open] > summary::before, .pagina-unidad[open] > summary::before { transform: rotate(90deg); }
    .pagina-cuatrimestre > summary { margin-bottom: 11px; }
    .pagina-unidad { margin: 0 0 9px 18px; }
    .pagina-unidad > summary { min-height: 43px; margin-bottom: 9px; font-size: 13px; font-weight: 600; }
    .paginas-grupo-contenido { display: grid; grid-template-columns: repeat(auto-fill, minmax(min(100%, 220px), 1fr)); gap: 16px; align-items: start; padding: 0 0 12px 18px; }
    .paginas-tarjeta-item { position: relative; min-width: 0; }
    .paginas-tarjeta-nueva-lista { grid-column: 1 / -1; }
    .paginas-tarjeta-nueva-lista .carpetas-tarjeta { width: 100%; min-height: 50px; aspect-ratio: auto; }
    .paginas-grilla-vista-lista { display: flex; flex-direction: column; gap: 8px; }
    .pagina-lista-item { position: relative; display: flex; min-width: 0; align-items: stretch; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); transition: border-color .15s, background .15s; }
    .pagina-lista-item:hover { border-color: var(--accent); background: var(--hover-bg); }
    .pagina-lista-abrir { display: grid; min-width: 0; flex: 1; grid-template-columns: minmax(130px, 1.5fr) minmax(90px, .8fr) minmax(110px, 1fr) minmax(110px, auto) auto; align-items: center; gap: 12px; padding: 12px 54px 12px 16px; border: 0; border-radius: inherit; background: transparent; color: var(--ink); text-align: left; cursor: pointer; }
    .pagina-lista-abrir:hover { background: transparent; }
    .pagina-lista-titulo { overflow: hidden; font-size: 13px; font-weight: 650; text-overflow: ellipsis; white-space: nowrap; }
    .pagina-lista-autor, .pagina-lista-pdf, .pagina-lista-fecha { overflow: hidden; color: var(--muted); font-size: 11px; text-overflow: ellipsis; white-space: nowrap; }
    .pagina-lista-acceso { color: var(--accent); font-size: 11px; font-weight: 650; white-space: nowrap; }
    .pagina-lista-item .paginas-accion-opciones { top: 50%; right: 7px; transform: translateY(-50%); }
    .pagina-lista-item .paginas-menu-opciones { top: 36px; }
    .paginas-organizacion { margin-top: 18px; }
    .paginas-pestanas { display: flex; overflow-x: auto; gap: 24px; border-bottom: 1px solid var(--line); scrollbar-width: none; }
    .paginas-pestanas::-webkit-scrollbar, .paginas-unidades::-webkit-scrollbar { display: none; }
    .paginas-pestana { display: inline-flex; min-height: 44px; flex: 0 0 auto; align-items: center; gap: 8px; padding: 0 2px 9px; border: 0; border-bottom: 2px solid transparent; margin-bottom: -1px; background: transparent; color: var(--muted); font: inherit; font-size: 13px; font-weight: 600; cursor: pointer; transition: color .15s, border-color .15s; }
    .paginas-pestana:hover { color: var(--ink); }
    .paginas-pestana[aria-selected="true"] { border-bottom-color: var(--accent); color: var(--ink); }
    .paginas-pestana-conteo { display: inline-grid; min-width: 20px; height: 19px; place-items: center; padding: 0 6px; border-radius: 10px; background: var(--hover-bg); color: var(--muted); font-size: 10px; font-weight: 600; }
    .paginas-unidades { display: flex; overflow-x: auto; gap: 8px; padding: 14px 0 5px; scrollbar-width: none; }
    .paginas-unidades[hidden] { display: none; }
    .paginas-unidad-chip { min-height: 29px; flex: 0 0 auto; padding: 5px 12px; border: 0; border-radius: 14px; background: var(--paper); color: var(--muted); font: inherit; font-size: 12px; cursor: pointer; transition: color .15s, background .15s; }
    .paginas-unidad-chip:hover { color: var(--ink); }
    .paginas-unidad-chip[aria-pressed="true"] { background: color-mix(in srgb, var(--accent) 15%, var(--paper)); color: var(--accent); font-weight: 600; }
    .pagina-organizacion-panel { min-height: 175px; padding-top: 18px; }
    .pagina-panel-contenido { animation: paginas-fade-in .15s ease both; }
    @keyframes paginas-fade-in { from { opacity: 0; } to { opacity: 1; } }
    .pagina-pestanas-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(min(100%, 220px), 1fr)); gap: 16px; align-items: start; }
    .pagina-pestanas-grid .paginas-tarjeta-item { transition: transform .2s ease; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-tarjeta-item:hover { z-index: 2; transform: translateY(-4px); }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-tarjeta { aspect-ratio: 4 / 3; border: 0; border-radius: 16px; box-shadow: 0 2px 12px rgb(0 0 0 / 8%); transition: transform .2s ease, box-shadow .2s ease; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-tarjeta:hover,
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-tarjeta:focus-visible { box-shadow: 0 12px 28px rgb(0 0 0 / 20%); transform: scale(1.01); }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-portada { object-fit: cover; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-superposicion { align-items: end; justify-items: start; padding: 14px; background: linear-gradient(0deg, rgb(0 0 0 / 78%), rgb(0 0 0 / 25%) 44%, transparent 78%); }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-titulo { color: #FFFFFF; font-size: 16px; font-weight: 500; text-align: left; text-shadow: 0 1px 3px rgb(0 0 0 / 55%); }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .carpetas-titulo-sin-portada { color: #FFFFFF; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-fecha { display: none; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-accion-opciones { opacity: 0; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-tarjeta-item:hover .paginas-accion-opciones,
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-tarjeta-item:focus-within .paginas-accion-opciones { opacity: 1; }
    .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-boton-opciones { width: 30px; height: 30px; border: 0; border-radius: 50%; background: rgb(0 0 0 / 55%); backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px); }
    .pagina-panel-contenido .paginas-grilla-vista-lista { display: flex; flex-direction: column; gap: 8px; }
    .paginas-pie-acciones { display: flex; justify-content: center; padding: 22px 0 4px; }
    .paginas-crear-en-tab { min-height: 38px; padding: 0 14px; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; font-weight: 600; cursor: pointer; }
    .paginas-crear-en-tab:hover { border-color: var(--accent); color: var(--accent); }
    .paginas-estado-vacio { display: grid; min-height: 240px; align-content: center; justify-items: center; gap: 10px; color: var(--muted); text-align: center; }
    .paginas-estado-vacio-icono { display: grid; width: 42px; height: 42px; place-items: center; border-radius: 50%; background: var(--hover-bg); color: var(--muted); font-size: 20px; }
    .paginas-estado-vacio-texto { margin: 0; color: var(--ink); font-size: 14px; font-weight: 550; }
    .paginas-estado-vacio .paginas-crear-en-tab { margin-top: 3px; border-color: transparent; background: var(--accent); color: var(--primary-contrast); }
    .busqueda-vista { width: min(100%, 1140px); padding: 38px clamp(18px, 4vw, 50px) 34px; margin: 52px auto 0; background: var(--section-bg); color: var(--ink); }
    .busqueda-vista h1 { margin: 0 0 8px; font-size: clamp(25px, 4vw, 34px); }
    .busqueda-vista > p { margin: 0 0 22px; color: var(--muted); font-size: 13px; }
    .busqueda-resultados { display: grid; gap: 8px; }
    .busqueda-resultado { display: grid; grid-template-columns: minmax(0, 1fr) minmax(0, 2fr) auto; align-items: center; gap: 14px; padding: 14px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); text-align: left; cursor: pointer; }
    .busqueda-resultado:hover { border-color: var(--accent); background: var(--hover-bg); }
    .busqueda-resultado-titulo { font-size: 13px; font-weight: 650; }
    .busqueda-resultado-detalle, .busqueda-resultado-accion { color: var(--muted); font-size: 11px; }
    .busqueda-resultado-accion { color: var(--accent); white-space: nowrap; }
    .paginas-tarjeta-item .carpetas-tarjeta { display: grid; place-items: center; }
    .paginas-tarjeta-item .carpetas-titulo-sin-portada { padding: 18px; }
    .paginas-tarjeta-item:not(.paginas-tarjeta-nueva) .carpetas-superposicion { align-items: end; justify-items: start; padding: 18px; }
    .paginas-tarjeta-item:not(.paginas-tarjeta-nueva) .carpetas-titulo { text-align: left; }
    .paginas-fecha { min-height: 1em; margin: 9px 2px 0; color: var(--muted); font-size: 11px; line-height: 1.4; }
    .paginas-accion-opciones { position: absolute; z-index: 2; top: 9px; right: 9px; opacity: 1; transition: opacity .2s; }
    .paginas-tarjeta-item:hover .paginas-accion-opciones, .paginas-tarjeta-item:focus-within .paginas-accion-opciones, .carpetas-item-con-opciones:hover .paginas-accion-opciones, .carpetas-item-con-opciones:focus-within .paginas-accion-opciones { opacity: 1; }
    .paginas-boton-opciones { display: grid; width: 33px; height: 33px; place-items: center; border: 1px solid var(--dark-control-border); background: var(--dark-control-bg); color: var(--cover-ink); font-size: 21px; line-height: 1; cursor: pointer; }
    .paginas-menu-opciones { position: absolute; top: 39px; right: 0; display: grid; width: 174px; gap: 2px; padding: 5px; border: 1px solid var(--line); background: var(--paper); box-shadow: var(--float-shadow); }
    .paginas-menu-opciones[hidden] { display: none; }
    .paginas-menu-opciones button, .paginas-menu-opciones label { display: flex; min-height: 33px; align-items: center; padding: 0 8px; border: 0; background: transparent; color: var(--ink); font: inherit; font-size: 12px; text-align: left; cursor: pointer; }
    .paginas-menu-opciones button:hover, .paginas-menu-opciones label:hover { background: var(--hover-bg); }
    .paginas-menu-opciones .paginas-color-opcion { justify-content: space-between; gap: 8px; }
    .paginas-menu-opciones input[type="color"] { width: 30px; height: 25px; padding: 1px; border: 1px solid var(--line); background: var(--input-bg); cursor: pointer; }
    .paginas-menu-opciones input[type="file"] { display: none; }
    .paginas-crear-tarjeta { display: grid !important; align-content: center; justify-items: center; gap: 8px; border: 1px dashed var(--line); background: var(--paper) !important; color: var(--muted) !important; }
    .paginas-crear-tarjeta:hover, .paginas-crear-tarjeta:focus-visible { border-color: var(--accent); background: var(--hover-bg) !important; color: var(--accent) !important; }
    .paginas-crear-mas { font-size: 34px; font-weight: 300; line-height: 1; }
    .paginas-crear-texto { font-size: 12px; font-weight: 650; }
    .pagina-unidad-acciones { display: flex; justify-content: flex-start; padding: 0 0 12px 18px; }
    .pagina-unidad-agregar { min-height: 34px; padding: 0 11px; border: 1px solid var(--line); border-radius: 7px; background: var(--paper); color: var(--ink); font: inherit; font-size: 11px; font-weight: 600; cursor: pointer; }
    .pagina-unidad-agregar:hover { border-color: var(--accent); color: var(--accent); }
    .pagina-grupo-conteo { margin-left: auto; color: var(--muted); font-size: 11px; font-weight: 500; white-space: nowrap; }
    .pagina-grupo-vacio { grid-column: 1 / -1; margin: 0; color: var(--muted); font-size: 12px; }
    .paginas-ayuda { max-width: none; margin: -5px 0 20px; font-size: 12px; }
    .paginas-mensaje-vacio { grid-column: 1 / -1; padding: 13px 15px; margin: 0 0 14px; border: 1px dashed var(--line); border-radius: 8px; color: var(--muted); font-size: 12px; }
    .paginas-paginacion { margin-top: 27px; }
    .paginas-enlaces { padding-top: 34px; }
    .paginas-enlaces .vista-enlaces-cabecera { margin: 0 0 14px; }
    .paginas-etiquetas-enlaces { display: flex; flex-wrap: wrap; gap: 8px; padding: 0; margin: 0 0 13px; list-style: none; }
    .paginas-etiqueta-enlace { display: inline-flex; min-height: 31px; align-items: center; gap: 8px; padding: 0 7px 0 10px; border: 1px solid var(--line); background: var(--paper); font-size: 11px; }
    .paginas-etiqueta-enlace a { overflow-wrap: anywhere; color: var(--accent); text-decoration: none; }
    .paginas-etiqueta-enlace a:hover { text-decoration: underline; }
    .paginas-quitar-enlace { display: grid; width: 21px; height: 21px; place-items: center; padding: 0; border: 0; background: transparent; color: var(--muted); font-size: 17px; cursor: pointer; }
    .paginas-quitar-enlace:hover { background: var(--hover-bg); color: var(--danger); }
    .paginas-agregar-enlace { min-height: 34px; padding: 0 11px; border: 1px solid var(--primary); background: var(--primary); color: var(--primary-contrast); font: inherit; font-size: 11px; cursor: pointer; }
    .paginas-agregar-enlace:hover { border-color: var(--accent); background: var(--accent); color: var(--primary-contrast); }
    .paginas-formulario-enlace[hidden] { display: none; }
    .pagina-pdf-adjunto { width: min(100%, 1000px); padding: 20px clamp(18px, 4vw, 36px); margin: 0 auto 36px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); }
    .pagina-pdf-adjunto[hidden] { display: none; }
    .pagina-pdf-encabezado { display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 12px; }
    .pagina-pdf-encabezado h2 { margin: 0; font-size: 17px; }
    .pagina-pdf-metadatos { margin: 5px 0 0; color: var(--muted); font-size: 12px; }
    .pagina-pdf-abrir { min-height: 38px; padding: 0 14px; border: 1px solid var(--primary); background: var(--primary); color: var(--primary-contrast); font: inherit; font-size: 12px; font-weight: 600; cursor: pointer; }
    .pagina-pdf-abrir:hover { border-color: var(--accent); background: var(--accent); }
    .pagina-pdf-descargar { min-height: 38px; padding: 0 14px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; cursor: pointer; }
    .pagina-pdf-descargar:hover { border-color: var(--accent); color: var(--accent); }
    .pagina-pdf-marco { display: block; width: 100%; height: min(78vh, 900px); min-height: 420px; margin-top: 16px; border: 1px solid var(--line); background: var(--canvas); }
    .pagina-pdf-marco[hidden] { display: none; }
    .pagina-pdf-marco { color-scheme: auto; }
    @media (max-width: 700px) {
      .paginas-vista { padding-top: 22px; }
      .paginas-filtro-contenedor { align-items: center; }
      .galeria-tamano-control { justify-content: space-between; }
      .paginas-lista-controles { flex: 0 1 auto; }
      .paginas-grupo-contenido:not(.paginas-grilla-vista-lista) { grid-template-columns: repeat(2, minmax(0, 1fr)); padding-left: 8px; }
      .pagina-unidad { margin-left: 8px; }
      .pagina-pestanas-grid { grid-template-columns: repeat(auto-fill, minmax(min(100%, 220px), 1fr)); }
      .pagina-pestanas-grid:not(.paginas-grilla-vista-lista) .paginas-accion-opciones { opacity: 1; }
      .paginas-pestanas { gap: 18px; }
      .pagina-lista-abrir { grid-template-columns: minmax(0, 1fr) auto; gap: 5px 10px; }
      .pagina-lista-titulo, .pagina-lista-pdf { grid-column: 1; }
      .pagina-lista-autor, .pagina-lista-fecha { grid-column: 1; }
      .pagina-lista-acceso { grid-column: 2; grid-row: 1 / span 3; }
      .pagina-unidad-acciones { padding-left: 8px; }
      .busqueda-resultado { grid-template-columns: minmax(0, 1fr) auto; gap: 7px 12px; }
      .busqueda-resultado-detalle { grid-column: 1; }
      .busqueda-resultado-accion { grid-column: 2; grid-row: 1 / span 2; }
    }

    .vista-contenido { width: min(100%, 820px); padding: 36px clamp(20px, 5vw, 58px) 70px; margin: 0 auto; }
    .config-contenido { width: min(100%, 900px); padding: 45px clamp(20px, 5vw, 58px) 70px; margin: 0 auto; }
    .vista-encabezado { margin-bottom: 30px; }
    .vista-volver { padding: 7px 0; border: 0; background: transparent; color: var(--muted); font-size: 13px; cursor: pointer; }
    .vista-volver:hover { color: var(--ink); }
    .vista-titulo, .config-titulo { margin: 12px 0 0; font-size: clamp(28px, 4vw, 40px); font-weight: 700; letter-spacing: -1px; }
    .config-campo { display: grid; gap: 8px; color: var(--muted); font-size: 12px; font-weight: 600; }
    .vista-subtitulo { margin: 0; font-size: 19px; }
    .vista-enlaces-cabecera { margin: 30px 0 14px; }
    .vista-formulario-enlace { display: grid; grid-template-columns: 1fr 1.5fr auto; gap: 9px; }
    .vista-campo, .config-campo input:not([type="file"]):not([type="color"]) { min-width: 0; min-height: 39px; padding: 0 11px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; }
    .vista-boton, .config-agregar { min-height: 39px; padding: 0 14px; border: 1px solid var(--primary); background: var(--primary); color: var(--primary-contrast); font: inherit; font-size: 12px; cursor: pointer; }
    .vista-enlaces { display: grid; gap: 8px; padding: 0; margin: 15px 0 0; list-style: none; }
    .vista-enlace { display: flex; min-height: 43px; align-items: center; justify-content: space-between; gap: 12px; padding: 8px 12px; border: 1px solid var(--line); background: var(--paper); font-size: 13px; }
    .vista-enlace a { overflow-wrap: anywhere; color: var(--accent); }
    .vista-eliminar-enlace { flex: 0 0 auto; border: 0; background: transparent; color: var(--danger); font-size: 11px; cursor: pointer; }
    .vista-eliminar-enlace:hover, .config-eliminar:hover, .pagina-eliminar:hover { color: var(--danger-hover); }
    .vista-sin-enlaces { padding: 12px 0; color: var(--muted); font-size: 12px; }
    .config-titulo { margin: 0; }
    .config-descripcion { margin: 10px 0 25px; color: var(--muted); font-size: 13px; line-height: 1.6; }
    .config-lista { display: grid; gap: 13px; }
    .config-tarjeta { display: grid; grid-template-columns: minmax(0, 1fr) 130px; gap: 20px; align-items: center; padding: 16px; border: 1px solid var(--line); background: var(--paper); }
    .config-campos { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 13px; }
    .config-campo input[type="file"] { max-width: 100%; color: var(--muted); font-size: 11px; }
    .config-color-campo input[type="color"] { width: 43px; height: 35px; padding: 2px; border: 1px solid var(--line); background: var(--paper); cursor: pointer; }
    .config-color-muestras { display: grid; grid-column: 1 / -1; grid-template-columns: repeat(6, minmax(0, 1fr)); gap: 6px; }
    .config-color-muestra { min-width: 0; height: 25px; padding: 0; border: 1px solid var(--line); background: var(--muestra-color); cursor: pointer; }
    .config-color-muestra[aria-pressed="true"] { outline: 2px solid var(--accent); outline-offset: 1px; }
    .config-color-muestra:hover { transform: translateY(-1px); }
    .config-vista-previa { display: grid; width: 130px; aspect-ratio: 1; place-items: center; overflow: hidden; color: var(--card-ink); font-size: 13px; text-align: center; }
    .config-vista-previa img { width: 100%; height: 100%; object-fit: cover; }
    .config-eliminar { justify-self: start; padding: 7px 0; border: 0; background: transparent; color: var(--danger); font: inherit; font-size: 11px; cursor: pointer; }
    .config-agregar { margin-top: 18px; }
    .secciones-ajustes { padding: 18px; margin: 20px 0; border: 1px solid var(--line); background: var(--paper); }
    .secciones-ajustes h2 { margin: 0 0 6px; font-size: 17px; }
    .secciones-ajustes > p { margin: 0 0 14px; color: var(--muted); font-size: 12px; }
    .secciones-ajustes-lista { display: grid; gap: 7px; }
    .secciones-ajustes-item { display: flex; align-items: center; gap: 8px; padding: 8px; border: 1px solid var(--line); background: var(--paper); transition: transform .2s ease, box-shadow .2s ease; }
    .secciones-ajustes-item.seccion-arrastrada { z-index: 4; pointer-events: none; box-shadow: var(--float-shadow); }
    .secciones-ajustes-placeholder { min-height: 52px; border: 1px dashed var(--accent); }
    .secciones-ajustes-handle { width: 30px; min-height: 34px; padding: 0; border: 0; background: transparent; color: var(--muted); font-size: 16px; letter-spacing: -4px; cursor: grab; touch-action: none; }
    .secciones-ajustes-item input { min-width: 0; min-height: 34px; flex: 1; padding: 0 8px; border: 1px solid var(--line); background: var(--input-bg); color: var(--ink); }
    .secciones-ajustes-item button { min-height: 32px; padding: 4px 8px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); cursor: pointer; }
    .secciones-ajustes-item [data-seccion-eliminar] { color: var(--danger); }
    .secciones-ajustes-vacia { color: var(--muted); font-size: 12px; }
    .config-vacio { color: var(--muted); font-size: 13px; }
    :root[data-theme="dark"] .vista-campo, :root[data-theme="dark"] .config-campo input:not([type="file"]):not([type="color"]) { border-color: var(--line); }
    .orden-seccion { padding: 17px; margin: 19px 0 26px; border: 1px solid var(--line); background: var(--paper); }
    .orden-encabezado { display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 12px; margin-bottom: 13px; }
    .orden-titulo { margin: 0; color: var(--ink); font-size: 15px; font-weight: 700; }
    .orden-ayuda { margin: 5px 0 0; color: var(--muted); font-size: 11px; line-height: 1.5; }
    .orden-acciones { display: flex; flex-wrap: wrap; gap: 7px; }
    .orden-accion { min-height: 32px; padding: 0 9px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 11px; cursor: pointer; }
    .orden-accion:hover:not(:disabled) { border-color: var(--accent); color: var(--accent); }
    .orden-accion:disabled { opacity: .55; cursor: default; }
    .orden-lista { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 8px; }
    .orden-bloque { display: flex; min-width: 0; min-height: 58px; align-items: center; gap: 8px; padding: 7px; border: 1px solid var(--line); background: var(--paper); transition: transform .2s ease, border-color .2s, box-shadow .2s, opacity .2s; }
    .orden-bloque:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
    .orden-bloque.orden-arrastrado { z-index: 20; opacity: .72; box-shadow: var(--float-shadow); transition: none !important; }
    .orden-mango { display: grid; width: 28px; height: 36px; flex: 0 0 auto; place-items: center; border: 0; background: transparent; color: var(--muted); font-size: 17px; letter-spacing: -4px; cursor: grab; touch-action: none; }
    .orden-mango:active { cursor: grabbing; }
    .orden-miniatura { display: grid; width: 40px; height: 40px; flex: 0 0 auto; overflow: hidden; place-items: center; background: var(--folder-color, var(--hover-bg)); color: var(--card-ink); font-size: 13px; font-weight: 700; }
    .orden-miniatura img { width: 100%; height: 100%; object-fit: cover; }
    .orden-nombre { min-width: 0; flex: 1; overflow: hidden; color: var(--ink); font-size: 12px; font-weight: 650; text-overflow: ellipsis; white-space: nowrap; }
    .orden-movimientos { display: flex; flex: 0 0 auto; gap: 3px; }
    .orden-mover { display: grid; width: 27px; height: 29px; place-items: center; padding: 0; border: 1px solid transparent; background: transparent; color: var(--muted); font-size: 13px; cursor: pointer; }
    .orden-mover:hover:not(:disabled) { border-color: var(--line); color: var(--accent); }
    .orden-mover:disabled { opacity: .35; cursor: default; }
    .orden-placeholder { min-height: 58px; border: 2px dashed var(--accent); background: var(--hover-bg); }
    .orden-aviso { min-height: 1em; margin: 10px 0 0; color: var(--muted); font-size: 11px; }
    .orden-vacio { padding: 14px; color: var(--muted); font-size: 12px; text-align: center; }
    .orden-anuncio { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; }
    .pagina-lista { display: grid; gap: 9px; margin-top: 22px; }
    .pagina-fila { display: flex; min-height: 58px; align-items: center; gap: 12px; padding: 9px 13px; border: 1px solid var(--line); background: var(--paper); }
    .pagina-fila[draggable="true"] { cursor: grab; }
    .pagina-fila.pagina-arrastrando { opacity: .5; }
    .pagina-fila.pagina-destino { border-color: var(--accent); }
    .pagina-abrir { display: flex; min-width: 0; flex: 1; align-items: center; gap: 12px; padding: 0; border: 0; background: transparent; color: var(--ink); text-align: left; cursor: pointer; }
    .pagina-abrir:hover .pagina-nombre { color: var(--accent); }
    .pagina-icono { display: inline-flex; flex: 0 0 auto; color: var(--accent); }
    .pagina-icono svg { width: 18px; height: 18px; }
    .pagina-resumen { min-width: 0; }
    .pagina-nombre { overflow: hidden; margin: 0; font-size: 13px; font-weight: 650; text-overflow: ellipsis; white-space: nowrap; }
    .pagina-renombrar { width: min(100%, 360px); padding: 3px 6px; border: 1px solid var(--accent); background: var(--paper); color: var(--ink); font: inherit; font-size: 13px; }
    .pagina-fecha { margin: 4px 0 0; color: var(--muted); font-size: 10px; }
    .pagina-eliminar { flex: 0 0 auto; padding: 6px; border: 0; background: transparent; color: var(--danger); font-size: 11px; cursor: pointer; }
    .pagina-nueva { justify-self: start; min-height: 40px; padding: 0 13px; margin-top: 12px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; cursor: pointer; }
    .pagina-nueva:hover { border-color: var(--accent); color: var(--accent); }
    .pagina-vacia { padding: 17px 0; color: var(--muted); font-size: 13px; }
    .pagina-editor { position: relative; isolation: isolate; width: min(100%, 1140px); padding: 36px clamp(20px, 5vw, 58px) 70px; margin: 0 auto; }
    .pagina-editor::before { position: fixed; z-index: -1; inset: 0; background-image: linear-gradient(color-mix(in srgb, var(--paper) var(--editor-vector-suavizado, 45%), transparent), color-mix(in srgb, var(--paper) var(--editor-vector-suavizado, 45%), transparent)), var(--editor-vector-image, none); background-repeat: no-repeat, var(--editor-vector-repeat, no-repeat); background-position: center, var(--editor-vector-position, center); background-size: cover, var(--editor-vector-size, cover); content: ""; pointer-events: none; }
    .fondo-capa { position: fixed; z-index: 0; top: 0; left: 0; width: 100vw; height: calc(100dvh + var(--fondo-recorrido)); overflow: hidden; pointer-events: none; transform: translate3d(0, 0, 0); will-change: transform; }
    .fondo-imagen { display: block; width: 100%; height: 100%; object-fit: cover; object-position: center; filter: none; }
    .lectura-panel { position: relative; isolation: isolate; width: min(100%, 1100px); padding: 65px; margin: 36px auto 30px; border: 1px solid color-mix(in srgb, var(--lectura-panel-fondo, var(--paper)) 28%, var(--line)); border-radius: 12px; background: color-mix(in srgb, var(--lectura-panel-fondo, var(--paper)) var(--lectura-opacidad-panel, 85%), transparent); color: var(--lectura-panel-texto, var(--ink)); backdrop-filter: blur(3px); -webkit-backdrop-filter: blur(3px); transition: width .25s ease, background .2s ease, backdrop-filter .2s ease; }
    .lectura-panel.lectura-tono-claro { --lectura-panel-fondo: #FFFFFF; --lectura-panel-texto: #17171A; }
    .lectura-panel.lectura-tono-oscuro { --lectura-panel-fondo: #111114; --lectura-panel-texto: #F2F2F0; }
    .lectura-panel.lectura-panel-solido { background: var(--lectura-panel-fondo, var(--paper)); backdrop-filter: none; -webkit-backdrop-filter: none; }
    .lectura-panel.lectura-panel-sin-panel { border-color: transparent; background: transparent; backdrop-filter: none; -webkit-backdrop-filter: none; }
    .lectura-panel.lectura-panel-sin-panel .page-title,
    .lectura-panel.lectura-panel-sin-panel .editor-contenido { color: var(--ink); text-shadow: 0 1px 3px color-mix(in srgb, var(--paper) 80%, transparent); }
    .lectura-panel .page-title { color: var(--lectura-panel-texto, var(--ink)); }
    .lectura-panel.lectura-panel-con-panel { width: min(100%, 1100px); }
    .editor-titulo-fila { position: relative; display: flex; align-items: center; gap: 14px; margin-bottom: 8px; }
    .pagina-editor .page-title { display: block; min-height: 1.2em; flex: 1; text-align: center; }
    .pagina-editor .page-title:empty::before { color: var(--page-placeholder); content: "Sin título"; pointer-events: none; }
    .pagina-editor .page-content { min-height: 55vh; white-space: normal; overflow-wrap: anywhere; }
    .pagina-editor .page-content:empty { position: relative; }
    .pagina-editor .page-content:empty::before { position: absolute; inset: 0 auto auto 0; color: var(--page-placeholder); content: "Presioná '/' para ver los comandos"; pointer-events: none; user-select: none; -webkit-user-select: none; }
    .editor-panel-toggle { display: inline-flex; min-height: 39px; flex: 0 0 auto; align-items: center; justify-content: center; gap: 8px; padding: 0 12px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; font-weight: 600; cursor: pointer; }
    .editor-bloque > .editor-panel-toggle { position: sticky; top: 78px; z-index: 2; float: right; margin-bottom: -39px; }
    .editor-panel-toggle:hover, .editor-panel-toggle[aria-expanded="true"] { border-color: var(--accent); color: var(--accent); }
    .editor-panel-toggle svg { width: 16px; height: 16px; }
    .editor-disposicion { display: grid; grid-template-columns: minmax(0, 1fr) 0px; gap: 0; align-items: start; transition: grid-template-columns .25s ease, gap .25s ease; }
    .editor-disposicion.panel-abierto { grid-template-columns: minmax(0, 1fr) minmax(0, 320px); gap: 20px; }
    .editor-bloque { position: relative; min-width: 0; width: 100%; max-width: 1000px; margin: 0 auto; }
    .panel-edicion { min-width: 0; max-height: calc(100vh - 150px); overflow: auto; padding: 0; border: 0 solid var(--line); background: var(--paper); color: var(--ink); opacity: 0; visibility: hidden; transition: opacity .18s ease, visibility .25s; }
    .panel-abierto .panel-edicion { padding: 12px; border-width: 1px; opacity: 1; visibility: visible; }
    .panel-edicion[hidden] { display: none; }
    .panel-encabezado { display: flex; align-items: center; justify-content: space-between; gap: 8px; margin-bottom: 12px; }
    .panel-titulo { margin: 0; font-size: 13px; font-weight: 700; }
    .panel-cerrar { display: grid; width: 30px; height: 30px; place-items: center; border: 1px solid var(--line); background: var(--paper); color: var(--muted); font-size: 19px; cursor: pointer; }
    .panel-seccion { border-top: 1px solid var(--line); }
    .panel-seccion-encabezado { margin: 0; }
    .panel-seccion-boton { display: flex; width: 100%; min-height: 39px; align-items: center; justify-content: space-between; padding: 0 2px; border: 0; background: transparent; color: var(--ink); font: inherit; font-size: 11px; font-weight: 750; letter-spacing: .7px; text-align: left; cursor: pointer; }
    .panel-seccion-boton::after { color: var(--muted); content: "⌄"; font-size: 16px; transition: transform .2s ease; }
    .panel-seccion-boton[aria-expanded="true"]::after { transform: rotate(180deg); }
    .panel-seccion-contenido { display: grid; gap: 9px; padding: 0 0 12px; }
    .panel-seccion-contenido[hidden] { display: none; }
    .panel-campo { display: grid; min-width: 0; gap: 5px; color: var(--muted); font-size: 11px; }
    .panel-campo select, .panel-campo input[type="color"] { width: 100%; min-height: 34px; padding: 4px 7px; border: 1px solid var(--line); background: var(--input-bg); color: var(--ink); font: inherit; font-size: 12px; }
    .panel-campo input[type="range"] { width: 100%; accent-color: var(--accent); }
    .panel-campo input[type="color"] { padding: 3px; cursor: pointer; }
    .panel-grupo { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 6px; }
    .panel-grupo.panel-grupo-inline { grid-template-columns: repeat(4, minmax(0, 1fr)); }
    .panel-control { min-height: 34px; padding: 5px 7px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 11px; cursor: pointer; }
    .panel-control:hover, .panel-control[aria-pressed="true"] { border-color: var(--accent); background: color-mix(in srgb, var(--accent) 9%, var(--paper)); color: var(--accent); }
    .panel-control:focus-visible, .panel-seccion-boton:focus-visible, .panel-cerrar:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
    .panel-historial { display: grid; grid-template-columns: 1fr 1fr; gap: 6px; }
    .panel-historial .panel-control { font-size: 12px; }
    .panel-archivo { display: grid; gap: 8px; }
    .panel-archivo-boton { min-height: 35px; padding: 6px 9px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); font: inherit; font-size: 11px; text-align: center; cursor: pointer; }
    .panel-archivo-boton:hover { border-color: var(--accent); color: var(--accent); }
    .panel-parallax { display: flex; align-items: flex-start; gap: 8px; color: var(--ink); font-size: 11px; line-height: 1.45; }
    .panel-parallax input { margin: 1px 0 0; accent-color: var(--accent); }
    .panel-ayuda { margin: 0; color: var(--muted); font-size: 10px; line-height: 1.5; }
    .editor-menu-comandos[hidden] { display: none; }
    .editor-menu-comandos { position: absolute; z-index: 45; display: flex; width: min(310px, calc(100vw - 24px)); max-height: min(330px, calc(100vh - 24px)); flex-direction: column; overflow: hidden; padding: 0; border: 1px solid color-mix(in srgb, var(--line) 75%, transparent); border-radius: 10px; background: var(--paper); box-shadow: var(--soft-shadow); animation: editor-menu-aparecer .1s ease-out both; }
    @keyframes editor-menu-aparecer { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: translateY(0); } }
    .editor-comandos-encabezado { display: flex; min-height: 39px; flex: 0 0 auto; align-items: center; gap: 8px; padding: 0 12px; border-bottom: 1px solid var(--line); color: var(--muted); font-size: 11px; }
    .editor-comandos-slash { color: var(--ink); font-size: 13px; font-weight: 650; }
    .editor-comandos-busqueda { color: var(--muted); }
    .editor-comandos-lista { min-height: 0; overflow: auto; padding: 5px; scrollbar-color: color-mix(in srgb, var(--muted) 42%, transparent) transparent; scrollbar-width: thin; }
    .editor-comandos-lista::-webkit-scrollbar { width: 6px; }
    .editor-comandos-lista::-webkit-scrollbar-track { background: transparent; }
    .editor-comandos-lista::-webkit-scrollbar-thumb { border-radius: 6px; background: color-mix(in srgb, var(--muted) 42%, transparent); }
    .editor-comandos-lista::-webkit-scrollbar-button { display: none; width: 0; height: 0; }
    .editor-comandos-seccion + .editor-comandos-seccion { margin-top: 5px; }
    .editor-comandos-seccion-titulo { padding: 7px 8px 4px; color: var(--muted); font-size: 10px; font-weight: 600; }
    .editor-menu-comandos button { display: grid; width: 100%; min-height: 32px; grid-template-columns: 24px minmax(0, 1fr) auto; align-items: center; gap: 8px; padding: 4px 8px; border: 0; border-radius: 6px; background: transparent; color: var(--ink); text-align: left; cursor: pointer; }
    .editor-menu-comandos button[aria-selected="true"] { background: color-mix(in srgb, var(--hover-bg) 72%, var(--paper)); color: var(--ink); }
    .editor-comando-icono { display: grid; width: 22px; height: 22px; place-items: center; color: var(--muted); }
    .editor-comando-icono svg { display: block; width: 19px; height: 19px; fill: none; stroke: currentColor; stroke-linecap: round; stroke-linejoin: round; stroke-width: 1.5; }
    .editor-comando-nombre { overflow: hidden; font-size: 12px; font-weight: 500; text-overflow: ellipsis; white-space: nowrap; }
    .editor-comando-atajo { color: var(--muted); font-family: ui-monospace, SFMono-Regular, Consolas, monospace; font-size: 10px; opacity: .72; }
    .editor-comandos-sin-resultados { padding: 12px 10px; color: var(--muted); font-size: 12px; }
    .editor-comandos-pie { display: flex; min-height: 34px; flex: 0 0 auto; align-items: center; justify-content: space-between; padding: 0 11px; border-top: 1px solid var(--line); color: var(--muted); font-size: 10px; }
    .editor-comandos-pie-tecla { padding: 2px 5px; border: 1px solid var(--line); border-radius: 4px; font-size: 9px; }
    .editor-dialog { position: fixed; z-index: 40; inset: 0; display: grid; place-items: center; padding: 18px; background: rgb(0 0 0 / 38%); }
    .editor-dialog[hidden] { display: none; }
    .editor-dialog-contenido { display: grid; width: min(100%, 360px); gap: 14px; padding: 20px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); box-shadow: var(--float-shadow); }
    .editor-dialog-contenido h2 { margin: 0; font-size: 17px; }
    .editor-dialog-contenido label { display: grid; gap: 5px; color: var(--muted); font-size: 12px; }
    .editor-dialog-contenido input { min-height: 36px; padding: 6px 9px; border: 1px solid var(--line); background: var(--input-bg); color: var(--ink); }
    .editor-dialog-acciones { display: flex; justify-content: flex-end; gap: 8px; }
    .editor-dialog-acciones button { min-height: 34px; padding: 6px 11px; border: 1px solid var(--line); background: var(--paper); color: var(--ink); cursor: pointer; }
    .editor-dialog-acciones button[type="submit"] { border-color: var(--primary); background: var(--primary); color: var(--primary-contrast); }
    .editor-bloque { position: relative; }
    .lectura-panel .editor-contenido { display: flow-root; min-height: 55vh; padding: 0 0 20px; outline: none; color: var(--lectura-panel-texto, var(--ink)); font-family: Arial, Helvetica, sans-serif; font-size: 17px; font-weight: 400; line-height: 1.75; }
    .editor-contenido:empty { position: relative; }
    .editor-contenido:empty::before { position: absolute; inset: 0 auto auto 0; color: var(--page-placeholder); content: "Presioná '/' para ver los comandos"; pointer-events: none; user-select: none; -webkit-user-select: none; }
    .editor-contenido p[data-placeholder],
    .editor-contenido div[data-placeholder] { position: relative; }
    .editor-contenido p[data-placeholder]::before,
    .editor-contenido div[data-placeholder]::before { position: absolute; inset: 0 auto auto 0; color: var(--page-placeholder); content: attr(data-placeholder); pointer-events: none; user-select: none; -webkit-user-select: none; }
    .editor-contenido > :first-child { margin-top: 0; }
    .pagina-editor .editor-contenido p { margin: 0 0 14px; text-indent: 0; text-wrap: pretty; }
    .pagina-editor .editor-contenido h1,
    .pagina-editor .editor-contenido h2,
    .pagina-editor .editor-contenido h3 { margin: 0 0 8px; text-wrap: pretty; }
    .pagina-editor .editor-contenido [data-editor-espaciado="chico"] { margin-block: 4px; }
    .pagina-editor .editor-contenido [data-editor-espaciado="grande"] { margin-block: 24px; }
    .editor-contenido br { line-height: 1.75; }
    .editor-contenido h1, .editor-contenido h2, .editor-contenido h3 { line-height: 1.3; }
    .editor-contenido h1 { font-size: 1.9em; }
    .editor-contenido h2 { font-size: 1.5em; }
    .editor-contenido h3 { font-size: 1.2em; }
    .editor-contenido pre { overflow-x: auto; padding: 12px 14px; border: 1px solid var(--line); background: color-mix(in srgb, var(--paper) 82%, var(--hover-bg)); font-family: Consolas, "Courier New", monospace; font-size: .9em; white-space: pre-wrap; }
    .editor-contenido ul, .editor-contenido ol { padding-left: 1.6em; }
    .editor-contenido .editor-tareas { padding-left: 0; list-style: none; }
    .editor-contenido .editor-tareas li { display: flex; align-items: flex-start; gap: 8px; }
    .editor-contenido .editor-tareas input[type="checkbox"] { margin-top: .5em; accent-color: var(--accent); }
    .editor-contenido hr { border: 0; border-top: 1px solid var(--line); margin: 22px 0; }
    .editor-contenido table { width: 100%; border-collapse: collapse; table-layout: fixed; }
    .editor-contenido td, .editor-contenido th { min-width: 50px; padding: 8px 10px; border: 1px solid var(--line); vertical-align: top; }
    .editor-contenido th { background: var(--hover-bg); font-weight: 650; }
    .editor-imagen { position: relative; display: flow-root; width: 60%; max-width: 100%; min-width: 120px; overflow: visible; margin: 14px 2% 14px 0; resize: both; }
    .editor-imagen { float: left; }
    .editor-imagen[data-image-alignment="derecha"], .editor-imagen[data-image-alignment-preview="derecha"] { float: right; margin: 14px 0 14px 2%; }
    .editor-imagen[data-image-alignment="centro"], .editor-imagen[data-image-alignment-preview="centro"] { float: none; margin: 14px auto; }
    .editor-imagen[data-image-alignment-preview="izquierda"] { float: left; margin: 14px 2% 14px 0; }
    .editor-imagen-marco { width: 100%; overflow: hidden; }
    .editor-imagen img { display: block; width: 100%; height: auto; }
    .editor-imagen[data-image-crop="true"] .editor-imagen-marco { aspect-ratio: 16 / 9; }
    .editor-imagen[data-image-crop="true"] .editor-imagen-marco img { width: 100%; height: 100%; object-fit: cover; }
    .editor-imagen-menu { position: absolute; z-index: 5; top: 5px; right: 5px; display: grid; width: 26px; height: 24px; place-items: center; padding: 0; border: 1px solid var(--line); border-radius: 5px; background: color-mix(in srgb, var(--paper) 94%, transparent); color: var(--ink); font: inherit; font-size: 18px; line-height: 1; cursor: pointer; box-shadow: var(--soft-shadow); }
    .editor-imagen-menu:hover, .editor-imagen-menu:focus-visible { border-color: var(--accent); color: var(--accent); }
    .editor-imagen:not(.editor-imagen-controles-ocultos) .editor-imagen-menu { display: none; }
    .editor-imagen-barra { position: absolute; z-index: 4; right: 0; left: 0; display: flex; min-height: 32px; flex-wrap: wrap; align-items: center; justify-content: center; gap: 7px; padding: 5px 8px; border: 1px solid var(--line); background: color-mix(in srgb, var(--paper) 94%, transparent); }
    .editor-imagen-barra-superior { top: 0; }
    .editor-imagen-controles-ocultos .editor-imagen-barra { display: none; }
    .editor-imagen-alineacion { min-height: 26px; padding: 3px 7px; border: 1px solid var(--line); border-radius: 5px; background: var(--paper); color: var(--ink); font: inherit; font-size: 11px; }
    .editor-imagen-aplicar { min-height: 26px; padding: 3px 9px; border: 1px solid var(--primary); border-radius: 5px; background: var(--primary); color: var(--primary-contrast); font: inherit; font-size: 11px; cursor: pointer; }
    .editor-imagen-aplicar[hidden] { display: none; }
    .editor-imagen-barra .editor-borrar-bloque { position: static; min-height: 26px; padding: 3px 9px; border-radius: 5px; box-shadow: none; }
    .editor-fondo-vector { display: grid; gap: 9px; padding-top: 10px; border-top: 1px solid var(--line); }
    .editor-fondo-vector h4 { margin: 0; color: var(--ink); font-size: 12px; }
    .editor-fondo-vector-galeria { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 6px; }
    .editor-fondo-vector-opcion { min-width: 0; height: 44px; border: 1px solid var(--line); border-radius: 5px; background-color: var(--paper); background-position: center; background-size: cover; cursor: pointer; }
    .editor-fondo-vector-opcion[aria-pressed="true"] { border: 2px solid var(--accent); }
    .editor-fondo-vector-opcion:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
    .editor-fondo-vector-ajustes { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 8px; }
    .editor-fondo-vector-ajustes .panel-campo { min-width: 0; }
    .editor-fondo-vector-ajustes input[type="range"] { width: 100%; }
    .editor-fondo-vector-acciones { display: flex; gap: 7px; }
    .editor-fondo-vector-acciones .panel-control { flex: 1; }
    .editor-imagen figcaption { padding: 4px; color: var(--muted); font-size: 11px; line-height: 1.4; }
    .editor-imagen, .editor-tabla-bloque { position: relative; }
    .editor-borrar-bloque { position: absolute; z-index: 2; top: 5px; right: 5px; min-height: 28px; padding: 4px 8px; border: 1px solid var(--line); border-radius: 5px; background: var(--paper); color: var(--danger); font: inherit; font-size: 11px; cursor: pointer; box-shadow: var(--soft-shadow); }
    .editor-borrar-bloque:hover { border-color: var(--danger); }
    .editor-tabla-bloque { padding-top: 34px; margin: 14px 0; }
    .editor-tabla-bloque table { margin: 0; }
    .editor-tabla-acciones { display: flex; flex-wrap: wrap; gap: 5px; padding: 7px 0; }
    .corrector-error { text-decoration: underline wavy var(--danger) 1.5px; text-decoration-skip-ink: none; cursor: help; }
    ::highlight(corrector-errors) { text-decoration: underline wavy var(--danger) 1.5px; text-decoration-skip-ink: none; }
    .pagina-miga { padding: 0; border: 0; background: transparent; color: inherit; cursor: pointer; }
    .pagina-miga:hover { color: var(--accent); }
    .pagina-editor-herramientas { display: flex; align-items: center; justify-content: space-between; gap: 12px; margin-bottom: 26px; }
    .corrector-estado { min-height: 1em; color: var(--muted); font-size: 11px; }
    .corrector-error { text-decoration: underline wavy var(--danger) 1.5px; text-decoration-skip-ink: none; cursor: help; }
    .corrector-globito { position: fixed; z-index: 20; max-width: min(260px, calc(100vw - 24px)); padding: 0; border: 1px solid var(--line); background: var(--paper); box-shadow: var(--soft-shadow); }
    .corrector-globito[hidden] { display: none; }
    .corrector-sugerencia { padding: 8px 12px; border: 0; background: transparent; color: var(--accent); font: inherit; font-size: 12px; cursor: pointer; }
    .corrector-sugerencia:hover { background: var(--hover-bg); }
    .corrector-aviso-config { margin: 7px 0 0; color: var(--muted); font-size: 11px; font-weight: 400; line-height: 1.5; }
    .corrector-opcion { padding: 15px; margin: 18px 0 24px; border: 1px solid var(--line); background: var(--paper); }
    .corrector-opcion label { display: flex; align-items: flex-start; gap: 9px; color: var(--ink); font-size: 12px; font-weight: 600; }
    .corrector-opcion input { margin: 2px 0 0; accent-color: var(--accent); }

    .editor { max-width: 720px; padding-top: 32px; margin: 0 auto; }
    .editor-tools { display: flex; align-items: center; justify-content: space-between; margin-bottom: 34px; }
    .back-button { display: inline-flex; align-items: center; gap: 7px; padding: 6px 0; border: 0; background: transparent; color: var(--muted); font-size: 11px; cursor: pointer; }
    .back-button:hover { color: var(--accent); }
    .back-button svg { width: 14px; height: 14px; }
    .editor-hint { color: var(--muted); font-size: 10px; }
    .page-title { width: 100%; padding: 0; border: 0; outline: none; background: transparent; color: var(--ink); font-size: clamp(30px, 4vw, 38px); font-weight: 650; letter-spacing: -1.5px; }
    .page-title::placeholder { color: var(--page-placeholder); }
    .page-content { min-height: 52vh; padding: 20px 0; outline: none; color: var(--ink); font-size: 15px; line-height: 1.85; }
    .page-content:empty::before { color: var(--page-placeholder); content: "Empieza a escribir lo que tienes en mente…"; pointer-events: none; }
    .page-content p { margin: 0 0 14px; }
    .page-footer { padding-top: 13px; border-top: 1px solid var(--line); color: var(--muted); font-size: 10px; }

    @media (max-width: 1050px) {
      .content { padding-right: 35px; padding-left: 35px; }
      .topbar { padding-right: 35px; padding-left: 35px; }
    }
    @media (max-width: 720px) {
      :root { --sidebar: 0px; }
      .sidebar { z-index: 31; width: min(86vw, 320px); align-items: stretch; padding: 25px 17px 18px; box-shadow: var(--float-shadow); }
      .menu-cerrado .sidebar { visibility: hidden; transform: translateX(-100%); }
      .menu-visible .sidebar { visibility: visible; transform: translateX(0); }
      .brand { padding: 0 9px; margin-bottom: 39px; }
      .brand-copy, .workspace-label, .nav-label, .section-heading, .folder-label, .profile-copy { display: revert; }
      .nav-item { width: 100%; height: auto; justify-content: flex-start; padding: 10px 11px; }
      .nav-item svg { width: 17px; height: 17px; }
      .folder-section { width: auto; margin-top: 29px; }
      .section-heading { display: flex; }
      .folder-list { justify-items: stretch; }
      .folder-nav { width: 100%; height: auto; justify-content: flex-start; padding: 8px 9px; }
      .profile { justify-content: flex-start; padding: 14px 8px 0; }
      .avatar { width: 32px; height: 32px; }
      .breadcrumbs .menu-toggle { width: 32px; height: 32px; }
      .menu-overlay:not([hidden]) { position: fixed; inset: 0; z-index: 30; display: block; border: 0; background: rgb(0 0 0 / 48%); }
      .menu-borde { display: none; }
      .topbar { height: 56px; gap: 8px; padding: 0 17px; }
      .breadcrumbs { gap: 5px; white-space: nowrap; }
      .top-actions { gap: 7px; }
      .search-input { width: min(32vw, 185px); }
      .content { padding: 37px 20px 55px; }
      .vista-contenido, .config-contenido { padding: 30px 20px 48px; }
      .paginas-vista { padding: 30px 20px 28px; margin-top: 39px; }
      .paginas-encabezado { min-height: 74px; align-content: end; }
      .paginas-encabezado .carpetas-encabezado { font-size: clamp(25px, 7vw, 34px); }
      .paginas-volver { top: 0; transform: none; }
      .paginas-accion-opciones { opacity: 1; }
      .pagina-editor { padding: 25px 20px 48px; }
      .pagina-editor-herramientas { align-items: flex-start; flex-direction: column; }
    }
    @media (max-width: 900px) {
      .carpetas-grilla { grid-template-columns: repeat(2, minmax(0, 1fr)); }
    }
    @media (max-width: 899px) {
      .editor-disposicion.panel-abierto { display: block; }
      .editor-disposicion.panel-abierto .editor-bloque { width: 100%; }
      .panel-edicion, .panel-abierto .panel-edicion { position: fixed; z-index: 25; right: 0; bottom: 0; left: var(--sidebar); width: auto; max-height: 60vh; padding: 12px 16px max(16px, env(safe-area-inset-bottom)); border: 1px solid var(--line); border-bottom: 0; background: var(--paper); box-shadow: var(--float-shadow); transform: translateY(105%); opacity: 0; visibility: hidden; transition: transform .25s ease, opacity .2s ease, visibility .25s; }
      .panel-abierto .panel-edicion { transform: translateY(0); opacity: 1; visibility: visible; }
    }
    @media (max-width: 560px) {
      .carpetas-seccion { padding: 29px 17px 25px; margin-top: 39px; }
      .paginas-vista { padding: 29px 17px 25px; }
      .carpetas-grilla { grid-template-columns: 1fr; gap: 17px; }
      .carpetas-filtro-contenedor { margin-bottom: 22px; }
      .carpetas-paginacion { gap: 3px; }
      .vista-formulario-enlace { grid-template-columns: 1fr; }
      .config-tarjeta { grid-template-columns: 1fr; }
      .config-vista-previa { width: 100%; max-height: 220px; aspect-ratio: 1.5; }
      .config-campos { grid-template-columns: 1fr; }
    }
    @media (max-width: 600px) {
      .orden-seccion { padding: 13px; }
      .orden-lista { grid-template-columns: 1fr; }
      .orden-encabezado { align-items: flex-start; flex-direction: column; }
      .orden-acciones { width: 100%; }
      .carpetas-item-con-opciones .paginas-accion-opciones { opacity: 1; }
    }
    @media (max-width: 380px) {
      .breadcrumbs .crumb-current { display: none; }
    }
    @media (max-width: 335px) {
      body { min-width: 0; }
    }
    @media (prefers-reduced-motion: reduce) {
      .sidebar, .main { transition: none; }
    }
    .home-navigation { display: none; min-width: 0; flex: 1; align-items: center; gap: 18px; }
    .home-brand { flex: 0 0 auto; padding: 0; border: 0; background: none; color: var(--accent); font-size: 17px; font-weight: 750; letter-spacing: -.4px; cursor: pointer; }
    .home-tabs { display: flex; min-width: 0; align-items: center; gap: 5px; overflow-x: auto; scrollbar-width: none; }
    .home-tabs::-webkit-scrollbar { display: none; }
    .home-tab-wrap { position: relative; display: flex; flex: 0 0 auto; align-items: center; }
    .home-tab { min-height: 38px; padding: 0 11px; border: 0; border-bottom: 2px solid transparent; background: transparent; color: var(--muted); font-size: 12px; white-space: nowrap; cursor: pointer; }
    .home-tab:hover { color: var(--ink); }
    .home-tab[aria-selected="true"] { border-bottom-color: var(--accent); color: var(--ink); font-weight: 650; }
    .home-section-menu-toggle, .home-add-section { display: inline-grid; width: 26px; height: 28px; flex: 0 0 auto; place-items: center; padding: 0; border: 0; border-radius: 6px; background: transparent; color: var(--muted); font-size: 17px; cursor: pointer; }
    .home-section-menu-toggle:hover, .home-add-section:hover { background: var(--hover-bg); color: var(--ink); }
    .home-add-section { border: 1px dashed var(--line); font-size: 20px; }
    .home-create-popover, .home-section-popover { position: absolute; top: calc(100% + 8px); z-index: 8; width: 240px; padding: 15px; border: 1px solid var(--line); border-radius: 12px; background: var(--paper); box-shadow: var(--float-shadow); color: var(--ink); }
    .home-create-popover { left: 0; }
    .home-section-popover { right: 0; display: grid; gap: 6px; }
    .home-create-popover[hidden], .home-section-popover[hidden] { display: none; }
    .home-create-popover label { display: grid; gap: 6px; margin-bottom: 10px; color: var(--muted); font-size: 11px; }
    .home-create-popover input, .home-create-popover select { width: 100%; min-height: 34px; padding: 0 9px; border: 1px solid var(--line); border-radius: 7px; background: var(--input-bg); color: var(--ink); }
    .home-create-popover button { width: 100%; min-height: 34px; border: 0; border-radius: 7px; background: var(--accent); color: #fff; font-size: 12px; font-weight: 650; cursor: pointer; }
    .home-section-popover button { padding: 8px 9px; border: 0; border-radius: 6px; background: transparent; color: var(--ink); text-align: left; font-size: 12px; cursor: pointer; }
    .home-section-popover button:hover { background: var(--hover-bg); }
    .app.home-mode .sidebar, .app.home-mode .menu-borde { display: none; }
    .app.home-mode .main { min-height: 100vh; margin-left: 0; background: #0e0e0e; }
    .app.home-mode .topbar { height: 64px; gap: 20px; padding: 0 30px; border-bottom-color: var(--line); background: var(--topbar-bg); color: var(--ink); }
    .app.home-mode .home-navigation { display: flex; }
    .app.home-mode .breadcrumbs { display: none; }
    .app.home-mode .search-input { width: clamp(145px, 18vw, 230px); }
    .app.home-mode #view { min-height: calc(100vh - 64px); }
    .home-page { width: 100%; padding: 0 0 70px; background: #0e0e0e; color: #f2f2f2; animation: home-fade .15s ease both; }
    .home-page:not(.home-page-has-hero) { padding-top: 84px; }
    @keyframes home-fade { from { opacity: .55; } to { opacity: 1; } }
    .home-hero { position: relative; display: flex; min-height: 320px; align-items: end; overflow: hidden; padding: 35px clamp(24px, 5vw, 72px); background-color: var(--home-hero-color, #34343b); background-position: center 42%; background-size: cover; color: #fff; }
    .home-hero::before { position: absolute; inset: 0; background: linear-gradient(90deg, rgb(0 0 0 / 80%), rgb(0 0 0 / 24%) 76%), linear-gradient(0deg, rgb(0 0 0 / 66%), transparent 76%); content: ""; }
    .home-hero-content { position: relative; z-index: 1; width: min(680px, 100%); }
    .home-hero-initial { position: absolute; right: 8%; bottom: -.3em; color: rgb(255 255 255 / 12%); font-size: clamp(170px, 27vw, 350px); font-weight: 800; line-height: 1; }
    .home-hero-kicker { margin: 0 0 9px; color: #f1f1f1; font-size: 12px; font-weight: 650; letter-spacing: .3px; }
    .home-hero h1 { overflow-wrap: anywhere; font-size: clamp(27px, 4vw, 38px); font-weight: 600; letter-spacing: -.6px; }
    .home-hero-meta { margin: 9px 0 17px; color: #ededed; font-size: 12px; }
    .home-hero-actions { display: flex; gap: 9px; }
    .home-hero-actions button { min-height: 37px; padding: 0 15px; border: 0; border-radius: 6px; background: #fff; color: #17171a; font-size: 12px; font-weight: 700; cursor: pointer; }
    .home-hero-actions button + button { background: rgb(255 255 255 / 24%); color: #fff; backdrop-filter: blur(8px); }
    .home-rows { display: grid; gap: 27px; padding-top: 22px; }
    .home-row { min-width: 0; padding: 0 clamp(20px, 4vw, 58px); }
    .home-row-heading { display: flex; align-items: center; justify-content: space-between; gap: 12px; margin-bottom: 11px; }
    .home-row-heading h2 { margin: 0; font-size: 15px; font-weight: 600; }
    .home-row-heading button { padding: 4px 0; border: 0; background: none; color: var(--muted); font-size: 11px; cursor: pointer; }
    .home-row-track { display: flex; align-items: start; gap: 13px; overflow-x: auto; overflow-y: visible; padding: 8px 3px 19px; scroll-snap-type: x mandatory; scrollbar-width: none; }
    .home-row-track::-webkit-scrollbar { display: none; }
    .home-card-wrap { position: relative; z-index: 0; min-width: 0; flex: 0 0 220px; scroll-snap-align: start; transition: flex-basis .2s ease, z-index 0s .3s; }
    .home-card { position: relative; display: block; width: 100%; overflow: hidden; padding: 0; border: 0; border-radius: 9px; aspect-ratio: 16 / 9; background: var(--home-card-color, #34343b); color: #fff; text-align: left; cursor: pointer; }
    .home-card img { width: 100%; height: 100%; object-fit: cover; transition: transform .2s ease; }
    .home-card::after { position: absolute; inset: 25% 0 0; background: linear-gradient(transparent, rgb(0 0 0 / 78%)); content: ""; pointer-events: none; }
    .home-card:hover img, .home-card:focus-visible img { transform: scale(1.04); }
    .home-card-title { position: absolute; z-index: 1; right: 12px; bottom: 11px; left: 12px; overflow: hidden; color: #fff; font-size: 14px; font-weight: 550; text-overflow: ellipsis; white-space: nowrap; }
    .home-card-menu-toggle { position: absolute; top: 8px; right: 8px; z-index: 2; display: grid; width: 30px; height: 30px; place-items: center; border: 0; border-radius: 50%; background: rgb(0 0 0 / 65%); color: #fff; font-size: 20px; opacity: 0; cursor: pointer; backdrop-filter: blur(8px); }
    .home-card-wrap:hover .home-card-menu-toggle, .home-card-wrap:focus-within .home-card-menu-toggle, .home-card-menu-toggle[aria-expanded="true"] { opacity: 1; }
    .home-card-popover { position: absolute; top: 42px; right: 7px; z-index: 4; display: grid; min-width: 152px; padding: 5px; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); box-shadow: var(--float-shadow); }
    .home-card-popover[hidden] { display: none; }
    .home-card-popover button { padding: 8px; border: 0; border-radius: 5px; background: transparent; color: var(--ink); text-align: left; font-size: 11px; cursor: pointer; }
    .home-card-popover button:hover { background: var(--hover-bg); }
    .home-card-info { max-height: 0; overflow: hidden; color: var(--muted); font-size: 11px; opacity: 0; transition: max-height .2s ease, opacity .2s ease; }
    .home-card-wrap:hover .home-card-info, .home-card-wrap:focus-within .home-card-info { max-height: 36px; padding: 6px 2px 0; opacity: 1; transition-delay: .3s; }
    .home-row-vertical .home-card-wrap { flex-basis: 150px; }
    .home-row-vertical .home-card { aspect-ratio: 2 / 3; }
    .home-row-empty { padding: 4px 0; color: var(--muted); font-size: 12px; }
    .home-create-card { display: grid; width: 100%; min-height: 124px; place-content: center; gap: 7px; border: 1px dashed var(--line); border-radius: 9px; background: transparent; color: var(--muted); font-size: 12px; cursor: pointer; }
    .home-create-card span { color: var(--accent); font-size: 30px; line-height: 1; }
    .home-empty { display: grid; min-height: 330px; place-content: center; justify-items: center; gap: 12px; color: var(--muted); text-align: center; }
    .home-empty-icon { color: var(--accent); font-size: 34px; }
    .home-empty button { min-height: 36px; padding: 0 14px; border: 0; border-radius: 7px; background: var(--accent); color: #fff; cursor: pointer; }
    .home-details-backdrop { position: fixed; inset: 0; z-index: 50; display: grid; place-items: center; padding: 20px; background: rgb(0 0 0 / 62%); }
    .home-details { width: min(480px, 100%); padding: 22px; border: 1px solid var(--line); border-radius: 13px; background: var(--paper); color: var(--ink); box-shadow: var(--float-shadow); }
    .home-details h2 { margin: 0 0 8px; font-size: 21px; }
    .home-details p { color: var(--muted); font-size: 13px; line-height: 1.5; }
    .home-details button { min-height: 34px; padding: 0 12px; border: 0; border-radius: 6px; background: var(--accent); color: #fff; cursor: pointer; }
    @media (max-width: 850px) {
      .app.home-mode .topbar { gap: 10px; padding: 0 16px; }
      .home-navigation { gap: 10px; }
      .home-brand { font-size: 14px; }
      .home-tab { padding: 0 8px; font-size: 11px; }
    }
    @media (max-width: 600px) {
      .app.home-mode .topbar { height: auto; min-height: 58px; flex-wrap: wrap; gap: 0 8px; padding: 7px 12px; }
      .app.home-mode .home-navigation { width: 100%; min-height: 36px; flex: 1 1 100%; order: 0; }
      .app.home-mode .top-actions { width: 100%; justify-content: flex-end; order: 1; }
      .app.home-mode .search-input { width: min(65vw, 260px); }
      .home-hero { min-height: 220px; padding: 24px 20px; }
      .home-hero h1 { font-size: 25px; }
      .home-card-wrap { flex-basis: 175px; }
      .home-row-vertical .home-card-wrap { flex-basis: 125px; }
      .home-card-menu-toggle { opacity: 1; }
      .home-hover-card { display: none !important; }
    }
    @media (hover: none) {
      .home-card-wrap:hover, .home-card-wrap:focus-within { flex-basis: inherit; transition-delay: 0s; }
      .home-card-info { display: none; }
      .home-card-menu-toggle { opacity: 1; }
    }
    .app.home-mode .topbar { position: fixed; inset: 0 0 auto; height: 68px; gap: 20px; padding: 0 4%; border-color: transparent; background: linear-gradient(180deg, rgb(0 0 0 / 76%), transparent); color: #fff; transition: background .3s ease, border-color .3s ease; }
    .app.home-mode .topbar.home-scrolled { border-bottom-color: rgb(255 255 255 / 7%); background: #0e0e0e; }
    .app.home-mode .home-brand { color: #e50914; font-size: 24px; font-weight: 800; letter-spacing: -.8px; }
    .app.home-mode .home-tab { min-height: 42px; padding: 0 9px; border-bottom: 0; color: rgb(255 255 255 / 72%); font-size: 14px; font-weight: 400; }
    .app.home-mode .home-tab:hover, .app.home-mode .home-tab[aria-selected="true"] { color: #fff; }
    .app.home-mode .home-tab[aria-selected="true"] { font-weight: 500; }
    .app.home-mode .home-section-menu-toggle { opacity: 0; color: #fff; transition: opacity .15s ease; }
    .app.home-mode .home-tab-wrap:hover .home-section-menu-toggle, .app.home-mode .home-tab-wrap:focus-within .home-section-menu-toggle, .app.home-mode .home-section-menu-toggle[aria-expanded="true"] { opacity: 1; }
    .app.home-mode .home-add-section, .app.home-mode .theme-toggle { color: #fff; }
    .app.home-mode .top-actions { position: relative; z-index: 1; }
    .app.home-mode .save-state { color: #eee; }
    .app.home-mode .theme-toggle { border-color: transparent; background: transparent; }
    .app.home-mode .search-input { border-color: rgb(255 255 255 / 35%); background: rgb(0 0 0 / 38%); color: #fff; }
    .app.home-mode .search-wrap svg { color: #fff; }
    .home-hero { width: 100%; height: 65vh; min-height: 420px; padding: 68px 4% 105px; background-color: var(--home-hero-color, #242428); background-position: center 38%; }
    .home-hero::before { background: linear-gradient(90deg, rgb(0 0 0 / 82%), rgb(0 0 0 / 48%) 38%, transparent 76%), linear-gradient(0deg, #0e0e0e 0%, rgb(14 14 14 / 78%) 12%, rgb(14 14 14 / 12%) 52%, rgb(14 14 14 / 18%) 100%); }
    .home-hero-content { width: min(40%, 720px); min-width: 330px; }
    .home-hero-initial { color: rgb(255 255 255 / 11%); font-size: clamp(220px, 34vw, 520px); }
    .home-hero-kicker { margin-bottom: 12px; color: #e5e5e5; font-weight: 500; letter-spacing: 1.5px; text-transform: uppercase; }
    .home-hero h1 { margin: 0 0 24px; font-size: clamp(32px, 4.5vw, 64px); font-weight: 700; line-height: 1.04; text-shadow: 0 2px 12px rgb(0 0 0 / 48%); }
    .home-hero-meta { display: none; }
    .home-hero-description { display: -webkit-box; overflow: hidden; max-width: 100%; margin: 14px 0 17px; color: #e5e5e5; font-size: 16px; line-height: 1.5; text-shadow: 0 1px 5px rgb(0 0 0 / 60%); -webkit-box-orient: vertical; -webkit-line-clamp: 3; }
    .home-hero-actions { flex-wrap: wrap; gap: 10px; }
    .home-hero-actions button { display: inline-flex; height: 44px; min-height: 44px; align-items: center; justify-content: center; gap: 8px; padding: 0 26px; border-radius: 4px; background: #f2f2f2; font-size: 16px; font-weight: 500; line-height: 1; }
    .home-hero-actions button + button { background: rgb(109 109 110 / 78%); color: #fff; }
    .home-hero-actions svg { width: 18px; height: 18px; flex: 0 0 auto; }
    .home-rows { position: relative; z-index: 2; gap: 44px; padding-top: 0; }
    .home-row { position: relative; padding: 0 4%; }
    .home-page-has-hero .home-row:first-child { margin-top: -60px; }
    .home-row-heading { margin-bottom: 10px; }
    .home-row-heading h2 { color: #e5e5e5; font-size: 20px; font-weight: 500; }
    .home-row-heading button { color: #b3b3b3; font-size: 12px; }
    .home-row-heading button:hover { color: #fff; }
    .home-row-track { gap: 6px; padding: 5px 0 22px; scroll-padding-inline: 4vw; }
    .home-row-controls { position: absolute; inset: 27px 0 22px; z-index: 4; display: flex; justify-content: space-between; opacity: 0; pointer-events: none; transition: opacity .2s ease; }
    .home-row:hover .home-row-controls { opacity: 1; }
    .home-row-controls button { position: absolute; top: 0; bottom: 0; display: grid; width: 4vw; max-width: 58px; place-items: center; padding: 0; border: 0; background: linear-gradient(90deg, rgb(14 14 14 / 85%), transparent); color: #fff; font-size: 24px; cursor: pointer; pointer-events: auto; }
    .home-row-controls button:first-child { left: 0; }
    .home-row-controls button:last-child { right: 0; background: linear-gradient(270deg, rgb(14 14 14 / 85%), transparent); }
    .home-row-controls button:hover { filter: brightness(1.2); }
    .home-row-controls button:disabled { opacity: 0; pointer-events: none; }
    .home-card-wrap { flex: 0 0 calc((100% - 30px) / 6.5); }
    .home-card { border-radius: 4px; background: var(--home-card-color, #242428); box-shadow: 0 2px 8px rgb(0 0 0 / 28%); }
    .home-card::after { background: linear-gradient(transparent, rgb(0 0 0 / 82%)); }
    .home-card-initial { position: absolute; inset: 0; display: grid; place-items: center; color: rgb(255 255 255 / 18%); font-size: clamp(36px, 6vw, 100px); font-weight: 750; line-height: 1; }
    .home-card-title { right: 10px; bottom: 9px; left: 10px; font-weight: 500; text-shadow: 0 1px 5px rgb(0 0 0 / 75%); }
    .home-card-wrap:hover, .home-card-wrap:focus-within { z-index: 0; }
    .home-card-wrap:hover .home-card, .home-card-wrap:focus-within .home-card { box-shadow: 0 2px 8px rgb(0 0 0 / 28%); transform: none; }
    .home-card-menu-toggle { z-index: 7; border: 1px solid rgb(255 255 255 / 55%); background: rgb(24 24 24 / 88%); }
    .home-card-popover { z-index: 9; }
    .home-card-info { display: none; }
    .home-card-info-title { overflow: hidden; font-size: 13px; font-weight: 500; text-overflow: ellipsis; white-space: nowrap; }
    .home-card-info-section { overflow: hidden; color: #a3a3a3; font-size: 11px; text-overflow: ellipsis; white-space: nowrap; }
    .home-card-info-actions { display: flex; gap: 6px; }
    .home-card-info-actions button { display: grid; width: 26px; height: 26px; place-items: center; padding: 0; border: 1px solid rgb(255 255 255 / 48%); border-radius: 50%; background: transparent; color: #fff; font-size: 13px; cursor: pointer; }
    .home-card-info-actions button:hover { border-color: #fff; background: #333; }
    .home-hover-card { position: fixed; z-index: 1000; overflow: visible; border-radius: 6px; background: #181818; box-shadow: 0 12px 34px rgb(0 0 0 / 72%); color: #f2f2f2; opacity: 1; transform: scale(1); transform-origin: center top; transition: opacity .2s ease, transform .2s ease; }
    .home-hover-card.is-entering, .home-hover-card.is-closing { opacity: 0; transform: scale(.9); }
    .home-hover-image { position: relative; overflow: visible; border-radius: 6px 6px 0 0; }
    .home-hover-image .home-card { aspect-ratio: 16 / 9 !important; border-radius: 6px 6px 0 0; box-shadow: none; }
    .home-hover-image .home-card-title { display: none; }
    .home-hover-image .paginas-accion-opciones { position: absolute; top: 8px; right: 8px; z-index: 5; }
    .home-hover-image .home-card-menu-toggle { display: none; }
    .home-hover-info { display: grid; max-height: none; gap: 6px; min-height: 74px; overflow: visible; padding: 9px 10px 10px; border-radius: 0 0 6px 6px; background: #181818; color: #e5e5e5; opacity: 1; transform: none; }
    .home-hover-info .home-card-info-actions { align-items: center; }
    .home-hover-info .home-card-info-title { color: #f2f2f2; font-size: 13px; font-weight: 500; }
    .home-hover-info .home-card-info-section { color: #a3a3a3; font-size: 11px; }
    .home-hover-card .home-card-popover { position: absolute; top: auto; right: 8px; bottom: 100%; }
    .home-hover-card .paginas-menu-opciones { border-color: #333; background: #181818; box-shadow: 0 8px 24px rgb(0 0 0 / 50%); }
    .home-hover-card .paginas-menu-opciones button, .home-hover-card .paginas-menu-opciones label { color: #f2f2f2; }
    .home-hover-card .paginas-menu-opciones button:hover, .home-hover-card .paginas-menu-opciones label:hover { background: #333; }
    .home-create-card { min-height: 0; aspect-ratio: 16 / 9; border-color: rgb(255 255 255 / 40%); border-radius: 4px; background: #181818; color: #d0d0d0; }
    .home-create-card:hover { border-color: #fff; color: #fff; }
    .home-create-card span { color: #fff; }
    .home-grid-page { min-height: 100vh; padding: 102px 4% 56px; background: #0e0e0e; color: #f2f2f2; animation: home-fade .15s ease both; }
    .home-grid-heading { display: flex; align-items: end; justify-content: space-between; gap: 16px; margin-bottom: 20px; }
    .home-grid-heading h1 { margin: 0; font-size: 28px; font-weight: 500; letter-spacing: 0; }
    .home-grid-sort { display: flex; align-items: center; gap: 9px; color: #b3b3b3; font-size: 12px; }
    .home-grid-sort select { min-height: 38px; appearance: none; padding: 8px 36px 8px 12px; border: 1px solid #333; border-radius: 6px; background-color: #1c1c1c; background-image: linear-gradient(45deg, transparent 50%, #f2f2f2 50%), linear-gradient(135deg, #f2f2f2 50%, transparent 50%); background-position: calc(100% - 15px) 16px, calc(100% - 10px) 16px; background-repeat: no-repeat; background-size: 5px 5px; color: #f2f2f2; }
    .home-grid { display: grid; grid-template-columns: repeat(5, minmax(0, 1fr)); gap: 6px; }
    .home-grid .home-card-wrap { min-width: 0; }
    .home-grid .home-card { aspect-ratio: 16 / 9; }
    .home-grid .home-card-info { display: none; }
    .home-grid-empty { padding: 32px 0; color: #b3b3b3; font-size: 14px; }
    @media (max-width: 850px) {
      .app.home-mode .topbar { gap: 10px; padding: 0 4%; }
      .home-tab { font-size: 13px; }
      .home-card-wrap { flex-basis: calc((100% - 24px) / 5.5); }
      .home-grid { grid-template-columns: repeat(3, minmax(0, 1fr)); }
    }
    @media (max-width: 600px) {
      .app.home-mode .topbar { height: auto; min-height: 98px; flex-wrap: wrap; gap: 0 8px; padding: 6px 4%; background: linear-gradient(180deg, rgb(0 0 0 / 78%), transparent); }
      .app.home-mode .topbar.home-scrolled { background: #0e0e0e; }
      .app.home-mode .home-navigation { width: 100%; min-height: 36px; flex: 1 1 100%; order: 0; }
      .app.home-mode .top-actions { width: 100%; justify-content: flex-end; order: 1; }
      .home-hero { height: 50vh; min-height: 350px; padding: 110px 4% 72px; }
      .home-page:not(.home-page-has-hero) { padding-top: 122px; }
      .home-hero-content { width: 100%; min-width: 0; }
      .home-hero h1 { font-size: clamp(32px, 8vw, 44px); }
      .home-hero-description { max-width: 82%; font-size: 14px; }
      .home-hero-actions button { height: 44px; min-height: 44px; padding: 0 16px; font-size: 14px; }
      .home-page-has-hero .home-row:first-child { margin-top: -42px; }
      .home-rows { gap: 32px; }
      .home-row-heading h2 { font-size: 17px; }
      .home-card-wrap { flex-basis: calc((100% - 12px) / 3.25); }
      .home-row-controls { display: none; }
      .home-card-menu-toggle { opacity: 1; }
      .home-grid-page { padding-top: 126px; }
      .home-grid-heading { align-items: start; flex-direction: column; }
      .home-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
    }
    @media (min-width: 601px) and (max-width: 1100px) {
      .home-grid { grid-template-columns: repeat(3, minmax(0, 1fr)); }
    }
    @media (hover: none) {
      .home-card-wrap:hover, .home-card-wrap:focus-within { z-index: 0; }
      .home-card-wrap:hover .home-card, .home-card-wrap:focus-within .home-card { box-shadow: 0 2px 8px rgb(0 0 0 / 28%); transform: none; transition-delay: 0s; }
      .home-card-info { display: none; }
      .home-card-menu-toggle { opacity: 1; }
    }
    .app.dashboard-mode .sidebar { display: flex; width: 248px; overflow-y: auto; padding: 25px 15px 18px; border-right-color: #e9e6df; background: #f4f2ed; }
    :root[data-theme="dark"] .app.dashboard-mode .sidebar { border-right-color: #2a2a2f; background: #17171a; }
    @media (min-width: 721px) {
      .app.dashboard-mode .sidebar { visibility: visible; transform: none; }
    }
    .app.dashboard-mode .main { min-height: 100vh; margin-left: var(--sidebar-width); background: var(--canvas); }
    .app.dashboard-mode .topbar { position: sticky; height: 62px; padding: 0 38px; border-bottom-color: var(--line); background: color-mix(in srgb, var(--canvas) 94%, transparent); color: var(--ink); }
    .app.dashboard-mode .topbar.home-scrolled { background: color-mix(in srgb, var(--canvas) 96%, transparent); }
    .app.dashboard-mode .home-navigation { display: none; }
    .app.dashboard-mode .breadcrumbs { display: flex; }
    .app.dashboard-mode #view { min-height: calc(100vh - 62px); }
    .app.dashboard-mode .brand { margin-bottom: 23px; }
    .dashboard-sidebar-search { position: relative; display: block; margin: 0 3px 22px; }
    .dashboard-sidebar-search svg { position: absolute; top: 10px; left: 11px; width: 16px; height: 16px; color: var(--muted); pointer-events: none; }
    .dashboard-sidebar-search input { width: 100%; height: 36px; padding: 0 10px 0 34px; border: 1px solid var(--line); border-radius: 7px; outline: none; background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; }
    .dashboard-sidebar-search input:focus { border-color: var(--accent); box-shadow: 0 0 0 3px color-mix(in srgb, var(--accent) 14%, transparent); }
    .dashboard-quick-links { display: grid; gap: 3px; }
    .dashboard-quick-links .nav-item { min-height: 36px; padding: 8px 10px; margin: 0; color: var(--muted); font-size: 12px; font-weight: 500; }
    .dashboard-quick-links .nav-item.active { background: #e9e7e1; color: var(--ink); font-weight: 650; }
    :root[data-theme="dark"] .dashboard-quick-links .nav-item.active { background: #242429; color: var(--ink); }
    .dashboard-quick-links .nav-item:hover { background: var(--hover-bg); color: var(--ink); }
    .dashboard-quick-links .nav-item svg { width: 16px; height: 16px; }
    .dashboard-create-page { margin-top: 11px !important; border: 1px solid var(--line) !important; background: var(--paper) !important; color: var(--ink) !important; }
    .dashboard-create-page:hover { border-color: color-mix(in srgb, var(--accent) 45%, var(--line)) !important; }
    .dashboard-secondary-links { display: grid; gap: 3px; margin-top: 19px; }
    .dashboard-secondary-links .nav-item { padding: 7px 10px; margin: 0; color: var(--muted); font-size: 11px; }
    .dashboard-secondary-links .nav-item svg { width: 15px; height: 15px; }
    .app.dashboard-mode .folder-section { margin-top: 24px; }
    .app.dashboard-mode .folder-nav { min-height: 34px; padding: 7px 8px; font-size: 12px; }
    .app.dashboard-mode .folder-nav.active { background: #e9e7e1; color: var(--ink); }
    :root[data-theme="dark"] .app.dashboard-mode .folder-nav.active { background: #242429; color: var(--ink); }
    .app.dashboard-mode .folder-nav-wrap { position: relative; display: flex; align-items: center; gap: 3px; }
    .app.dashboard-mode .folder-nav-wrap .folder-nav { min-width: 0; flex: 1; }
    .dashboard-section-menu { display: grid; width: 25px; height: 27px; flex: 0 0 auto; place-items: center; padding: 0; border: 0; border-radius: 5px; background: transparent; color: var(--muted); opacity: 0; cursor: pointer; }
    .folder-nav-wrap:hover .dashboard-section-menu, .folder-nav-wrap:focus-within .dashboard-section-menu, .dashboard-section-menu[aria-expanded="true"] { opacity: 1; }
    .dashboard-section-menu:hover { background: var(--hover-bg); color: var(--ink); }
    .folder-nav-wrap .home-section-popover { top: calc(100% + 2px); right: 0; z-index: 10; }
    .dashboard-shortcuts { display: grid; gap: 3px; margin-top: 19px; }
    .dashboard-shortcuts .nav-item { min-height: 33px; padding: 7px 10px; margin: 0; color: var(--muted); font-size: 11px; }
    .dashboard-shortcuts .nav-item svg { width: 15px; height: 15px; }
    .app:not(.dashboard-mode) .dashboard-sidebar-search,
    .app:not(.dashboard-mode) #materias-link,
    .app:not(.dashboard-mode) [data-dashboard-target],
    .app:not(.dashboard-mode) .dashboard-shortcuts { display: none; }
    .dashboard-page { width: min(1120px, 100%); padding: 28px clamp(22px, 4vw, 54px) 56px; margin: 0 auto; color: var(--ink); animation: home-fade .18s ease both; }
    .dashboard-topline { display: flex; align-items: center; justify-content: space-between; gap: 14px; padding-bottom: 16px; margin-bottom: 29px; border-bottom: 1px solid var(--line); }
    .dashboard-breadcrumb { display: flex; align-items: center; gap: 8px; color: var(--muted); font-size: 11px; }
    .dashboard-breadcrumb strong { color: var(--ink); font-weight: 550; }
    .dashboard-actions { position: relative; display: flex; align-items: center; gap: 5px; }
    .dashboard-actions button { display: inline-flex; min-height: 31px; align-items: center; gap: 6px; padding: 0 9px; border: 1px solid transparent; border-radius: 6px; background: transparent; color: var(--muted); font: inherit; font-size: 11px; cursor: pointer; transition: background .15s ease, color .15s ease, border-color .15s ease; }
    .dashboard-actions button:hover { border-color: var(--line); background: var(--paper); color: var(--ink); }
    .dashboard-actions button svg { width: 15px; height: 15px; }
    .dashboard-more-menu { position: absolute; top: calc(100% + 5px); right: 0; z-index: 12; display: grid; width: 180px; gap: 3px; padding: 5px; border: 1px solid var(--line); border-radius: 8px; background: var(--paper); box-shadow: var(--float-shadow); }
    .dashboard-more-menu[hidden] { display: none; }
    .dashboard-more-menu button { justify-content: flex-start; width: 100%; border: 0; }
    .dashboard-greeting { display: flex; align-items: end; justify-content: space-between; gap: 18px; margin-bottom: 29px; }
    .dashboard-eyebrow { margin: 0 0 8px; color: var(--muted); font-size: 10px; font-weight: 650; letter-spacing: 1.2px; text-transform: uppercase; }
    .dashboard-greeting h1 { margin: 0; font-family: Georgia, "Times New Roman", serif; font-size: clamp(32px, 4vw, 43px); font-weight: 400; letter-spacing: -.8px; line-height: 1.08; }
    .dashboard-date { color: var(--muted); font-size: 12px; white-space: nowrap; }
    .dashboard-search { position: relative; max-width: 520px; margin: 0 0 36px; }
    .dashboard-search svg { position: absolute; top: 13px; left: 13px; width: 17px; height: 17px; color: var(--muted); pointer-events: none; }
    .dashboard-search input { width: 100%; height: 43px; padding: 0 14px 0 40px; border: 1px solid var(--line); border-radius: 8px; outline: none; background: var(--paper); color: var(--ink); font: inherit; font-size: 12px; box-shadow: 0 2px 9px rgb(25 28 35 / 3%); }
    .dashboard-search input:focus { border-color: color-mix(in srgb, var(--accent) 55%, var(--line)); box-shadow: 0 0 0 3px color-mix(in srgb, var(--accent) 12%, transparent); }
    .dashboard-section { margin-top: 31px; }
    .dashboard-section-heading { display: flex; align-items: end; justify-content: space-between; gap: 12px; margin-bottom: 13px; }
    .dashboard-section-heading h2 { margin: 0; font-family: Georgia, "Times New Roman", serif; font-size: 21px; font-weight: 400; letter-spacing: -.2px; }
    .dashboard-section-heading p { margin: 4px 0 0; color: var(--muted); font-size: 11px; }
    .dashboard-text-button { display: inline-flex; align-items: center; gap: 6px; padding: 7px 0; border: 0; background: transparent; color: var(--accent); font: inherit; font-size: 11px; font-weight: 600; cursor: pointer; }
    .dashboard-subject-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 12px; }
    .dashboard-subject-card { display: grid; min-width: 0; grid-template-columns: 38px minmax(0, 1fr); gap: 12px; padding: 15px; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); color: var(--ink); text-align: left; cursor: pointer; box-shadow: 0 2px 8px rgb(25 28 35 / 3%); transition: border-color .16s ease, box-shadow .16s ease, transform .16s ease; }
    .dashboard-subject-card[hidden], .dashboard-filter-empty[hidden] { display: none; }
    .dashboard-subject-card:hover { transform: translateY(-2px); border-color: color-mix(in srgb, var(--subject-accent) 35%, var(--line)); box-shadow: 0 7px 17px rgb(25 28 35 / 7%); }
    .dashboard-subject-icon { display: grid; width: 38px; height: 38px; place-items: center; border-radius: 8px; background: var(--subject-tint); color: var(--subject-accent); }
    .dashboard-subject-icon svg { width: 20px; height: 20px; }
    .dashboard-subject-copy { min-width: 0; }
    .dashboard-subject-title { display: block; overflow: hidden; margin: 1px 0 5px; font-size: 13px; font-weight: 650; text-overflow: ellipsis; white-space: nowrap; }
    .dashboard-subject-meta { display: block; color: var(--muted); font-size: 10px; }
    .dashboard-progress-meta { display: flex; justify-content: space-between; gap: 8px; margin-top: 14px; color: var(--muted); font-size: 9px; }
    .dashboard-progress-track { height: 4px; overflow: hidden; margin-top: 6px; border-radius: 9px; background: var(--section-bg); }
    .dashboard-progress-track span { display: block; height: 100%; border-radius: inherit; background: var(--subject-accent); }
    .dashboard-add-subject { display: grid; min-height: 95px; place-items: center; gap: 6px; padding: 13px; border: 1px dashed var(--line); border-radius: 9px; background: transparent; color: var(--muted); font: inherit; font-size: 11px; cursor: pointer; transition: border-color .15s ease, color .15s ease, background .15s ease; }
    .dashboard-add-subject:hover { border-color: var(--accent); background: var(--paper); color: var(--accent); }
    .dashboard-add-subject svg { width: 18px; height: 18px; }
    .dashboard-lower-grid { display: grid; grid-template-columns: minmax(0, 1.3fr) minmax(260px, .7fr); gap: 14px; margin-top: 35px; }
    .dashboard-panel { min-width: 0; padding: 17px; border: 1px solid var(--line); border-radius: 9px; background: var(--paper); box-shadow: 0 2px 8px rgb(25 28 35 / 3%); }
    .dashboard-panel-heading { display: flex; align-items: center; justify-content: space-between; gap: 10px; margin-bottom: 14px; }
    .dashboard-panel-heading h2 { margin: 0; font-family: Georgia, "Times New Roman", serif; font-size: 18px; font-weight: 400; }
    .dashboard-panel-heading span { color: var(--muted); font-size: 10px; }
    .dashboard-task-list { display: grid; gap: 2px; }
    .dashboard-task { display: grid; grid-template-columns: 18px minmax(0, 1fr) auto; align-items: start; gap: 9px; padding: 9px 2px; border-bottom: 1px solid color-mix(in srgb, var(--line) 72%, transparent); }
    .dashboard-task:last-child { border-bottom: 0; }
    .dashboard-task input { width: 15px; height: 15px; margin: 1px 0 0; accent-color: var(--accent); cursor: pointer; }
    .dashboard-task-title { color: var(--ink); font-size: 11px; line-height: 1.4; }
    .dashboard-task-title.is-complete { color: var(--muted); text-decoration: line-through; }
    .dashboard-task-source { overflow: hidden; max-width: 105px; color: var(--muted); font-size: 9px; text-overflow: ellipsis; white-space: nowrap; }
    .dashboard-empty-note { padding: 10px 0; color: var(--muted); font-size: 11px; line-height: 1.5; }
    .dashboard-week-total { display: flex; align-items: baseline; gap: 5px; margin: 2px 0 3px; }
    .dashboard-week-total strong { font-family: Georgia, "Times New Roman", serif; font-size: 30px; font-weight: 400; }
    .dashboard-week-total span { color: var(--muted); font-size: 10px; }
    .dashboard-chart { display: grid; height: 96px; grid-template-columns: repeat(7, minmax(0, 1fr)); align-items: end; gap: 8px; padding: 8px 0 0; border-bottom: 1px solid var(--line); }
    .dashboard-chart-day { display: grid; height: 100%; grid-template-rows: 1fr auto; align-items: end; justify-items: center; gap: 6px; color: var(--muted); font-size: 9px; }
    .dashboard-chart-bar-wrap { display: flex; width: 100%; height: 100%; align-items: end; justify-content: center; }
    .dashboard-chart-bar { width: min(20px, 70%); min-height: 2px; border-radius: 4px 4px 0 0; background: color-mix(in srgb, var(--accent) 28%, var(--section-bg)); }
    .dashboard-chart-caption { margin-top: 9px; color: var(--muted); font-size: 9px; line-height: 1.45; }
    .dashboard-favorites { margin-top: 31px; scroll-margin-top: 24px; }
    .dashboard-favorites .home-row { padding: 0; }
    .dashboard-favorites .home-row-heading { margin-bottom: 8px; }
    .dashboard-favorites .home-row-heading h2 { color: var(--ink); font-family: Georgia, "Times New Roman", serif; font-size: 21px; font-weight: 400; }
    .dashboard-favorites .home-row-track { gap: 10px; padding: 4px 0 12px; }
    .dashboard-favorites .home-card-wrap { flex-basis: 190px; }
    .dashboard-favorites .home-card { border: 1px solid var(--line); border-radius: 7px; }
    .dashboard-favorites .home-card-menu-toggle { opacity: 1; }
    .dashboard-favorites .home-row-controls { display: none; }
    .dashboard-filter-empty { grid-column: 1 / -1; padding: 18px; border: 1px dashed var(--line); border-radius: 8px; color: var(--muted); font-size: 11px; text-align: center; }
    .app.dashboard-mode .home-details-backdrop { background: rgb(20 22 26 / 42%); }
    .app.dashboard-mode .home-details { border-color: var(--line); background: var(--paper); color: var(--ink); }
    .app.dashboard-mode .home-details p { color: var(--muted); }
    @media (max-width: 1050px) {
      .dashboard-subject-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
      .dashboard-page { padding-right: 28px; padding-left: 28px; }
    }
    @media (max-width: 720px) {
      .app.dashboard-mode .sidebar { display: flex; }
      .app.dashboard-mode .main { margin-left: 0; }
      .app.dashboard-mode .topbar { position: sticky; height: 56px; gap: 8px; padding: 0 16px; }
      .app.dashboard-mode .breadcrumbs { flex: 1; gap: 5px; }
      .app.dashboard-mode .breadcrumbs .crumb-current { display: none; }
      .app.dashboard-mode .top-actions { gap: 6px; }
      .app.dashboard-mode .search-input { width: min(35vw, 175px); }
      .dashboard-page { padding: 22px 19px 42px; }
      .dashboard-topline { margin-bottom: 24px; }
      .dashboard-greeting { align-items: start; flex-direction: column; gap: 8px; margin-bottom: 23px; }
      .dashboard-greeting h1 { font-size: 34px; }
      .dashboard-search { margin-bottom: 28px; }
      .dashboard-lower-grid { grid-template-columns: 1fr; }
    }
    @media (max-width: 520px) {
      .app.dashboard-mode .topbar { flex-wrap: nowrap; }
      .app.dashboard-mode .top-actions { width: auto; flex: 0 0 auto; order: 0; }
      .app.dashboard-mode #search-wrap { display: none; }
      .dashboard-page { padding-right: 15px; padding-left: 15px; }
      .dashboard-topline { align-items: start; }
      .dashboard-actions button { padding: 0 6px; font-size: 10px; }
      .dashboard-subject-grid { grid-template-columns: 1fr; gap: 9px; }
      .dashboard-subject-card { padding: 13px; }
      .dashboard-panel { padding: 14px; }
      .dashboard-task-source { max-width: 78px; }
    }
    @media (hover: none) {
      .app.dashboard-mode .dashboard-section-menu { opacity: 1; }
    }
    body.tree-app-active { min-width: 320px; background: #191919; color: #e6e6e6; }
    body.tree-app-active > .app,
    body.tree-app-active > .menu-borde,
    body.tree-app-active > .editor-dialog { display: none !important; }
    #tree-app-root { --tree-bg: #191919; --tree-sidebar: #202020; --tree-panel: #222; --tree-panel-hover: #2a2a2a; --tree-border: #303030; --tree-ink: #e6e6e6; --tree-muted: #9b9b9b; --tree-accent: #8a9cff; --tree-danger: #e58f8f; position: fixed; inset: 0; z-index: 100; display: grid; grid-template-columns: 272px minmax(0, 1fr); overflow: hidden; background: var(--tree-bg); color: var(--tree-ink); font: 14px/1.5 Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", sans-serif; }
    #tree-app-root *, #tree-app-root *::before, #tree-app-root *::after { box-sizing: border-box; }
    #tree-app-root button, #tree-app-root input, #tree-app-root textarea, #tree-app-root select { color: inherit; font: inherit; }
    #tree-app-root button { cursor: pointer; }
    #tree-app-root button:focus-visible, #tree-app-root input:focus-visible, #tree-app-root textarea:focus-visible, #tree-app-root [contenteditable="true"]:focus-visible { outline: 2px solid var(--tree-accent); outline-offset: 2px; }
    .tree-sidebar { display: flex; min-width: 0; min-height: 0; flex-direction: column; overflow: hidden; padding: 15px 10px 13px; border-right: 1px solid #2b2b2b; background: var(--tree-sidebar); }
    .tree-sidebar-scroll { min-height: 0; flex: 1 1 auto; overflow-x: hidden; overflow-y: auto; scrollbar-color: #555 transparent; scrollbar-width: thin; }
    .tree-sidebar-scroll::-webkit-scrollbar { width: 5px; }
    .tree-sidebar-scroll::-webkit-scrollbar-track { background: transparent; }
    .tree-sidebar-scroll::-webkit-scrollbar-thumb { border-radius: 5px; background: #555; }
    .tree-sidebar-scroll::-webkit-scrollbar-thumb:hover { background: #777; }
    .tree-sidebar-brand { display: flex; align-items: center; gap: 10px; padding: 4px 8px 15px; }
    .tree-brand-mark { display: grid; width: 29px; height: 29px; place-items: center; border: 1px solid #414141; border-radius: 7px; background: #292929; color: #eee; font-family: Georgia, serif; font-size: 17px; }
    .tree-brand-name { overflow: hidden; font-size: 13px; font-weight: 650; text-overflow: ellipsis; white-space: nowrap; }
    .tree-sidebar-controls { display: flex; align-items: center; gap: 4px; margin: 0 4px 14px; }
    .tree-search-wrap { position: relative; min-width: 0; flex: 1; }
    .tree-search-wrap svg { position: absolute; top: 9px; left: 9px; width: 15px; height: 15px; color: #8d8d8d; pointer-events: none; }
    .tree-search { width: 100%; height: 33px; padding: 0 9px 0 31px; border: 1px solid #363636; border-radius: 6px; outline: 0; background: #272727; font-size: 11px !important; }
    .tree-search::placeholder { color: #858585; }
    .tree-icon-button { display: inline-grid; width: 30px; height: 30px; flex: 0 0 auto; place-items: center; padding: 0; border: 0; border-radius: 5px; background: transparent; color: #a5a5a5; }
    .tree-icon-button:hover { background: #303030; color: #f0f0f0; }
    .tree-icon-button svg, .tree-row-action svg, .tree-content-action svg { width: 16px; height: 16px; }
    .tree-side-section { margin: 0 0 12px; }
    .tree-side-heading { display: flex; align-items: center; justify-content: space-between; padding: 0 8px 5px; color: #858585; font-size: 10px; font-weight: 600; letter-spacing: .04em; text-transform: uppercase; }
    .tree-side-heading button { border: 0; background: transparent; color: #858585; font-size: 14px; }
    .tree-tree { display: grid; gap: 1px; }
    .tree-node-row { position: relative; display: flex; min-width: 0; align-items: center; gap: 2px; min-height: 29px; padding-right: 2px; border-radius: 5px; }
    .tree-node-row:hover, .tree-node-row.is-selected { background: #2c2c2c; }
    .tree-node-row.is-drop-target { background: #323948; box-shadow: inset 0 0 0 1px var(--tree-accent); }
    .tree-disclosure { display: inline-grid; width: 21px; height: 25px; flex: 0 0 auto; place-items: center; padding: 0; border: 0; background: transparent; color: #8c8c8c; }
    .tree-disclosure svg { width: 12px; height: 12px; transition: transform .16s ease; }
    .tree-disclosure[aria-expanded="true"] svg { transform: rotate(90deg); }
    .tree-node-main { display: flex; min-width: 0; flex: 1; align-items: center; gap: 7px; overflow: hidden; padding: 4px 1px; border: 0; background: transparent; color: #c8c8c8; text-align: left; font-size: 12px !important; }
    .tree-node-icon { width: 16px; flex: 0 0 16px; color: #a9a9a9; text-align: center; font-size: 12px; }
    .tree-node-name { overflow: hidden; flex: 1; text-overflow: ellipsis; white-space: nowrap; }
    .tree-children { animation: tree-expand .14s ease both; transform-origin: top; }
    @keyframes tree-expand { from { opacity: 0; transform: translateY(-3px); } to { opacity: 1; transform: translateY(0); } }
    .tree-row-action { display: none; width: 24px; height: 24px; flex: 0 0 auto; place-items: center; padding: 0; border: 0; border-radius: 4px; background: transparent; color: #999; }
    #tree-app-root .tree-node-row:has(> [data-tree-menu-toggle]) { padding-right: 4px; }
    #tree-app-root .tree-node-row:has(> [data-tree-menu-toggle]) > .tree-node-main { padding-right: 36px; }
    #tree-app-root .tree-node-row:has(> [data-tree-add]):has(> [data-tree-menu-toggle]) > .tree-node-main { padding-right: 66px; }
    #tree-app-root .tree-node-row > [data-tree-menu-toggle] {
      position: absolute; top: 50%; right: 6px; z-index: 3; display: inline-grid !important; width: 26px; min-width: 26px; height: 26px;
      flex: 0 0 26px; place-items: center; margin: 0; padding: 0; transform: translateY(-50%); visibility: visible !important; opacity: .7 !important;
      border: 0; border-radius: 5px; background: transparent; color: var(--tree-muted, #9b9b9b); font-weight: 400;
    }
    #tree-app-root .tree-node-row > [data-tree-menu-toggle] svg, #tree-app-root .tree-child-menu-toggle svg { width: 15px; height: 15px; }
    #tree-app-root .tree-node-row > [data-tree-add] { position: absolute; top: 50%; right: 34px; z-index: 3; transform: translateY(-50%); }
    #tree-app-root .tree-node-row:has(> [data-tree-favorite]):has(> [data-tree-delete]) > .tree-node-main { padding-right: 94px; }
    #tree-app-root .tree-node-row > [data-tree-favorite] { position: absolute; top: 50%; right: 62px; z-index: 3; transform: translateY(-50%); }
    #tree-app-root .tree-node-row > [data-tree-delete] { position: absolute; top: 50%; right: 34px; z-index: 3; transform: translateY(-50%); }
    #tree-app-root .tree-node-row > [data-tree-menu-toggle]:hover, #tree-app-root .tree-node-row > [data-tree-menu-toggle][aria-expanded="true"] { background: var(--tree-hover, rgba(128,128,128,.28)); color: var(--tree-ink, #fff); opacity: 1 !important; }
    .tree-node-row:hover .tree-row-action, .tree-node-row:focus-within .tree-row-action { display: inline-grid; }
    .tree-row-action:hover { background: #3a3a3a; color: #f5f5f5; }
    .tree-inline-menu { position: fixed; top: calc(100% + 2px); right: 2px; z-index: 10; display: grid; width: 174px; gap: 2px; padding: 5px; border: 1px solid #3b3b3b; border-radius: 7px; background: #252525; box-shadow: 0 10px 28px #0008; }
    .tree-inline-menu[hidden] { display: none; }
    .tree-inline-menu button { padding: 6px 8px; border: 0; border-radius: 4px; background: transparent; color: #d0d0d0; text-align: left; font-size: 11px !important; }
    .tree-inline-menu button:hover { background: #343434; }
    .tree-inline-menu button[data-tree-delete] { color: var(--tree-danger); }
    .tree-sidebar-spacer { flex: 1 0 18px; }
    .tree-private-hint { padding: 4px 8px 7px; color: #777; font-size: 10px; }
    .tree-sidebar-footer { display: flex; align-items: center; justify-content: space-between; gap: 8px; padding: 8px 6px 0; border-top: 1px solid #303030; color: #858585; font-size: 10px; }
    .tree-save-status { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
    .tree-main { min-width: 0; min-height: 0; overflow: auto; background: var(--tree-bg); }
    .tree-mobile-top { display: none; }
    .tree-view { width: min(100%, 900px); min-height: 100%; padding: 32px 54px 72px; margin: 0 auto; }
    .tree-breadcrumb { display: flex; min-height: 27px; flex-wrap: wrap; align-items: center; gap: 6px; margin: 0 0 34px; color: #818181; font-size: 11px; }
    .tree-breadcrumb button { max-width: 220px; overflow: hidden; padding: 2px 0; border: 0; background: transparent; color: #a0a0a0; text-overflow: ellipsis; white-space: nowrap; }
    .tree-breadcrumb button:hover { color: var(--tree-ink); }
    .tree-breadcrumb strong { max-width: 270px; overflow: hidden; color: #dedede; font-weight: 500; text-overflow: ellipsis; white-space: nowrap; }
    .tree-heading { display: flex; align-items: center; gap: 15px; margin: 0 0 20px; }
    .tree-heading-icon { width: 48px; flex: 0 0 48px; font-size: 39px; line-height: 1; text-align: center; }
    .tree-title-input { min-width: 0; flex: 1; padding: 0; border: 0; outline: 0; background: transparent; color: #ededed; font-family: Georgia, "Times New Roman", serif !important; font-size: clamp(28px, 4vw, 38px) !important; font-weight: 400 !important; letter-spacing: -.035em; }
    .tree-title-input:focus { border-bottom: 1px solid #494949; }
    .tree-view-subtitle { margin: -8px 0 24px 63px; color: #858585; font-size: 11px; }
    .tree-view-actions { display: flex; flex-wrap: wrap; gap: 6px; margin: 0 0 20px 63px; }
    .tree-content-action { display: inline-flex; min-height: 29px; align-items: center; gap: 6px; padding: 0 8px; border: 1px solid #333; border-radius: 5px; background: #222; color: #bdbdbd; font-size: 11px !important; }
    .tree-content-action:hover { background: #2b2b2b; color: #eee; }
    .tree-content-action svg { width: 14px; height: 14px; }
    .tree-child-list { display: grid; gap: 1px; margin: 15px 0 0 63px; border-top: 1px solid #292929; }
    .tree-child-item { position: relative; display: flex; min-width: 0; align-items: center; border-bottom: 1px solid #292929; }
    .tree-child-link { display: flex; min-height: 39px; align-items: center; gap: 9px; padding: 7px 7px; border: 0; border-bottom: 1px solid #292929; background: transparent; color: #c9c9c9; text-align: left; font-size: 12px !important; }
    .tree-child-item .tree-child-link { min-width: 0; flex: 1; border-bottom: 0; }
    .tree-child-menu-toggle { display: inline-grid; margin: 0 4px; }
    .tree-child-link:hover { background: #222; color: #fff; }
    .tree-child-link span:first-child { width: 19px; color: #999; text-align: center; }
    .tree-child-link small { margin-left: auto; color: #737373; font-size: 10px; }
    .tree-empty { padding: 13px 0; color: #777; font-size: 12px; }
    .tree-editor { width: min(100% - 63px, 700px); margin: 0 0 0 63px; color: #d4d4d4; font-size: 14px; line-height: 1.75; }
    .tree-block { position: relative; display: flex; min-height: 30px; align-items: flex-start; gap: 9px; margin: 1px 0; }
    .tree-block-content { min-width: 0; flex: 1; padding: 3px 0; border: 0; outline: 0; color: #d0d0d0; white-space: pre-wrap; overflow-wrap: anywhere; }
    .tree-block-content:empty::before { color: #666; content: attr(data-placeholder); pointer-events: none; }
    .tree-block-content[data-type="h1"] { padding-top: 12px; color: #ededed; font-family: Georgia, "Times New Roman", serif; font-size: 28px; line-height: 1.3; }
    .tree-block-content[data-type="h2"] { padding-top: 10px; color: #e9e9e9; font-family: Georgia, "Times New Roman", serif; font-size: 22px; line-height: 1.35; }
    .tree-block-content[data-type="h3"] { padding-top: 8px; color: #e6e6e6; font-size: 17px; font-weight: 600; }
    .tree-block-content[data-type="quote"] { padding: 4px 0 4px 12px; border-left: 2px solid #5c5c5c; color: #bcbcbc; }
    .tree-block-content[data-type="code"] { padding: 10px 12px; border: 1px solid #303030; border-radius: 5px; background: #202020; font: 12px/1.6 ui-monospace, SFMono-Regular, Consolas, monospace; }
    .tree-block-marker { width: 18px; flex: 0 0 18px; margin-top: 5px; color: #777; text-align: center; }
    .tree-block-marker input { width: 14px; height: 14px; margin: 2px 0 0; accent-color: var(--tree-accent); }
    .tree-block[data-type="divider"] { min-height: 18px; align-items: center; }
    .tree-block[data-type="divider"] hr { width: 100%; border: 0; border-top: 1px solid #373737; }
    .tree-toggle-body { width: 100%; margin: 3px 0 5px 27px; padding: 4px 8px; border-left: 1px solid #3a3a3a; color: #a8a8a8; }
    .tree-block-tools { position: absolute; top: 0; left: -29px; display: none; }
    .tree-block:hover .tree-block-tools, .tree-block:focus-within .tree-block-tools { display: block; }
    .tree-block-tools button { width: 22px; height: 25px; padding: 0; border: 0; border-radius: 4px; background: transparent; color: #808080; }
    .tree-block-tools button:hover { background: #292929; color: #ddd; }
    .tree-slash-menu { position: fixed; z-index: 130; width: 260px; max-height: min(360px, calc(100vh - 20px)); overflow: auto; padding: 5px; border: 1px solid #3a3a3a; border-radius: 8px; background: #242424; box-shadow: 0 12px 35px #0009; }
    .tree-slash-menu[hidden] { display: none; }
    .tree-slash-menu button { display: flex; width: 100%; min-height: 34px; align-items: center; gap: 10px; padding: 5px 8px; border: 0; border-radius: 5px; background: transparent; color: #d8d8d8; text-align: left; font-size: 12px !important; }
    .tree-slash-menu button:hover, .tree-slash-menu button[aria-selected="true"] { background: #343434; }
    .tree-slash-menu button span:first-child { width: 19px; color: #aaa; text-align: center; }
    .tree-slash-menu button small { margin-left: auto; color: #777; font-size: 10px; }
    .tree-search-results { display: grid; gap: 4px; margin: 0 0 14px; }
    .tree-search-result { width: 100%; padding: 8px 10px; border: 1px solid #303030; border-radius: 6px; background: #222; color: #ddd; text-align: left; }
    .tree-search-result:hover { background: #2b2b2b; }
    .tree-search-result strong { display: block; font-size: 12px; font-weight: 550; }
    .tree-search-result small { color: #858585; font-size: 10px; }
    .tree-search-empty { padding: 12px; color: #858585; font-size: 11px; }
    .tree-mobile-overlay { display: none; }
    @media (max-width: 760px) {
      #tree-app-root { display: block; }
      .tree-sidebar { position: fixed; inset: 0 auto 0 0; z-index: 150; width: min(86vw, 310px); transform: translateX(-102%); transition: transform .2s ease; box-shadow: 12px 0 35px #0007; }
      #tree-app-root.is-sidebar-open .tree-sidebar { transform: translateX(0); }
      .tree-sidebar.is-open { transform: translateX(0); }
      .tree-mobile-overlay { position: fixed; inset: 0; z-index: 140; display: block; border: 0; background: #0008; opacity: 0; pointer-events: none; transition: opacity .2s ease; }
      #tree-app-root.is-sidebar-open .tree-mobile-overlay { opacity: 1; pointer-events: auto; }
      .tree-mobile-overlay.is-open { opacity: 1; pointer-events: auto; }
      .tree-main { width: 100%; height: 100%; }
      .tree-mobile-top { position: sticky; top: 0; z-index: 20; display: flex; height: 48px; align-items: center; gap: 8px; padding: 0 14px; border-bottom: 1px solid #292929; background: #191919ed; backdrop-filter: blur(10px); }
      .tree-mobile-top strong { overflow: hidden; flex: 1; color: #bbb; font-size: 11px; font-weight: 500; text-overflow: ellipsis; white-space: nowrap; }
      .tree-view { padding: 23px 20px 50px; }
      .tree-breadcrumb { margin-bottom: 26px; }
      .tree-heading { gap: 10px; }
      .tree-heading-icon { width: 39px; flex-basis: 39px; font-size: 32px; }
      .tree-title-input { font-size: 30px !important; }
      .tree-view-subtitle, .tree-view-actions, .tree-child-list { margin-left: 49px; }
      .tree-editor { width: calc(100% - 49px); margin-left: 49px; }
    }
    @media (prefers-reduced-motion: reduce) {
      #tree-app-root :not(.tree-fx-layer, .tree-fx-layer *, .tree-block-content, .tree-title-input),
      #tree-app-root :not(.tree-fx-layer, .tree-fx-layer *, .tree-block-content, .tree-title-input)::before,
      #tree-app-root :not(.tree-fx-layer, .tree-fx-layer *, .tree-block-content, .tree-title-input)::after { scroll-behavior: auto !important; transition-duration: .01ms !important; animation-duration: .01ms !important; }
    }

    /* ===== Ajustes estilo Notion para el editor de páginas ===== */
    #tree-app-root { --tree-text: #ebebeb; --tree-text-2: #9b9b9b; --tree-hover: rgba(255,255,255,.055); }

    /* Contenedor único de 700px: ícono, título, bloques y subpáginas comparten margen izquierdo */
    #tree-app-root .tree-view { width: min(100%, 800px); padding: 28px 50px 120px; }
    #tree-app-root .tree-breadcrumb { margin: 0 0 28px; }
    #tree-app-root .tree-view-subtitle,
    #tree-app-root .tree-view-actions,
    #tree-app-root .tree-child-list { margin-left: 0; }
    #tree-app-root .tree-editor { width: 100%; margin: 0; font-size: 16px; line-height: 1.6; color: var(--tree-text); }

    /* Ícono grande arriba del título */
    #tree-app-root .tree-heading { flex-direction: column; align-items: flex-start; gap: 4px; margin: 0 0 10px; }
    #tree-app-root .tree-heading-icon { width: 78px; height: 78px; flex: none; margin-left: -6px; font-size: 62px; line-height: 1; border-radius: 8px; }
    #tree-app-root .tree-heading-icon:hover { background: var(--tree-hover); }

    /* Título: misma tipografía que el cuerpo, en negrita, sin recuadro */
    #tree-app-root .tree-title-input {
      width: 100%; flex: none; font-family: inherit !important; font-size: clamp(30px, 6vw, 40px) !important;
      font-weight: 700 !important; letter-spacing: -.02em; line-height: 1.2; color: var(--tree-text);
    }
    #tree-app-root .tree-title-input::placeholder { color: #5a5a5a; }
    #tree-app-root .tree-title-input:focus,
    #tree-app-root .tree-title-input:focus-visible { border: 0; outline: none; box-shadow: none; }

    /* Sin bordes azules de foco: solo se ve el cursor */
    #tree-app-root .tree-block-content:focus,
    #tree-app-root .tree-block-content:focus-visible,
    #tree-app-root .tree-toggle-body:focus,
    #tree-app-root .tree-toggle-body:focus-visible { outline: none; box-shadow: none; }

    /* Bloques con espaciado uniforme (el bloque activo no cambia de tamaño) */
    #tree-app-root .tree-block { min-height: 0; gap: 4px; margin: 0; }
    #tree-app-root .tree-block-content { padding: 3px 2px; color: var(--tree-text); line-height: 1.6; }
    #tree-app-root .tree-block-content:empty::before { color: #5f5f5f; }
    #tree-app-root .tree-block-marker { width: 24px; flex: 0 0 24px; margin-top: 3px; line-height: 1.6; color: var(--tree-text-2); }
    #tree-app-root .tree-block-marker[aria-hidden="true"]:empty { display: none; }
    #tree-app-root .tree-block-content[data-type="h1"] { padding-top: 22px; font-family: inherit; font-size: 30px; font-weight: 700; line-height: 1.25; }
    #tree-app-root .tree-block-content[data-type="h2"] { padding-top: 16px; font-family: inherit; font-size: 24px; font-weight: 650; line-height: 1.3; }
    #tree-app-root .tree-block-content[data-type="h3"] { padding-top: 12px; font-size: 19px; font-weight: 600; line-height: 1.35; }
    #tree-app-root .tree-toggle-body { margin: 2px 0 4px 4px; padding: 2px 10px; min-height: 0; }

    /* Controles del bloque (+) solo al pasar el mouse, alineados en el margen izquierdo */
    #tree-app-root .tree-block-tools { display: block; top: 2px; left: -30px; opacity: 0; transition: opacity .12s ease; }
    #tree-app-root .tree-block:hover .tree-block-tools { opacity: 1; }
    #tree-app-root .tree-block:focus-within .tree-block-tools { opacity: 0; }
    #tree-app-root .tree-block:focus-within:hover .tree-block-tools { opacity: 1; }
    #tree-app-root .tree-block-tools button { width: 24px; height: 26px; font-size: 18px; line-height: 1; color: #6f6f6f; }

    /* Botones Bloque / Subpágina / Favorito: discretos, aparecen al pasar el mouse por el encabezado */
    #tree-app-root .tree-heading + .tree-view-actions { margin: 0 0 6px -8px; opacity: 0; transition: opacity .15s ease; }
    #tree-app-root .tree-heading:hover + .tree-view-actions,
    #tree-app-root .tree-heading + .tree-view-actions:hover,
    #tree-app-root .tree-heading + .tree-view-actions:focus-within { opacity: 1; }
    #tree-app-root .tree-heading + .tree-view-actions .tree-content-action { border: 0; background: transparent; color: var(--tree-text-2); }
    #tree-app-root .tree-heading + .tree-view-actions .tree-content-action:hover { background: var(--tree-hover); color: var(--tree-text); }
    @media (hover: none) { #tree-app-root .tree-heading + .tree-view-actions { opacity: 1; } }

    /* Subpáginas: lista simple, sin bordes ni etiqueta de tipo */
    #tree-app-root .tree-editor + .tree-view-subtitle { margin: 36px 0 4px; color: var(--tree-text-2); font-size: 12px; font-weight: 600; }
    #tree-app-root .tree-child-list { gap: 0; margin-top: 4px; border-top: 0; }
    #tree-app-root .tree-child-link { min-height: 32px; padding: 4px 6px; border: 0; border-radius: 6px; color: var(--tree-text); }
    #tree-app-root .tree-child-link:hover { background: var(--tree-hover); }
    #tree-app-root .tree-child-link small { display: none; }
    #tree-app-root .tree-child-link span:first-child { width: 22px; font-size: 16px; }

    @media (max-width: 760px) {
      #tree-app-root .tree-view { padding: 22px 20px 90px; }
      #tree-app-root .tree-heading-icon { width: 64px; height: 64px; font-size: 50px; }
      #tree-app-root .tree-heading + .tree-view-actions { margin-left: -8px; }
      #tree-app-root .tree-block-tools { left: -22px; }
    }

    /* Link a subpágina dentro del documento */
    #tree-app-root .tree-pagelink { display: flex; width: 100%; align-items: center; gap: 8px; padding: 4px 6px; margin: 1px 0; border: 0; border-radius: 6px; background: transparent; color: var(--tree-text); text-align: left; font-size: 16px; line-height: 1.6; }
    #tree-app-root .tree-pagelink:hover { background: var(--tree-hover); }
    #tree-app-root .tree-pagelink-icon { width: 22px; flex: 0 0 22px; text-align: center; }
    #tree-app-root .tree-pagelink-title { overflow: hidden; font-weight: 500; text-decoration: underline; text-decoration-color: rgba(255,255,255,.28); text-underline-offset: 3px; text-overflow: ellipsis; white-space: nowrap; }

    /* ===== Título centrado, sin emoji ni botones sueltos ===== */
    #tree-app-root .tree-heading-icon { display: none; }
    #tree-app-root .tree-heading { align-items: stretch; margin: 18px 0 24px; }
    #tree-app-root .tree-title-input { text-align: center; }
    #tree-app-root .tree-heading + .tree-view-subtitle { text-align: center; margin-left: 0; }

    /* ===== Favoritos: botones para quitar y eliminar ===== */
    #tree-app-root [data-tree-favorites] .tree-row-action[data-tree-favorite] { color: #e8b931; }
    #tree-app-root [data-tree-favorites] .tree-row-action[data-tree-delete] { color: var(--tree-danger); }
    @media (hover: none) { #tree-app-root [data-tree-favorites] .tree-row-action { display: inline-grid; } }

    /* ===== Panel del texto (transparente → vidrio → sólido) ===== */
    #tree-app-root { --tree-panel-rgb: 25,25,25; --tree-edge-rgb: 255,255,255; color-scheme: dark; }
    #tree-app-root .tree-main { background-color: var(--tree-bg); background-position: center; background-size: cover; background-repeat: no-repeat; }
    #tree-app-root .tree-view {
      margin: 20px auto; min-height: calc(100% - 40px); border-radius: 16px;
      background: rgba(var(--tree-panel-rgb), var(--panel-a, 0));
      -webkit-backdrop-filter: blur(var(--panel-blur, 0px)); backdrop-filter: blur(var(--panel-blur, 0px));
      box-shadow: 0 0 0 1px rgba(var(--tree-edge-rgb), calc(var(--panel-a, 0) * .14));
    }
    @media (max-width: 760px) { #tree-app-root .tree-view { margin: 0 auto; min-height: 100%; border-radius: 0; } }

    /* ===== Barra de esquina: favorito, tema, configuración ===== */
    #tree-app-root .tree-toolbar { position: absolute; top: 12px; right: 24px; z-index: 60; display: flex; align-items: center; gap: 6px; }
    #tree-app-root .tree-tool-btn {
      display: inline-grid; width: 34px; height: 34px; place-items: center; padding: 0; border: 1px solid var(--tree-border); border-radius: 9px;
      background: rgba(var(--tree-panel-rgb), .72); color: var(--tree-text-2);
      -webkit-backdrop-filter: blur(10px); backdrop-filter: blur(10px); transition: background .12s ease, color .12s ease;
    }
    #tree-app-root .tree-tool-btn:hover { background: var(--tree-panel-hover); color: var(--tree-text); }
    #tree-app-root .tree-tool-btn svg { width: 17px; height: 17px; }
    #tree-app-root .tree-tool-btn.is-active { color: #f2c94c; }
    #tree-app-root .tree-tool-btn.is-active svg { fill: currentColor; }
    #tree-app-root .tree-tool-btn[aria-expanded="true"] { background: var(--tree-panel-hover); color: var(--tree-text); }
    #tree-app-root .tree-icon-sun { display: none; }
    #tree-app-root[data-theme="dark"] .tree-icon-moon { display: none; }
    #tree-app-root[data-theme="dark"] .tree-icon-sun { display: block; }
    #tree-app-root .tree-tool-btn:focus-visible { outline: 2px solid var(--tree-accent); outline-offset: 2px; }

    #tree-app-root .tree-settings {
      position: absolute; top: 42px; right: 0; width: 304px; padding: 14px; border: 1px solid var(--tree-border); border-radius: 12px;
      background: var(--tree-sidebar); color: var(--tree-text); box-shadow: 0 14px 40px rgba(0,0,0,.35);
    }
    #tree-app-root .tree-settings[hidden] { display: none; }
    #tree-app-root .tree-settings h3 { margin: 0 0 12px; font-size: 13px; font-weight: 650; }
    #tree-app-root .tree-set-group { display: grid; gap: 8px; padding: 12px 0; border-top: 1px solid var(--tree-border); }
    #tree-app-root .tree-set-group:first-of-type { padding-top: 0; border-top: 0; }
    #tree-app-root .tree-set-label { color: var(--tree-text-2); font-size: 11px; font-weight: 600; letter-spacing: .03em; text-transform: uppercase; }
    #tree-app-root .tree-set-row { display: flex; gap: 6px; }
    #tree-app-root .tree-set-btn {
      flex: 1; min-height: 32px; padding: 0 10px; border: 1px solid var(--tree-border); border-radius: 7px; background: transparent;
      color: var(--tree-text); font-size: 12px !important;
    }
    #tree-app-root .tree-set-btn:hover { background: var(--tree-hover); }
    #tree-app-root .tree-set-btn.is-active { border-color: var(--tree-accent); }
    #tree-app-root .tree-set-hint { margin: 0; color: var(--tree-text-2); font-size: 11px; line-height: 1.4; }
    #tree-app-root .tree-set-presets { display: grid; grid-template-columns: repeat(3, 1fr); gap: 6px; }
    #tree-app-root .tree-set-slider { display: grid; gap: 4px; color: var(--tree-text); font-size: 12px; }
    #tree-app-root .tree-set-slider span { display: flex; justify-content: space-between; }
    #tree-app-root .tree-set-slider output { color: var(--tree-text-2); }
    #tree-app-root .tree-set-slider input { width: 100%; accent-color: var(--tree-accent); }
    @media (max-width: 760px) { #tree-app-root .tree-toolbar { top: 7px; right: 58px; } #tree-app-root .tree-settings { right: -48px; width: min(304px, calc(100vw - 20px)); } }

    /* ===== Modo claro ===== */
    #tree-app-root[data-theme="light"] {
      --tree-bg: #ffffff; --tree-sidebar: #f7f7f5; --tree-panel: #ffffff; --tree-panel-hover: #efefed; --tree-border: #e4e4e0;
      --tree-ink: #37352f; --tree-muted: #787774; --tree-accent: #2f6fed; --tree-danger: #d44c47;
      --tree-text: #37352f; --tree-text-2: #787774; --tree-hover: rgba(55,53,47,.08);
      --tree-panel-rgb: 255,255,255; --tree-edge-rgb: 0,0,0; color-scheme: light;
    }
    #tree-app-root[data-theme="light"] .tree-sidebar { border-right-color: #e4e4e0; }
    #tree-app-root[data-theme="light"] .tree-brand-mark { border-color: #dcdcd8; background: #fff; color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-search { border-color: #e0e0dc; background: #fff; }
    #tree-app-root[data-theme="light"] .tree-search::placeholder { color: #9b9a97; }
    #tree-app-root[data-theme="light"] .tree-search-wrap svg { color: #9b9a97; }
    #tree-app-root[data-theme="light"] .tree-icon-button { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-icon-button:hover { background: rgba(55,53,47,.08); color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-side-heading,
    #tree-app-root[data-theme="light"] .tree-side-heading button,
    #tree-app-root[data-theme="light"] .tree-private-hint,
    #tree-app-root[data-theme="light"] .tree-sidebar-footer,
    #tree-app-root[data-theme="light"] .tree-disclosure { color: #9b9a97; }
    #tree-app-root[data-theme="light"] .tree-sidebar-footer { border-top-color: #e4e4e0; }
    #tree-app-root[data-theme="light"] .tree-node-row:hover,
    #tree-app-root[data-theme="light"] .tree-node-row.is-selected { background: rgba(55,53,47,.08); }
    #tree-app-root[data-theme="light"] .tree-node-row.is-drop-target { background: #e3ecfb; }
    #tree-app-root[data-theme="light"] .tree-node-main { color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-node-icon { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-row-action { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-row-action:hover { background: rgba(55,53,47,.1); color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-inline-menu,
    #tree-app-root[data-theme="light"] .tree-slash-menu { border-color: #e0e0dc; background: #fff; box-shadow: 0 10px 28px rgba(0,0,0,.14); }
    #tree-app-root[data-theme="light"] .tree-inline-menu button,
    #tree-app-root[data-theme="light"] .tree-slash-menu button { color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-inline-menu button:hover,
    #tree-app-root[data-theme="light"] .tree-slash-menu button:hover,
    #tree-app-root[data-theme="light"] .tree-slash-menu button[aria-selected="true"] { background: #f1f1ef; }
    #tree-app-root[data-theme="light"] .tree-inline-menu button[data-tree-delete] { color: #d44c47; }
    #tree-app-root[data-theme="light"] .tree-slash-menu button span:first-child { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-slash-menu button small { color: #9b9a97; }
    #tree-app-root[data-theme="light"] .tree-breadcrumb,
    #tree-app-root[data-theme="light"] .tree-view-subtitle,
    #tree-app-root[data-theme="light"] .tree-empty { color: #9b9a97; }
    #tree-app-root[data-theme="light"] .tree-breadcrumb button { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-breadcrumb button:hover,
    #tree-app-root[data-theme="light"] .tree-breadcrumb strong { color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-title-input::placeholder { color: #c4c4c0; }
    #tree-app-root[data-theme="light"] .tree-content-action { border-color: #e0e0dc; background: #fff; color: #787774; }
    #tree-app-root[data-theme="light"] .tree-content-action:hover { background: #f1f1ef; color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-block-content,
    #tree-app-root[data-theme="light"] .tree-block-content[data-type="h1"],
    #tree-app-root[data-theme="light"] .tree-block-content[data-type="h2"],
    #tree-app-root[data-theme="light"] .tree-block-content[data-type="h3"] { color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-block-content:empty::before { color: #b8b8b4; }
    #tree-app-root[data-theme="light"] .tree-block-content[data-type="quote"] { border-left-color: #cfcfcb; color: #5f5e5b; }
    #tree-app-root[data-theme="light"] .tree-block-content[data-type="code"] { border-color: #e0e0dc; background: #f7f7f5; }
    #tree-app-root[data-theme="light"] .tree-block-marker { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-block[data-type="divider"] hr { border-top-color: #e0e0dc; }
    #tree-app-root[data-theme="light"] .tree-toggle-body { border-left-color: #dcdcd8; color: #5f5e5b; }
    #tree-app-root[data-theme="light"] .tree-block-tools button { color: #b0b0ac; }
    #tree-app-root[data-theme="light"] .tree-block-tools button:hover { background: rgba(55,53,47,.08); color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-pagelink-title { text-decoration-color: rgba(0,0,0,.25); }
    #tree-app-root[data-theme="light"] .tree-search-result { border-color: #e4e4e0; background: #fff; color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-search-result:hover { background: #f1f1ef; }
    #tree-app-root[data-theme="light"] .tree-mobile-top { border-bottom-color: #e4e4e0; background: rgba(255,255,255,.93); }
    #tree-app-root[data-theme="light"] .tree-mobile-top strong { color: #787774; }
    #tree-app-root[data-theme="light"] .tree-settings { box-shadow: 0 14px 40px rgba(0,0,0,.14); }

    /* ===== Sidebar más grande y legible ===== */
    #tree-app-root { grid-template-columns: 340px minmax(0, 1fr); }
    #tree-app-root .tree-sidebar { padding: 18px 8px 10px 12px; }
    #tree-app-root .tree-brand-mark { width: 34px; height: 34px; font-size: 19px; }
    #tree-app-root .tree-brand-name { font-size: 14px; }
    #tree-app-root .tree-sidebar-brand { padding: 4px 8px 18px; }
    #tree-app-root .tree-search { height: 38px; padding-left: 34px; font-size: 13px !important; }
    #tree-app-root .tree-search-wrap svg { top: 11px; width: 16px; height: 16px; }
    #tree-app-root .tree-icon-button { width: 34px; height: 34px; }
    #tree-app-root .tree-icon-button svg { width: 18px; height: 18px; }
    #tree-app-root .tree-side-section { margin: 0 0 16px; }
    #tree-app-root .tree-side-heading { padding: 0 10px 7px; font-size: 11.5px; }
    #tree-app-root .tree-tree { gap: 2px; }
    #tree-app-root .tree-node-row { min-height: 36px; border-radius: 7px; gap: 3px; }
    #tree-app-root .tree-disclosure { width: 24px; height: 30px; }
    #tree-app-root .tree-disclosure svg { width: 14px; height: 14px; }
    #tree-app-root .tree-node-main { gap: 9px; padding: 6px 2px; font-size: 14.5px !important; }
    #tree-app-root .tree-node-icon { width: 22px; flex: 0 0 22px; font-size: 18px; line-height: 1; }
    #tree-app-root .tree-row-action { width: 28px; height: 28px; font-size: 16px; }
    #tree-app-root .tree-row-action svg { width: 17px; height: 17px; }
    #tree-app-root .tree-inline-menu { width: 200px; }
    #tree-app-root .tree-inline-menu button { padding: 8px 10px; font-size: 13px !important; }
    #tree-app-root .tree-private-hint { font-size: 12px; }
    #tree-app-root .tree-sidebar-footer { flex: 0 0 auto; font-size: 12px; }
    #tree-app-root .tree-sidebar-footer .tree-save-status { margin-left: auto; }
    @media (max-width: 760px) { #tree-app-root .tree-sidebar { width: min(92vw, 360px); } }

    /* El panel nunca tapa el fondo por completo: siempre queda margen a los lados */
    #tree-app-root .tree-view { width: min(calc(100% - 32px), 800px); }
    @media (max-width: 760px) { #tree-app-root .tree-view { width: 100%; } }

    /* Placeholder en cada línea vacía donde está el cursor */

    /* ===== Modos de vista ===== */
    #tree-app-root .tree-modes { width: 320px; }
    #tree-app-root .tree-mode-list { display: grid; gap: 6px; margin-top: 10px; }
    #tree-app-root .tree-mode-opt { display: flex; align-items: center; gap: 10px; width: 100%; padding: 8px 10px; border: 1px solid var(--tree-border); border-radius: 9px; background: transparent; color: var(--tree-text); text-align: left; cursor: pointer; }
    #tree-app-root .tree-mode-opt:hover { background: var(--tree-hover); }
    #tree-app-root .tree-mode-opt.is-active { border-color: var(--tree-accent); box-shadow: inset 0 0 0 1px var(--tree-accent); }
    #tree-app-root .tree-mode-opt b { display: block; font-size: 13px; font-weight: 650; }
    #tree-app-root .tree-mode-opt small { display: block; color: var(--tree-text-2); font-size: 11px; line-height: 1.3; }
    #tree-app-root .tree-mode-sw { flex: none; width: 30px; height: 30px; border-radius: 8px; border: 1px solid rgba(128,128,128,.4); background: linear-gradient(135deg, var(--c1) 55%, var(--c2) 55%); }
    #tree-app-root .tree-tool-btn[data-tool-theme][disabled] { opacity: .4; cursor: not-allowed; }
    @media (max-width: 760px) { #tree-app-root .tree-modes { right: -96px; width: min(320px, calc(100vw - 20px)); } }
    #tree-app-root[data-viewmode="netflix"] { --tree-bg:#141414;--tree-sidebar:#000;--tree-panel:#1f1f1f;--tree-panel-hover:#2a2a2a;--tree-border:#2b2b2b;--tree-ink:#fff;--tree-muted:#b3b3b3;--tree-accent:#e50914;--tree-danger:#ff6b6b;--tree-text:#fff;--tree-text-2:#b3b3b3;--tree-hover:rgba(255,255,255,.09);--tree-panel-rgb:20,20,20;--tree-edge-rgb:255,255,255;color-scheme:dark; }
    #tree-app-root[data-viewmode="netflix"] .tree-brand-mark { background: #e50914; border-color: #e50914; color: #fff; }
    #tree-app-root[data-viewmode="netflix"] .tree-brand-name { font-weight: 800; letter-spacing: .02em; }
    #tree-app-root[data-viewmode="netflix"] .tree-main { background-image: linear-gradient(180deg, rgba(229,9,20,.16), transparent 220px); }
    #tree-app-root[data-viewmode="enfoque"] { --tree-bg:#f4ecd8;--tree-sidebar:#ebe1c8;--tree-panel:#faf4e4;--tree-panel-hover:#e3d8bd;--tree-border:#d9ccab;--tree-ink:#433422;--tree-muted:#8a7a5f;--tree-accent:#a0522d;--tree-danger:#b3402f;--tree-text:#433422;--tree-text-2:#8a7a5f;--tree-hover:rgba(67,52,34,.09);--tree-panel-rgb:250,244,228;--tree-edge-rgb:67,52,34;color-scheme:light; }
    #tree-app-root[data-viewmode="enfoque"] .tree-view { font-family: Georgia, "Times New Roman", serif; }
    #tree-app-root[data-viewmode="biblioteca"] { --tree-bg:#13201b;--tree-sidebar:#0e1814;--tree-panel:#1a2c25;--tree-panel-hover:#223a31;--tree-border:#2c4a3e;--tree-ink:#ece6d2;--tree-muted:#9db3a6;--tree-accent:#d4af37;--tree-danger:#e58f8f;--tree-text:#ece6d2;--tree-text-2:#9db3a6;--tree-hover:rgba(212,175,55,.1);--tree-panel-rgb:19,32,27;--tree-edge-rgb:212,175,55;color-scheme:dark; }
    #tree-app-root[data-viewmode="biblioteca"] .tree-view { font-family: Georgia, "Times New Roman", serif; }
    #tree-app-root[data-viewmode="pastel"] { --tree-bg:#fdf3f7;--tree-sidebar:#f6e7f5;--tree-panel:#fffafc;--tree-panel-hover:#f1dff0;--tree-border:#ecd3e6;--tree-ink:#4a3b57;--tree-muted:#8e7ba0;--tree-accent:#9b7ede;--tree-danger:#e0607e;--tree-text:#4a3b57;--tree-text-2:#8e7ba0;--tree-hover:rgba(155,126,222,.12);--tree-panel-rgb:255,250,252;--tree-edge-rgb:74,59,87;color-scheme:light; }
    #tree-app-root[data-viewmode="medianoche"] { --tree-bg:#0b1020;--tree-sidebar:#080c18;--tree-panel:#121a30;--tree-panel-hover:#1a2442;--tree-border:#1f2a4a;--tree-ink:#dbe6ff;--tree-muted:#7f90b8;--tree-accent:#38bdf8;--tree-danger:#f87171;--tree-text:#dbe6ff;--tree-text-2:#7f90b8;--tree-hover:rgba(56,189,248,.1);--tree-panel-rgb:11,16,32;--tree-edge-rgb:120,160,255;color-scheme:dark; }
    #tree-app-root[data-viewmode="contraste"] { --tree-bg:#ffffff;--tree-sidebar:#f2f2f2;--tree-panel:#ffffff;--tree-panel-hover:#e0e0e0;--tree-border:#000;--tree-ink:#000;--tree-muted:#222;--tree-accent:#0000cc;--tree-danger:#b00020;--tree-text:#000;--tree-text-2:#222;--tree-hover:rgba(0,0,0,.12);--tree-panel-rgb:255,255,255;--tree-edge-rgb:0,0,0;color-scheme:light; }

    /* ===== Tarjetas de materias (estilo catálogo) ===== */
    #tree-app-root .tree-view.is-grid { width: min(100%, 1180px); }
    #tree-app-root .tree-card-filter { position: relative; display: flex; align-items: center; gap: 8px; margin: 18px 0 16px; padding: 0 12px; height: 40px; max-width: 420px; border: 1px solid var(--tree-border); border-radius: 10px; background: rgba(var(--tree-panel-rgb), .7); color: var(--tree-text-2); }
    #tree-app-root .tree-card-filter:focus-within { border-color: var(--tree-accent); }
    #tree-app-root .tree-card-filter svg { width: 16px; height: 16px; flex: none; }
    #tree-app-root .tree-card-filter input { flex: 1; min-width: 0; border: 0; outline: 0; background: transparent; color: var(--tree-text); font: inherit; font-size: 13px; }
    #tree-app-root .tree-card-count { font-size: 11px; color: var(--tree-text-2); }
    #tree-app-root .tree-card-toolbar { display: flex; flex-wrap: wrap; align-items: center; justify-content: space-between; gap: 10px; margin: 18px 0 16px; }
    #tree-app-root .tree-card-toolbar .tree-card-filter { margin: 0; }
    #tree-app-root .tree-card-size-controls { display: flex; align-items: center; gap: 7px; color: var(--tree-text-2); font-size: 11px; }
    #tree-app-root .tree-card-size-controls button { display: grid; width: 30px; height: 30px; place-items: center; border: 1px solid var(--tree-border); border-radius: 7px; background: var(--tree-panel); color: var(--tree-text); font-size: 18px; line-height: 1; }
    #tree-app-root .tree-card-size-controls button:hover:not(:disabled) { border-color: var(--tree-accent); color: var(--tree-accent); }
    #tree-app-root .tree-card-size-controls button:disabled { opacity: .45; cursor: default; }
    #tree-app-root .tree-card-size-controls output { min-width: 48px; color: var(--tree-text); text-align: center; font-variant-numeric: tabular-nums; }
    #tree-app-root .tree-card-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(min(100%, var(--tree-card-size, 250px)), var(--tree-card-size, 250px))); gap: 14px; padding: 6px 2px 10px; }
    #tree-app-root .tree-card { position: relative; aspect-ratio: 16 / 9; transition: transform .22s cubic-bezier(.2,.7,.3,1), z-index 0s .22s; }
    #tree-app-root .tree-card[hidden] { display: none; }
    #tree-app-root .tree-card:hover, #tree-app-root .tree-card:focus-within, #tree-app-root .tree-card.is-menu-open { transform: scale(1.07); z-index: 5; transition-delay: 0s; }
    #tree-app-root .tree-card-face { position: absolute; inset: 0; display: grid; place-items: center; width: 100%; padding: 12px; overflow: hidden; border: 1px solid rgba(var(--tree-edge-rgb), .12); border-radius: 10px; color: #fff; cursor: pointer; text-align: center;
      background: linear-gradient(135deg, hsl(var(--h, 220) 55% 34%), hsl(calc(var(--h, 220) + 40) 60% 20%)); background-size: cover; background-position: center; transition: box-shadow .22s ease, border-color .22s ease; }
    #tree-app-root .tree-card:hover .tree-card-face, #tree-app-root .tree-card:focus-within .tree-card-face { box-shadow: 0 14px 34px rgba(0,0,0,.45); border-color: var(--tree-accent); }
    #tree-app-root .tree-card-shade { position: absolute; inset: 0; background: radial-gradient(circle at center, rgba(0,0,0,.28), rgba(0,0,0,.62)); transition: opacity .22s ease; }
    #tree-app-root .tree-card:hover .tree-card-shade { opacity: .55; }
    #tree-app-root .tree-card-body { position: relative; display: grid; gap: 2px; justify-items: center; max-width: 100%; }
    #tree-app-root .tree-card-icon { font-size: 22px; line-height: 1; transition: transform .22s ease; }
    #tree-app-root .tree-card-icon:empty { display: none; }
    #tree-app-root .tree-card-title { max-width: 100%; font-size: 17px; font-weight: 800; line-height: 1.2; letter-spacing: .01em; text-shadow: 0 2px 10px rgba(0,0,0,.7); overflow-wrap: anywhere; display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden; transition: letter-spacing .22s ease; }
    #tree-app-root .tree-card-sub { font-size: 11px; opacity: 0; transform: translateY(4px); transition: opacity .22s ease, transform .22s ease; text-shadow: 0 1px 6px rgba(0,0,0,.7); }
    #tree-app-root .tree-card:hover .tree-card-sub, #tree-app-root .tree-card:focus-within .tree-card-sub { opacity: .9; transform: none; }
    #tree-app-root .tree-card:hover .tree-card-icon { transform: scale(1.18); }
    #tree-app-root .tree-card:hover .tree-card-title { letter-spacing: .04em; }
    #tree-app-root .tree-card-dots { position: absolute; top: 6px; right: 6px; z-index: 2; display: grid; place-items: center; width: 28px; height: 28px; padding: 0; border: 0; border-radius: 50%; background: rgba(0,0,0,.5); color: #fff; opacity: 0; transition: opacity .15s ease, background .15s ease; cursor: pointer; }
    #tree-app-root .tree-card-dots svg { width: 16px; height: 16px; }
    #tree-app-root .tree-card:hover .tree-card-dots, #tree-app-root .tree-card:focus-within .tree-card-dots, #tree-app-root .tree-card.is-menu-open .tree-card-dots { opacity: 1; }
    #tree-app-root .tree-card-dots:hover { background: rgba(0,0,0,.8); }
    @media (hover: none) { #tree-app-root .tree-card-dots { opacity: 1; } #tree-app-root .tree-card-sub { opacity: .9; transform: none; } }
    #tree-app-root .tree-card-menu { position: absolute; top: 38px; right: 6px; z-index: 6; display: grid; min-width: 148px; padding: 4px; border: 1px solid var(--tree-border); border-radius: 10px; background: var(--tree-sidebar); box-shadow: 0 12px 32px rgba(0,0,0,.4); }
    #tree-app-root .tree-card-menu[hidden] { display: none; }
    #tree-app-root .tree-card-menu button { padding: 7px 10px; border: 0; border-radius: 7px; background: transparent; color: var(--tree-text); font-size: 12.5px; text-align: left; cursor: pointer; }
    #tree-app-root .tree-card-menu button:hover { background: var(--tree-hover); }
    #tree-app-root .tree-card-add .tree-card-face { border: 2px dashed rgba(var(--tree-edge-rgb), .25); background: transparent; color: var(--tree-text-2); }
    #tree-app-root .tree-card-add:hover .tree-card-face { color: var(--tree-text); }
    #tree-app-root .tree-card-plus { font-size: 30px; line-height: 1; font-weight: 300; }
    #tree-app-root .tree-card-add .tree-card-title { font-size: 13px; font-weight: 600; text-shadow: none; }
    #tree-app-root .tree-card-none { color: var(--tree-text-2); font-size: 13px; }
    @media (max-width: 760px) { #tree-app-root .tree-card-grid { gap: 10px; } #tree-app-root .tree-card-title { font-size: 14px; } }
    /* ===== Bloque de imagen: ocupa su propia fila, el texto nunca se parte ===== */
    #tree-app-root .tree-block[data-type="image"] { display: block; margin: 10px 0; }
    #tree-app-root .tree-img { position: relative; display: block; max-width: 100%; margin-top: 0; margin-bottom: 0; margin-right: 0; border-radius: 8px; outline: none; }
    #tree-app-root .tree-img img { display: block; width: 100%; height: auto; border-radius: 8px; background: var(--tree-hover); user-select: none; -webkit-user-drag: none; }
    #tree-app-root .tree-img:focus-visible img { box-shadow: 0 0 0 2px var(--tree-accent); }
    #tree-app-root .tree-img-controls { position: absolute; left: 50%; bottom: 8px; z-index: 5; display: none; max-width: calc(100% - 12px); flex-wrap: wrap; justify-content: center; gap: 2px; padding: 4px; border: 1px solid #3b3b3b; border-radius: 9px; background: rgba(28,28,28,.94); transform: translateX(-50%); backdrop-filter: blur(8px); }
    #tree-app-root .tree-img.is-editing .tree-img-controls { display: flex; }
    #tree-app-root .tree-img.is-editing img { box-shadow: 0 0 0 2px var(--tree-accent); }
    #tree-app-root .tree-img-edit { position: absolute; top: 8px; right: 8px; z-index: 5; display: none; width: 30px; height: 30px; padding: 0; border: 0; border-radius: 50%; background: rgba(0,0,0,.6); color: #fff; font-size: 14px; }
    #tree-app-root .tree-img:not(.is-editing):hover .tree-img-edit, #tree-app-root .tree-img:not(.is-editing):focus-within .tree-img-edit { display: block; }
    @media (hover: none) { #tree-app-root .tree-img:not(.is-editing) .tree-img-edit { display: block; } }
    #tree-app-root .tree-img-controls button[data-tree-img-act="apply"] { padding: 0 12px; background: var(--tree-accent); color: #111; font-weight: 600; }
    #tree-app-root .tree-img-controls button[data-tree-img-act="apply"]:hover { filter: brightness(1.1); background: var(--tree-accent); }
    #tree-app-root .tree-img-controls button { min-width: 28px; height: 28px; padding: 0 7px; border: 0; border-radius: 6px; background: transparent; color: #e6e6e6; font-size: 13px; line-height: 1; }
    #tree-app-root .tree-img-controls button:hover { background: #3a3a3a; }
    #tree-app-root .tree-img-controls button[data-tree-img-act="delete"] { color: var(--tree-danger); }
    #tree-app-root .tree-img-controls .tree-img-sep { width: 1px; margin: 4px 3px; background: #444; }
    #tree-app-root[data-theme="light"] .tree-img-controls { border-color: #d9d9d5; background: rgba(255,255,255,.96); }
    #tree-app-root[data-theme="light"] .tree-img-controls button { color: #37352f; }
    #tree-app-root[data-theme="light"] .tree-img-controls button:hover { background: #ececea; }
    /* ===== Texto que rodea a la imagen (estilo libro / Wikipedia) ===== */
    #tree-app-root .tree-block[data-type="paragraph"], #tree-app-root .tree-block[data-type="legacy"],
    #tree-app-root .tree-block[data-type="h1"], #tree-app-root .tree-block[data-type="h2"], #tree-app-root .tree-block[data-type="h3"] { display: block; }
    #tree-app-root .tree-block[data-type="image"].is-float { float: left; max-width: 100%; margin: 6px 18px 10px 0; }
    #tree-app-root .tree-block[data-type="image"].is-float-right { float: right; margin: 6px 0 10px 18px; }
    #tree-app-root .tree-block[data-type="image"].is-float .tree-img { width: 100%; }
    #tree-app-root .tree-block[data-type="image"].is-float-right .tree-block-tools { display: none; }
    #tree-app-root .tree-editor::after { content: ""; display: block; clear: both; }
    @media (max-width: 560px) { #tree-app-root .tree-block[data-type="image"].is-float { float: none; width: 100% !important; margin: 8px 0; } }
    #tree-app-root .tree-block-content a { color: inherit; text-decoration: underline; }

    /* ===== Tipografía y tamaño del texto (Configuración) ===== */
    #tree-app-root .tree-view { font-family: var(--page-font, inherit); }
    #tree-app-root .tree-editor { font-family: var(--page-font, inherit); font-size: var(--page-size, 16px); }
    #tree-app-root .tree-block-content[data-type="h1"] { font-family: var(--page-font, inherit); font-size: 1.875em; }
    #tree-app-root .tree-block-content[data-type="h2"] { font-family: var(--page-font, inherit); font-size: 1.5em; }
    #tree-app-root .tree-block-content[data-type="h3"] { font-family: var(--page-font, inherit); font-size: 1.1875em; }
    #tree-app-root .tree-set-select { width: 100%; padding: 7px 8px; border: 1px solid var(--tree-border); border-radius: 6px; background: var(--tree-panel); color: var(--tree-text); font-size: 12px; }
    /* ===== Correcciones: menú de 3 puntitos, texto junto a imágenes, íconos con imagen ===== */
    #tree-app-root .tree-children { animation: tree-expand .14s ease backwards; }
    /* Fix: títulos largos ensanchaban la columna del árbol y empujaban los 3 puntitos fuera del menú lateral */
    #tree-app-root .tree-sidebar-scroll, #tree-app-root .tree-side-section { min-width: 0; }
    #tree-app-root .tree-tree { grid-template-columns: minmax(0, 1fr); }
    #tree-app-root .tree-node-group, #tree-app-root .tree-children, #tree-app-root .tree-node-row { min-width: 0; max-width: 100%; }
    #tree-app-root .tree-inline-menu { position: fixed; z-index: 400; top: 0; left: 0; right: auto; max-height: calc(100vh - 16px); overflow: auto; pointer-events: auto; }
    #tree-app-root .tree-inline-menu button { width: 100%; cursor: pointer; pointer-events: auto; }
    #tree-app-root .tree-node-row:has(.tree-inline-menu:not([hidden])) .tree-row-action { display: inline-grid; }
    #tree-app-root .tree-block[data-type="image"].is-float { max-width: 55%; }
    #tree-app-root .tree-editor:has(.is-float):has(.is-float-right) .tree-block[data-type="image"].is-float,
    #tree-app-root .tree-editor:has(.is-float):has(.is-float-right) .tree-block[data-type="image"].is-float-right { max-width: 36%; }
    #tree-app-root .tree-block[data-type="image"].is-float-right { max-width: 55%; }
    #tree-app-root .tree-block-content { overflow-wrap: break-word; }
    #tree-app-root .tree-block[data-type="image"].is-float .tree-img,
    #tree-app-root .tree-block[data-type="image"].is-float-right .tree-img { margin: 0; width: 100%; max-width: 100%; }
    #tree-app-root .tree-block[data-type="image"].is-float,
    #tree-app-root .tree-block[data-type="image"].is-float-right { box-sizing: border-box; }
    #tree-app-root .tree-img-controls { z-index: 20; }
    #tree-app-root img.tree-icon-img { display: inline-block; width: 1.15em; height: 1.15em; border-radius: .22em; object-fit: cover; vertical-align: -.2em; }
    #tree-app-root .tree-node-icon img.tree-icon-img { width: 22px; height: 22px; vertical-align: middle; }
    #tree-app-root .tree-heading-icon img.tree-icon-img { width: 48px; height: 48px; border-radius: 10px; }
    #tree-app-root .tree-card-icon img.tree-icon-img { width: 34px; height: 34px; border-radius: 8px; }
    .tree-icon-dialog { position: fixed; inset: 0; z-index: 500; display: grid; place-items: center; padding: 16px; background: rgba(0,0,0,.55); }
    .tree-icon-dialog[hidden] { display: none; }
    .tree-icon-box { width: min(100%, 380px); display: grid; gap: 12px; padding: 18px; border: 1px solid #3b3b3b; border-radius: 12px; background: #242424; color: #e6e6e6; font: 14px/1.4 system-ui, sans-serif; box-shadow: 0 20px 50px #000a; }
    .tree-icon-box h3 { margin: 0; font-size: 16px; }
    .tree-icon-box label { display: grid; gap: 4px; font-size: 12px; color: #aaa; }
    .tree-icon-box input[type="text"] { padding: 8px 10px; border: 1px solid #444; border-radius: 6px; background: #1c1c1c; color: #eee; font-size: 16px; }
    .tree-icon-preview { display: grid; width: 72px; height: 72px; place-items: center; border: 1px dashed #555; border-radius: 12px; font-size: 38px; overflow: hidden; }
    .tree-icon-preview img { width: 100%; height: 100%; object-fit: cover; }
    .tree-icon-row { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; }
    .tree-icon-box button { padding: 8px 12px; border: 1px solid #444; border-radius: 7px; background: #2d2d2d; color: #eee; cursor: pointer; font: inherit; font-size: 13px; }
    .tree-icon-box button:hover { background: #3a3a3a; }
    .tree-icon-box button.is-primary { border-color: #8a9cff; background: #8a9cff; color: #111; font-weight: 600; }
    .tree-icon-msg { min-height: 1.2em; color: #e58f8f; font-size: 12px; }
    .tree-icon-hint { color: #888; font-size: 11px; }
    /* ===== Efectos y animaciones (fondo y texto) ===== */
    #tree-app-root .tree-main { isolation: isolate; }
    #tree-app-root .tree-view { position: relative; z-index: 1; }
    #tree-app-root .tree-fxpanel { width: 320px; max-height: calc(100vh - 70px); overflow: auto; }
    @media (max-width: 760px) { #tree-app-root .tree-fxpanel { right: -96px; width: min(320px, calc(100vw - 20px)); } }
    #tree-app-root .tree-fx-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 6px; }
    #tree-app-root .tree-fx-grid .tree-set-btn { min-height: 36px; padding: 0 8px; text-align: left; }
    #tree-app-root .tree-set-color { display: flex; align-items: center; justify-content: space-between; gap: 8px; color: var(--tree-text); font-size: 12px; }
    #tree-app-root .tree-set-color input { width: 42px; height: 28px; padding: 1px; border: 1px solid var(--tree-border); border-radius: 6px; background: transparent; cursor: pointer; }

    .tree-fx-layer { position: fixed; z-index: 0; overflow: hidden; pointer-events: none; opacity: var(--fx-i, .7); }
    .tree-fx-layer:not([data-fx]), .tree-fx-layer[data-fx="none"] { display: none; }
    .tree-fx-layer::before, .tree-fx-layer::after { pointer-events: none; }
    /* Brillo */
    .tree-fx-layer[data-fx="shine"]::before { content: ""; position: absolute; top: -20%; bottom: -20%; left: -60%; width: 60%; transform: skewX(-18deg);
      background: linear-gradient(105deg, transparent 0, rgba(255,255,255,.1) 25%, rgba(255,255,255,.6) 50%, rgba(255,255,255,.1) 75%, transparent 100%);
      animation: fx-shine calc(5s / var(--fx-s, 1)) ease-in-out infinite; }
    @keyframes fx-shine { 0% { left: -60%; } 60%, 100% { left: 120%; } }
    /* Aurora: fondo en movimiento */
    .tree-fx-layer[data-fx="aurora"] { background: radial-gradient(45% 55% at 20% 25%, #7c5cff 0, transparent 70%), radial-gradient(40% 50% at 80% 20%, #00d4ff 0, transparent 70%), radial-gradient(50% 60% at 65% 85%, #ff4fd8 0, transparent 70%), radial-gradient(35% 45% at 15% 85%, #3dffa0 0, transparent 70%); background-size: 180% 180%; animation: fx-aurora calc(16s / var(--fx-s, 1)) ease-in-out infinite alternate; }
    @keyframes fx-aurora { 0% { background-position: 0% 0%; filter: blur(22px) saturate(1.4) hue-rotate(0deg); } 100% { background-position: 100% 100%; filter: blur(22px) saturate(1.4) hue-rotate(90deg); } }
    /* Arcoíris: colores brillantes */
    .tree-fx-layer[data-fx="rainbow"] { background: linear-gradient(120deg, #ff0080, #ff8c00, #ffee00, #00e676, #00b0ff, #7c4dff, #ff0080); background-size: 300% 300%; animation: fx-rainbow calc(9s / var(--fx-s, 1)) ease-in-out infinite alternate; }
    @keyframes fx-rainbow { 0% { background-position: 0% 50%; } 100% { background-position: 100% 50%; } }
    /* Fiesta: luces que parpadean */
    .tree-fx-layer[data-fx="party"]::before, .tree-fx-layer[data-fx="party"]::after { content: ""; position: absolute; inset: 0; mix-blend-mode: screen; }
    .tree-fx-layer[data-fx="party"]::before { background: radial-gradient(circle at 12% 22%, #ff2bd6 0, transparent 24%), radial-gradient(circle at 88% 18%, #00e5ff 0, transparent 26%), radial-gradient(circle at 50% 88%, #ffe600 0, transparent 28%); animation: fx-party-a calc(2s / var(--fx-s, 1)) ease-in-out infinite alternate; }
    .tree-fx-layer[data-fx="party"]::after { background: radial-gradient(circle at 80% 70%, #7cff4f 0, transparent 26%), radial-gradient(circle at 20% 75%, #ff8a00 0, transparent 24%), radial-gradient(circle at 55% 12%, #8a4dff 0, transparent 26%); animation: fx-party-b calc(2s / var(--fx-s, 1)) ease-in-out infinite alternate; }
    @keyframes fx-party-a { 0% { opacity: .1; transform: scale(.9); } 100% { opacity: 1; transform: scale(1.15); } }
    @keyframes fx-party-b { 0% { opacity: 1; transform: scale(1.15); } 100% { opacity: .1; transform: scale(.9); } }
    /* Disco: rayos que giran */
    .tree-fx-layer[data-fx="disco"]::before { content: ""; position: absolute; inset: -60%; filter: blur(10px); animation: fx-spin calc(12s / var(--fx-s, 1)) linear infinite;
      background: conic-gradient(from 0deg at 50% 50%, transparent 0 8%, rgba(255,0,200,.55) 10% 14%, transparent 16% 33%, rgba(0,200,255,.55) 35% 39%, transparent 41% 58%, rgba(255,230,0,.55) 60% 64%, transparent 66% 83%, rgba(0,255,120,.55) 85% 89%, transparent 91%); }
    .tree-fx-layer[data-fx="disco"]::after { content: ""; position: absolute; inset: 0; background: radial-gradient(circle at 50% 50%, rgba(255,255,255,.18), transparent 60%); animation: fx-breathe calc(2.4s / var(--fx-s, 1)) ease-in-out infinite alternate; }
    @keyframes fx-spin { to { transform: rotate(360deg); } }
    /* Estrellas que titilan */
    .tree-fx-layer[data-fx="stars"]::before, .tree-fx-layer[data-fx="stars"]::after { content: ""; position: absolute; inset: 0; }
    .tree-fx-layer[data-fx="stars"]::before { background-size: 200px 200px; animation: fx-twinkle calc(3s / var(--fx-s, 1)) ease-in-out infinite alternate;
      background-image: radial-gradient(2px 2px at 20px 30px, #fff, transparent), radial-gradient(1.5px 1.5px at 90px 80px, #cfe8ff, transparent), radial-gradient(2px 2px at 150px 20px, #fff6c8, transparent), radial-gradient(1.5px 1.5px at 60px 140px, #fff, transparent), radial-gradient(2px 2px at 180px 160px, #ffd0f4, transparent); }
    .tree-fx-layer[data-fx="stars"]::after { background-size: 270px 270px; animation: fx-twinkle calc(4s / var(--fx-s, 1)) ease-in-out -1.5s infinite alternate;
      background-image: radial-gradient(2px 2px at 40px 60px, #fff, transparent), radial-gradient(1.5px 1.5px at 120px 120px, #bfe3ff, transparent), radial-gradient(2px 2px at 220px 40px, #fff, transparent), radial-gradient(1.5px 1.5px at 200px 210px, #ffe9a8, transparent); }
    @keyframes fx-twinkle { 0% { opacity: .2; } 100% { opacity: 1; } }
    /* Marco de neón */
    .tree-fx-layer[data-fx="frame"]::before { content: ""; position: absolute; inset: 0; box-shadow: inset 0 0 50px 6px #ff00cc, inset 0 0 110px 18px #00d9ff; animation: fx-hue calc(7s / var(--fx-s, 1)) linear infinite, fx-breathe calc(2.6s / var(--fx-s, 1)) ease-in-out infinite alternate; }
    @keyframes fx-hue { to { filter: hue-rotate(360deg); } }
    @keyframes fx-breathe { from { opacity: .5; } to { opacity: 1; } }
    /* Confeti */
    .tree-fx-layer[data-fx="confetti"] i { position: absolute; top: -5%; left: var(--x); width: 8px; height: 14px; border-radius: 2px; background: var(--c); animation: fx-fall calc(var(--d) / var(--fx-s, 1)) linear var(--w) infinite; }
    @keyframes fx-fall { 0% { transform: translateY(-10vh) rotate(0deg); } 100% { transform: translateY(115vh) rotate(720deg); } }

    /* ----- Animaciones de texto ----- */
    #tree-app-root .tree-main[data-tx="neon"] :is(.tree-block-content, .tree-title-input) { animation: tx-neon calc(2.4s / var(--tx-s, 1)) ease-in-out infinite alternate; }
    @keyframes tx-neon { from { text-shadow: 0 0 2px var(--tx-c, #00e5ff), 0 0 7px var(--tx-c, #00e5ff), 0 0 14px var(--tx-c, #00e5ff); } to { text-shadow: 0 0 4px var(--tx-c, #00e5ff), 0 0 14px var(--tx-c, #00e5ff), 0 0 30px var(--tx-c, #00e5ff), 0 0 46px var(--tx-c, #00e5ff); } }
    #tree-app-root .tree-main[data-tx="lights"] :is(.tree-block-content, .tree-title-input) { animation: tx-lights calc(3s / var(--tx-s, 1)) ease-in-out infinite; }
    @keyframes tx-lights {
      0%, 100% { opacity: 1; text-shadow: 0 0 6px var(--tx-c, #ffd54a), 0 0 18px var(--tx-c, #ffd54a); }
      35% { opacity: 1; text-shadow: 0 0 6px var(--tx-c, #ffd54a), 0 0 18px var(--tx-c, #ffd54a); }
      50% { opacity: .12; text-shadow: none; }
      65% { opacity: 1; text-shadow: 0 0 6px var(--tx-c, #ffd54a), 0 0 18px var(--tx-c, #ffd54a); } }
    #tree-app-root .tree-main[data-tx="fade"] :is(.tree-block-content, .tree-title-input) { animation: tx-fade calc(2.5s / var(--tx-s, 1)) ease-in-out infinite; }
    @keyframes tx-fade { 0%, 100% { opacity: 1; } 50% { opacity: .05; } }
    #tree-app-root .tree-main[data-tx="rainbow"] :is(.tree-block-content, .tree-title-input) { background-image: linear-gradient(90deg, #ff3b6b, #ff9d00, #ffe600, #36e08a, #27c1ff, #8a5cff, #ff3b6b); background-size: 200% auto; -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent; animation: tx-slide calc(4s / var(--tx-s, 1)) linear infinite; }
    @keyframes tx-slide { to { background-position: 200% center; } }
    #tree-app-root .tree-main[data-tx="party"] :is(.tree-block-content, .tree-title-input) { animation: tx-party calc(3.6s / var(--tx-s, 1)) ease-in-out infinite; }
    #tree-app-root .tree-main[data-tx="party"] .tree-block:nth-child(3n+2) .tree-block-content { animation-delay: calc(-1.2s / var(--tx-s, 1)); }
    #tree-app-root .tree-main[data-tx="party"] .tree-block:nth-child(3n) .tree-block-content { animation-delay: calc(-2.4s / var(--tx-s, 1)); }
    @keyframes tx-party { 0% { color: #ff3b6b; } 16% { color: #ff9d00; } 33% { color: #ffe600; } 50% { color: #36e08a; } 66% { color: #27c1ff; } 83% { color: #c25cff; } 100% { color: #ff3b6b; } }
    #tree-app-root .tree-main[data-tx="gold"] :is(.tree-block-content, .tree-title-input) { background-image: linear-gradient(100deg, #b8860b 20%, #fff6b0 40%, #ffd700 50%, #b8860b 80%); background-size: 250% auto; -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent; animation: tx-sweep calc(3.5s / var(--tx-s, 1)) linear infinite; }
    @keyframes tx-sweep { from { background-position: 150% center; } to { background-position: -100% center; } }
    #tree-app-root .tree-main[data-tx="wave"] .tree-block-content, #tree-app-root .tree-main[data-tx="wave"] .tree-title-input { animation: tx-wave calc(2.4s / var(--tx-s, 1)) ease-in-out infinite; }
    #tree-app-root .tree-main[data-tx="wave"] .tree-block:nth-child(5n+2) .tree-block-content { animation-delay: calc(-.5s / var(--tx-s, 1)); }
    #tree-app-root .tree-main[data-tx="wave"] .tree-block:nth-child(5n+3) .tree-block-content { animation-delay: calc(-1s / var(--tx-s, 1)); }
    #tree-app-root .tree-main[data-tx="wave"] .tree-block:nth-child(5n+4) .tree-block-content { animation-delay: calc(-1.5s / var(--tx-s, 1)); }
    #tree-app-root .tree-main[data-tx="wave"] .tree-block:nth-child(5n) .tree-block-content { animation-delay: calc(-2s / var(--tx-s, 1)); }
    @keyframes tx-wave { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-4px); } }
    #tree-app-root .tree-main[data-tx="glitch"] :is(.tree-block-content, .tree-title-input) { text-shadow: 2px 0 #ff2bd6, -2px 0 #00e5ff; animation: tx-glitch calc(3.2s / var(--tx-s, 1)) linear infinite; }
    @keyframes tx-glitch { 0%, 100% { transform: none; text-shadow: 1px 0 #ff2bd6, -1px 0 #00e5ff; } 50% { transform: translateX(-1px); text-shadow: 3px 0 #ff2bd6, -3px 0 #00e5ff; } }
    #tree-app-root .tree-main[data-tx="pulse"] :is(.tree-block-content, .tree-title-input) { transform-origin: left center; animation: tx-pulse calc(1.4s / var(--tx-s, 1)) ease-in-out infinite; }
    @keyframes tx-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.025); } }
    /* El bloque que estás escribiendo se queda quieto y legible */
    #tree-app-root .tree-main[data-tx] :is(.tree-block-content, .tree-title-input):focus { animation: none; opacity: 1; transform: none; text-shadow: none; background-image: none; -webkit-text-fill-color: currentColor; }
  </style>
</head>
<body>
  <div class="app">
    <aside class="sidebar" id="sidebar">
      <div class="brand">
        <div class="brand-mark" aria-hidden="true"><svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M5 4h14v4h-5v12h-4V8H5z"/></svg></div>
        <div class="brand-copy">
          <span class="brand-name">Tus carpetas</span>
          <span class="brand-subtitle">en un solo lugar</span>
        </div>
      </div>
      <p class="workspace-label">Mis carpetas</p>
      <label class="dashboard-sidebar-search">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><circle cx="10.8" cy="10.8" r="6.8"/><path d="m16 16 4.5 4.5"/></svg>
        <input type="search" data-dashboard-search placeholder="Buscar en mis materias" aria-label="Buscar en mis materias">
      </label>
      <div class="dashboard-quick-links">
      <button class="nav-item active" id="home-link" type="button">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m3 10 9-7 9 7v10a1 1 0 0 1-1 1h-5v-7H9v7H4a1 1 0 0 1-1-1z"/></svg>
        <span class="nav-label">Inicio</span>
      </button>
      <button class="nav-item" id="materias-link" type="button" data-seccion-id="mis-carpetas">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 5.5A2.5 2.5 0 0 1 6.5 3H20v17H6.5A2.5 2.5 0 0 1 4 17.5z"/><path d="M4 6h13M8 10h8m-8 4h6"/></svg>
        <span class="nav-label">Materias</span>
      </button>
      <button class="nav-item" type="button" data-dashboard-target="dashboard-study-hours">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="3.5" y="5" width="17" height="16" rx="2"/><path d="M7.5 3v4m9-4v4M3.5 10h17M8 14h3m2 0h3m-8 3h3"/></svg>
        <span class="nav-label">Calendario</span>
      </button>
      <button class="nav-item" id="new-folder-nav" type="button">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M3 7a2 2 0 0 1 2-2h5l2 2h7a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><path d="M12 10v6m-3-3h6"/></svg>
        <span class="nav-label">Nueva página</span>
      </button>
      <button class="nav-item" id="new-section-nav" type="button" aria-label="Crear nueva sección">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M12 5v14M5 12h14"/></svg>
        <span class="nav-label">+ Nueva sección</span>
      </button>
      </div>
      <button class="nav-item" id="settings-link" type="button">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="12" cy="12" r="3"/><path d="m19.4 15 .1.1a1.7 1.7 0 0 1-2.4 2.4l-.1-.1a1.7 1.7 0 0 0-2.9 1.2v.2a1.7 1.7 0 0 1-3.4 0v-.2a1.7 1.7 0 0 0-2.9-1.2l-.1.1a1.7 1.7 0 0 1-2.4-2.4l.1-.1a1.7 1.7 0 0 0-1.2-2.9H4a1.7 1.7 0 0 1 0-3.4h.2a1.7 1.7 0 0 0 1.2-2.9l-.1-.1a1.7 1.7 0 0 1 2.4-2.4l.1.1a1.7 1.7 0 0 0 2.9-1.2V4a1.7 1.7 0 0 1 3.4 0v.2a1.7 1.7 0 0 0 2.9 1.2l.1-.1a1.7 1.7 0 0 1 2.4 2.4l-.1.1a1.7 1.7 0 0 0 1.2 2.9h.2a1.7 1.7 0 0 1 0 3.4h-.2a1.7 1.7 0 0 0-1.2 2.9Z"/></svg>
        <span class="nav-label">Configuración</span>
      </button>
      <div class="folder-section">
        <div class="section-heading">
          <p class="workspace-label">Secciones</p>
          <button class="icon-button" id="add-folder-icon" type="button" aria-label="Nueva sección" title="Nueva sección">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg>
          </button>
        </div>
        <div class="folder-list" id="folder-list"></div>
      </div>
      <div class="sidebar-spacer"></div>
      <div class="dashboard-shortcuts">
        <button class="nav-item" type="button" data-dashboard-favorites>
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m12 3 2.8 5.7 6.2.9-4.5 4.4 1.1 6.2-5.6-3-5.6 3 1.1-6.2L3 9.6l6.2-.9z"/></svg>
          <span class="nav-label">Favoritos</span>
        </button>
        <button class="nav-item" type="button" data-dashboard-archive>
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 4h16v4H4zM5 8v12h14V8m-9 4h4"/></svg>
          <span class="nav-label">Archivo personal</span>
        </button>
        <button class="nav-item" type="button" data-dashboard-info="updates">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M12 3v4m0 10v4m9-9h-4M7 12H3m15.4-6.4-2.8 2.8m-7.2 7.2-2.8 2.8m0-12.8 2.8 2.8m7.2 7.2 2.8 2.8"/><circle cx="12" cy="12" r="3"/></svg>
          <span class="nav-label">Actualizaciones</span>
        </button>
        <button class="nav-item" type="button" data-dashboard-info="help">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="12" cy="12" r="9"/><path d="M9.6 9a2.5 2.5 0 0 1 4.8.9c0 1.7-2.4 2.1-2.4 4m0 3h.01"/></svg>
          <span class="nav-label">Ayuda</span>
        </button>
      </div>
      <div class="profile">
        <div class="avatar" aria-hidden="true">T</div>
        <div class="profile-copy">
          <div class="profile-name">Tu espacio</div>
          <div class="profile-caption">Plan personal</div>
        </div>
      </div>
    </aside>
    <button class="menu-overlay" id="menu-overlay" type="button" aria-label="Cerrar menú lateral" hidden></button>
    <main class="main">
      <header class="topbar">
        <nav class="home-navigation" id="home-navigation" aria-label="Navegación principal"></nav>
        <div class="breadcrumbs" id="breadcrumbs"></div>
        <div class="top-actions">
          <span class="save-state" id="save-state" aria-live="polite"></span>
          <button class="icon-button theme-toggle" id="theme-toggle" type="button" aria-label="Activar modo oscuro" title="Activar modo oscuro"></button>
          <label class="search-wrap" id="search-wrap" aria-label="Buscar en todo el material">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><circle cx="10.8" cy="10.8" r="6.8"/><path d="m16 16 4.5 4.5"/></svg>
            <input class="search-input" id="search" type="search" placeholder="Buscar páginas, autores o PDF..." autocomplete="off">
          </label>
        </div>
      </header>
      <div id="view"></div>
    </main>
  </div>
  <button class="menu-borde" id="menu-borde" type="button" aria-label="Mostrar menú lateral" title="Mostrar menú lateral"></button>
  <div class="editor-dialog" id="dialog-interaccion" role="dialog" aria-modal="true" aria-labelledby="dialog-interaccion-titulo" hidden>
    <form class="editor-dialog-contenido" id="dialog-interaccion-form"></form>
  </div>
  <div id="tree-app-root" aria-label="Tu cuaderno en un solo lugar">
    <aside class="tree-sidebar" aria-label="Árbol de páginas">
      <div class="tree-sidebar-scroll">
      <div class="tree-sidebar-brand"><span class="tree-brand-mark" aria-hidden="true">T</span><span class="tree-brand-name">Tu cuaderno en un solo lugar</span></div>
      <div class="tree-sidebar-controls"><label class="tree-search-wrap"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><circle cx="10.8" cy="10.8" r="6.8"/><path d="m16 16 4.5 4.5"/></svg><input class="tree-search" type="search" placeholder="Buscar todo · Ctrl K" aria-label="Buscar en títulos y contenido"></label></div>
      <section class="tree-side-section"><div class="tree-side-heading"><span>Espacio</span></div><div class="tree-tree" data-tree-regular></div></section>
      <section class="tree-side-section"><div class="tree-side-heading"><span>Favoritos</span></div><div class="tree-tree" data-tree-favorites></div></section>
      <section class="tree-side-section"><div class="tree-side-heading"><span>Privado</span><button type="button" data-tree-create-private aria-label="Crear carpeta privada" title="Crear carpeta privada">+</button></div><div class="tree-tree" data-tree-private></div><div class="tree-private-hint" data-tree-private-empty hidden>Las carpetas privadas aparecen solo aquí.</div></section>
      </div>
      <footer class="tree-sidebar-footer"><span>Tu espacio de estudio</span><span class="tree-save-status" aria-live="polite">Guardado</span><button class="tree-icon-button" type="button" data-tree-create-root aria-label="Crear carpeta" title="Crear carpeta"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg></button></footer>
    </aside>
    <button class="tree-mobile-overlay" type="button" data-tree-close-sidebar aria-label="Cerrar navegación"></button>
    <main class="tree-main">
      <header class="tree-mobile-top"><button class="tree-icon-button" type="button" data-tree-toggle-sidebar aria-label="Abrir árbol de páginas"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M4 6h16M4 12h16M4 18h16"/></svg></button><strong data-tree-mobile-title>Tu cuaderno en un solo lugar</strong><button class="tree-icon-button" type="button" data-tree-create-current aria-label="Crear página"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg></button></header>
      <div class="tree-view" data-tree-view></div>
    </main>
    <div class="tree-toolbar" data-tree-toolbar>
      <button class="tree-tool-btn" type="button" data-tool-modes aria-label="Modos de vista" aria-expanded="false" title="Modos de vista"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="3" y="4" width="18" height="13" rx="2"/><path d="M8 21h8M12 17v4"/></svg></button>
      <button class="tree-tool-btn" type="button" data-tool-fx aria-label="Efectos de fondo" aria-expanded="false" title="Efectos de fondo y luces"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M12 3l1.8 4.7 4.7 1.8-4.7 1.8L12 16l-1.8-4.7L5.5 9.5l4.7-1.8z"/><path d="M19 15l.8 2.2 2.2.8-2.2.8L19 21l-.8-2.2L16 18l2.2-.8z"/></svg></button>
      <button class="tree-tool-btn" type="button" data-tool-tx aria-label="Animaciones de texto" aria-expanded="false" title="Animaciones de texto"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 19L9 5l5 14M6 14h6"/><path d="M19 3v5M16.5 5.5h5"/></svg></button>
      <button class="tree-tool-btn" type="button" data-tool-image aria-label="Insertar imagen" title="Insertar imagen"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="3" y="4" width="18" height="16" rx="2"/><circle cx="9" cy="10" r="1.8"/><path d="m21 16-5-5-8 8"/></svg></button>
      <input type="file" accept="image/*" hidden data-tree-image-file>
      <button class="tree-tool-btn" type="button" data-tool-fav data-tree-favorite="" aria-label="Favorito" aria-pressed="false" title="Favorito"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linejoin="round"><path d="M12 3.5l2.6 5.4 5.9.8-4.3 4.1 1 5.9L12 16.9l-5.2 2.8 1-5.9L3.5 9.7l5.9-.8z"/></svg></button>
      <button class="tree-tool-btn" type="button" data-tool-theme aria-label="Cambiar entre modo claro y oscuro" title="Modo claro / oscuro"><svg class="tree-icon-moon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round"><path d="M20 14.5A8 8 0 019.5 4 8 8 0 1020 14.5z"/></svg><svg class="tree-icon-sun" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><circle cx="12" cy="12" r="4"/><path d="M12 2.5v2M12 19.5v2M2.5 12h2M19.5 12h2M5.3 5.3l1.4 1.4M17.3 17.3l1.4 1.4M5.3 18.7l1.4-1.4M17.3 6.7l1.4-1.4"/></svg></button>
      <button class="tree-tool-btn" type="button" data-tool-settings aria-label="Configuración de la página" aria-expanded="false" title="Configuración"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.7 1.7 0 00.3 1.8l.1.1a2 2 0 11-2.8 2.8l-.1-.1a1.7 1.7 0 00-1.8-.3 1.7 1.7 0 00-1 1.5V21a2 2 0 11-4 0v-.1a1.7 1.7 0 00-1.1-1.5 1.7 1.7 0 00-1.8.3l-.1.1a2 2 0 11-2.8-2.8l.1-.1a1.7 1.7 0 00.3-1.8 1.7 1.7 0 00-1.5-1H3a2 2 0 110-4h.1a1.7 1.7 0 001.5-1.1 1.7 1.7 0 00-.3-1.8l-.1-.1a2 2 0 112.8-2.8l.1.1a1.7 1.7 0 001.8.3H9a1.7 1.7 0 001-1.5V3a2 2 0 114 0v.1a1.7 1.7 0 001 1.5 1.7 1.7 0 001.8-.3l.1-.1a2 2 0 112.8 2.8l-.1.1a1.7 1.7 0 00-.3 1.8V9a1.7 1.7 0 001.5 1H21a2 2 0 110 4h-.1a1.7 1.7 0 00-1.5 1z"/></svg></button>
      <section class="tree-settings tree-fxpanel" data-tree-fx hidden aria-label="Efectos de fondo">
        <h3>Efectos de fondo</h3>
        <div class="tree-set-group">
          <div class="tree-fx-grid" role="radiogroup" aria-label="Efecto de fondo">
            <button class="tree-set-btn" type="button" data-fx-opt="none">🚫 Ninguno</button>
            <button class="tree-set-btn" type="button" data-fx-opt="shine">✨ Brillo</button>
            <button class="tree-set-btn" type="button" data-fx-opt="aurora">🌌 Aurora</button>
            <button class="tree-set-btn" type="button" data-fx-opt="rainbow">🌈 Arcoíris</button>
            <button class="tree-set-btn" type="button" data-fx-opt="party">🎉 Fiesta</button>
            <button class="tree-set-btn" type="button" data-fx-opt="disco">🪩 Disco</button>
            <button class="tree-set-btn" type="button" data-fx-opt="stars">⭐ Estrellas</button>
            <button class="tree-set-btn" type="button" data-fx-opt="frame">💡 Marco neón</button>
            <button class="tree-set-btn" type="button" data-fx-opt="confetti">🎊 Confeti</button>
          </div>
        </div>
        <div class="tree-set-group">
          <label class="tree-set-slider"><span>Intensidad <output data-fx-out-i>70%</output></span><input type="range" min="10" max="100" step="1" value="70" data-fx-i aria-label="Intensidad del efecto"></label>
          <label class="tree-set-slider"><span>Velocidad <output data-fx-out-s>1x</output></span><input type="range" min="0.1" max="3" step="0.1" value="1" data-fx-s aria-label="Velocidad del efecto"></label>
          <p class="tree-set-hint">Se guarda en cada página. Los efectos con luces que parpadean pueden molestar a personas sensibles a las luces; bajá la velocidad o elegí “Ninguno” si es tu caso.</p>
        </div>
      </section>
      <section class="tree-settings tree-fxpanel" data-tree-tx hidden aria-label="Animaciones de texto">
        <h3>Animaciones de texto</h3>
        <div class="tree-set-group">
          <div class="tree-fx-grid" role="radiogroup" aria-label="Animación de texto">
            <button class="tree-set-btn" type="button" data-tx-opt="none">🚫 Ninguna</button>
            <button class="tree-set-btn" type="button" data-tx-opt="neon">💡 Neón</button>
            <button class="tree-set-btn" type="button" data-tx-opt="lights">🔦 Prender y apagar</button>
            <button class="tree-set-btn" type="button" data-tx-opt="fade">👻 Aparece y desaparece</button>
            <button class="tree-set-btn" type="button" data-tx-opt="rainbow">🌈 Arcoíris</button>
            <button class="tree-set-btn" type="button" data-tx-opt="party">🎉 Colores fiesta</button>
            <button class="tree-set-btn" type="button" data-tx-opt="gold">🏆 Brillo dorado</button>
            <button class="tree-set-btn" type="button" data-tx-opt="wave">🌊 Ondas</button>
            <button class="tree-set-btn" type="button" data-tx-opt="glitch">📺 Glitch</button>
            <button class="tree-set-btn" type="button" data-tx-opt="pulse">💓 Latido</button>
          </div>
        </div>
        <div class="tree-set-group">
          <label class="tree-set-color"><span>Color del brillo</span><input type="color" value="#00e5ff" data-tx-color aria-label="Color del brillo del texto"></label>
          <label class="tree-set-slider"><span>Velocidad <output data-tx-out-s>1x</output></span><input type="range" min="0.1" max="3" step="0.1" value="1" data-tx-s aria-label="Velocidad de la animación del texto"></label>
          <p class="tree-set-hint">El bloque que estás escribiendo se queda quieto para que se lea bien. Se guarda en cada página.</p>
        </div>
      </section>
      <section class="tree-settings tree-modes" data-tree-modes hidden aria-label="Modos de vista">
        <h3>Modo de vista</h3>
        <p class="tree-set-hint">Elige cómo quieres ver tus apuntes. Se recuerda en este navegador.</p>
        <div class="tree-mode-list" role="radiogroup" aria-label="Modos de vista">
          <button class="tree-mode-opt" type="button" role="radio" data-mode="default"><span class="tree-mode-sw" style="--c1:#191919;--c2:#8a9cff"></span><span><b>Clásico</b><small>Claro u oscuro, como siempre</small></span></button>
          <button class="tree-mode-opt" type="button" role="radio" data-mode="netflix"><span class="tree-mode-sw" style="--c1:#141414;--c2:#e50914"></span><span><b>Netflix</b><small>Negro cine con acento rojo, para maratones de estudio</small></span></button>
          <button class="tree-mode-opt" type="button" role="radio" data-mode="enfoque"><span class="tree-mode-sw" style="--c1:#f4ecd8;--c2:#a0522d"></span><span><b>Lectura</b><small>Tono papel sepia, menos fatiga visual</small></span></button>
          <button class="tree-mode-opt" type="button" role="radio" data-mode="biblioteca"><span class="tree-mode-sw" style="--c1:#13201b;--c2:#d4af37"></span><span><b>Biblioteca</b><small>Verde profundo y dorado, ambiente académico</small></span></button>
          <button class="tree-mode-opt" type="button" role="radio" data-mode="pastel"><span class="tree-mode-sw" style="--c1:#fdf3f7;--c2:#9b7ede"></span><span><b>Pastel</b><small>Colores suaves y amables, ideal para repasar</small></span></button>
          <button class="tree-mode-opt" type="button" role="radio" data-mode="medianoche"><span class="tree-mode-sw" style="--c1:#0b1020;--c2:#38bdf8"></span><span><b>Medianoche</b><small>Azul profundo para estudiar de madrugada</small></span></button>
          <button class="tree-mode-opt" type="button" role="radio" data-mode="contraste"><span class="tree-mode-sw" style="--c1:#fff;--c2:#000"></span><span><b>Alto contraste</b><small>Blanco y negro nítido, máxima legibilidad</small></span></button>
        </div>
      </section>
      <section class="tree-settings" data-tree-settings hidden aria-label="Configuración de la página">
        <h3>Personalizar página</h3>
        <div class="tree-set-group">
          <span class="tree-set-label">Fondo de pantalla</span>
          <div class="tree-set-row"><button class="tree-set-btn" type="button" data-set-pick>Elegir imagen…</button><button class="tree-set-btn" type="button" data-set-remove style="flex:0 0 auto">Quitar</button></div>
          <input type="file" accept="image/*" hidden data-set-file>
          <p class="tree-set-hint" data-set-hint>Las imágenes (JPG, PNG…) se convierten a WebP automáticamente.</p>
        </div>
        <div class="tree-set-group">
          <span class="tree-set-label">Panel del texto</span>
          <div class="tree-set-presets"><button class="tree-set-btn" type="button" data-set-preset="clear">Transparente</button><button class="tree-set-btn" type="button" data-set-preset="glass">Vidrio</button><button class="tree-set-btn" type="button" data-set-preset="solid">Sólido</button></div>
          <label class="tree-set-slider"><span>Opacidad <output data-set-out-a>0%</output></span><input type="range" min="0" max="100" step="1" value="0" data-set-a></label>
          <label class="tree-set-slider"><span>Desenfoque (vidrio) <output data-set-out-b>0px</output></span><input type="range" min="0" max="30" step="1" value="0" data-set-b></label>
        </div>
        <div class="tree-set-group">
          <span class="tree-set-label">Texto de la página</span>
          <label class="tree-set-slider"><span>Tipografía</span><select class="tree-set-select" data-set-font aria-label="Tipografía">
            <option value="default">Predeterminada</option>
            <option value="sans">Sans moderna</option>
            <option value="serif">Serif clásica (Georgia)</option>
            <option value="libro">Libro (Palatino)</option>
            <option value="redonda">Redondeada (Trebuchet)</option>
            <option value="mono">Máquina de escribir</option>
            <option value="manuscrita">Manuscrita</option>
          </select></label>
          <label class="tree-set-slider"><span>Tamaño <output data-set-out-s>16px</output></span><input type="range" min="12" max="30" step="1" value="16" data-set-size aria-label="Tamaño del texto"></label>
          <button class="tree-set-btn" type="button" data-set-text-reset>Restablecer texto</button>
        </div>
        <div class="tree-set-group"><button class="tree-set-btn" type="button" data-tool-icon data-tree-change-icon="">Cambiar ícono de la página (barra lateral)…</button></div>
      </section>
    </div>
    <div class="tree-slash-menu" data-tree-slash-menu hidden role="listbox" aria-label="Insertar bloque"></div>
  </div>
  <script>
    const STORAGE_KEY = "nido-study-folders-v1";
    const SECTIONS_STORAGE_KEY = "nido-content-sections-v1";
    const THEME_KEY = "nido-theme";
    const CORRECTOR_KEY = "nido-languagetool-enabled";
    const EDITOR_PANEL_KEY = "tus-carpetas-editor-panel-open-v1";
    const EDITOR_PANEL_SECTIONS_KEY = "tus-carpetas-editor-panel-sections-v1";
    const MENU_LATERAL_KEY = "tus-carpetas-menu-lateral-abierto-v1";
    const PAGINAS_VISTA_KEY = "nido-pages-view-v1";
    const GALERIA_TAMANO_KEY = "nido-gallery-card-size-v1";
    const HOME_TAB_KEY = "nido-home-tab-v1";
    const LEGACY_FOLDER_CLEANUP_KEY = "nido-requested-folder-cleanup-legacy-v1";
    const CARPETAS_ELIMINADAS_NOMBRES = new Set([
      "ciencias", "ingles", "literatura", "geografia", "arte",
      "programacion", "fisica", "musica", "biologica", "biologia", "ccys"
    ]);
    const normalizarNombreCarpeta = titulo => String(titulo || "").toLocaleLowerCase("es")
      .normalize("NFD").replace(/[\u0300-\u036f]/g, "").replace(/[^a-z0-9]/g, "");
    const esCarpetaSolicitadaEliminar = carpeta => CARPETAS_ELIMINADAS_NOMBRES.has(normalizarNombreCarpeta(carpeta?.titulo ?? carpeta?.title));
    const COLORES_CARPETA = ["#FFD93D", "#FF9F43", "#FF6B6B", "#FF8FB1", "#B79CFF", "#6EC6FF", "#5EE6B0", "#C6F135", "#2EC4B6", "#FFB38A", "#8AB4FF", "#7BE495"];
    const COLORES_PASTEL_CARPETA = new Set([
      "#dfe5df", "#e7ece8", "#e5ebef", "#eee8df", "#ebe5ef", "#e4ece9",
      "#eee9dc", "#f0e5e3", "#e4e8ed", "#e8e8f0", "#f0e9de", "#e3ece2",
      "#e9e9e6"
    ]);
    const COLORES_PAGINA = ["#E7ECE8", "#E5EBEF", "#EEE8DF", "#EBE5EF", "#E4ECE9", "#EEE9DC", "#F0E5E3", "#E4E8ED"];
    const carpetasIniciales = [
      { titulo: "Matemáticas", portada: "", color: COLORES_CARPETA[0], textoHover: "Abrir apuntes" },
      { titulo: "Ciencias", portada: "", color: COLORES_CARPETA[1], textoHover: "Explorar ciencias" },
      { titulo: "Historia", portada: "", color: COLORES_CARPETA[2], textoHover: "Abrir historia" },
      { titulo: "Literatura", portada: "", color: COLORES_CARPETA[3], textoHover: "Leer apuntes" },
      { titulo: "Inglés", portada: "", color: COLORES_CARPETA[4], textoHover: "Practicar inglés" },
      { titulo: "Geografía", portada: "", color: COLORES_CARPETA[5], textoHover: "Explorar mapas" },
      { titulo: "Arte", portada: "", color: COLORES_CARPETA[6], textoHover: "Ver carpeta" },
      { titulo: "Programación", portada: "", color: COLORES_CARPETA[7], textoHover: "Abrir apuntes" },
      { titulo: "Física", portada: "", color: COLORES_CARPETA[8], textoHover: "Abrir apuntes" },
      { titulo: "Música", portada: "", color: COLORES_CARPETA[9], textoHover: "Escuchar y aprender" },
      { titulo: "Biología", portada: "", color: COLORES_CARPETA[10], textoHover: "Explorar biología" },
      { titulo: "Apuntes", portada: "", color: COLORES_CARPETA[11], textoHover: "Abrir apuntes" }
    ].map((carpeta, index) => ({
      id: `materia-${index + 1}`,
      ...carpeta,
      seccionId: "mis-carpetas",
      materias: [],
      paginas: [],
      enlaces: []
    })).filter(carpeta => !esCarpetaSolicitadaEliminar(carpeta));
    const seccionesIniciales = [
      { id: "mis-carpetas", titulo: "Mis carpetas", tipo: "carpetas" },
      { id: "mis-juegos", titulo: "Mis juegos", tipo: "juegos" },
      { id: "mis-cuadernos-notebooklm", titulo: "Mis cuadernos de NotebookLM", tipo: "notebooklm" }
    ];

    function normalizarPaginasGuardadas(paginasGuardadas, marcarMigracion) {
      return paginasGuardadas.map((pagina, index) => {
        const colorValido = typeof pagina.color === "string" && /^#[0-9a-f]{6}$/i.test(pagina.color);
        const portadaValida = typeof pagina.portada === "string" ? pagina.portada : "";
        const tituloAnterior = typeof pagina.titulo === "string" ? pagina.titulo : "";
        const cardTitle = typeof pagina.cardTitle === "string" ? pagina.cardTitle : tituloAnterior;
        const pageTitle = typeof pagina.pageTitle === "string" ? pagina.pageTitle : tituloAnterior;
        const cuatrimestre = ["primer-cuatrimestre", "segundo-cuatrimestre"].includes(pagina.cuatrimestre) ? pagina.cuatrimestre : "";
        const unidadGuardada = typeof pagina.unidad === "string" ? pagina.unidad.trim() : "";
        const unidadCoincidente = unidadGuardada.match(/^unidad\s*([1-4])$/i);
        const unidad = unidadCoincidente ? `Unidad ${unidadCoincidente[1]}` : "";
        const pdfValido = pagina.pdf && typeof pagina.pdf === "object" &&
          typeof pagina.pdf.key === "string" && /^[a-z0-9-]+$/i.test(pagina.pdf.key);
        if (!colorValido || typeof pagina.portada !== "string" ||
          typeof pagina.cardTitle !== "string" || typeof pagina.pageTitle !== "string" ||
          (pagina.cuatrimestre !== undefined && pagina.cuatrimestre !== cuatrimestre) ||
          (pagina.unidad !== undefined && pagina.unidad !== unidad) ||
          (pagina.pdf && !pdfValido)) marcarMigracion();
        return {
          ...pagina,
          id: pagina.id,
          titulo: cardTitle,
          cardTitle,
          pageTitle,
          contenido: typeof pagina.contenido === "string" ? pagina.contenido : "",
          actualizado: Number.isFinite(pagina.actualizado) ? pagina.actualizado : Date.now(),
          portada: portadaValida,
          color: colorValido ? pagina.color : COLORES_PAGINA[index % COLORES_PAGINA.length],
          cuatrimestre,
          unidad,
          pdf: pdfValido ? {
            key: pagina.pdf.key,
            title: typeof pagina.pdf.title === "string" ? pagina.pdf.title : "",
            author: typeof pagina.pdf.author === "string" ? pagina.pdf.author : "",
            fileName: typeof pagina.pdf.fileName === "string" ? pagina.pdf.fileName : "material.pdf"
          } : null
        };
      });
    }

    function cargarCarpetas() {
      try {
        const limpiezaPendiente = localStorage.getItem(LEGACY_FOLDER_CLEANUP_KEY) !== "done";
        const guardadas = localStorage.getItem(STORAGE_KEY);
        if (!guardadas) {
          if (limpiezaPendiente) localStorage.setItem(LEGACY_FOLDER_CLEANUP_KEY, "done");
          return carpetasIniciales;
        }
        const resultado = JSON.parse(guardadas);
        if (!Array.isArray(resultado) || resultado.some(carpeta => !carpeta || typeof carpeta.id !== "string" || typeof carpeta.titulo !== "string")) {
          throw new Error("El formato de las carpetas guardadas no es válido.");
        }
        let migracionRealizada = false;
        resultado.forEach(carpeta => {
          if (typeof carpeta.portada !== "string" || !carpeta.portada.includes("placehold.co")) return;
          const colorPlacehold = carpeta.portada.match(/\/\d+x\d+\/([0-9a-f]{6})\//i);
          if (colorPlacehold) carpeta.color = `#${colorPlacehold[1].toLowerCase()}`;
          carpeta.portada = "";
          migracionRealizada = true;
        });
        const carpetasNormalizadas = resultado.map((carpeta, index) => {
          const colorCarpetaValido = typeof carpeta.color === "string" && /^#[0-9a-f]{6}$/i.test(carpeta.color);
          const colorCarpetaAntiguo = colorCarpetaValido && COLORES_PASTEL_CARPETA.has(carpeta.color.toLowerCase());
          const colorCarpeta = colorCarpetaValido && !colorCarpetaAntiguo
            ? carpeta.color
            : COLORES_CARPETA[index % COLORES_CARPETA.length];
          if (!colorCarpetaValido || colorCarpetaAntiguo) migracionRealizada = true;
          const paginasGuardadas = Array.isArray(carpeta.paginas)
            ? carpeta.paginas.filter(pagina => pagina && typeof pagina.id === "string")
            : [];
          const esCarpetaEstudio = !carpeta.seccionId || carpeta.seccionId === "mis-carpetas";
          const paginas = esCarpetaEstudio
            ? normalizarPaginasGuardadas(paginasGuardadas, () => { migracionRealizada = true; })
            : paginasGuardadas;
          const notaYaGuardadaEnMateria = Array.isArray(carpeta.materias) && carpeta.materias.some(materia =>
            Array.isArray(materia?.paginas) && materia.paginas.some(pagina =>
              (pagina?.cardTitle || pagina?.titulo) === "Notas" && pagina.contenido === carpeta.notas
            )
          );
          if (esCarpetaEstudio && typeof carpeta.notas === "string" && carpeta.notas.trim() &&
            !paginas.some(pagina => pagina.titulo === "Notas" && pagina.contenido === carpeta.notas) &&
            !notaYaGuardadaEnMateria) {
            paginas.unshift({
              id: makeId(),
              titulo: "Notas",
              cardTitle: "Notas",
              pageTitle: "Notas",
              contenido: carpeta.notas,
              actualizado: Date.now(),
              cuatrimestre: "",
              unidad: "",
              pdf: null
            });
            migracionRealizada = true;
          }
          const materiasGuardadas = Array.isArray(carpeta.materias) ? carpeta.materias : [];
          let materias = materiasGuardadas
            .filter(materia => materia && typeof materia.id === "string" && typeof materia.titulo === "string")
            .map((materia, materiaIndex) => {
              const materiasPaginas = Array.isArray(materia.paginas)
                ? materia.paginas.filter(pagina => pagina && typeof pagina.id === "string")
                : [];
              const colorMateriaValido = typeof materia.color === "string" && /^#[0-9a-f]{6}$/i.test(materia.color);
              if (!colorMateriaValido || !Array.isArray(materia.paginas)) migracionRealizada = true;
              return {
                ...materia,
                id: materia.id,
                titulo: materia.titulo,
                portada: typeof materia.portada === "string" ? materia.portada : "",
                color: colorMateriaValido ? materia.color : COLORES_CARPETA[materiaIndex % COLORES_CARPETA.length],
                paginas: normalizarPaginasGuardadas(materiasPaginas, () => { migracionRealizada = true; })
              };
            });
          if (esCarpetaEstudio && paginas.length) {
            let materiaApuntes = materias.find(materia => materia.titulo === "Mis apuntes");
            if (!materiaApuntes) {
              materiaApuntes = {
                id: makeId(),
                titulo: "Mis apuntes",
                portada: "",
                color: colorCarpeta,
                paginas: []
              };
              materias.push(materiaApuntes);
            }
            const idsExistentes = new Set(materias.flatMap(materia => materia.paginas.map(pagina => pagina.id)));
            paginas.forEach(pagina => {
              if (idsExistentes.has(pagina.id)) pagina.id = makeId();
              materiaApuntes.paginas.push(pagina);
              idsExistentes.add(pagina.id);
            });
            migracionRealizada = true;
          }
          return {
            ...carpeta,
            id: carpeta.id,
            titulo: carpeta.titulo,
            seccionId: typeof carpeta.seccionId === "string" ? carpeta.seccionId : "mis-carpetas",
            enlaceExterno: typeof carpeta.enlaceExterno === "string" ? carpeta.enlaceExterno : "",
            portada: typeof carpeta.portada === "string" ? carpeta.portada : "",
            textoHover: typeof carpeta.textoHover === "string" ? carpeta.textoHover : "Abrir carpeta",
            color: colorCarpeta,
            materias,
            paginas: esCarpetaEstudio ? [] : paginas,
            enlaces: Array.isArray(carpeta.enlaces) ? carpeta.enlaces.filter(enlace => enlace && typeof enlace.id === "string" && typeof enlace.titulo === "string" && typeof enlace.url === "string") : []
          };
        });
        const carpetasConservadas = limpiezaPendiente
          ? carpetasNormalizadas.filter(carpeta => !esCarpetaSolicitadaEliminar(carpeta))
          : carpetasNormalizadas;
        if (carpetasConservadas.length !== carpetasNormalizadas.length) migracionRealizada = true;
        if (migracionRealizada) {
          try {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(carpetasConservadas));
            if (limpiezaPendiente) localStorage.setItem(LEGACY_FOLDER_CLEANUP_KEY, "done");
          } catch (error) {
            console.error("No se pudieron guardar los cambios de la migración:", error);
          }
        } else if (limpiezaPendiente) {
          localStorage.setItem(LEGACY_FOLDER_CLEANUP_KEY, "done");
        }
        return carpetasConservadas;
      } catch (error) {
        console.error("No se pudieron cargar las carpetas guardadas:", error);
        return carpetasIniciales;
      }
    }

    const carpetas = cargarCarpetas();
    function cargarSecciones() {
      try {
        const guardadas = localStorage.getItem(SECTIONS_STORAGE_KEY);
        if (!guardadas) return seccionesIniciales.map(seccion => ({ ...seccion }));
        const secciones = JSON.parse(guardadas);
        if (!Array.isArray(secciones) || !secciones.length || secciones.some(seccion =>
          !seccion || typeof seccion.id !== "string" || typeof seccion.titulo !== "string" ||
          !["carpetas", "juegos", "notebooklm"].includes(seccion.tipo)
        )) throw new Error("El formato de las secciones guardadas no es válido.");
        return secciones;
      } catch (error) {
        console.error("No se pudieron cargar las secciones guardadas:", error);
        return seccionesIniciales.map(seccion => ({ ...seccion }));
      }
    }
    const secciones = cargarSecciones();
    let seccionRecuperada = false;
    carpetas.forEach(carpeta => {
      if (!secciones.some(section => section.id === carpeta.seccionId)) {
        carpeta.seccionId = secciones[0]?.id || "mis-carpetas";
        seccionRecuperada = true;
      }
    });
    const view = document.getElementById("view");
    const search = document.getElementById("search");
    const saveState = document.getElementById("save-state");
    const themeToggle = document.getElementById("theme-toggle");
    let modoVistaPaginas = (() => {
      try { return localStorage.getItem(PAGINAS_VISTA_KEY) === "lista" ? "lista" : "cuadricula"; }
      catch (error) { console.error("No se pudo leer la preferencia de vista de páginas:", error); return "cuadricula"; }
    })();
    let tamanosGaleriaPorSeccion = (() => {
      try {
        const guardados = JSON.parse(localStorage.getItem(GALERIA_TAMANO_KEY) || "{}");
        if (!guardados || typeof guardados !== "object" || Array.isArray(guardados)) return {};
        return Object.fromEntries(Object.entries(guardados).filter(([, size]) =>
          Number.isInteger(size) && size >= 140 && size <= 360
        ));
      } catch (error) {
        console.error("No se pudieron leer los tamaños de galería guardados:", error);
        return {};
      }
    })();
    if (seccionRecuperada) {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(carpetas));
      } catch (error) {
        console.error("No se pudieron reasignar los elementos de secciones eliminadas:", error);
        saveState.textContent = "No se pudo guardar";
      }
    }

    function cargarPreferenciaCorrector() {
      try {
        return localStorage.getItem(CORRECTOR_KEY) === "true";
      } catch (error) {
        console.error("No se pudo leer la preferencia de LanguageTool:", error);
        return false;
      }
    }

    function guardarPreferenciaCorrector(activo) {
      try {
        localStorage.setItem(CORRECTOR_KEY, String(activo));
        correctorLanguageToolActivo = activo;
        return true;
      } catch (error) {
        console.error("No se pudo guardar la preferencia de LanguageTool:", error);
        saveState.textContent = "No se pudo guardar la configuración";
        return false;
      }
    }
    let carpetaActualId = null;
    let materiaActivaId = null;
    let elementoGaleriaActivoId = null;
    let seccionActivaId = "mis-carpetas";
    let inicioDashboard = true;
    let contadorTarjetasInicio = 0;
    let homeGridView = false;
    let homeGridSort = "recent";
    let homeScrollListenerAttached = false;
    let homeHoverListenersAttached = false;
    let homeHoverCardState = null;
    let homeRenderedSectionId;
    let paginaActivaId = null;
    let vistaActual = "inicio";
    let controlesTamanoGaleriaAbiertos = false;
    let paginaActual = 1;
    let filtroTexto = "";
    let filtroPaginasTexto = "";
    const pestanaPaginasPorMateria = new Map();
    let vistaBusquedaAnterior = "inicio";
    let carpetaBusquedaAnteriorId = null;
    let materiaBusquedaAnteriorId = null;
    let paginaBusquedaAnteriorId = null;
    let elementoGaleriaBusquedaAnteriorId = null;
    let correctorLanguageToolActivo = cargarPreferenciaCorrector();
    let ultimoOrdenAnterior = null;
    let estadoArrastreOrden = null;
    let temporizadorGuardadoPagina = null;
    let temporizadorAbrirPagina = null;
    let temporizadorCorrector = null;
    let solicitudCorrector = null;
    let temporizadorOcultarSugerencia = null;
    let sugerenciaCorrectorActual = null;
    let sugerenciasCorrector = [];
    let rangoEditorGuardado = null;
    let imagenEditorActiva = null;
    let imagenEditorActivaId = "";
    let tablaEditorActivaId = "";
    let editorTablaPendiente = null;
    let marcadorCursorPendiente = false;
    let indiceComandoActivo = 0;
    let temporizadorCerrarComandos = null;
    let panelEditorAbierto = false;
    let menuLateralPreferidoAbierto = true;
    let menuLateralMovilAbierto = false;
    let menuLateralFlotante = false;
    let seccionesPanelEditor = { texto: true };
    let manejarDesplazamientoFondoEditor = null;
    let manejarCambioMovimientoReducidoFondo = null;
    let animacionFondoEditorPendiente = 0;
    let fondoCapaEditor = null;
    let fondoGifUrlEditor = null;
    let pdfVistaUrlEditor = null;
    let fondoCargaVersion = 0;
    const COMANDOS_EDITOR = [
      { id: "paragraph", group: "Sugeridos", shortcut: "", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M13 4v16"/><path d="M17 4v16"/><path d="M13 4h-3a4 4 0 0 0 0 8h3"/></svg>', label: "Texto", description: "Párrafo sin formato", keywords: "texto normal párrafo parrafo" },
      { id: "h1", group: "Sugeridos", shortcut: "#", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 5v14"/><path d="M12 5v14"/><path d="M3 10h10"/><path d="M3 14h10"/><path d="M19 8h2v8"/></svg>', label: "Título 1", description: "Título principal", keywords: "titulo título encabezado heading" },
      { id: "h2", group: "Bloques básicos", shortcut: "##", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 5v14"/><path d="M12 5v14"/><path d="M3 10h10"/><path d="M3 14h10"/><path d="M17 10h4a2 2 0 0 1 1 3.7L17 18h5"/></svg>', label: "Título 2", description: "Subtítulo", keywords: "titulo título subtitulo subtítulo encabezado heading" },
      { id: "h3", group: "Bloques básicos", shortcut: "###", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 5v14"/><path d="M12 5v14"/><path d="M3 10h10"/><path d="M3 14h10"/><path d="M17 10h3a2 2 0 0 1 0 4h-3"/><path d="M20 14a2 2 0 0 1 0 4h-3"/></svg>', label: "Título 3", description: "Subtítulo pequeño", keywords: "titulo título subtitulo subtítulo pequeño encabezado heading" },
      { id: "ul", group: "Bloques básicos", shortcut: "-", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M9 6h11"/><path d="M9 12h11"/><path d="M9 18h11"/><path d="M4 6h.01"/><path d="M4 12h.01"/><path d="M4 18h.01"/></svg>', label: "Lista con viñetas", description: "Lista sin ordenar", keywords: "lista viñetas puntos bullet" },
      { id: "ol", group: "Bloques básicos", shortcut: "1.", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M10 6h10"/><path d="M10 12h10"/><path d="M10 18h10"/><path d="M4 5h2v4"/><path d="M4 9h3"/><path d="M4 14h3l-3 4h3"/></svg>', label: "Lista numerada", description: "Lista ordenada", keywords: "lista numerada orden" },
      { id: "task", group: "Bloques básicos", shortcut: "[]", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><rect x="3" y="4" width="6" height="6" rx="1"/><path d="m4.5 7 1.3 1.3L8 5.8"/><path d="M13 7h8"/><rect x="3" y="14" width="6" height="6" rx="1"/><path d="M13 17h8"/></svg>', label: "Lista de tareas", description: "Casillas para marcar", keywords: "lista tareas tarea checklist casilla" },
      { id: "blockquote", group: "Bloques básicos", shortcut: ">", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 6h16"/><path d="M4 12h10"/><path d="M4 18h16"/><path d="m17 10 3 2-3 2"/></svg>', label: "Cita", description: "Bloque de cita", keywords: "cita quote" },
      { id: "hr", group: "Bloques básicos", shortcut: "---", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M4 12h16"/></svg>', label: "Divisor", description: "Línea divisoria", keywords: "divisor separador linea línea horizontal" },
      { id: "code-block", group: "Bloques básicos", shortcut: "```", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><path d="m8 8-4 4 4 4"/><path d="m16 8 4 4-4 4"/><path d="m14 5-4 14"/></svg>', label: "Bloque de código", description: "Código en formato monoespaciado", keywords: "codigo código programación pre" },
      { id: "image", group: "Multimedia", shortcut: "", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><path d="m21 15-5-5L5 21"/></svg>', label: "Imagen", description: "Insertar una imagen", keywords: "imagen foto" },
      { id: "table", group: "Multimedia", shortcut: "", icon: '<svg viewBox="0 0 24 24" aria-hidden="true"><rect x="3" y="4" width="18" height="16" rx="2"/><path d="M3 10h18"/><path d="M3 15h18"/><path d="M9 4v16"/><path d="M15 4v16"/></svg>', label: "Tabla", description: "Insertar una tabla", keywords: "tabla cuadrícula grilla" }
    ];
    let baseDatosImagenes = null;

    function cargarPreferenciasPanelEditor() {
      try {
        panelEditorAbierto = localStorage.getItem(EDITOR_PANEL_KEY) === "true";
        const guardadas = JSON.parse(localStorage.getItem(EDITOR_PANEL_SECTIONS_KEY) || "{}");
        seccionesPanelEditor = {
          ...guardadas,
          texto: typeof guardadas.texto === "boolean" ? guardadas.texto : true,
          pagina: typeof guardadas.pagina === "boolean" ? guardadas.pagina : false
        };
      } catch (error) {
        console.error("No se pudieron leer las preferencias del panel del editor:", error);
        panelEditorAbierto = false;
        seccionesPanelEditor = { texto: true, pagina: false };
      }
    }

    function guardarPreferenciasPanelEditor() {
      try {
        localStorage.setItem(EDITOR_PANEL_KEY, String(panelEditorAbierto));
        localStorage.setItem(EDITOR_PANEL_SECTIONS_KEY, JSON.stringify(seccionesPanelEditor));
      } catch (error) {
        console.error("No se pudieron guardar las preferencias del panel del editor:", error);
        saveState.textContent = "No se pudo guardar la vista del panel";
      }
    }

    function actualizarPanelEdicion() {
      const layout = document.getElementById("editor-disposicion");
      const panel = document.getElementById("editor-panel");
      const toggle = document.getElementById("editor-panel-toggle");
      const readingPanel = document.getElementById("lectura-panel");
      if (!layout || !panel || !toggle || !readingPanel) return;
      layout.classList.toggle("panel-abierto", panelEditorAbierto);
      readingPanel.classList.toggle("lectura-panel-con-panel", panelEditorAbierto);
      toggle.setAttribute("aria-expanded", String(panelEditorAbierto));
      panel.setAttribute("aria-hidden", String(!panelEditorAbierto));
      panel.inert = !panelEditorAbierto;
      guardarPreferenciasPanelEditor();
    }

    function configurarPanelEdicion() {
      cargarPreferenciasPanelEditor();
      actualizarPanelEdicion();
      document.querySelector(".pagina-editor").addEventListener("keydown", event => {
        if (event.key !== "Escape" || !panelEditorAbierto) return;
        event.preventDefault();
        panelEditorAbierto = false;
        actualizarPanelEdicion();
        document.getElementById("editor-panel-toggle").focus();
      });
      document.getElementById("editor-panel-toggle").addEventListener("click", () => {
        panelEditorAbierto = !panelEditorAbierto;
        actualizarPanelEdicion();
      });
      document.getElementById("editor-panel-cerrar").addEventListener("click", () => {
        panelEditorAbierto = false;
        actualizarPanelEdicion();
        document.getElementById("editor-panel-toggle").focus();
      });
      document.querySelectorAll("[data-panel-acordeon]").forEach(button => {
        const key = button.dataset.panelAcordeon;
        const open = typeof seccionesPanelEditor[key] === "boolean" ? seccionesPanelEditor[key] : true;
        button.setAttribute("aria-expanded", String(open));
        document.getElementById(button.getAttribute("aria-controls")).hidden = !open;
        button.addEventListener("click", () => {
          const next = button.getAttribute("aria-expanded") !== "true";
          button.setAttribute("aria-expanded", String(next));
          document.getElementById(button.getAttribute("aria-controls")).hidden = !next;
          seccionesPanelEditor[key] = next;
          guardarPreferenciasPanelEditor();
        });
      });
    }

    function abrirBaseDatosImagenes() {
      if (baseDatosImagenes) return baseDatosImagenes;
      baseDatosImagenes = new Promise((resolve, reject) => {
        if (!("indexedDB" in window)) {
          reject(new Error("Este navegador no ofrece IndexedDB para guardar imágenes."));
          return;
        }
        const solicitud = indexedDB.open("carpeta-estudio-imagenes-v1", 1);
        solicitud.onupgradeneeded = () => {
          if (!solicitud.result.objectStoreNames.contains("imagenes")) solicitud.result.createObjectStore("imagenes");
        };
        solicitud.onsuccess = () => resolve(solicitud.result);
        solicitud.onerror = () => reject(solicitud.error || new Error("No se pudo abrir el almacén de imágenes."));
      });
      return baseDatosImagenes;
    }

    async function guardarImagenEnBaseDatos(key, blob) {
      const db = await abrirBaseDatosImagenes();
      await new Promise((resolve, reject) => {
        const transaction = db.transaction("imagenes", "readwrite");
        transaction.objectStore("imagenes").put(blob, key);
        transaction.oncomplete = resolve;
        transaction.onerror = () => reject(transaction.error || new Error("No se pudo guardar la imagen."));
        transaction.onabort = () => reject(transaction.error || new Error("Se canceló el guardado de la imagen."));
      });
    }

    async function leerImagenDeBaseDatos(key) {
      const db = await abrirBaseDatosImagenes();
      return new Promise((resolve, reject) => {
        const request = db.transaction("imagenes", "readonly").objectStore("imagenes").get(key);
        request.onsuccess = () => resolve(request.result || null);
        request.onerror = () => reject(request.error || new Error("No se pudo leer una imagen guardada."));
      });
    }

    async function limpiarImagenesHuerfanas() {
      if (!baseDatosImagenes) return;
      const referencias = new Set();
      const parser = new DOMParser();
      carpetas.forEach(carpeta => [
        ...todasLasPaginas(carpeta),
        ...(Array.isArray(carpeta.paginas) ? carpeta.paginas : []),
        ...(esElementoGaleria(carpeta) ? [carpeta] : [])
      ].forEach(pagina => {
        if (/^[a-z0-9-]+$/i.test(pagina.fondoGifKey || "")) referencias.add(pagina.fondoGifKey);
        if (/^[a-z0-9-]+$/i.test(pagina.pdf?.key || "")) referencias.add(pagina.pdf.key);
        const documento = parser.parseFromString(pagina.contenido || "", "text/html");
        documento.querySelectorAll("img[data-image-key]").forEach(image => {
          const key = image.dataset.imageKey;
          if (/^[a-z0-9-]+$/i.test(key || "")) referencias.add(key);
        });
      }));
      const db = await abrirBaseDatosImagenes();
      await new Promise((resolve, reject) => {
        const transaction = db.transaction("imagenes", "readwrite");
        const store = transaction.objectStore("imagenes");
        const cursorRequest = store.openCursor();
        cursorRequest.onsuccess = () => {
          const cursor = cursorRequest.result;
          if (!cursor) return;
          if (!referencias.has(String(cursor.key))) cursor.delete();
          cursor.continue();
        };
        cursorRequest.onerror = () => reject(cursorRequest.error || new Error("No se pudieron revisar las imágenes guardadas."));
        transaction.oncomplete = resolve;
        transaction.onerror = () => reject(transaction.error || new Error("No se pudieron limpiar las imágenes antiguas."));
        transaction.onabort = () => reject(transaction.error || new Error("Se canceló la limpieza de imágenes."));
      });
    }

    function guardarCarpetas() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(carpetas));
        saveState.textContent = "Guardado";
        window.setTimeout(() => { if (saveState.textContent === "Guardado") saveState.textContent = ""; }, 1600);
        if (baseDatosImagenes) {
          limpiarImagenesHuerfanas().catch(error => {
            console.error("No se pudieron limpiar las imágenes que ya no se usan:", error);
            saveState.textContent = "No se pudieron liberar imágenes";
          });
        }
        return true;
      } catch (error) {
        console.error("No se pudieron guardar las carpetas:", error);
        saveState.textContent = "No se pudo guardar";
        window.alert("No se pudieron guardar los cambios. Puede que el almacenamiento del navegador esté lleno.");
        return false;
      }
    }

    function guardarSecciones() {
      try {
        localStorage.setItem(SECTIONS_STORAGE_KEY, JSON.stringify(secciones));
        saveState.textContent = "Guardado";
        window.setTimeout(() => { if (saveState.textContent === "Guardado") saveState.textContent = ""; }, 1600);
        return true;
      } catch (error) {
        console.error("No se pudieron guardar las secciones:", error);
        saveState.textContent = "No se pudo guardar";
        window.alert("No se pudieron guardar las secciones. Puede que el almacenamiento del navegador esté lleno.");
        return false;
      }
    }

    function escapeHtml(value) {
      return String(value).replace(/[&<>"']/g, character => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", "\"": "&quot;", "'": "&#39;" })[character]);
    }

    function tituloTarjetaPagina(pagina) {
      return pagina.cardTitle || pagina.titulo || "Sin título";
    }

    function materiasDeCarpeta(carpeta) {
      return Array.isArray(carpeta?.materias) ? carpeta.materias : [];
    }

    function esCarpetaDeEstudio(carpeta) {
      return Boolean(carpeta && (!carpeta.seccionId || carpeta.seccionId === "mis-carpetas"));
    }

    function esElementoGaleria(carpeta) {
      if (!carpeta || esCarpetaDeEstudio(carpeta)) return false;
      const section = secciones.find(item => item.id === carpeta.seccionId);
      return Boolean(section && section.tipo !== "notebooklm");
    }

    function tamanoGaleria(sectionId) {
      return tamanosGaleriaPorSeccion[sectionId] || 240;
    }

    function guardarTamanoGaleria(sectionId, size) {
      const anterior = tamanosGaleriaPorSeccion[sectionId];
      tamanosGaleriaPorSeccion[sectionId] = size;
      try {
        localStorage.setItem(GALERIA_TAMANO_KEY, JSON.stringify(tamanosGaleriaPorSeccion));
        return true;
      } catch (error) {
        if (anterior === undefined) delete tamanosGaleriaPorSeccion[sectionId];
        else tamanosGaleriaPorSeccion[sectionId] = anterior;
        console.error("No se pudo guardar el tamaño de la galería:", error);
        saveState.textContent = "No se pudo guardar el tamaño de las tarjetas";
        return false;
      }
    }

    function paginasDeMateria(materia) {
      return Array.isArray(materia?.paginas) ? materia.paginas : [];
    }

    function encontrarPagina(carpeta, paginaId, materiaId = materiaActivaId) {
      const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaId);
      return paginasDeMateria(materia).find(pagina => pagina.id === paginaId);
    }

    function paginaActualAbierta() {
      if (elementoGaleriaActivoId) {
        const elemento = carpetas.find(item => item.id === elementoGaleriaActivoId);
        return esElementoGaleria(elemento) ? elemento : null;
      }
      return encontrarPagina(carpetas.find(item => item.id === carpetaActualId), paginaActivaId);
    }

    function abrirElementoGaleria(elementoId) {
      const elemento = carpetas.find(item => item.id === elementoId);
      if (!esElementoGaleria(elemento)) return;
      elemento.cardTitle = typeof elemento.cardTitle === "string" ? elemento.cardTitle : elemento.titulo;
      elemento.pageTitle = typeof elemento.pageTitle === "string" ? elemento.pageTitle : elemento.titulo;
      elemento.contenido = typeof elemento.contenido === "string" ? elemento.contenido : "";
      if (!["html", "texto"].includes(elemento.contenidoFormato)) elemento.contenidoFormato = "texto";
      elemento.actualizado = Number.isFinite(elemento.actualizado) ? elemento.actualizado : Date.now();
      elemento.pdf = elemento.pdf && typeof elemento.pdf === "object" ? elemento.pdf : null;
      abrirPagina(elemento.id, elemento.id, null, elemento);
    }

    function todasLasPaginas(carpeta) {
      return materiasDeCarpeta(carpeta).flatMap(materia => paginasDeMateria(materia));
    }

    function makeId() {
      return window.crypto && typeof window.crypto.randomUUID === "function" ? window.crypto.randomUUID() : `carpeta-${Date.now()}-${Math.random().toString(16).slice(2)}`;
    }

    function setTheme(theme) {
      const oscuro = theme === "dark";
      document.documentElement.dataset.theme = oscuro ? "dark" : "light";
      document.querySelector('meta[name="theme-color"]').content = getComputedStyle(document.documentElement).getPropertyValue("--canvas").trim();
      themeToggle.setAttribute("aria-label", oscuro ? "Activar modo claro" : "Activar modo oscuro");
      themeToggle.title = oscuro ? "Activar modo claro" : "Activar modo oscuro";
      themeToggle.innerHTML = oscuro
        ? '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><circle cx="12" cy="12" r="4"/><path d="M12 2v2m0 16v2M4.93 4.93l1.42 1.42m11.3 11.3 1.42 1.42M2 12h2m16 0h2M4.93 19.07l1.42-1.42m11.3-11.3 1.42-1.42"/></svg>'
        : '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M20.9 13A9 9 0 0 1 11 3.1 9 9 0 1 0 20.9 13Z"/></svg>';
    }

    try { setTheme(localStorage.getItem(THEME_KEY) === "dark" ? "dark" : "light"); }
    catch (error) { console.error("No se pudo leer el tema guardado:", error); setTheme("light"); }

    function actualizarNavegacion() {
      const app = document.querySelector(".app");
      app?.classList.toggle("home-mode", vistaActual === "inicio");
      app?.classList.toggle("dashboard-mode", vistaActual === "inicio" && inicioDashboard);
      const home = document.getElementById("home-link");
      const settings = document.getElementById("settings-link");
      home.classList.toggle("active", vistaActual === "inicio" && inicioDashboard);
      document.getElementById("materias-link")?.classList.toggle("active", vistaActual !== "configuracion" && !inicioDashboard && seccionActivaId === "mis-carpetas");
      settings.classList.toggle("active", vistaActual === "configuracion");
      const folder = carpetas.find(item => item.id === carpetaActualId);
      const materia = materiasDeCarpeta(folder).find(item => item.id === materiaActivaId);
      const page = elementoGaleriaActivoId ? paginaActualAbierta() : encontrarPagina(folder, paginaActivaId);
      const crumbs = document.getElementById("breadcrumbs");
      const menuToggle = '<button class="menu-toggle" id="menu-toggle" type="button" aria-label="Mostrar menú lateral" aria-expanded="false" aria-controls="sidebar" title="Mostrar u ocultar el menú">☰</button>';
      const homeCrumb = '<button type="button" data-go-home>Inicio</button>';
      if (vistaActual === "inicio" && inicioDashboard) {
        crumbs.innerHTML = `${menuToggle}<span class="crumb-current">Mi espacio académico</span>`;
      } else if (vistaActual === "busqueda") {
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><span class="crumb-current">Resultados de búsqueda</span>`;
      } else if (elementoGaleriaActivoId && folder && page && vistaActual === "pagina") {
        const section = secciones.find(item => item.id === folder.seccionId);
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><button class="pagina-miga" type="button" data-seccion-id="${escapeHtml(section?.id || "")}">${escapeHtml(section?.titulo || "Galería")}</button><span aria-hidden="true">/</span><span class="crumb-current">${escapeHtml(page.cardTitle || page.titulo || "Sin título")}</span>`;
      } else if (folder && page && vistaActual === "pagina") {
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><button class="pagina-miga" type="button" data-open-study="${escapeHtml(folder.id)}">${escapeHtml(folder.titulo || "Sin título")}</button><span aria-hidden="true">/</span><button class="pagina-miga" type="button" data-open-materia="${escapeHtml(materia?.id || "")}">${escapeHtml(materia?.titulo || "Materia")}</button><span aria-hidden="true">/</span><span class="crumb-current">${escapeHtml(page.pageTitle || "Sin título")}</span>`;
      } else if (folder && materia && vistaActual === "materia") {
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><button class="pagina-miga" type="button" data-open-study="${escapeHtml(folder.id)}">${escapeHtml(folder.titulo || "Sin título")}</button><span aria-hidden="true">/</span><span class="crumb-current">${escapeHtml(materia.titulo)}</span>`;
      } else if (folder && vistaActual === "carpeta") {
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><span class="crumb-current">${escapeHtml(folder.titulo || "Sin título")}</span>`;
      } else if (vistaActual === "configuracion") {
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><span class="crumb-current">Configuración</span>`;
      } else {
        const section = secciones.find(item => item.id === seccionActivaId);
        const sectionTitle = section?.id === "mis-carpetas" ? "Carpetas" : section?.titulo || "Mis carpetas";
        crumbs.innerHTML = `${menuToggle}${homeCrumb}<span aria-hidden="true">/</span><span class="crumb-current">${escapeHtml(sectionTitle)}</span>`;
      }
      actualizarMenuLateral();
    }

    function seccionActiva() {
      return secciones.find(seccion => seccion.id === seccionActivaId) || secciones[0];
    }

    const menuLateralMovilQuery = window.matchMedia("(max-width: 720px)");

    function guardarPreferenciaMenuLateral(abierto) {
      menuLateralPreferidoAbierto = abierto;
      try {
        localStorage.setItem(MENU_LATERAL_KEY, String(abierto));
      } catch (error) {
        console.error("No se pudo guardar la preferencia del menú lateral:", error);
        saveState.textContent = "No se pudo guardar la vista del menú";
      }
    }

    function actualizarMenuLateral() {
      const root = document.documentElement;
      const sidebar = document.getElementById("sidebar");
      const toggle = document.getElementById("menu-toggle");
      const overlay = document.getElementById("menu-overlay");
      const esMovil = menuLateralMovilQuery.matches;
      const visible = esMovil ? menuLateralMovilAbierto : menuLateralPreferidoAbierto || menuLateralFlotante;
      root.classList.toggle("menu-cerrado", esMovil ? !menuLateralMovilAbierto : !menuLateralPreferidoAbierto);
      root.classList.toggle("menu-visible", visible);
      root.classList.toggle("menu-flotante", !esMovil && menuLateralFlotante && !menuLateralPreferidoAbierto);
      sidebar.inert = !visible;
      sidebar.setAttribute("aria-hidden", String(!visible));
      toggle.setAttribute("aria-expanded", String(visible));
      toggle.setAttribute("aria-label", visible ? "Cerrar menú lateral" : "Abrir menú lateral");
      overlay.hidden = !esMovil || !menuLateralMovilAbierto;
    }

    function configurarMenuLateral() {
      try {
        const guardada = localStorage.getItem(MENU_LATERAL_KEY);
        menuLateralPreferidoAbierto = guardada === null ? true : guardada === "true";
      } catch (error) {
        console.error("No se pudo leer la preferencia del menú lateral:", error);
        menuLateralPreferidoAbierto = true;
      }
      menuLateralMovilAbierto = false;
      actualizarMenuLateral();
      document.addEventListener("click", event => {
        if (!event.target.closest("#menu-toggle")) return;
        if (menuLateralMovilQuery.matches) {
          menuLateralMovilAbierto = !menuLateralMovilAbierto;
          guardarPreferenciaMenuLateral(menuLateralMovilAbierto);
        } else {
          const estabaVisible = menuLateralPreferidoAbierto || menuLateralFlotante;
          menuLateralFlotante = false;
          guardarPreferenciaMenuLateral(!estabaVisible);
        }
        actualizarMenuLateral();
      });
      document.getElementById("menu-overlay").addEventListener("click", () => {
        menuLateralMovilAbierto = false;
        guardarPreferenciaMenuLateral(false);
        actualizarMenuLateral();
      });
      document.getElementById("menu-borde").addEventListener("pointerenter", () => {
        if (menuLateralMovilQuery.matches || menuLateralPreferidoAbierto) return;
        menuLateralFlotante = true;
        actualizarMenuLateral();
      });
      document.getElementById("menu-borde").addEventListener("focus", () => {
        if (menuLateralMovilQuery.matches || menuLateralPreferidoAbierto) return;
        menuLateralFlotante = true;
        actualizarMenuLateral();
      });
      document.getElementById("menu-borde").addEventListener("click", () => {
        menuLateralFlotante = false;
        guardarPreferenciaMenuLateral(true);
        actualizarMenuLateral();
      });
      document.getElementById("menu-borde").addEventListener("pointerleave", event => {
        if (menuLateralMovilQuery.matches || menuLateralPreferidoAbierto || event.relatedTarget?.closest?.("#sidebar")) return;
        menuLateralFlotante = false;
        actualizarMenuLateral();
      });
      document.getElementById("sidebar").addEventListener("pointerleave", event => {
        if (menuLateralMovilQuery.matches || menuLateralPreferidoAbierto) return;
        if (event.relatedTarget?.closest?.("#menu-borde, #menu-toggle")) return;
        menuLateralFlotante = false;
        actualizarMenuLateral();
      });
      document.getElementById("sidebar").addEventListener("click", event => {
        if (!menuLateralMovilQuery.matches || !event.target.closest("button")) return;
        menuLateralMovilAbierto = false;
        guardarPreferenciaMenuLateral(false);
        actualizarMenuLateral();
      });
      document.addEventListener("keydown", event => {
        const objetivo = event.target;
        const escribiendo = objetivo instanceof HTMLElement &&
          (objetivo.isContentEditable || objetivo.matches("input, textarea, select"));
        if (event.key === "Escape" && menuLateralMovilQuery.matches && menuLateralMovilAbierto) {
          menuLateralMovilAbierto = false;
          guardarPreferenciaMenuLateral(false);
          actualizarMenuLateral();
          return;
        }
        if (!event.ctrlKey || event.altKey || event.metaKey || event.key !== "\\" || escribiendo || event.isComposing) return;
        event.preventDefault();
        if (menuLateralMovilQuery.matches) {
          menuLateralMovilAbierto = !menuLateralMovilAbierto;
          guardarPreferenciaMenuLateral(menuLateralMovilAbierto);
        } else {
          menuLateralFlotante = false;
          guardarPreferenciaMenuLateral(!menuLateralPreferidoAbierto);
        }
        actualizarMenuLateral();
      });
      menuLateralMovilQuery.addEventListener("change", () => {
        menuLateralMovilAbierto = false;
        menuLateralFlotante = false;
        actualizarMenuLateral();
      });
    }

    function renderSidebar() {
      const section = seccionActiva();
      document.getElementById("folder-list").innerHTML = secciones.map(item => {
        const color = item.tipo === "juegos" ? "#FF9F43" : item.tipo === "notebooklm" ? "#6EC6FF" : "#6B6B72";
        const titulo = item.id === "mis-carpetas" ? "Carpetas" : item.titulo;
        const sectionId = escapeHtml(item.id);
        const active = item.id === seccionActivaId && (vistaActual === "inicio" || vistaActual === "busqueda" || vistaActual === "carpeta" || vistaActual === "materia" || vistaActual === "pagina");
        if (inicioDashboard) {
          return `<div class="folder-nav-wrap" data-home-section-tab="${sectionId}" draggable="true"><button class="folder-nav ${active ? "active" : ""}" type="button" data-seccion-id="${sectionId}" aria-current="${active ? "page" : "false"}"><span class="folder-dot" aria-hidden="true" style="--folder-color:${color}"></span><span class="folder-label">${escapeHtml(titulo)}</span></button><button class="dashboard-section-menu" type="button" data-home-section-menu="${sectionId}" aria-label="Administrar ${escapeHtml(titulo)}" aria-expanded="false" aria-controls="sidebar-section-menu-${sectionId}">⋯</button><div class="home-section-popover" id="sidebar-section-menu-${sectionId}" hidden><button type="button" data-home-rename-section="${sectionId}">Renombrar</button><button type="button" data-home-change-icon="${sectionId}">Cambiar ícono</button><button type="button" data-home-delete-section="${sectionId}">Eliminar sección</button></div></div>`;
        }
        return `<button class="folder-nav ${active ? "active" : ""}" type="button" data-seccion-id="${sectionId}" aria-current="${active ? "page" : "false"}"><span class="folder-dot" aria-hidden="true" style="--folder-color:${color}"></span><span class="folder-label">${escapeHtml(titulo)}</span></button>`;
      }).join("");
      document.querySelectorAll("#folder-list [data-home-section-tab]").forEach(tab => {
        tab.addEventListener("dragstart", event => {
          event.dataTransfer.setData("text/plain", tab.dataset.homeSectionTab);
          event.dataTransfer.effectAllowed = "move";
        });
        tab.addEventListener("dragover", event => event.preventDefault());
        tab.addEventListener("drop", event => {
          event.preventDefault();
          const targetIndex = secciones.findIndex(item => item.id === tab.dataset.homeSectionTab);
          moverSeccionA(event.dataTransfer.getData("text/plain"), targetIndex);
        });
      });
      const newFolderLabel = document.querySelector("#new-folder-nav .nav-label");
      if (newFolderLabel) newFolderLabel.textContent = inicioDashboard ? "Nueva página" : section?.tipo === "juegos" ? "Nuevo juego" : section?.tipo === "notebooklm" ? "Nuevo cuaderno" : section?.id === "mis-carpetas" ? "Nueva carpeta" : "Nuevo elemento";
    }

    function nombreSeccionInicio(section) {
      return section.id === "mis-carpetas" ? "Carpetas" : section.titulo;
    }

    function itemsDeSeccionInicio(sectionId) {
      const section = secciones.find(item => item.id === sectionId);
      if (!section) return [];
      if (section.id === "mis-carpetas") {
        return carpetas.filter(esCarpetaDeEstudio).flatMap(folder => [
          { kind: "folder", id: folder.id, title: folder.titulo || "Sin título", cover: folder.portada, color: folder.color, updated: Number(folder.actualizado) || 0, sectionId, folder },
          ...materiasDeCarpeta(folder).flatMap(materia => [
            { kind: "materia", id: materia.id, title: materia.titulo || "Sin título", cover: materia.portada, color: materia.color, updated: Number(materia.actualizado) || 0, sectionId, folder, materia },
            ...paginasDeMateria(materia).map(page => ({
              kind: "page", id: page.id, title: tituloTarjetaPagina(page), cover: page.portada, color: page.color,
              updated: Number(page.actualizado) || 0, sectionId, folder, materia, page,
              sectionLabel: `${folder.titulo || "Carpetas"} · ${materia.titulo || "Materia"}`
            }))
          ])
        ]);
      }
      return carpetas.filter(item => item.seccionId === section.id).map(element => ({
        kind: section.tipo === "notebooklm" ? "notebook" : "gallery",
        id: element.id, title: element.cardTitle || element.titulo || "Sin título", cover: element.portada,
        color: element.color, updated: Number(element.actualizado) || 0, sectionId, section, element,
        sectionLabel: nombreSeccionInicio(section)
      }));
    }

    function todosLosItemsInicio() {
      return secciones.flatMap(section => itemsDeSeccionInicio(section.id));
    }

    function colorDesaturadoInicio(title) {
      const colors = ["#292735", "#263139", "#34302a", "#27342e", "#35272c", "#252e3b"];
      let hash = 0;
      for (const character of title || "") hash = (hash * 31 + character.codePointAt(0)) | 0;
      return colors[Math.abs(hash) % colors.length];
    }

    function portadaItemInicio(item) {
      const color = item.cover && /^#[0-9a-f]{6}$/i.test(item.color || "") ? item.color : colorDesaturadoInicio(item.title);
      return item.cover
        ? `background-image:url("${escapeHtml(item.cover).replace(/["\\]/g, "\\$&")}");--home-hero-color:${color}`
        : `--home-hero-color:${color}`;
    }

    function menuItemInicio(item, uid) {
      const id = escapeHtml(item.id);
      if (item.kind === "folder") {
        return `<div class="paginas-accion-opciones"><button class="home-card-menu-toggle" type="button" data-elemento-opciones="${id}" aria-label="Opciones de ${escapeHtml(item.title)}" aria-expanded="false" aria-controls="menu-elemento-${id}-${uid}">⋯</button><div class="paginas-menu-opciones" id="menu-elemento-${id}-${uid}" hidden><button type="button" data-renombrar-elemento="${id}">Renombrar</button><button type="button" data-cambiar-portada-elemento="${id}">Cambiar portada</button><input type="file" accept="image/*" data-elemento-portada="${id}" aria-label="Elegir portada de ${escapeHtml(item.title)}"><label class="paginas-color-opcion">Cambiar color<input type="color" value="${escapeHtml(item.color || "#6B6B72")}" data-elemento-color="${id}" aria-label="Elegir color de ${escapeHtml(item.title)}"></label><button type="button" data-eliminar-elemento="${id}">Eliminar</button></div></div>`;
      }
      if (item.kind === "materia") {
        return `<div class="paginas-accion-opciones"><button class="home-card-menu-toggle" type="button" data-materia-opciones="${id}" aria-label="Opciones de ${escapeHtml(item.title)}" aria-expanded="false" aria-controls="menu-materia-${id}-${uid}">⋯</button><div class="paginas-menu-opciones" id="menu-materia-${id}-${uid}" hidden><button type="button" data-renombrar-materia="${id}">Cambiar título</button><button type="button" data-cambiar-portada-materia="${id}">Cambiar portada</button><input type="file" accept="image/*" data-materia-portada="${id}" aria-label="Elegir portada de ${escapeHtml(item.title)}"><button type="button" data-eliminar-materia="${id}">Eliminar materia</button></div></div>`;
      }
      if (item.kind === "page") {
        return `<div class="paginas-accion-opciones"><button class="home-card-menu-toggle" type="button" data-pagina-opciones="${id}" aria-label="Opciones de ${escapeHtml(item.title)}" aria-expanded="false" aria-controls="home-menu-pagina-${id}-${uid}">⋯</button><div class="home-card-popover paginas-menu-opciones" id="home-menu-pagina-${id}-${uid}" hidden><button type="button" data-home-favorite-page="${id}">${item.page.favorito ? "Quitar de favoritos" : "Agregar a favoritos"}</button><button type="button" data-home-edit-page="${id}" data-home-folder="${escapeHtml(item.folder.id)}" data-home-materia="${escapeHtml(item.materia.id)}">Abrir para editar</button><button type="button" data-home-rename-page="${id}">Renombrar tarjeta</button><button type="button" data-home-cover-trigger="${id}">Cambiar portada</button><input type="file" accept="image/*" data-pagina-portada="${id}" data-home-page-cover="${id}" data-home-folder="${escapeHtml(item.folder.id)}" data-home-materia="${escapeHtml(item.materia.id)}" aria-label="Elegir portada de ${escapeHtml(item.title)}"><button type="button" data-home-delete-page="${id}">Eliminar página</button></div></div>`;
      }
      return `<div class="paginas-accion-opciones"><button class="home-card-menu-toggle" type="button" data-elemento-opciones="${id}" aria-label="Opciones de ${escapeHtml(item.title)}" aria-expanded="false" aria-controls="menu-elemento-${id}-${uid}">⋯</button><div class="paginas-menu-opciones" id="menu-elemento-${id}-${uid}" hidden><button type="button" data-home-favorite-element="${id}">${item.element.favorito ? "Quitar de favoritos" : "Agregar a favoritos"}</button><button type="button" data-renombrar-elemento="${id}">Renombrar</button><button type="button" data-cambiar-portada-elemento="${id}">Cambiar portada</button><input type="file" accept="image/*" data-elemento-portada="${id}" aria-label="Elegir portada de ${escapeHtml(item.title)}"><label class="paginas-color-opcion">Cambiar color<input type="color" value="${escapeHtml(item.color || "#6B6B72")}" data-elemento-color="${id}" aria-label="Elegir color de ${escapeHtml(item.title)}"></label><button type="button" data-eliminar-elemento="${id}">Eliminar</button></div></div>`;
    }

    function tarjetaInicio(item) {
      const uid = `home-${++contadorTarjetasInicio}`;
      const id = escapeHtml(item.id);
      let openAttributes = "";
      if (item.kind === "folder") openAttributes = `data-open-study="${id}"`;
      else if (item.kind === "materia") openAttributes = `data-open-materia="${id}" data-materia-folder="${escapeHtml(item.folder.id)}"`;
      else if (item.kind === "page") openAttributes = `data-home-open-page="${id}" data-home-folder="${escapeHtml(item.folder.id)}" data-home-materia="${escapeHtml(item.materia.id)}"`;
      else if (item.kind === "notebook" && item.element.enlaceExterno) openAttributes = `data-open-notebook="${id}"`;
      else openAttributes = `data-open-gallery="${id}"`;
      const cover = item.cover ? `<img src="${escapeHtml(item.cover)}" alt="" loading="lazy">` : "";
      const sublabel = item.sectionLabel || (item.kind === "folder" ? "Carpeta" : item.kind === "materia" ? item.folder.titulo : nombreSeccionInicio(secciones.find(section => section.id === item.sectionId) || { id: "", titulo: "Sección" }));
      const supportsFavorite = ["page", "gallery", "notebook"].includes(item.kind);
      const favorite = item.kind === "page" ? item.page.favorito : item.element?.favorito;
      const favoriteAction = item.kind === "page" ? `data-home-favorite-page="${id}"` : `data-home-favorite-element="${id}"`;
      return `<article class="home-card-wrap" data-home-card="${id}" data-home-card-kind="${item.kind}" role="listitem">
        <button class="home-card" type="button" ${openAttributes} aria-label="Abrir ${escapeHtml(item.title)}" style="--home-card-color:${item.cover && /^#[0-9a-f]{6}$/i.test(item.color || "") ? item.color : colorDesaturadoInicio(item.title)}">${cover}${!item.cover ? `<span class="home-card-initial" aria-hidden="true">${escapeHtml((item.title || "•").trim().charAt(0).toLocaleUpperCase("es"))}</span>` : ""}<span class="home-card-title">${escapeHtml(item.title)}</span></button>
        ${menuItemInicio(item, uid)}
        <div class="home-card-info"><div class="home-card-info-actions"><button type="button" data-home-card-open="${item.kind}" data-home-card-id="${id}" aria-label="Abrir ${escapeHtml(item.title)}">↗</button>${supportsFavorite ? `<button type="button" ${favoriteAction} aria-label="${favorite ? "Quitar de favoritos" : "Agregar a favoritos"}" aria-pressed="${Boolean(favorite)}">★</button>` : ""}<button type="button" data-home-card-more aria-label="Más opciones de ${escapeHtml(item.title)}">⋯</button></div><span class="home-card-info-title">${escapeHtml(item.title)}</span><span class="home-card-info-section">${escapeHtml(sublabel || "")}</span></div>
      </article>`;
    }

    function filaInicio(titulo, items, { vertical = false, sectionId = "", folderId = "", materiaId = "", crear = "", href = "" } = {}) {
      if (!items.length && !crear) return "";
      const cards = items.map(tarjetaInicio).join("");
      const add = crear ? `<article class="home-card-wrap" role="listitem"><button class="home-create-card" type="button" data-home-create="${escapeHtml(crear)}" ${sectionId ? `data-home-create-section="${escapeHtml(sectionId)}"` : ""} ${folderId ? `data-home-create-folder="${escapeHtml(folderId)}"` : ""} ${materiaId ? `data-home-create-materia="${escapeHtml(materiaId)}"` : ""}><span aria-hidden="true">+</span>${escapeHtml(crear)}</button></article>` : "";
      const trackId = `home-track-${++contadorTarjetasInicio}`;
      return `<section class="home-row ${vertical ? "home-row-vertical" : ""}" aria-label="${escapeHtml(titulo)}"><div class="home-row-heading"><h2>${escapeHtml(titulo)}</h2>${href ? `<button type="button" data-seccion-id="${escapeHtml(href)}">Ver todo →</button>` : ""}</div><div class="home-row-controls"><button type="button" data-home-scroll="-1" data-home-track="${trackId}" aria-label="Desplazar hacia la izquierda">‹</button><button type="button" data-home-scroll="1" data-home-track="${trackId}" aria-label="Desplazar hacia la derecha">›</button></div><div class="home-row-track" id="${trackId}" role="list">${cards}${add}</div></section>`;
    }

    function textoDescripcionInicio(item) {
      const source = item.kind === "page" ? item.page.contenido : item.element?.contenido;
      if (!source) return "";
      const temporary = document.createElement("div");
      temporary.innerHTML = source;
      return (temporary.textContent || "").replace(/\s+/g, " ").trim().slice(0, 360);
    }

    function cerrarTarjetaFlotanteInicio(animar = false) {
      const state = homeHoverCardState;
      if (!state) return;
      window.clearTimeout(state.openTimer);
      window.clearTimeout(state.closeTimer);
      if (state.actions && state.actionsParent?.isConnected) {
        if (state.actionsNextSibling?.parentNode === state.actionsParent) {
          state.actionsParent.insertBefore(state.actions, state.actionsNextSibling);
        } else {
          state.actionsParent.append(state.actions);
        }
      }
      if (state.portal && animar) {
        state.portal.classList.add("is-closing");
        window.setTimeout(() => state.portal?.remove(), 200);
      } else {
        state.portal?.remove();
      }
      homeHoverCardState = null;
    }

    function programarCierreTarjetaFlotanteInicio() {
      const state = homeHoverCardState;
      if (!state) return;
      window.clearTimeout(state.closeTimer);
      state.closeTimer = window.setTimeout(() => {
        if (homeHoverCardState === state) cerrarTarjetaFlotanteInicio(true);
      }, 100);
    }

    function mostrarTarjetaFlotanteInicio(state) {
      const anchor = state.anchor;
      const card = anchor.querySelector(".home-card");
      const info = anchor.querySelector(".home-card-info");
      const actions = anchor.querySelector(".paginas-accion-opciones");
      if (!card || !info || !anchor.isConnected) {
        cerrarTarjetaFlotanteInicio();
        return;
      }

      const portal = document.createElement("div");
      document.querySelectorAll(".home-hover-card.is-closing").forEach(closingCard => closingCard.remove());
      portal.className = "home-hover-card is-entering";
      portal.setAttribute("role", "group");
      portal.setAttribute("aria-label", `Vista previa de ${card.getAttribute("aria-label")?.replace(/^Abrir /, "") || "página"}`);
      const image = document.createElement("div");
      image.className = "home-hover-image";
      image.append(card.cloneNode(true));
      const expandedInfo = info.cloneNode(true);
      expandedInfo.classList.add("home-hover-info");
      expandedInfo.querySelectorAll("[data-home-card-more]").forEach(button => {
        button.dataset.homeCardMore = "true";
        button.dataset.homeCardId = anchor.dataset.homeCard;
      });
      portal.append(image, expandedInfo);
      document.body.append(portal);

      if (actions) {
        state.actionsParent = actions.parentNode;
        state.actionsNextSibling = actions.nextSibling;
        state.actions = actions;
        image.append(actions);
      }
      state.portal = portal;
      const originalRect = card.getBoundingClientRect();
      const track = anchor.closest(".home-row-track");
      const trackRect = track?.getBoundingClientRect();
      const width = originalRect.width * 1.3;
      const leftEdge = trackRect ? Math.max(trackRect.left, 8) : 8;
      const rightEdge = trackRect ? Math.min(trackRect.right, window.innerWidth - 8) : window.innerWidth - 8;
      const atLeft = originalRect.left <= leftEdge + 8;
      const atRight = originalRect.right >= rightEdge - 16;
      let left = atLeft ? originalRect.left : atRight ? originalRect.right - width : originalRect.left + (originalRect.width - width) / 2;
      left = Math.max(8, Math.min(left, window.innerWidth - width - 8));
      portal.style.width = `${width}px`;
      portal.style.left = `${left}px`;
      const height = portal.getBoundingClientRect().height;
      portal.style.top = `${Math.max(8, Math.min(originalRect.top, window.innerHeight - height - 8))}px`;
      portal.addEventListener("pointerenter", () => window.clearTimeout(state.closeTimer));
      portal.addEventListener("pointerleave", event => {
        if (event.relatedTarget instanceof Node && anchor.contains(event.relatedTarget)) return;
        programarCierreTarjetaFlotanteInicio();
      });
      requestAnimationFrame(() => {
        if (homeHoverCardState === state) portal.classList.remove("is-entering");
      });
    }

    function configurarHoverTarjetasInicio() {
      cerrarTarjetaFlotanteInicio();
      if (!homeHoverListenersAttached) {
        window.addEventListener("scroll", () => cerrarTarjetaFlotanteInicio(true), true);
        window.addEventListener("resize", () => cerrarTarjetaFlotanteInicio(true));
        homeHoverListenersAttached = true;
      }
      if (!window.matchMedia("(hover: hover) and (pointer: fine)").matches) return;
      view.querySelectorAll(".home-card-wrap[data-home-card]").forEach(anchor => {
        anchor.addEventListener("pointerenter", () => {
          if (homeHoverCardState?.anchor === anchor) {
            window.clearTimeout(homeHoverCardState.closeTimer);
            return;
          }
          cerrarTarjetaFlotanteInicio();
          const state = { anchor, openTimer: 0, closeTimer: 0, portal: null, actions: null };
          homeHoverCardState = state;
          state.openTimer = window.setTimeout(() => {
            if (homeHoverCardState === state && anchor.matches(":hover")) mostrarTarjetaFlotanteInicio(state);
            else if (homeHoverCardState === state) cerrarTarjetaFlotanteInicio();
          }, 300);
        });
        anchor.addEventListener("pointerleave", event => {
          const state = homeHoverCardState;
          if (!state || state.anchor !== anchor) return;
          if (event.relatedTarget instanceof Node && state.portal?.contains(event.relatedTarget)) return;
          programarCierreTarjetaFlotanteInicio();
        });
      });
    }

    function renderHomeNavigation() {
      const nav = document.getElementById("home-navigation");
      if (!nav) return;
      const current = inicioDashboard ? "inicio" : seccionActivaId;
      const tabs = [`<button class="home-tab" id="home-tab-inicio" type="button" role="tab" data-home-dashboard aria-selected="${current === "inicio"}" aria-controls="home-panel" tabindex="${current === "inicio" ? "0" : "-1"}">Inicio</button>`, ...secciones.map(section => {
        const active = current === section.id;
        const icon = section.icon || (section.tipo === "juegos" ? "🎮" : section.tipo === "notebooklm" ? "📓" : section.id === "mis-carpetas" ? "📁" : "📚");
        const sectionId = escapeHtml(section.id);
        return `<span class="home-tab-wrap" role="presentation" draggable="true" data-home-section-tab="${sectionId}"><button class="home-tab" id="home-tab-${sectionId}" type="button" role="tab" data-seccion-id="${sectionId}" aria-selected="${active}" aria-controls="home-panel" tabindex="${active ? "0" : "-1"}">${escapeHtml(icon)} ${escapeHtml(section.id === "mis-carpetas" ? "Carpetas" : section.titulo)}</button><button class="home-section-menu-toggle" type="button" data-home-section-menu="${sectionId}" aria-label="Administrar ${escapeHtml(section.titulo)}" aria-expanded="false" aria-controls="home-section-menu-${sectionId}">⋯</button><div class="home-section-popover" id="home-section-menu-${sectionId}" hidden><button type="button" data-home-rename-section="${sectionId}">Renombrar</button><button type="button" data-home-change-icon="${sectionId}">Cambiar ícono</button><button type="button" data-home-delete-section="${sectionId}">Eliminar sección</button></div></span>`;
      })].join("");
      nav.innerHTML = `<button class="home-brand" type="button" data-home-dashboard aria-label="Ir al inicio">Tus carpetas</button><div class="home-tabs" role="tablist" aria-label="Secciones">${tabs}<span class="home-tab-wrap" role="presentation"><button class="home-add-section" type="button" data-home-add-section aria-label="Agregar sección" aria-expanded="false" aria-controls="home-create-popover">+</button><div class="home-create-popover" id="home-create-popover" hidden><label>Nombre de la sección<input type="text" id="home-new-section-name" maxlength="100" placeholder="Mis películas"></label><label>Ícono<select id="home-new-section-icon"><option value="📚">📚 Libros</option><option value="🎮">🎮 Juegos</option><option value="🎬">🎬 Películas</option><option value="📖">📖 Lectura</option><option value="💡">💡 Ideas</option></select></label><button type="button" data-home-create-section-submit>Crear sección</button></div></span></div>`;
      nav.querySelector('[role="tablist"]').addEventListener("keydown", event => {
        if (!["ArrowLeft", "ArrowRight", "Home", "End"].includes(event.key)) return;
        const tabList = event.currentTarget;
        const buttons = Array.from(tabList.querySelectorAll('[role="tab"]'));
        const index = buttons.indexOf(document.activeElement);
        if (index < 0) return;
        event.preventDefault();
        const next = event.key === "Home" ? 0 : event.key === "End" ? buttons.length - 1 : (index + (event.key === "ArrowRight" ? 1 : -1) + buttons.length) % buttons.length;
        buttons[next].focus();
        buttons[next].click();
      });
      nav.querySelectorAll("[data-home-section-tab]").forEach(tab => {
        tab.addEventListener("dragstart", event => { event.dataTransfer.setData("text/plain", tab.dataset.homeSectionTab); event.dataTransfer.effectAllowed = "move"; });
        tab.addEventListener("dragover", event => { event.preventDefault(); });
        tab.addEventListener("drop", event => {
          event.preventDefault();
          const draggedId = event.dataTransfer.getData("text/plain");
          const targetIndex = secciones.findIndex(section => section.id === tab.dataset.homeSectionTab);
          moverSeccionA(draggedId, targetIndex);
        });
      });
    }

    function materiasDashboardInicio() {
      return carpetas.filter(esCarpetaDeEstudio).flatMap(folder => {
        const subjects = materiasDeCarpeta(folder);
        if (subjects.length) {
          return subjects.map(materia => ({
            folder,
            materia,
            title: materia.titulo || "Sin título",
            pages: paginasDeMateria(materia),
            color: materia.color
          }));
        }
        return [{
          folder,
          materia: null,
          title: folder.titulo || "Sin título",
          pages: Array.isArray(folder.paginas) ? folder.paginas : [],
          color: folder.color
        }];
      });
    }

    function tareasDashboardInicio() {
      const parser = new DOMParser();
      const tareas = [];
      todosLosItemsInicio().filter(item => ["page", "gallery", "notebook"].includes(item.kind)).forEach(item => {
        const content = item.kind === "page" ? item.page.contenido : item.element?.contenido;
        if (!content) return;
        const documentContent = parser.parseFromString(content, "text/html");
        const allInputs = Array.from(documentContent.querySelectorAll("input"));
        documentContent.querySelectorAll(".editor-tareas input[type='checkbox']").forEach((checkbox, index) => {
          const label = checkbox.closest("li")?.textContent?.replace(/\s+/g, " ").trim() || "Tarea sin título";
          tareas.push({
            page: item.kind === "page" ? item.page : item.element,
            folder: item.folder || null,
            materia: item.materia || null,
            pageTitle: item.title,
            label,
            completed: checkbox.hasAttribute("checked"),
            taskIndex: index,
            inputIndex: allInputs.indexOf(checkbox)
          });
        });
      });
      return tareas;
    }

    function alternarCheckboxGuardado(contenido, inputIndex) {
      let index = 0;
      return String(contenido || "").replace(/<input\b[^>]*>/gi, tag => {
        if (index++ !== inputIndex) return tag;
        const checked = /\schecked(?:\s*=\s*(?:"[^"]*"|'[^']*'|[^\s>]+))?/i;
        if (checked.test(tag)) return tag.replace(checked, "");
        return tag.replace(/\s*\/?>$/, ending => ` checked${ending}`);
      });
    }

    function iconoMateriaDashboard() {
      return '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 5.5A2.5 2.5 0 0 1 6.5 3H20v17H6.5A2.5 2.5 0 0 1 4 17.5z"/><path d="M4 6h13M8 10h8m-8 4h6"/></svg>';
    }

    function renderDashboardAcademico() {
      const palette = [
        ["#7185a0", "#e8edf3"], ["#a77d75", "#f3e9e5"], ["#7b8b75", "#e9eee6"],
        ["#9a8669", "#f1ece4"], ["#827c9a", "#eeecf4"], ["#748e8b", "#e7efed"]
      ];
      const subjects = materiasDashboardInicio();
      const cards = subjects.map((subject, index) => {
        const [accent, tint] = palette[index % palette.length];
        const units = new Set(subject.pages.map(page => page.unidad).filter(unit => /^unidad\s*\d+/i.test(String(unit || ""))));
        const progress = Math.min(100, Math.round(units.size / 4 * 100));
        const subjectAttributes = subject.materia
          ? `data-open-materia="${escapeHtml(subject.materia.id)}" data-materia-folder="${escapeHtml(subject.folder.id)}"`
          : `data-open-study="${escapeHtml(subject.folder.id)}"`;
        const subtext = `${subject.pages.length} ${subject.pages.length === 1 ? "página" : "páginas"} · ${units.size} ${units.size === 1 ? "unidad" : "unidades"}${subject.materia ? "" : " · Carpeta"}`;
        return `<button class="dashboard-subject-card" type="button" data-dashboard-subject="${escapeHtml(subject.title.toLocaleLowerCase("es"))}" ${subjectAttributes} style="--subject-accent:${accent};--subject-tint:${tint}"><span class="dashboard-subject-icon">${iconoMateriaDashboard()}</span><span class="dashboard-subject-copy"><span class="dashboard-subject-title">${escapeHtml(subject.title)}</span><span class="dashboard-subject-meta">${escapeHtml(subtext)}</span><span class="dashboard-progress-meta"><span>Unidades con apuntes</span><strong>${progress}%</strong></span><span class="dashboard-progress-track" aria-label="${progress}% de unidades con apuntes"><span style="width:${progress}%"></span></span></span></button>`;
      }).join("");
      const tasks = tareasDashboardInicio();
      const pending = tasks.filter(task => !task.completed);
      const taskMarkup = pending.length
        ? pending.slice(0, 6).map(task => `<label class="dashboard-task"><input type="checkbox" data-dashboard-task-page="${escapeHtml(task.page.id)}" data-dashboard-task-index="${task.taskIndex}" data-dashboard-input-index="${task.inputIndex}"><span class="dashboard-task-title">${escapeHtml(task.label)}</span><span class="dashboard-task-source">${escapeHtml(task.pageTitle)}</span></label>`).join("")
        : `<p class="dashboard-empty-note">${tasks.length ? "Ya completaste las tareas guardadas en tus páginas." : "Las tareas con casillas que agregues en tus apuntes aparecerán aquí."}</p>`;
      const date = new Intl.DateTimeFormat("es", { weekday: "long", day: "numeric", month: "long", year: "numeric" }).format(new Date());
      const days = Array.from({ length: 7 }, (_, index) => {
        const day = new Date();
        day.setDate(day.getDate() - (6 - index));
        return new Intl.DateTimeFormat("es", { weekday: "narrow" }).format(day);
      });
      const favoriteItems = todosLosItemsInicio().filter(item =>
        ["page", "gallery", "notebook"].includes(item.kind) &&
        Boolean(item.kind === "page" ? item.page.favorito : item.element?.favorito)
      );
      const latestItem = todosLosItemsInicio()
        .filter(item => ["page", "gallery", "notebook"].includes(item.kind))
        .sort((a, b) => b.updated - a.updated)[0];
      const latestIsFavorite = latestItem && Boolean(latestItem.kind === "page" ? latestItem.page.favorito : latestItem.element?.favorito);
      const topFavoriteAction = latestItem
        ? `<button type="button" ${latestItem.kind === "page" ? `data-home-favorite-page="${escapeHtml(latestItem.id)}"` : `data-home-favorite-element="${escapeHtml(latestItem.id)}"`} aria-label="${latestIsFavorite ? "Quitar de favoritos" : "Marcar como favorito"}: ${escapeHtml(latestItem.title)}" aria-pressed="${Boolean(latestIsFavorite)}" title="${latestIsFavorite ? "Quitar de favoritos" : "Marcar como favorito"}"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m12 3 2.8 5.7 6.2.9-4.5 4.4 1.1 6.2-5.6-3-5.6 3 1.1-6.2L3 9.6l6.2-.9z"/></svg>${latestIsFavorite ? "En favoritos" : "Favorito"}</button>`
        : `<button type="button" data-dashboard-favorites><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m12 3 2.8 5.7 6.2.9-4.5 4.4 1.1 6.2-5.6-3-5.6 3 1.1-6.2L3 9.6l6.2-.9z"/></svg>Favoritos</button>`;
      const favoritesRow = filaInicio("Favoritos", favoriteItems.slice(0, 10), { vertical: true });
      return `<div class="dashboard-page" id="home-panel" role="tabpanel" aria-labelledby="home-tab-inicio">
        <div class="dashboard-topline">
          <div class="dashboard-actions">
            <button type="button" data-dashboard-share><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><path d="m8.7 10.7 6.6-4.4m-6.6 7 6.6 4.4"/></svg>Compartir</button>
            ${topFavoriteAction}
            <button type="button" data-dashboard-more aria-label="Más opciones" aria-expanded="false"><svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><circle cx="5" cy="12" r="1.7"/><circle cx="12" cy="12" r="1.7"/><circle cx="19" cy="12" r="1.7"/></svg></button>
            <div class="dashboard-more-menu" hidden><button type="button" data-dashboard-new-subject>Nueva materia</button><button type="button" data-dashboard-open-settings>Configuración</button></div>
          </div>
        </div>
        <header class="dashboard-greeting">
          <div><p class="dashboard-eyebrow">Tu espacio de estudio</p><h1>Buenos días, Lucía</h1></div>
          <time class="dashboard-date">${escapeHtml(date)}</time>
        </header>
        <label class="dashboard-search"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><circle cx="10.8" cy="10.8" r="6.8"/><path d="m16 16 4.5 4.5"/></svg><input type="search" data-dashboard-filter placeholder="Busca una materia" aria-label="Filtrar materias"></label>
        <section class="dashboard-section" id="dashboard-subjects" aria-labelledby="dashboard-subjects-title">
          <div class="dashboard-section-heading"><div><h2 id="dashboard-subjects-title">Materias del cuatrimestre</h2><p>${subjects.length} ${subjects.length === 1 ? "materia o carpeta" : "materias y carpetas"} en tu espacio</p></div><button class="dashboard-text-button" type="button" data-dashboard-new-subject>Agregar materia <span aria-hidden="true">+</span></button></div>
          <div class="dashboard-subject-grid">${cards || `<p class="dashboard-filter-empty">Todavía no hay materias. Crea una carpeta para empezar a organizar tu estudio.</p>`}<p class="dashboard-filter-empty" data-dashboard-no-results hidden>No encontramos materias con ese nombre.</p></div>
        </section>
        <div class="dashboard-lower-grid">
          <section class="dashboard-panel" id="dashboard-tasks" aria-labelledby="dashboard-tasks-title">
            <div class="dashboard-panel-heading"><h2 id="dashboard-tasks-title">Tareas pendientes</h2><span>${pending.length} ${pending.length === 1 ? "pendiente" : "pendientes"}</span></div>
            <div class="dashboard-task-list">${taskMarkup}</div>
          </section>
          <section class="dashboard-panel" id="dashboard-study-hours" aria-labelledby="dashboard-hours-title">
            <div class="dashboard-panel-heading"><h2 id="dashboard-hours-title">Horas de estudio</h2><span>Esta semana</span></div>
            <div class="dashboard-week-total"><strong>0</strong><span>horas registradas</span></div>
            <div class="dashboard-chart" role="img" aria-label="Sin horas de estudio registradas en los últimos siete días">${days.map((day, index) => `<div class="dashboard-chart-day"><span class="dashboard-chart-bar-wrap"><i class="dashboard-chart-bar" style="height:2px" aria-hidden="true"></i></span><span>${escapeHtml(day)}</span></div>`).join("")}</div>
            <p class="dashboard-chart-caption">Todavía no hay sesiones registradas. El gráfico se actualizará cuando la app tenga datos de estudio.</p>
          </section>
        </div>
        <section class="dashboard-favorites" id="dashboard-favorites" aria-label="Páginas favoritas">${favoritesRow || `<div class="dashboard-panel"><div class="dashboard-panel-heading"><h2>Favoritos</h2></div><p class="dashboard-empty-note">Las páginas que marques como favoritas aparecerán en esta sección.</p></div>`}</section>
      </div>`;
    }

    function renderContenidoInicio(sectionId) {
      contadorTarjetasInicio = 0;
      const dashboard = sectionId === null;
      const section = dashboard ? null : secciones.find(item => item.id === sectionId);
      const allItems = todosLosItemsInicio();
      const playableItems = allItems.filter(item => ["page", "gallery", "notebook"].includes(item.kind));
      if (dashboard) return renderDashboardAcademico();
      if (!dashboard && !itemsDeSeccionInicio(sectionId).length) {
        const action = section.tipo === "notebooklm" ? "Crear cuaderno" : section.id === "mis-carpetas" ? "Crear carpeta" : "Agregar página";
        return `<div class="home-page" id="home-panel" role="tabpanel" aria-label="${escapeHtml(nombreSeccionInicio(section))}"><div class="home-empty"><span class="home-empty-icon" aria-hidden="true">${escapeHtml(section.icon || "✦")}</span><strong>Esta sección está vacía</strong><button type="button" data-home-create="${action}" data-home-create-section="${escapeHtml(section.id)}">Crear página</button></div></div>`;
      }
      const featured = (dashboard ? playableItems : itemsDeSeccionInicio(sectionId).filter(item => ["page", "gallery", "notebook"].includes(item.kind)))
        .slice().sort((a, b) => Number(Boolean(b.cover)) - Number(Boolean(a.cover)) || b.updated - a.updated)[0] || (dashboard ? null : itemsDeSeccionInicio(sectionId)[0]);
      if (!dashboard && !section) return;
      if (!dashboard && homeGridView) {
        const gridItems = itemsDeSeccionInicio(sectionId).slice();
        if (homeGridSort === "name") gridItems.sort((a, b) => a.title.localeCompare(b.title, "es", { sensitivity: "base" }));
        else if (homeGridSort === "favorites") gridItems.sort((a, b) => Number(Boolean(b.kind === "page" ? b.page.favorito : b.element?.favorito)) - Number(Boolean(a.kind === "page" ? a.page.favorito : a.element?.favorito)) || b.updated - a.updated);
        else gridItems.sort((a, b) => b.updated - a.updated);
        const createAction = section.tipo === "notebooklm" ? "Crear cuaderno" : section.id === "mis-carpetas" ? "Crear carpeta" : "Agregar página";
        const cards = gridItems.map(tarjetaInicio).join("");
        const add = `<article class="home-card-wrap" role="listitem"><button class="home-create-card" type="button" data-home-create="${escapeHtml(createAction)}" data-home-create-section="${escapeHtml(section.id)}"><span aria-hidden="true">+</span>${escapeHtml(createAction)}</button></article>`;
        return `<div class="home-grid-page" id="home-panel" role="tabpanel" aria-labelledby="home-tab-${escapeHtml(section.id)}"><div class="home-grid-heading"><h1>${escapeHtml(nombreSeccionInicio(section))}</h1><label class="home-grid-sort">Ordenar por<select data-home-grid-sort aria-label="Ordenar páginas"><option value="recent" ${homeGridSort === "recent" ? "selected" : ""}>Recientes</option><option value="name" ${homeGridSort === "name" ? "selected" : ""}>Nombre A-Z</option><option value="favorites" ${homeGridSort === "favorites" ? "selected" : ""}>Favoritas</option></select></label></div>${gridItems.length ? `<div class="home-grid" role="list">${cards}${add}</div>` : `<div class="home-grid-empty">Todavía no hay páginas acá</div>`}</div>`;
      }
      let rows = "";
      if (dashboard) {
        const recent = [...playableItems].sort((a, b) => b.updated - a.updated);
        rows += filaInicio(featured && recent.some(item => item.id === featured.id) ? "Editadas recientemente" : "Continuar donde lo dejaste", recent.slice(0, 10));
        rows += filaInicio("Favoritos", playableItems.filter(item => (item.kind === "page" ? item.page.favorito : item.element.favorito)).slice(0, 12), { vertical: true });
        secciones.forEach(item => {
          const sectionItems = itemsDeSeccionInicio(item.id).filter(entry => entry.kind === "folder" || entry.kind === "gallery" || entry.kind === "notebook");
          rows += filaInicio(nombreSeccionInicio(item), sectionItems, { sectionId: item.id, crear: item.id === "mis-carpetas" ? "Crear carpeta" : item.tipo === "notebooklm" ? "Crear cuaderno" : "Agregar página", href: item.id });
        });
        rows += filaInicio("Agregadas recientemente", [...playableItems].sort((a, b) => b.updated - a.updated).slice(0, 12));
      } else if (section.id === "mis-carpetas") {
        const folders = carpetas.filter(esCarpetaDeEstudio);
        rows += filaInicio("Carpetas", folders.map(folder => ({
          kind: "folder", id: folder.id, title: folder.titulo || "Sin título", cover: folder.portada, color: folder.color,
          updated: Number(folder.actualizado) || 0, sectionId, folder
        })), { sectionId, crear: "Crear carpeta" });
        folders.forEach(folder => {
          const subjects = materiasDeCarpeta(folder);
          rows += filaInicio(folder.titulo || "Carpeta", subjects.map(materia => ({
            kind: "materia", id: materia.id, title: materia.titulo || "Sin título", cover: materia.portada,
            color: materia.color, updated: Number(materia.actualizado) || 0, sectionId, folder, materia
          })), { sectionId, folderId: folder.id, crear: "Crear materia" });
          const academicGroups = new Map();
          subjects.forEach(materia => {
            const pages = paginasDeMateria(materia).map(page => ({
              kind: "page", id: page.id, title: tituloTarjetaPagina(page), cover: page.portada, color: page.color,
              updated: Number(page.actualizado) || 0, sectionId, folder, materia, page,
              sectionLabel: `${folder.titulo || "Carpetas"} · ${materia.titulo || "Materia"}`
            }));
            rows += filaInicio(materia.titulo || "Materia", pages, { sectionId, folderId: folder.id, materiaId: materia.id, crear: "Agregar página" });
            pages.forEach(item => {
              const term = item.page.cuatrimestre === "primer-cuatrimestre" ? "Primer Cuatrimestre" : item.page.cuatrimestre === "segundo-cuatrimestre" ? "Segundo Cuatrimestre" : "";
              const unit = item.page.unidad || "";
              if (!term && !unit) return;
              const groupKey = `${term}|${unit}`;
              if (!academicGroups.has(groupKey)) academicGroups.set(groupKey, { term, unit, subjects: new Map() });
              academicGroups.get(groupKey).subjects.set(materia.id, {
                kind: "materia", id: materia.id, title: materia.titulo || "Sin título", cover: materia.portada,
                color: materia.color, updated: Number(materia.actualizado) || 0, sectionId, folder, materia
              });
            });
          });
          academicGroups.forEach(group => {
            const label = [folder.titulo || "Carpeta", group.term, group.unit].filter(Boolean).join(" · ");
            rows += filaInicio(label, Array.from(group.subjects.values()), { sectionId });
          });
        });
      } else {
        const entries = itemsDeSeccionInicio(sectionId);
        rows += filaInicio("Todas", entries, { sectionId, crear: section.tipo === "notebooklm" ? "Crear cuaderno" : "Agregar página" });
        const groups = new Map();
        entries.forEach(item => {
          const group = item.element?.etiqueta || item.element?.categoria;
          if (group) groups.set(group, [...(groups.get(group) || []), item]);
        });
        groups.forEach((groupItems, groupName) => { rows += filaInicio(groupName, groupItems, { sectionId, crear: "Agregar página" }); });
      }
      const hero = featured ? `<section class="home-hero" style="${portadaItemInicio(featured)}">${featured.cover ? "" : `<span class="home-hero-initial" aria-hidden="true">${escapeHtml((featured.title || "•").trim().charAt(0).toLocaleUpperCase("es"))}</span>`}<div class="home-hero-content"><p class="home-hero-kicker">${escapeHtml(nombreSeccionInicio(secciones.find(item => item.id === featured.sectionId) || { id: "", titulo: "Carpetas" }).toLocaleUpperCase("es"))}</p><h1>${escapeHtml(featured.title)}</h1>${textoDescripcionInicio(featured) ? `<p class="home-hero-description">${escapeHtml(textoDescripcionInicio(featured))}</p>` : ""}<p class="home-hero-meta">${escapeHtml(featured.sectionLabel || nombreSeccionInicio(secciones.find(item => item.id === featured.sectionId) || { id: "", titulo: "Carpetas" }))}${featured.updated ? ` · Editado ${new Intl.DateTimeFormat("es", { dateStyle: "medium" }).format(featured.updated)}` : ""}</p><div class="home-hero-actions"><button type="button" data-home-open-feature="${featured.kind}" data-home-feature-id="${escapeHtml(featured.id)}" data-home-folder="${escapeHtml(featured.folder?.id || "")}" data-home-materia="${escapeHtml(featured.materia?.id || "")}"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="m8 5 11 7-11 7z" fill="currentColor"/></svg>Abrir</button><button type="button" data-home-details="${featured.kind}" data-home-feature-id="${escapeHtml(featured.id)}"><svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="12" r="9" fill="none" stroke="currentColor" stroke-width="2"/><path d="M12 11v6m0-10h.01" fill="none" stroke="currentColor" stroke-width="2"/></svg>Más información</button></div></div></section>` : "";
      const empty = `<div class="home-empty"><span class="home-empty-icon" aria-hidden="true">${section?.icon || "✦"}</span><strong>${dashboard ? "Todavía no hay páginas" : "Esta sección está vacía"}</strong><button type="button" data-home-create="${section?.tipo === "notebooklm" ? "Crear cuaderno" : section?.id === "mis-carpetas" ? "Crear carpeta" : "Agregar página"}" ${section ? `data-home-create-section="${escapeHtml(section.id)}"` : ""}>Crear página</button></div>`;
      return `<div class="home-page${featured ? " home-page-has-hero" : ""}" id="home-panel" role="tabpanel" aria-labelledby="${dashboard ? "home-tab-inicio" : `home-tab-${escapeHtml(section.id)}`}">${hero}${rows ? `<div class="home-rows">${rows}</div>` : empty}</div>`;
    }

    function renderHome(sectionId = seccionActivaId, gridView = sectionId !== null) {
      cerrarTarjetaFlotanteInicio();
      if (homeRenderedSectionId !== sectionId) window.scrollTo(0, 0);
      homeRenderedSectionId = sectionId;
      if (vistaActual === "pagina") guardarPaginaAbierta();
      elementoGaleriaActivoId = null;
      cerrarVistaPdfEditor();
      detenerParallaxFondoEditor();
      detenerCorrector();
      vistaActual = "inicio";
      inicioDashboard = sectionId === null;
      homeGridView = !inicioDashboard && gridView;
      if (secciones.some(section => section.id === sectionId)) seccionActivaId = sectionId;
      try {
        localStorage.setItem(HOME_TAB_KEY, inicioDashboard ? "inicio" : seccionActivaId);
      } catch (error) {
        console.error("No se pudo guardar la última pestaña visitada:", error);
      }
      carpetaActualId = null;
      materiaActivaId = null;
      paginaActivaId = null;
      filtroTexto = "";
      search.value = "";
      document.getElementById("search-wrap").hidden = false;
      actualizarNavegacion();
      renderSidebar();
      renderHomeNavigation();
      view.innerHTML = renderContenidoInicio(inicioDashboard ? null : seccionActivaId);
      configurarHoverTarjetasInicio();
      const dashboardFilter = view.querySelector("[data-dashboard-filter]");
      const sidebarFilter = document.querySelector("[data-dashboard-search]");
      const filterSubjects = value => {
        const query = String(value || "").trim().toLocaleLowerCase("es").normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        let visibleCount = 0;
        view.querySelectorAll(".dashboard-subject-card").forEach(card => {
          const title = card.dataset.dashboardSubject.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
          const matches = !query || title.includes(query);
          card.hidden = !matches;
          if (matches) visibleCount++;
        });
        const noResults = view.querySelector("[data-dashboard-no-results]");
        if (noResults) noResults.hidden = !query || visibleCount > 0;
      };
      if (dashboardFilter && sidebarFilter) {
        const syncFilter = event => {
          const value = event.currentTarget.value;
          dashboardFilter.value = value;
          sidebarFilter.value = value;
          filterSubjects(value);
        };
        dashboardFilter.addEventListener("input", syncFilter);
        sidebarFilter.oninput = syncFilter;
        filterSubjects(sidebarFilter.value);
      }
      const gridSort = view.querySelector("[data-home-grid-sort]");
      if (gridSort) gridSort.addEventListener("change", () => {
        homeGridSort = gridSort.value;
        renderHome(seccionActivaId, true);
      });
      const topbar = document.querySelector(".app .topbar");
      if (topbar) topbar.classList.toggle("home-scrolled", window.scrollY > 20);
      if (!homeScrollListenerAttached) {
        window.addEventListener("scroll", () => {
          const bar = document.querySelector(".app .topbar");
          if (bar) bar.classList.toggle("home-scrolled", document.querySelector(".app")?.classList.contains("home-mode") && window.scrollY > 20);
        }, { passive: true });
        homeScrollListenerAttached = true;
      }
      view.querySelectorAll(".home-row").forEach(row => {
        const track = row.querySelector(".home-row-track");
        const update = () => row.querySelectorAll("[data-home-scroll]").forEach(button => {
          button.disabled = button.dataset.homeScroll === "-1" ? track.scrollLeft <= 1 : track.scrollLeft + track.clientWidth >= track.scrollWidth - 1;
        });
        track.addEventListener("scroll", update, { passive: true });
        update();
      });
      view.querySelectorAll(".home-row-track, .home-grid").forEach(track => track.addEventListener("keydown", event => {
        if (!["ArrowLeft", "ArrowRight"].includes(event.key)) return;
        const cards = Array.from(track.querySelectorAll(".home-card"));
        const index = cards.indexOf(document.activeElement);
        if (index < 0) return;
        event.preventDefault();
        cards[Math.max(0, Math.min(cards.length - 1, index + (event.key === "ArrowRight" ? 1 : -1)))].focus();
      }));
      return;
      const section = seccionActiva();
      const createLabel = section.tipo === "juegos" ? "Nuevo juego" : section.tipo === "notebooklm" ? "Nuevo cuaderno" : section.id === "mis-carpetas" ? "Nueva carpeta" : "Nuevo elemento";
      const sectionTitle = section.id === "mis-carpetas" ? "Carpetas" : section.titulo;
      view.innerHTML = `
        <section class="carpetas-seccion" id="carpetas-estudio" aria-labelledby="carpetas-titulo">
          <h1 class="carpetas-encabezado" id="carpetas-titulo">${escapeHtml(sectionTitle)}</h1>
          ${section.id === "mis-carpetas" ? '<p class="intro">Abre una carpeta para crear y organizar sus materias.</p>' : ""}
          <div class="carpetas-filtro-contenedor">
            <button class="carpetas-filtro-boton" id="carpetas-filtro-boton" type="button" aria-expanded="false" aria-controls="carpetas-filtro">
              Filtrar
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 6h16M7 12h10m-7 6h4"/><circle cx="8" cy="6" r="1"/><circle cx="15" cy="12" r="1"/><circle cx="11" cy="18" r="1"/></svg>
            </button>
            <label class="carpetas-filtro" id="carpetas-filtro" hidden>
              <span class="carpetas-solo-lector">${section.id === "mis-carpetas" ? "Filtrar carpetas, materias o páginas" : "Filtrar elementos por título"}</span>
              <input id="carpetas-filtro-entrada" type="search" placeholder="${section.id === "mis-carpetas" ? "Buscar carpetas, materias o páginas..." : "Buscar elementos..."}" autocomplete="off">
            </label>
            ${section.id !== "mis-carpetas" && section.tipo !== "notebooklm" ? `
              <div class="galeria-tamano">
                <button class="galeria-tamano-boton" id="galeria-tamano-boton" type="button" aria-expanded="${controlesTamanoGaleriaAbiertos}" aria-controls="galeria-tamano-control">Tamaño de cuadrícula</button>
                <label class="galeria-tamano-control" id="galeria-tamano-control" ${controlesTamanoGaleriaAbiertos ? "" : "hidden"}>
                  <span>Tarjetas: <output id="galeria-tamano-valor">${tamanoGaleria(section.id)} px</output></span>
                  <input id="galeria-tamano" type="range" min="140" max="360" step="10" value="${tamanoGaleria(section.id)}" aria-label="Tamaño de las tarjetas de la galería">
                </label>
              </div>
            ` : ""}
          </div>
          <div class="carpetas-grilla ${section.id !== "mis-carpetas" && section.tipo !== "notebooklm" ? "galeria-grilla" : ""}" id="carpetas-grilla" data-etiqueta-nuevo="${escapeHtml(createLabel)}" data-seccion="${escapeHtml(section.id)}" style="--gallery-card-size:${tamanoGaleria(section.id)}px"></div>
          <nav class="carpetas-paginacion" id="carpetas-paginacion" aria-label="Paginación de ${escapeHtml(section.id === "mis-carpetas" ? "Carpetas" : section.titulo)}"></nav>
        </section>`;
      renderCarpetas();
      document.getElementById("carpetas-filtro-boton").addEventListener("click", event => {
        const filter = document.getElementById("carpetas-filtro");
        filter.hidden = !filter.hidden;
        event.currentTarget.setAttribute("aria-expanded", String(!filter.hidden));
        if (!filter.hidden) document.getElementById("carpetas-filtro-entrada").focus();
        else { filtroTexto = ""; paginaActual = 1; renderCarpetas(); }
      });
      document.getElementById("carpetas-filtro-entrada").addEventListener("input", event => { filtroTexto = event.target.value; paginaActual = 1; renderCarpetas(); });
      const gallerySizeButton = document.getElementById("galeria-tamano-boton");
      const gallerySizeInput = document.getElementById("galeria-tamano");
      if (gallerySizeButton && gallerySizeInput) {
        gallerySizeButton.addEventListener("click", () => {
          controlesTamanoGaleriaAbiertos = !controlesTamanoGaleriaAbiertos;
          gallerySizeButton.setAttribute("aria-expanded", String(controlesTamanoGaleriaAbiertos));
          document.getElementById("galeria-tamano-control").hidden = !controlesTamanoGaleriaAbiertos;
        });
        gallerySizeInput.addEventListener("input", event => {
          const size = Number(event.currentTarget.value);
          document.getElementById("galeria-tamano-valor").textContent = `${size} px`;
          document.getElementById("carpetas-grilla").style.setProperty("--gallery-card-size", `${size}px`);
        });
        gallerySizeInput.addEventListener("change", event => {
          guardarTamanoGaleria(section.id, Number(event.currentTarget.value));
        });
      }
      document.getElementById("carpetas-paginacion").addEventListener("click", event => {
        const boton = event.target.closest("[data-pagina]");
        if (!boton || boton.disabled) return;
        paginaActual = Number(boton.dataset.pagina);
        renderCarpetas();
      });
      document.getElementById("carpetas-grilla").addEventListener("error", event => {
        const imagen = event.target;
        if (!(imagen instanceof HTMLImageElement) || !imagen.matches(".carpetas-portada")) return;
        const tarjeta = imagen.closest(".carpetas-tarjeta");
        if (!tarjeta) return;
        tarjeta.classList.remove("carpetas-tarjeta-con-portada");
        const titulo = tarjeta.querySelector(".carpetas-titulo");
        titulo?.classList.remove("carpetas-titulo-con-portada");
        titulo?.classList.add("carpetas-titulo-sin-portada");
        imagen.remove();
      }, true);
    }

    function renderBusquedaGlobal(texto) {
      const busqueda = texto.trim().toLocaleLowerCase("es");
      if (!busqueda) return;
      if (vistaActual !== "busqueda") {
        vistaBusquedaAnterior = ["carpeta", "materia", "pagina"].includes(vistaActual) ? vistaActual : "inicio";
        carpetaBusquedaAnteriorId = ["carpeta", "materia", "pagina"].includes(vistaBusquedaAnterior) ? carpetaActualId : null;
        materiaBusquedaAnteriorId = ["materia", "pagina"].includes(vistaBusquedaAnterior) ? materiaActivaId : null;
        paginaBusquedaAnteriorId = vistaBusquedaAnterior === "pagina" ? paginaActivaId : null;
        elementoGaleriaBusquedaAnteriorId = vistaBusquedaAnterior === "pagina" ? elementoGaleriaActivoId : null;
      }
      if (vistaActual === "pagina") guardarPaginaAbierta();
      elementoGaleriaActivoId = null;
      cerrarVistaPdfEditor();
      detenerParallaxFondoEditor();
      detenerCorrector();
      vistaActual = "busqueda";
      paginaActivaId = null;
      document.getElementById("search-wrap").hidden = false;
      actualizarNavegacion();
      renderSidebar();
      const carpetasCoincidentes = carpetas.filter(carpeta =>
        (carpeta.titulo || "").toLocaleLowerCase("es").includes(busqueda) ||
        (esElementoGaleria(carpeta) && [
          carpeta.cardTitle, carpeta.pageTitle, carpeta.contenido, carpeta.pdf?.title, carpeta.pdf?.author
        ].some(valor => String(valor || "").toLocaleLowerCase("es").includes(busqueda)))
      );
      const paginasCoincidentes = carpetas.filter(esCarpetaDeEstudio).flatMap(carpeta => materiasDeCarpeta(carpeta).flatMap(materia =>
        paginasDeMateria(materia)
          .filter(pagina => [
            carpeta.titulo, materia.titulo, tituloTarjetaPagina(pagina),
            pagina.pageTitle, pagina.pdf?.title, pagina.pdf?.author
          ].some(valor => (valor || "").toLocaleLowerCase("es").includes(busqueda)))
          .map(pagina => ({ carpeta, materia, pagina }))
      ));
      const resultadosCarpetas = carpetasCoincidentes.map(carpeta => {
        const section = secciones.find(item => item.id === carpeta.seccionId);
        const esEstudio = esCarpetaDeEstudio(carpeta);
        const atributoApertura = esEstudio
          ? `data-open-study="${escapeHtml(carpeta.id)}"`
          : section?.tipo === "notebooklm" ? `data-open-notebook="${escapeHtml(carpeta.id)}"` : `data-open-gallery="${escapeHtml(carpeta.id)}"`;
        const etiqueta = esEstudio ? "Carpeta" : section?.titulo || "Elemento";
        const accion = esEstudio ? "Abrir carpeta →" : section?.tipo === "notebooklm" ? "Abrir cuaderno →" : "Abrir →";
        const tag = atributoApertura ? "button" : "div";
        return `<${tag} class="busqueda-resultado" ${tag === "button" ? `type="button" ${atributoApertura}` : ""}>
          <span class="busqueda-resultado-titulo">${escapeHtml(carpeta.titulo || "Sin título")}</span>
          <span class="busqueda-resultado-detalle">${escapeHtml(etiqueta)}</span>
          <span class="busqueda-resultado-accion">${accion}</span>
        </${tag}>`;
      }).join("");
      const resultadosPaginas = paginasCoincidentes.map(({ carpeta, materia, pagina }) => `
        <button class="busqueda-resultado" type="button" data-abrir-resultado-carpeta="${escapeHtml(carpeta.id)}" data-abrir-resultado-materia="${escapeHtml(materia.id)}" data-abrir-resultado-pagina="${escapeHtml(pagina.id)}">
          <span class="busqueda-resultado-titulo">${escapeHtml(tituloTarjetaPagina(pagina))}</span>
          <span class="busqueda-resultado-detalle">${escapeHtml(carpeta.titulo)} · ${escapeHtml(materia.titulo)}${pagina.pdf?.title ? ` · PDF: ${escapeHtml(pagina.pdf.title)}` : ""}${pagina.pdf?.author ? ` · ${escapeHtml(pagina.pdf.author)}` : ""}</span>
          <span class="busqueda-resultado-accion">Abrir página →</span>
        </button>`).join("");
      view.innerHTML = `
        <section class="busqueda-vista" aria-labelledby="busqueda-titulo">
          <h1 id="busqueda-titulo">Resultados de búsqueda</h1>
          <p>Coincidencias para «${escapeHtml(texto.trim())}»</p>
          <div class="busqueda-resultados">${resultadosCarpetas}${resultadosPaginas || ""}</div>
          ${!resultadosCarpetas && !resultadosPaginas ? '<p class="carpetas-vacio">No se encontró material que coincida con la búsqueda.</p>' : ""}
        </section>`;
    }

    function renderCarpetas() {
      const grid = document.getElementById("carpetas-grilla");
      if (!grid) return;
      const section = seccionActiva();
      const busqueda = filtroTexto.trim().toLocaleLowerCase("es");
      const filtradas = carpetas.filter(carpeta => {
        if ((carpeta.seccionId || "mis-carpetas") !== section.id) return false;
        if (!busqueda) return true;
        return (carpeta.titulo || "").toLocaleLowerCase("es").includes(busqueda) ||
          (esElementoGaleria(carpeta) && [
            carpeta.cardTitle, carpeta.pageTitle, carpeta.contenido, carpeta.pdf?.title, carpeta.pdf?.author
          ].some(valor => String(valor || "").toLocaleLowerCase("es").includes(busqueda))) ||
          (esCarpetaDeEstudio(carpeta) && materiasDeCarpeta(carpeta).some(materia =>
            materia.titulo.toLocaleLowerCase("es").includes(busqueda) ||
            paginasDeMateria(materia).some(pagina => [
              tituloTarjetaPagina(pagina), pagina.pageTitle, pagina.pdf?.title, pagina.pdf?.author
            ].some(valor => (valor || "").toLocaleLowerCase("es").includes(busqueda)))
          ));
      });
      const porPagina = 5;
      const addCreateTile = true;
      const paginas = Math.max(1, Math.ceil(filtradas.length / porPagina));
      paginaActual = Math.max(1, Math.min(paginaActual, paginas || 1));
      const visibles = filtradas.slice((paginaActual - 1) * porPagina, paginaActual * porPagina);
      const cards = visibles.map(carpeta => {
        const tienePortada = typeof carpeta.portada === "string" && carpeta.portada.trim() !== "";
        const titulo = escapeHtml(carpeta.titulo);
        const textoHover = escapeHtml(carpeta.textoHover || "Abrir carpeta");
        const esNotebook = section.tipo === "notebooklm";
        const metadatosMateria = section.id === "mis-carpetas"
          ? `<span class="carpetas-subtitulo">${materiasDeCarpeta(carpeta).length} ${materiasDeCarpeta(carpeta).length === 1 ? "materia" : "materias"}</span>`
          : "";
        const portadaYTitulo = tienePortada
          ? `<img class="carpetas-portada" src="${escapeHtml(carpeta.portada)}" alt="Portada de ${titulo}" loading="lazy"><span class="carpetas-superposicion" aria-hidden="true"><span class="carpetas-detalles"><span class="carpetas-titulo carpetas-titulo-con-portada">${titulo}</span>${metadatosMateria}</span></span>`
          : `<span class="carpetas-superposicion" aria-hidden="true"><span class="carpetas-detalles"><span class="carpetas-titulo carpetas-titulo-sin-portada">${titulo}</span>${metadatosMateria}</span></span>`;
        const claseMateria = section.id === "mis-carpetas" ? "materia-tarjeta" : "";
        const estiloTarjeta = section.id === "mis-carpetas"
          ? `--materia-color:${escapeHtml(carpeta.color)}`
          : `background-color:${escapeHtml(carpeta.color)}`;
        const etiquetaApertura = esNotebook
          ? `data-open-notebook="${escapeHtml(carpeta.id)}"`
          : esCarpetaDeEstudio(carpeta)
            ? `data-open-study="${escapeHtml(carpeta.id)}"`
            : `data-open-gallery="${escapeHtml(carpeta.id)}"`;
        const contenidoTarjeta = `
          ${portadaYTitulo}
          <span class="carpetas-accion" aria-hidden="true">${esNotebook && carpeta.enlaceExterno ? "Abrir cuaderno" : textoHover}</span>
        `;
        const etiquetaTarjeta = esNotebook
          ? `Abrir enlace de ${titulo}`
          : esCarpetaDeEstudio(carpeta) ? `Abrir carpeta ${titulo}` : `Abrir ${titulo}`;
        const card = `<button class="carpetas-tarjeta ${claseMateria} ${tienePortada ? "carpetas-tarjeta-con-portada" : ""}" type="button" ${etiquetaApertura} aria-label="${etiquetaTarjeta}" style="${estiloTarjeta}">${contenidoTarjeta}</button>`;
        const id = escapeHtml(carpeta.id);
        return `<article class="carpetas-item-con-opciones ${section.id === "mis-carpetas" ? "materia-item" : ""}" data-elemento-id="${id}">
          ${card}
          <div class="paginas-accion-opciones">
            <button class="paginas-boton-opciones" type="button" data-elemento-opciones="${id}" aria-label="Opciones de ${titulo}" aria-expanded="false" aria-controls="menu-elemento-${id}">⋯</button>
            <div class="paginas-menu-opciones" id="menu-elemento-${id}" hidden>
              <button type="button" data-renombrar-elemento="${id}">Renombrar</button>
              <button type="button" data-cambiar-portada-elemento="${id}">Cambiar portada</button>
              <input type="file" accept="image/*" data-elemento-portada="${id}" aria-label="Elegir portada de ${titulo}">
              <label class="paginas-color-opcion">Cambiar color<input type="color" value="${escapeHtml(carpeta.color)}" data-elemento-color="${id}" aria-label="Elegir color de ${titulo}"></label>
              <button type="button" data-quitar-portada-elemento="${id}" ${tienePortada ? "" : "disabled"}>Quitar portada</button>
              <button type="button" data-eliminar-elemento="${id}">Eliminar</button>
            </div>
          </div>
        </article>`;
      }).join("");
      const createTile = addCreateTile && paginaActual === 1
        ? `<button class="carpetas-tarjeta carpetas-nuevo-elemento" type="button" data-crear-elemento aria-label="${escapeHtml(grid.dataset.etiquetaNuevo)}"><span aria-hidden="true">+</span>${escapeHtml(grid.dataset.etiquetaNuevo)}</button>`
        : "";
      grid.innerHTML = `${cards}${createTile}${!filtradas.length && filtroTexto ? '<p class="carpetas-vacio">No hay elementos que coincidan con la búsqueda.</p>'             : !filtradas.length && section.id === "mis-carpetas" ? '<p class="carpetas-vacio">Todavía no hay carpetas. Crea una para empezar.</p>' : ""}`;
      const pagination = document.getElementById("carpetas-paginacion");
      pagination.innerHTML = paginas > 1 ? `<button class="carpetas-pagina carpetas-pagina-texto" type="button" data-pagina="${paginaActual - 1}" ${paginaActual === 1 ? "disabled" : ""}>Anterior</button>${Array.from({ length: paginas }, (_, index) => `<button class="carpetas-pagina" type="button" data-pagina="${index + 1}" aria-label="Página ${index + 1}" ${index + 1 === paginaActual ? 'aria-current="page"' : ""}>${index + 1}</button>`).join("")}<button class="carpetas-pagina carpetas-pagina-texto" type="button" data-pagina="${paginaActual + 1}" ${paginaActual === paginas ? "disabled" : ""}>Siguiente</button>` : "";
    }

    function renderMateriasCarpeta(carpeta) {
      const grid = document.getElementById("materias-grilla");
      if (!grid) return;
      const query = (document.getElementById("materias-filtro")?.value || "").trim().toLocaleLowerCase("es");
      grid.classList.toggle("materias-grilla-filtradas", Boolean(query));
      grid.classList.toggle("vista-lista", modoVistaPaginas === "lista");
      const materias = materiasDeCarpeta(carpeta).filter(materia =>
        !query || materia.titulo.toLocaleLowerCase("es").includes(query)
      );
      const tarjetas = materias.map(materia => {
        const id = escapeHtml(materia.id);
        const titulo = escapeHtml(materia.titulo);
        const tienePortada = Boolean(materia.portada);
        const imagen = tienePortada ? `<img class="carpetas-portada" src="${escapeHtml(materia.portada)}" alt="Portada de ${titulo}" loading="lazy">` : "";
        const conteo = paginasDeMateria(materia).length;
        const menu = `<div class="paginas-accion-opciones">
          <button class="paginas-boton-opciones" type="button" data-materia-opciones="${id}" aria-label="Opciones de ${titulo}" aria-expanded="false" aria-controls="menu-materia-${id}">⋯</button>
          <div class="paginas-menu-opciones" id="menu-materia-${id}" hidden>
            <button type="button" data-renombrar-materia="${id}">Cambiar título</button>
            <button type="button" data-cambiar-portada-materia="${id}">Cambiar portada</button>
            <input type="file" accept="image/*" data-materia-portada="${id}" aria-label="Elegir portada de ${titulo}">
            <label class="paginas-color-opcion">Cambiar color<input type="color" value="${escapeHtml(materia.color)}" data-materia-color="${id}" aria-label="Elegir color de ${titulo}"></label>
            <button type="button" data-quitar-portada-materia="${id}" ${tienePortada ? "" : "disabled"}>Quitar portada</button>
            <button type="button" data-eliminar-materia="${id}">Eliminar materia</button>
          </div>
        </div>`;
        if (modoVistaPaginas === "lista") {
          return `<article class="pagina-lista-item materia-lista-item" data-materia-item="${id}">
            <button class="pagina-lista-abrir" type="button" data-open-materia="${id}" data-materia-folder="${escapeHtml(carpeta.id)}" aria-label="Abrir materia: ${titulo}">
              <span class="pagina-lista-titulo">${titulo}</span>
              <span class="pagina-lista-autor">Materia</span>
              <span class="pagina-lista-pdf">${conteo} ${conteo === 1 ? "página" : "páginas"}</span>
              <span class="pagina-lista-acceso">Abrir →</span>
            </button>${menu}
          </article>`;
        }
        return `<article class="paginas-tarjeta-item materia-tarjeta-item" data-materia-item="${id}">
          <button class="carpetas-tarjeta materia-tarjeta ${tienePortada ? "carpetas-tarjeta-con-portada" : ""}" type="button" data-open-materia="${id}" data-materia-folder="${escapeHtml(carpeta.id)}" aria-label="Abrir materia: ${titulo}" style="--materia-color:${escapeHtml(materia.color)}">
            ${imagen}
            <span class="carpetas-superposicion" aria-hidden="true"><span class="carpetas-detalles"><span class="carpetas-titulo ${tienePortada ? "carpetas-titulo-con-portada" : "carpetas-titulo-sin-portada"}">${titulo}</span><span class="carpetas-subtitulo">${conteo} ${conteo === 1 ? "página" : "páginas"}</span></span></span>
            <span class="carpetas-accion" aria-hidden="true">Abrir materia</span>
          </button>${menu}
        </article>`;
      }).join("");
      const nueva = modoVistaPaginas === "lista"
        ? '<button class="pagina-unidad-agregar materia-nueva-lista" type="button" data-crear-materia>+ Nueva materia</button>'
        : '<button class="carpetas-tarjeta paginas-crear-tarjeta materia-nueva-tarjeta" type="button" data-crear-materia><span class="paginas-crear-mas" aria-hidden="true">+</span><span class="paginas-crear-texto">Nueva materia</span></button>';
      grid.innerHTML = `${tarjetas}${!query ? nueva : ""}${query && !materias.length ? '<p class="paginas-mensaje-vacio">No hay materias que coincidan con la búsqueda.</p>' : ""}`;
    }

    function abrirCarpeta(id) {
      cerrarVistaPdfEditor();
      detenerParallaxFondoEditor();
      const carpeta = carpetas.find(item => item.id === id);
      if (!esCarpetaDeEstudio(carpeta)) return;
      if (vistaActual === "pagina") guardarPaginaAbierta();
      elementoGaleriaActivoId = null;
      if (carpetaActualId !== id) {
        filtroPaginasTexto = "";
      }
      detenerCorrector();
      carpetaActualId = id;
      materiaActivaId = null;
      paginaActivaId = null;
      vistaActual = "carpeta";
      document.getElementById("search-wrap").hidden = false;
      search.value = "";
      actualizarNavegacion();
      view.innerHTML = `
        <section class="paginas-vista">
          <div class="paginas-encabezado">
            <button class="paginas-volver" type="button" data-volver aria-label="Volver a Carpetas">← Carpetas</button>
            <h1 class="carpetas-encabezado">${escapeHtml(carpeta.titulo || "Sin título")}</h1>
          </div>
          <p class="intro paginas-ayuda">Materias dentro de esta carpeta. Cada materia tiene sus cuatrimestres, unidades y páginas.</p>
          <div class="materias-herramientas">
            <label class="carpetas-filtro materias-filtro"><span class="carpetas-solo-lector">Buscar materias</span><input id="materias-filtro" type="search" placeholder="Buscar materias..." autocomplete="off"></label>
            <div class="paginas-vista-selector" role="group" aria-label="Vista de materias">
              <button class="paginas-vista-boton" type="button" data-pages-view="cuadricula" aria-pressed="${modoVistaPaginas === "cuadricula"}"><span aria-hidden="true">▦</span><span>Cuadrícula</span></button>
              <button class="paginas-vista-boton" type="button" data-pages-view="lista" aria-pressed="${modoVistaPaginas === "lista"}"><span aria-hidden="true">☰</span><span>Lista</span></button>
            </div>
          </div>
          <div class="carpetas-grilla materias-grilla" id="materias-grilla"></div>
          <section class="paginas-enlaces" aria-labelledby="paginas-enlaces-titulo">
            <div class="vista-enlaces-cabecera"><h2 class="vista-subtitulo" id="paginas-enlaces-titulo">Enlaces de la carpeta</h2></div>
            <ul class="paginas-etiquetas-enlaces" id="vista-enlaces"></ul>
            <button class="paginas-agregar-enlace" id="paginas-agregar-enlace" type="button" aria-expanded="false" aria-controls="vista-formulario-enlace">+ Agregar enlace</button>
            <form class="vista-formulario-enlace paginas-formulario-enlace" id="vista-formulario-enlace" hidden>
              <input class="vista-campo" name="titulo" placeholder="Nombre del enlace" aria-label="Nombre del enlace" required>
              <input class="vista-campo" name="url" type="url" placeholder="https://..." aria-label="Dirección del enlace" required>
              <button class="vista-boton" type="submit">Guardar enlace</button>
            </form>
          </section>
        </section>`;
      renderMateriasCarpeta(carpeta);
      document.getElementById("materias-filtro").addEventListener("input", () => renderMateriasCarpeta(carpeta));
      view.querySelectorAll("[data-pages-view]").forEach(button => button.addEventListener("click", () => {
        modoVistaPaginas = button.dataset.pagesView;
        try { localStorage.setItem(PAGINAS_VISTA_KEY, modoVistaPaginas); }
        catch (error) { console.error("No se pudo guardar la preferencia de vista:", error); }
        abrirCarpeta(carpeta.id);
      }));
      document.getElementById("paginas-agregar-enlace").addEventListener("click", event => {
        const form = document.getElementById("vista-formulario-enlace");
        form.hidden = !form.hidden;
        event.currentTarget.setAttribute("aria-expanded", String(!form.hidden));
        if (!form.hidden) form.elements.titulo.focus();
      });
      renderEnlaces(carpeta);
      document.getElementById("vista-formulario-enlace").addEventListener("submit", event => {
        event.preventDefault();
        const formData = new FormData(event.currentTarget);
        const titulo = String(formData.get("titulo") || "").trim();
        const rawUrl = String(formData.get("url") || "").trim();
        let parsed;
        try { parsed = new URL(rawUrl); } catch (error) { window.alert("Escribe una dirección web válida."); return; }
        if (!["http:", "https:"].includes(parsed.protocol)) { window.alert("Solo se permiten enlaces http o https."); return; }
        if (!titulo) { window.alert("Escribe un nombre para el enlace."); return; }
        carpeta.enlaces.push({ id: makeId(), titulo, url: parsed.href });
        if (!guardarCarpetas()) {
          carpeta.enlaces.pop();
          return;
        }
        renderEnlaces(carpeta);
        event.currentTarget.reset();
        event.currentTarget.hidden = true;
        document.getElementById("paginas-agregar-enlace").setAttribute("aria-expanded", "false");
      });
    }

    function abrirMateria(carpetaId, materiaId) {
      cerrarVistaPdfEditor();
      detenerParallaxFondoEditor();
      const carpeta = carpetas.find(item => item.id === carpetaId);
      const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaId);
      if (!esCarpetaDeEstudio(carpeta) || !materia) return;
      if (vistaActual === "pagina") guardarPaginaAbierta();
      elementoGaleriaActivoId = null;
      if (carpetaActualId !== carpetaId || materiaActivaId !== materiaId) {
        filtroPaginasTexto = "";
      }
      detenerCorrector();
      carpetaActualId = carpetaId;
      materiaActivaId = materiaId;
      paginaActivaId = null;
      vistaActual = "materia";
      document.getElementById("search-wrap").hidden = false;
      search.value = "";
      actualizarNavegacion();
      view.innerHTML = `
        <section class="paginas-vista pagina-vista-organizada">
          <div class="paginas-encabezado">
            <button class="paginas-volver" type="button" data-volver-materias="${escapeHtml(carpeta.id)}">← ${escapeHtml(carpeta.titulo || "Sin título")}</button>
            <h1 class="carpetas-encabezado">${escapeHtml(materia.titulo)}</h1>
          </div>
          <div class="carpetas-filtro-contenedor paginas-filtro-contenedor">
            <button class="carpetas-filtro-boton" id="paginas-filtro-boton" type="button" aria-expanded="${filtroPaginasTexto ? "true" : "false"}" aria-controls="paginas-filtro">
              Filtrar
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M4 6h16M7 12h10m-7 6h4"/><circle cx="8" cy="6" r="1"/><circle cx="15" cy="12" r="1"/><circle cx="11" cy="18" r="1"/></svg>
            </button>
            <div class="paginas-lista-controles">
              <label class="carpetas-filtro" id="paginas-filtro" ${filtroPaginasTexto ? "" : "hidden"}>
                <span class="carpetas-solo-lector">Filtrar páginas por título, autor o PDF</span>
                <input id="pagina-filtro" type="search" placeholder="Buscar páginas, autor o PDF..." aria-label="Filtrar páginas por título, autor o PDF" autocomplete="off" value="${escapeHtml(filtroPaginasTexto)}">
              </label>
              <div class="paginas-vista-selector" role="group" aria-label="Vista de páginas">
                <button class="paginas-vista-boton" type="button" data-pages-view="cuadricula" aria-label="Vista de cuadrícula" aria-pressed="${modoVistaPaginas === "cuadricula"}" title="Vista de cuadrícula"><span aria-hidden="true">▦</span><span>Cuadrícula</span></button>
                <button class="paginas-vista-boton" type="button" data-pages-view="lista" aria-label="Vista de lista" aria-pressed="${modoVistaPaginas === "lista"}" title="Vista de lista"><span aria-hidden="true">☰</span><span>Lista</span></button>
              </div>
            </div>
          </div>
          <div class="paginas-grupos" id="pagina-lista"></div>
        </section>`;
      renderPaginas(materia);
      document.getElementById("pagina-filtro").addEventListener("input", event => {
        filtroPaginasTexto = event.currentTarget.value;
        renderPaginas(materia);
      });
      document.getElementById("paginas-filtro-boton").addEventListener("click", event => {
        const filter = document.getElementById("paginas-filtro");
        filter.hidden = !filter.hidden;
        event.currentTarget.setAttribute("aria-expanded", String(!filter.hidden));
        if (!filter.hidden) document.getElementById("pagina-filtro").focus();
        else {
          filtroPaginasTexto = "";
          document.getElementById("pagina-filtro").value = "";
          renderPaginas(materia);
        }
      });
      view.querySelectorAll("[data-pages-view]").forEach(button => button.addEventListener("click", () => {
        modoVistaPaginas = button.dataset.pagesView;
        view.querySelectorAll("[data-pages-view]").forEach(control => {
          control.setAttribute("aria-pressed", String(control.dataset.pagesView === modoVistaPaginas));
        });
        try { localStorage.setItem(PAGINAS_VISTA_KEY, modoVistaPaginas); }
        catch (error) { console.error("No se pudo guardar la preferencia de vista de páginas:", error); }
        renderPaginas(materia);
      }));
      view.onchange = async event => {
        const input = event.target.closest("[data-pagina-portada]");
        if (input?.files?.[0]) await cambiarPortadaPagina(materia, input);
        const color = event.target.closest("[data-pagina-color]");
        if (!color) return;
        const pagina = materia.paginas.find(item => item.id === color.dataset.paginaColor);
        if (!pagina) return;
        const anterior = pagina.color;
        pagina.color = color.value;
        if (!guardarCarpetas()) pagina.color = anterior;
        renderPaginas(materia);
      };
    }

    function renderPaginas(materia) {
      const list = document.getElementById("pagina-lista");
      if (!list) return;
      const paginasMateria = paginasDeMateria(materia);
      const busqueda = filtroPaginasTexto.trim().toLocaleLowerCase("es");
      const paginasVisibles = paginasMateria.filter(pagina => [
        tituloTarjetaPagina(pagina), pagina.pageTitle, pagina.pdf?.title, pagina.pdf?.author
      ].some(valor => (valor || "").toLocaleLowerCase("es").includes(busqueda)));
      const renderTarjeta = pagina => {
        const id = escapeHtml(pagina.id);
        const tituloTexto = tituloTarjetaPagina(pagina);
        const titulo = escapeHtml(tituloTexto);
        const tienePortada = Boolean(pagina.portada);
        const actualizado = Number.isFinite(pagina.actualizado)
          ? new Date(pagina.actualizado).toLocaleString("es", { dateStyle: "medium", timeStyle: "short" })
          : "";
        const autor = escapeHtml(pagina.pdf?.author || "Autor sin indicar");
        const tituloPdf = escapeHtml(pagina.pdf?.title || "Sin PDF adjunto");
        const opciones = `<div class="paginas-accion-opciones">
            <button class="paginas-boton-opciones" type="button" data-pagina-opciones="${id}" aria-label="Opciones de ${titulo}" aria-expanded="false" aria-controls="menu-pagina-${id}">⋯</button>
            <div class="paginas-menu-opciones" id="menu-pagina-${id}" hidden>
              <button type="button" data-renombrar-pagina="${id}">Renombrar tarjeta</button>
              <button type="button" data-cambiar-portada-pagina="${id}">Cambiar portada</button>
              <input type="file" accept="image/*" data-pagina-portada="${id}" aria-label="Elegir portada de ${titulo}">
              <label class="paginas-color-opcion">Cambiar color<input type="color" value="${escapeHtml(pagina.color)}" data-pagina-color="${id}" aria-label="Elegir color de ${titulo}"></label>
              <button type="button" data-quitar-portada-pagina="${id}" ${tienePortada ? "" : "disabled"}>Quitar portada</button>
              <button type="button" data-eliminar-pagina-tarjeta="${id}">Eliminar</button>
            </div>
          </div>`;
        if (modoVistaPaginas === "lista") {
          return `<article class="pagina-lista-item" role="listitem" data-pagina-fila="${id}" draggable="true">
            <button class="pagina-lista-abrir" type="button" data-abrir-pagina="${id}" aria-label="Abrir página: ${titulo}">
              <span class="pagina-lista-titulo">${titulo}</span>
              <span class="pagina-lista-autor">${pagina.pdf?.author ? `Autor: ${autor}` : "Sin autor"}</span>
              <span class="pagina-lista-pdf">${pagina.pdf?.title ? `PDF: ${tituloPdf}` : "Sin PDF adjunto"}</span>
              <time class="pagina-lista-fecha">${escapeHtml(actualizado)}</time>
              <span class="pagina-lista-acceso">Abrir →</span>
            </button>${opciones}
          </article>`;
        }
        const tituloTarjeta = tienePortada
          ? `<span class="carpetas-superposicion" aria-hidden="true"><span class="carpetas-titulo carpetas-titulo-con-portada">${titulo}</span></span>`
          : `<span class="carpetas-superposicion" aria-hidden="true"><span class="carpetas-titulo carpetas-titulo-sin-portada">${titulo}</span></span>`;
        const imagen = tienePortada
          ? `<img class="carpetas-portada" src="${escapeHtml(pagina.portada)}" alt="Portada de ${titulo}" loading="lazy">`
          : "";
        return `<article class="paginas-tarjeta-item" role="listitem" data-pagina-fila="${id}" draggable="true">
          <button class="carpetas-tarjeta ${tienePortada ? "carpetas-tarjeta-con-portada" : ""}" type="button" data-abrir-pagina="${id}" aria-label="Abrir página: ${titulo}" style="background-color:${escapeHtml(pagina.color)}">
            ${imagen}${tituloTarjeta}<span class="carpetas-accion" aria-hidden="true">Abrir página</span>
          </button>
          <p class="paginas-fecha">${pagina.pdf?.author ? `Autor: ${escapeHtml(pagina.pdf.author)} · ` : ""}Editada ${escapeHtml(actualizado)}${pagina.pdf?.title ? ` · PDF: ${escapeHtml(pagina.pdf.title)}` : ""}</p>
          ${opciones}
        </article>`;
      };
      const nombresCuatrimestre = [
        { id: "primer-cuatrimestre", nombre: "Primer Cuatrimestre" },
        { id: "segundo-cuatrimestre", nombre: "Segundo Cuatrimestre" }
      ];
      const unidades = ["Unidad 1", "Unidad 2", "Unidad 3", "Unidad 4"];
      const esSinOrganizar = pagina =>
        !nombresCuatrimestre.some(cuatrimestre => cuatrimestre.id === pagina.cuatrimestre) ||
        !unidades.includes(pagina.unidad);
      const paginasSinOrganizar = paginasMateria.filter(esSinOrganizar);
      const guardado = pestanaPaginasPorMateria.get(materia.id);
      const estadoInicial = guardado && (
        guardado.cuatrimestre === "sin-organizar" ||
        nombresCuatrimestre.some(cuatrimestre => cuatrimestre.id === guardado.cuatrimestre)
      ) ? guardado : { cuatrimestre: "primer-cuatrimestre", unidad: "todas" };
      const pestanaActiva = estadoInicial.cuatrimestre;
      const unidadActiva = unidades.includes(estadoInicial.unidad) ? estadoInicial.unidad : "todas";
      const paginasDelCuatrimestre = paginasMateria.filter(pagina => pagina.cuatrimestre === pestanaActiva && unidades.includes(pagina.unidad));
      const paginasDelFiltro = pestanaActiva === "sin-organizar"
        ? paginasSinOrganizar
        : paginasDelCuatrimestre.filter(pagina => unidadActiva === "todas" || pagina.unidad === unidadActiva);
      const paginasMostradas = paginasDelFiltro.filter(pagina => paginasVisibles.includes(pagina));
      const tieneUnidades = pestanaActiva !== "sin-organizar" && paginasDelCuatrimestre.length > 0;
      const opcionesPestanas = [
        ...nombresCuatrimestre.map(cuatrimestre => ({
          ...cuatrimestre,
          cantidad: paginasMateria.filter(pagina => pagina.cuatrimestre === cuatrimestre.id && unidades.includes(pagina.unidad)).length
        })),
        { id: "sin-organizar", nombre: "Sin organizar", cantidad: paginasSinOrganizar.length }
      ];
      const atributoUnidad = unidadActiva === "todas" ? "Unidad 1" : unidadActiva;
      const terminoCrear = pestanaActiva === "sin-organizar" ? "" : pestanaActiva;
      const unidadCrear = pestanaActiva === "sin-organizar" ? "" : atributoUnidad;
      const botonCrear = `<button class="paginas-crear-en-tab" type="button" data-crear-pagina data-page-term="${terminoCrear}" data-page-unit="${unidadCrear}">＋ Crear página</button>`;
      const contenidoPanel = paginasMostradas.length
        ? `<div class="pagina-panel-contenido">
            <div class="pagina-pestanas-grid ${modoVistaPaginas === "lista" ? "paginas-grilla-vista-lista" : ""}" role="list">
              ${paginasMostradas.map(renderTarjeta).join("")}
            </div>
            <div class="paginas-pie-acciones">${botonCrear}</div>
          </div>`
        : `<div class="pagina-panel-contenido paginas-estado-vacio">
            <span class="paginas-estado-vacio-icono" aria-hidden="true">▧</span>
            <p class="paginas-estado-vacio-texto">Todavía no hay páginas acá</p>
            ${botonCrear}
          </div>`;
      list.innerHTML = `
        <div class="paginas-organizacion">
          <div class="paginas-pestanas" role="tablist" aria-label="Cuatrimestres y organización">
            ${opcionesPestanas.map(pestana => `<button class="paginas-pestana" id="tab-paginas-${pestana.id}" type="button" role="tab" data-paginas-tab="${pestana.id}" aria-selected="${pestana.id === pestanaActiva}" aria-controls="pagina-tabpanel" tabindex="${pestana.id === pestanaActiva ? "0" : "-1"}">${pestana.nombre}<span class="paginas-pestana-conteo">${pestana.cantidad}</span></button>`).join("")}
          </div>
          <div class="paginas-unidades" role="group" aria-label="Filtrar por unidad" ${tieneUnidades ? "" : "hidden"}>
            <button class="paginas-unidad-chip" type="button" data-paginas-unidad="todas" aria-pressed="${unidadActiva === "todas"}">Todas</button>
            ${unidades.map(unidad => `<button class="paginas-unidad-chip" type="button" data-paginas-unidad="${unidad}" aria-pressed="${unidadActiva === unidad}">${unidad}</button>`).join("")}
          </div>
          <div class="pagina-organizacion-panel" id="pagina-tabpanel" role="tabpanel" aria-labelledby="tab-paginas-${pestanaActiva}">
            ${contenidoPanel}
          </div>
        </div>`;
      list.onclick = event => {
        const tab = event.target.closest("[data-paginas-tab]");
        if (tab) {
          pestanaPaginasPorMateria.set(materia.id, { cuatrimestre: tab.dataset.paginasTab, unidad: "todas" });
          renderPaginas(materia);
          list.querySelector(`[data-paginas-tab="${CSS.escape(tab.dataset.paginasTab)}"]`)?.focus();
          return;
        }
        const chip = event.target.closest("[data-paginas-unidad]");
        if (chip) {
          pestanaPaginasPorMateria.set(materia.id, { cuatrimestre: pestanaActiva, unidad: chip.dataset.paginasUnidad });
          renderPaginas(materia);
          list.querySelector(`[data-paginas-unidad="${CSS.escape(chip.dataset.paginasUnidad)}"]`)?.focus();
        }
      };
      list.onkeydown = event => {
        const tab = event.target.closest("[data-paginas-tab]");
        if (!tab || !["ArrowLeft", "ArrowRight", "Home", "End"].includes(event.key)) return;
        event.preventDefault();
        const tabs = Array.from(list.querySelectorAll("[data-paginas-tab]"));
        const index = tabs.indexOf(tab);
        const siguiente = event.key === "Home" ? 0
          : event.key === "End" ? tabs.length - 1
            : (index + (event.key === "ArrowRight" ? 1 : -1) + tabs.length) % tabs.length;
        tabs[siguiente].click();
      };
      let paginaArrastradaId = null;
      list.ondragstart = event => {
        const fila = event.target.closest("[data-pagina-fila]");
        if (!fila) return;
        paginaArrastradaId = fila.dataset.paginaFila;
        fila.classList.add("pagina-arrastrando");
        event.dataTransfer.effectAllowed = "move";
        event.dataTransfer.setData("text/plain", paginaArrastradaId);
      };
      list.ondragover = event => {
        const fila = event.target.closest("[data-pagina-fila]");
        if (!fila || !paginaArrastradaId || fila.dataset.paginaFila === paginaArrastradaId) return;
        event.preventDefault();
        list.querySelectorAll(".pagina-destino").forEach(item => item.classList.remove("pagina-destino"));
        fila.classList.add("pagina-destino");
      };
      list.ondrop = event => {
        const fila = event.target.closest("[data-pagina-fila]");
        if (!fila || !paginaArrastradaId) return;
        event.preventDefault();
        const desde = paginasMateria.findIndex(pagina => pagina.id === paginaArrastradaId);
        if (desde < 0 || paginasMateria.findIndex(pagina => pagina.id === fila.dataset.paginaFila) < 0) return;
        if (paginaArrastradaId === fila.dataset.paginaFila) return;
        const rect = fila.getBoundingClientRect();
        const insertarDespues = event.clientY >= rect.top + rect.height / 2;
        const paginasAnteriores = [...paginasMateria];
        const [pagina] = paginasMateria.splice(desde, 1);
        const indiceDestino = paginasMateria.findIndex(item => item.id === fila.dataset.paginaFila);
        paginasMateria.splice(indiceDestino + (insertarDespues ? 1 : 0), 0, pagina);
        if (!guardarCarpetas()) materia.paginas = paginasAnteriores;
        renderPaginas(materia);
      };
      list.ondragend = () => {
        paginaArrastradaId = null;
        list.querySelectorAll(".pagina-arrastrando, .pagina-destino").forEach(fila => fila.classList.remove("pagina-arrastrando", "pagina-destino"));
      };
    }

    async function leerArchivoComoDataURL(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => typeof reader.result === "string"
          ? resolve(reader.result)
          : reject(new Error("No se pudo leer la imagen."));
        reader.onerror = () => reject(reader.error || new Error("No se pudo leer la imagen."));
        reader.readAsDataURL(file);
      });
    }

    async function comprimirImagenDataURL(file, dimensionMaxima = 2200, calidad = 0.88, conservarOriginal = false, limiteCaracteres = Infinity, formatoSalida = "image/jpeg", alturaMaxima = dimensionMaxima) {
      const bitmap = await createImageBitmap(file);
      try {
        const scale = Math.min(1, dimensionMaxima / bitmap.width, alturaMaxima / bitmap.height);
        if (conservarOriginal && formatoSalida === file.type && scale === 1) {
          const original = await leerArchivoComoDataURL(file);
          if (original.length <= limiteCaracteres) return original;
        }
        const canvas = document.createElement("canvas");
        canvas.width = Math.max(1, Math.round(bitmap.width * scale));
        canvas.height = Math.max(1, Math.round(bitmap.height * scale));
        const context = canvas.getContext("2d");
        if (!context) throw new Error("No se pudo preparar la imagen.");
        context.imageSmoothingEnabled = true;
        context.imageSmoothingQuality = "high";
        context.drawImage(bitmap, 0, 0, canvas.width, canvas.height);
        const dataURL = canvas.toDataURL(formatoSalida, calidad);
        if (!dataURL.startsWith(`data:${formatoSalida};`)) {
          throw new Error(`Este navegador no pudo convertir la imagen a ${formatoSalida}.`);
        }
        return dataURL;
      } finally {
        bitmap.close();
      }
    }

    async function cambiarPortadaMateria(input) {
      const materiaId = input.dataset.materiaPortada;
      const carpeta = carpetas.find(item => materiasDeCarpeta(item).some(materia => materia.id === materiaId));
      const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaId);
      const file = input.files?.[0];
      if (!carpeta || !materia || !file) return;
      if (!file.type.startsWith("image/")) {
        window.alert("Elige un archivo de imagen.");
        input.value = "";
        return;
      }
      if (file.size > 15 * 1024 * 1024) {
        window.alert("La imagen debe ocupar menos de 15 MB.");
        input.value = "";
        return;
      }
      const anterior = materia.portada;
      try {
        materia.portada = await comprimirImagenDataURL(file, 1200, 0.78);
        if (materia.portada.length > 1400000) {
          materia.portada = anterior;
          window.alert("La imagen sigue siendo demasiado grande; intenta con otra.");
          return;
        }
        if (!guardarCarpetas()) {
          materia.portada = anterior;
          return;
        }
        abrirCarpeta(carpeta.id);
      } catch (error) {
        materia.portada = anterior;
        console.error("No se pudo procesar la portada de la materia:", error);
        window.alert("No se pudo abrir la imagen. Prueba con otro archivo.");
      } finally {
        input.value = "";
      }
    }

    async function cambiarPortadaPagina(materia, input) {
      const pagina = paginasDeMateria(materia).find(item => item.id === input.dataset.paginaPortada);
      const file = input.files?.[0];
      if (!pagina || !file) return;
      if (!file.type.startsWith("image/")) {
        window.alert("Elige un archivo de imagen.");
        input.value = "";
        return;
      }
      if (file.size > 15 * 1024 * 1024) {
        window.alert("La imagen debe ocupar menos de 15 MB.");
        input.value = "";
        return;
      }
      try {
        const anterior = pagina.portada;
        pagina.portada = await comprimirImagenDataURL(file);
        if (pagina.portada.length > 1400000) {
          pagina.portada = anterior;
          window.alert("La imagen comprimida sigue siendo demasiado grande. Prueba con otra.");
          return;
        }
        if (!guardarCarpetas()) pagina.portada = anterior;
        if (vistaActual === "inicio") renderHome(inicioDashboard ? null : seccionActivaId);
        else renderPaginas(materia);
      } catch (error) {
        console.error("No se pudo procesar la portada de la página:", error);
        window.alert("No se pudo abrir la imagen. Prueba con otro archivo.");
      } finally {
        input.value = "";
      }
    }

    function actualizarPdfAdjuntoEditor(pagina) {
      const status = document.querySelector("[data-page-pdf-status]");
      const remove = document.querySelector("[data-page-pdf-remove]");
      const section = document.querySelector("[data-page-pdf-viewer]");
      const title = document.querySelector("[data-page-pdf-title]");
      const author = document.querySelector("[data-page-pdf-author]");
      const download = section?.querySelector("[data-page-pdf-download]");
      if (!status || !remove || !section || !title || !author || !download) return;
      status.textContent = pagina.pdf
        ? `${pagina.pdf.title || pagina.pdf.fileName}${pagina.pdf.author ? ` · ${pagina.pdf.author}` : ""}`
        : "No hay un PDF adjunto.";
      remove.hidden = !pagina.pdf;
      section.hidden = !pagina.pdf;
      title.textContent = pagina.pdf?.title || pagina.pdf?.fileName || "";
      author.textContent = pagina.pdf?.author ? `Autor: ${pagina.pdf.author}` : "";
      download.dataset.pagePdfDownload = pagina.pdf?.key || "";
      download.hidden = !pagina.pdf;
      if (!pagina.pdf) cerrarVistaPdfEditor();
    }

    async function cambiarPdfPaginaEditor(input, pagina) {
      const file = input.files?.[0];
      if (!file) return;
      if ((file.type && file.type !== "application/pdf") || !/\.pdf$/i.test(file.name)) {
        window.alert("Elige un archivo PDF.");
        input.value = "";
        return;
      }
      if (file.size > 25 * 1024 * 1024) {
        window.alert("El PDF debe ocupar 25 MB o menos.");
        input.value = "";
        return;
      }
      const titleDefault = file.name.replace(/\.pdf$/i, "").trim() || "Material PDF";
      const metadata = await pedirDatosDialogo("Datos del PDF", [
        { nombre: "title", etiqueta: "Título del PDF", valor: titleDefault, maxlength: 160 },
        { nombre: "author", etiqueta: "Nombre del autor", valor: "", requerido: false, maxlength: 120 }
      ]);
      if (!metadata) {
        input.value = "";
        return;
      }
      const title = String(metadata.title || "").trim();
      if (!title) {
        window.alert("El título del PDF no puede quedar vacío.");
        input.value = "";
        return;
      }
      const previous = pagina.pdf;
      const previousUpdated = pagina.actualizado;
      const key = makeId();
      try {
        await guardarImagenEnBaseDatos(key, file);
        pagina.pdf = { key, title, author: String(metadata.author || "").trim(), fileName: file.name };
        pagina.actualizado = Date.now();
        if (!guardarCarpetas()) {
          pagina.pdf = previous || null;
          pagina.actualizado = previousUpdated;
          await limpiarImagenesHuerfanas();
          return;
        }
        cerrarVistaPdfEditor();
        actualizarPdfAdjuntoEditor(pagina);
      } catch (error) {
        pagina.pdf = previous || null;
        pagina.actualizado = previousUpdated;
        console.error("No se pudo guardar el PDF de la página:", error);
        window.alert("No se pudo guardar el PDF. Revisa el espacio disponible e inténtalo de nuevo.");
      } finally {
        input.value = "";
      }
    }

    function cerrarVistaPdfEditor() {
      const frame = document.querySelector("[data-page-pdf-frame]");
      const button = document.querySelector("[data-page-pdf-open]");
      if (frame) {
        frame.removeAttribute("src");
        frame.hidden = true;
      }
      if (button) {
        button.textContent = "Abrir PDF";
        button.setAttribute("aria-expanded", "false");
      }
      if (pdfVistaUrlEditor) {
        URL.revokeObjectURL(pdfVistaUrlEditor);
        pdfVistaUrlEditor = null;
      }
    }

    async function alternarVistaPdfEditor(button, pagina) {
      const frame = document.querySelector("[data-page-pdf-frame]");
      if (!frame || !pagina.pdf) return;
      if (!frame.hidden) {
        cerrarVistaPdfEditor();
        return;
      }
      const pdfKey = pagina.pdf.key;
      try {
        const blob = await leerImagenDeBaseDatos(pdfKey);
        if (!blob) {
          window.alert("No se encontró el archivo PDF adjunto.");
          return;
        }
        const paginaActual = paginaActualAbierta();
        if (vistaActual !== "pagina" || paginaActivaId !== pagina.id || pagina.pdf?.key !== pdfKey ||
          paginaActual?.pdf?.key !== pdfKey || !frame.isConnected || document.querySelector("[data-page-pdf-frame]") !== frame ||
          !button.isConnected || document.querySelector("[data-page-pdf-open]") !== button) return;
        cerrarVistaPdfEditor();
        pdfVistaUrlEditor = URL.createObjectURL(blob);
        frame.src = pdfVistaUrlEditor;
        frame.hidden = false;
        button.textContent = "Cerrar PDF";
        button.setAttribute("aria-expanded", "true");
      } catch (error) {
        console.error("No se pudo mostrar el PDF adjunto:", error);
        window.alert("No se pudo abrir el archivo PDF.");
      }
    }

    async function descargarPdfPaginaEditor(button) {
      const key = button.dataset.pagePdfDownload;
      const pagina = paginaActualAbierta();
      if (!pagina?.pdf || pagina.pdf.key !== key || !/^[a-z0-9-]+$/i.test(key || "")) return;
      try {
        const blob = await leerImagenDeBaseDatos(key);
        if (!blob) {
          window.alert("No se encontró el archivo PDF adjunto.");
          return;
        }
        const url = URL.createObjectURL(blob);
        const link = document.createElement("a");
        link.href = url;
        link.download = pagina.pdf.fileName || `${pagina.pdf.title || "material"}.pdf`;
        link.click();
        window.setTimeout(() => URL.revokeObjectURL(url), 60000);
      } catch (error) {
        console.error("No se pudo descargar el PDF adjunto:", error);
        window.alert("No se pudo descargar el archivo PDF.");
      }
    }

    async function quitarPdfPaginaEditor(pagina) {
      if (!pagina.pdf || !window.confirm(`¿Quitar el PDF «${pagina.pdf.title || pagina.pdf.fileName}»?`)) return;
      const previous = pagina.pdf;
      const previousUpdated = pagina.actualizado;
      pagina.pdf = null;
      pagina.actualizado = Date.now();
      if (!guardarCarpetas()) {
        pagina.pdf = previous;
        pagina.actualizado = previousUpdated;
        return;
      }
      actualizarPdfAdjuntoEditor(pagina);
    }

    function detenerParallaxFondoEditor() {
      fondoCargaVersion += 1;
      if (manejarDesplazamientoFondoEditor) {
        window.removeEventListener("scroll", manejarDesplazamientoFondoEditor);
        manejarDesplazamientoFondoEditor = null;
      }
      if (manejarCambioMovimientoReducidoFondo) {
        window.matchMedia("(prefers-reduced-motion: reduce)").removeEventListener("change", manejarCambioMovimientoReducidoFondo);
        manejarCambioMovimientoReducidoFondo = null;
      }
      if (animacionFondoEditorPendiente) {
        window.cancelAnimationFrame(animacionFondoEditorPendiente);
        animacionFondoEditorPendiente = 0;
      }
      fondoCapaEditor?.remove();
      fondoCapaEditor = null;
      if (fondoGifUrlEditor) {
        URL.revokeObjectURL(fondoGifUrlEditor);
        fondoGifUrlEditor = null;
      }
      document.querySelector(".app")?.classList.remove("fondo-imagen-activa", "lectura-tono-claro", "lectura-tono-oscuro");
    }

    function tonoEfectivoPanelLectura(pagina) {
      const modo = ["claro", "oscuro"].includes(pagina.fondoTonoPanel) ? pagina.fondoTonoPanel : "automatico";
      if (modo === "automatico") return pagina.fondoTonoAutomatico === "oscuro" ? "oscuro" : "claro";
      return modo;
    }

    function aplicarEstiloPanelLectura(pagina) {
      const panel = document.getElementById("lectura-panel");
      const app = document.querySelector(".app");
      if (!panel || !app) return;
      const tono = tonoEfectivoPanelLectura(pagina);
      panel.classList.remove("lectura-tono-claro", "lectura-tono-oscuro");
      panel.classList.add(`lectura-tono-${tono}`);
      const opacidad = Number.isFinite(pagina.fondoOpacidadPanel)
        ? Math.max(40, Math.min(100, Math.round(pagina.fondoOpacidadPanel)))
        : 85;
      panel.style.setProperty("--lectura-opacidad-panel", `${opacidad}%`);
      const controlOpacidad = document.getElementById("editor-panel-opacidad");
      const valorOpacidad = document.getElementById("editor-panel-opacidad-valor");
      if (controlOpacidad) controlOpacidad.value = String(opacidad);
      if (valorOpacidad) valorOpacidad.value = `${opacidad}%`;
      const controlTono = document.getElementById("editor-panel-tono");
      if (controlTono) controlTono.value = ["claro", "oscuro"].includes(pagina.fondoTonoPanel) ? pagina.fondoTonoPanel : "automatico";
      app.classList.toggle("lectura-tono-claro", tono === "claro");
      app.classList.toggle("lectura-tono-oscuro", tono === "oscuro");
    }

    function fondosVectorialesEditor() {
      return [
        { id: "cuadricula", nombre: "Cuadrícula", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 160 160"><path d="M0 0h160v160H0zM40 0v160M80 0v160M120 0v160M0 40h160M0 80h160M0 120h160" fill="none" stroke="currentColor" stroke-width="1.5"/></svg>' },
        { id: "ondas", nombre: "Ondas", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 240 160"><path d="M-20 35c30-30 50-30 80 0s50 30 80 0 50-30 80 0M-20 80c30-30 50-30 80 0s50 30 80 0 50-30 80 0M-20 125c30-30 50-30 80 0s50 30 80 0 50-30 80 0" fill="none" stroke="currentColor" stroke-width="3"/></svg>' },
        { id: "circulos", nombre: "Círculos", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 180 180"><circle cx="35" cy="35" r="24" fill="none" stroke="currentColor" stroke-width="5"/><circle cx="120" cy="65" r="42" fill="none" stroke="currentColor" stroke-width="5"/><circle cx="55" cy="145" r="32" fill="none" stroke="currentColor" stroke-width="5"/></svg>' },
        { id: "lineas", nombre: "Líneas", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 180 180"><path d="M-40 180 140 0M-10 210 170 30M20 240 200 60M50 270 230 90" fill="none" stroke="currentColor" stroke-width="3"/></svg>' },
        { id: "puntos", nombre: "Puntos", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 120 120"><circle cx="15" cy="15" r="4" fill="currentColor"/><circle cx="60" cy="15" r="4" fill="currentColor"/><circle cx="105" cy="15" r="4" fill="currentColor"/><circle cx="15" cy="60" r="4" fill="currentColor"/><circle cx="60" cy="60" r="4" fill="currentColor"/><circle cx="105" cy="60" r="4" fill="currentColor"/><circle cx="15" cy="105" r="4" fill="currentColor"/><circle cx="60" cy="105" r="4" fill="currentColor"/><circle cx="105" cy="105" r="4" fill="currentColor"/></svg>' },
        { id: "hexagonos", nombre: "Hexágonos", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 180 156"><path d="m45 3 42 24v48L45 99 3 75V27zm90 54 42 24v48l-42 24-42-24V81z" fill="none" stroke="currentColor" stroke-width="4"/></svg>' },
        { id: "abstracto", nombre: "Abstracto", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 160"><path d="M18 118c-8-37 26-87 67-86 42 1 42 34 88 22 15 38-17 91-58 99-35 7-79 5-97-35z" fill="currentColor" opacity=".32"/><circle cx="55" cy="48" r="18" fill="none" stroke="currentColor" stroke-width="4"/><path d="m130 30 35 54h-70z" fill="none" stroke="currentColor" stroke-width="4"/></svg>' },
        { id: "arcos", nombre: "Arcos", svg: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 180 180"><path d="M15 180V90a75 75 0 0 1 150 0v90M40 180V90a50 50 0 0 1 100 0v90M65 180V90a25 25 0 0 1 50 0v90" fill="none" stroke="currentColor" stroke-width="4"/></svg>' }
      ];
    }

    function sanitizarSvgFondo(source) {
      if (typeof source !== "string" || !source.trim() || source.length > 100000) {
        throw new Error("El SVG está vacío o supera el límite de 100 KB.");
      }
      const documentSvg = new DOMParser().parseFromString(source, "image/svg+xml");
      const root = documentSvg.documentElement;
      if (root.tagName.toLocaleLowerCase("en") !== "svg" || documentSvg.querySelector("parsererror")) {
        throw new Error("El archivo no contiene un SVG válido.");
      }
      if (root.namespaceURI && root.namespaceURI !== "http://www.w3.org/2000/svg") {
        throw new Error("El SVG usa un espacio de nombres no permitido.");
      }
      const svgSinEspacioDeNombres = !root.namespaceURI;
      if (!root.namespaceURI) root.setAttribute("xmlns", "http://www.w3.org/2000/svg");
      const etiquetasPermitidas = new Set(["svg", "g", "defs", "path", "rect", "circle", "ellipse", "line", "polyline", "polygon", "lineargradient", "radialgradient", "stop", "clippath", "mask", "pattern", "symbol", "use"]);
      const atributosPermitidos = new Set(["xmlns", "viewBox", "width", "height", "preserveAspectRatio", "id", "href", "xlink:href", "fill", "stroke", "stroke-width", "stroke-linecap", "stroke-linejoin", "fill-rule", "clip-rule", "fill-opacity", "stroke-opacity", "opacity", "color", "transform", "d", "x", "y", "x1", "y1", "x2", "y2", "cx", "cy", "r", "rx", "ry", "points", "offset", "stop-color", "stop-opacity", "gradientUnits", "gradientTransform", "spreadMethod", "clipPathUnits", "maskUnits", "patternUnits", "patternContentUnits", "patternTransform"]);
      const propiedadesEstiloPermitidas = new Set(["fill", "stroke", "stroke-width", "stroke-linecap", "stroke-linejoin", "fill-rule", "clip-rule", "fill-opacity", "stroke-opacity", "opacity", "color", "stop-color", "stop-opacity"]);
      const valorSeguro = value => !/(?:javascript:|data:|https?:|\/\/|file:|@import|expression\s*\()/i.test(value) &&
        (!/url\s*\(/i.test(value) || /^url\(\s*['"]?#[\w.-]+['"]?\s*\)$/i.test(value));
      const limpiarAtributosSvg = element => {
        Array.from(element.attributes).forEach(attribute => {
          const name = attribute.name;
          if (/^on/i.test(name) || /^(?:src|xlink:href|href)$/i.test(name) && !/^#[\w.-]+$/.test(attribute.value)) {
            element.removeAttributeNode(attribute);
            return;
          }
          if (name.toLocaleLowerCase("en") === "style") {
            const styleSafe = attribute.value.split(";").map(declaration => declaration.trim()).filter(Boolean).filter(declaration => {
              const separator = declaration.indexOf(":");
              if (separator < 1) return false;
              const property = declaration.slice(0, separator).trim().toLocaleLowerCase("en");
              const value = declaration.slice(separator + 1).trim();
              return propiedadesEstiloPermitidas.has(property) && valorSeguro(value);
            }).join(";");
            if (styleSafe) element.setAttribute("style", styleSafe);
            else element.removeAttribute("style");
            return;
          }
          const isSvgNamespace = name === "xmlns" && attribute.value === "http://www.w3.org/2000/svg";
          if (!isSvgNamespace && (!atributosPermitidos.has(name) || !valorSeguro(attribute.value))) element.removeAttributeNode(attribute);
        });
      };
      const quitarNodosNoPermitidos = parent => {
        Array.from(parent.children).forEach(element => {
          const namespacePermitido = element.namespaceURI === "http://www.w3.org/2000/svg" || (svgSinEspacioDeNombres && !element.namespaceURI);
          if (!namespacePermitido || !etiquetasPermitidas.has(element.localName.toLocaleLowerCase("en"))) {
            element.remove();
            return;
          }
          limpiarAtributosSvg(element);
          quitarNodosNoPermitidos(element);
        });
      };
      limpiarAtributosSvg(root);
      quitarNodosNoPermitidos(root);
      const ids = new Set(Array.from(root.querySelectorAll("[id]"), element => element.id));
      root.querySelectorAll("*").forEach(element => {
        Array.from(element.attributes).forEach(attribute => {
          if (/^(?:href|xlink:href)$/i.test(attribute.name) && !ids.has(attribute.value.slice(1))) {
            element.removeAttributeNode(attribute);
            return;
          }
          if (/url\s*\(/i.test(attribute.value)) {
            const referencedId = attribute.value.match(/^url\(\s*['"]?#([\w.-]+)['"]?\s*\)$/i)?.[1];
            if (!referencedId || !ids.has(referencedId)) element.removeAttributeNode(attribute);
          }
        });
      });
      return new XMLSerializer().serializeToString(root);
    }

    function svgFondoConAjustes(source, color, opacity) {
      const documentSvg = new DOMParser().parseFromString(sanitizarSvgFondo(source), "image/svg+xml");
      const root = documentSvg.documentElement;
      const safeColor = /^#[0-9a-f]{6}$/i.test(color) ? color : "#64748B";
      const shapes = Array.from(root.querySelectorAll("[fill], [stroke], [stop-color]"));
      const solidColors = new Set();
      shapes.forEach(shape => {
        ["fill", "stroke", "stop-color"].forEach(attribute => {
          const value = shape.getAttribute(attribute);
          if (value && value.toLocaleLowerCase("en") !== "none" && value.toLocaleLowerCase("en") !== "currentcolor" && !/url\s*\(/i.test(value)) solidColors.add(value.toLocaleLowerCase("en"));
        });
      });
      root.setAttribute("color", safeColor);
      if (solidColors.size === 1) {
        const originalColor = Array.from(solidColors)[0];
        shapes.forEach(shape => {
          ["fill", "stroke", "stop-color"].forEach(attribute => {
            const value = shape.getAttribute(attribute);
            if (value?.toLocaleLowerCase("en") === originalColor) shape.setAttribute(attribute, safeColor);
          });
        });
      }
      const previousOpacity = Number.parseFloat(root.getAttribute("opacity") || "1");
      const vectorOpacity = Math.max(0, Math.min(100, Number(opacity))) / 100;
      root.setAttribute("opacity", String((Number.isFinite(previousOpacity) ? previousOpacity : 1) * vectorOpacity));
      return new XMLSerializer().serializeToString(root);
    }

    function estadoFondoVectorial(pagina) {
      const settings = pagina.fondoVectorial;
      if (!settings || typeof settings.svg !== "string") return null;
      const color = /^#[0-9a-f]{6}$/i.test(settings.color) ? settings.color : "#64748B";
      const size = ["cover", "repeat", "scale"].includes(settings.size) ? settings.size : "cover";
      const position = ["center", "top", "bottom", "left", "right"].includes(settings.position) ? settings.position : "center";
      return {
        id: typeof settings.id === "string" ? settings.id : "custom",
        svg: sanitizarSvgFondo(settings.svg),
        color,
        opacity: Number.isFinite(settings.opacity) ? Math.max(0, Math.min(100, Math.round(settings.opacity))) : 45,
        size,
        scale: Number.isFinite(settings.scale) ? Math.max(25, Math.min(250, Math.round(settings.scale))) : 100,
        position,
        soften: Number.isFinite(settings.soften) ? Math.max(0, Math.min(80, Math.round(settings.soften))) : 45
      };
    }

    function aplicarFondoVectorialEditor(pagina) {
      const contenedor = document.querySelector(".pagina-editor");
      if (!contenedor) return;
      for (const variable of ["--editor-vector-image", "--editor-vector-size", "--editor-vector-repeat", "--editor-vector-position", "--editor-vector-suavizado"]) {
        contenedor.style.removeProperty(variable);
      }
      const settings = estadoFondoVectorial(pagina);
      if (!settings) return;
      const svg = svgFondoConAjustes(settings.svg, settings.color, settings.opacity);
      const position = { center: "center", top: "top center", bottom: "bottom center", left: "center left", right: "center right" }[settings.position];
      contenedor.style.setProperty("--editor-vector-image", `url("data:image/svg+xml;charset=utf-8,${encodeURIComponent(svg)}")`);
      contenedor.style.setProperty("--editor-vector-size", settings.size === "cover" ? "cover" : settings.size === "repeat" ? "120px 120px" : `${settings.scale}% auto`);
      contenedor.style.setProperty("--editor-vector-repeat", settings.size === "repeat" ? "repeat" : "no-repeat");
      contenedor.style.setProperty("--editor-vector-position", position);
      contenedor.style.setProperty("--editor-vector-suavizado", `${settings.soften}%`);
    }

    async function detectarTonoAutomaticoFondo(source) {
      const image = new Image();
      image.src = source;
      await image.decode();
      const canvas = document.createElement("canvas");
      canvas.width = 32;
      canvas.height = 32;
      const context = canvas.getContext("2d", { willReadFrequently: true });
      if (!context) throw new Error("No se pudo analizar el brillo de la imagen.");
      context.drawImage(image, 0, 0, 32, 32);
      const { data } = context.getImageData(0, 0, 32, 32);
      let luminancia = 0;
      for (let index = 0; index < data.length; index += 4) {
        const canales = [data[index], data[index + 1], data[index + 2]].map(value => {
          const srgb = value / 255;
          return srgb <= 0.04045 ? srgb / 12.92 : ((srgb + 0.055) / 1.055) ** 2.4;
        });
        luminancia += 0.2126 * canales[0] + 0.7152 * canales[1] + 0.0722 * canales[2];
      }
      const promedio = luminancia / (32 * 32);
      return { tono: promedio >= 0.35 ? "claro" : "oscuro", luminancia: promedio };
    }

    async function asegurarTonoAutomaticoFondo(pagina, recalcular = false) {
      if (!pagina.fondoImagen && !pagina.fondoGifKey) {
        aplicarEstiloPanelLectura(pagina);
        return;
      }
      if (!recalcular && ["claro", "oscuro"].includes(pagina.fondoTonoAutomatico) && Number.isFinite(pagina.fondoLuminanciaPromedio)) {
        aplicarEstiloPanelLectura(pagina);
        return;
      }
      const anterior = pagina.fondoTonoAutomatico;
      const luminanciaAnterior = pagina.fondoLuminanciaPromedio;
      let objectUrl = null;
      try {
        let source = pagina.fondoImagen;
        if (pagina.fondoGifKey) {
          const blob = await leerImagenDeBaseDatos(pagina.fondoGifKey);
          if (!blob) {
            aplicarEstiloPanelLectura(pagina);
            return;
          }
          objectUrl = URL.createObjectURL(blob);
          source = objectUrl;
        }
        const analisis = await detectarTonoAutomaticoFondo(source);
        pagina.fondoTonoAutomatico = analisis.tono;
        pagina.fondoLuminanciaPromedio = analisis.luminancia;
      } catch (error) {
        console.error("No se pudo detectar el tono automático del fondo:", error);
        pagina.fondoTonoAutomatico = "claro";
        pagina.fondoLuminanciaPromedio = 1;
      } finally {
        if (objectUrl) URL.revokeObjectURL(objectUrl);
      }
      aplicarEstiloPanelLectura(pagina);
      if (!guardarCarpetas()) {
        if (anterior === undefined) delete pagina.fondoTonoAutomatico;
        else pagina.fondoTonoAutomatico = anterior;
        if (luminanciaAnterior === undefined) delete pagina.fondoLuminanciaPromedio;
        else pagina.fondoLuminanciaPromedio = luminanciaAnterior;
        aplicarEstiloPanelLectura(pagina);
      }
    }

    async function aplicarFondoPaginaEditor(pagina) {
      const paginaId = pagina.id;
      detenerParallaxFondoEditor();
      const cargaVersion = fondoCargaVersion;
      aplicarEstiloPanelLectura(pagina);
      try {
        aplicarFondoVectorialEditor(pagina);
      } catch (error) {
        console.error("No se pudo aplicar el fondo vectorial de la página:", error);
        const status = document.getElementById("corrector-estado");
        if (status) {
          status.textContent = "No se pudo cargar el fondo vectorial guardado.";
          status.hidden = false;
        }
      }
      const source = typeof pagina.fondoImagen === "string" && /^data:image\/(?:jpeg|png|webp);base64,[a-z0-9+/=]+$/i.test(pagina.fondoImagen)
        ? pagina.fondoImagen
        : "";
      let fondoSource = source;
      let objectUrl = null;
      if (pagina.fondoGifKey) {
        try {
          const blob = await leerImagenDeBaseDatos(pagina.fondoGifKey);
          if (!blob) return;
          objectUrl = URL.createObjectURL(blob);
          fondoSource = objectUrl;
        } catch (error) {
          console.error("No se pudo leer el GIF de fondo guardado:", error);
          return;
        }
      }
      if (!fondoSource) return;
      if (cargaVersion !== fondoCargaVersion || vistaActual !== "pagina" || paginaActivaId !== paginaId) {
        if (objectUrl) URL.revokeObjectURL(objectUrl);
        return;
      }
      const app = document.querySelector(".app");
      if (!app) {
        if (objectUrl) URL.revokeObjectURL(objectUrl);
        return;
      }
      const layer = document.createElement("div");
      layer.className = "fondo-capa";
      layer.setAttribute("aria-hidden", "true");
      const image = document.createElement("img");
      image.className = "fondo-imagen";
      image.alt = "";
      image.src = fondoSource;
      layer.append(image);
      document.body.insertBefore(layer, app);
      fondoCapaEditor = layer;
      fondoGifUrlEditor = objectUrl;
      app.classList.add("fondo-imagen-activa");
      const movimientoReducido = window.matchMedia("(prefers-reduced-motion: reduce)");
      const aplicarDesplazamiento = () => {
        animacionFondoEditorPendiente = 0;
        const movimiento = pagina.fondoParallax !== false && !movimientoReducido.matches
          ? Math.min((window.scrollY || document.scrollingElement?.scrollTop || 0) * 0.4, 200)
          : 0;
        layer.style.transform = `translate3d(0, ${-Math.round(movimiento)}px, 0)`;
      };
      manejarDesplazamientoFondoEditor = () => {
        if (animacionFondoEditorPendiente) return;
        animacionFondoEditorPendiente = window.requestAnimationFrame(aplicarDesplazamiento);
      };
      if (pagina.fondoParallax !== false) window.addEventListener("scroll", manejarDesplazamientoFondoEditor, { passive: true });
      manejarCambioMovimientoReducidoFondo = manejarDesplazamientoFondoEditor;
      movimientoReducido.addEventListener("change", manejarCambioMovimientoReducidoFondo);
      aplicarDesplazamiento();
    }

    async function cambiarFondoPaginaEditor(input) {
      const pagina = paginaActualAbierta();
      const file = input.files?.[0];
      if (!pagina || !file) return;
      if (!file.type.startsWith("image/")) {
        window.alert("Elige un archivo de imagen.");
        input.value = "";
        return;
      }
      if (file.size > 15 * 1024 * 1024) {
        window.alert("La imagen debe ocupar menos de 15 MB.");
        input.value = "";
        return;
      }
      const anterior = pagina.fondoImagen;
      const gifAnterior = pagina.fondoGifKey;
      const tonoAutomaticoAnterior = pagina.fondoTonoAutomatico;
      const luminanciaAnterior = pagina.fondoLuminanciaPromedio;
      try {
        let fondoImagen = await comprimirImagenDataURL(file, 1920, 0.88, true, 2500000, "image/webp", 1080);
        if (fondoImagen.length > 2500000) {
          fondoImagen = await comprimirImagenDataURL(file, 1920, 0.88, false, 2500000, "image/webp", 1080);
          if (fondoImagen.length > 2500000) {
            window.alert("La imagen comprimida sigue siendo demasiado grande para guardarla en esta página. Prueba con otra.");
            return;
          }
        }
        const analisis = await detectarTonoAutomaticoFondo(fondoImagen);
        pagina.fondoTonoAutomatico = analisis.tono;
        pagina.fondoLuminanciaPromedio = analisis.luminancia;
        pagina.fondoImagen = fondoImagen;
        delete pagina.fondoGifKey;
        if (!guardarCarpetas()) {
          if (anterior === undefined) delete pagina.fondoImagen;
          else pagina.fondoImagen = anterior;
          if (gifAnterior === undefined) delete pagina.fondoGifKey;
          else pagina.fondoGifKey = gifAnterior;
          if (tonoAutomaticoAnterior === undefined) delete pagina.fondoTonoAutomatico;
          else pagina.fondoTonoAutomatico = tonoAutomaticoAnterior;
          if (luminanciaAnterior === undefined) delete pagina.fondoLuminanciaPromedio;
          else pagina.fondoLuminanciaPromedio = luminanciaAnterior;
          await aplicarFondoPaginaEditor(pagina);
          return;
        }
        document.getElementById("editor-fondo-quitar").disabled = false;
        await aplicarFondoPaginaEditor(pagina);
      } catch (error) {
        if (anterior === undefined) delete pagina.fondoImagen;
        else pagina.fondoImagen = anterior;
        if (gifAnterior === undefined) delete pagina.fondoGifKey;
        else pagina.fondoGifKey = gifAnterior;
        if (tonoAutomaticoAnterior === undefined) delete pagina.fondoTonoAutomatico;
        else pagina.fondoTonoAutomatico = tonoAutomaticoAnterior;
        if (luminanciaAnterior === undefined) delete pagina.fondoLuminanciaPromedio;
        else pagina.fondoLuminanciaPromedio = luminanciaAnterior;
        console.error("No se pudo cambiar la imagen de fondo de la página:", error);
        window.alert("No se pudo procesar la imagen de fondo. Prueba con otro archivo.");
      } finally {
        input.value = "";
      }
    }

    async function cambiarFondoGifEditor(input) {
      const pagina = paginaActualAbierta();
      const file = input.files?.[0];
      if (!pagina || !file) return;
      if (file.type !== "image/gif") {
        window.alert("Elige un archivo GIF animado.");
        input.value = "";
        return;
      }
      if (file.size > 25 * 1024 * 1024) {
        window.alert("El GIF debe ocupar 25 MB o menos.");
        input.value = "";
        return;
      }
      const clave = makeId();
      const anteriores = {
        fondoImagen: pagina.fondoImagen,
        fondoGifKey: pagina.fondoGifKey,
        fondoTonoAutomatico: pagina.fondoTonoAutomatico,
        fondoLuminanciaPromedio: pagina.fondoLuminanciaPromedio
      };
      let objectUrl = null;
      try {
        await guardarImagenEnBaseDatos(clave, file);
        objectUrl = URL.createObjectURL(file);
        const analisis = await detectarTonoAutomaticoFondo(objectUrl);
        pagina.fondoGifKey = clave;
        delete pagina.fondoImagen;
        pagina.fondoTonoAutomatico = analisis.tono;
        pagina.fondoLuminanciaPromedio = analisis.luminancia;
        if (!guardarCarpetas()) {
          for (const [propiedad, valor] of Object.entries(anteriores)) {
            if (valor === undefined) delete pagina[propiedad];
            else pagina[propiedad] = valor;
          }
          limpiarImagenesHuerfanas().catch(error => console.error("No se pudo limpiar el GIF de fondo no guardado:", error));
          return;
        }
        await aplicarFondoPaginaEditor(pagina);
        document.getElementById("editor-fondo-quitar").disabled = false;
      } catch (error) {
        for (const [propiedad, valor] of Object.entries(anteriores)) {
          if (valor === undefined) delete pagina[propiedad];
          else pagina[propiedad] = valor;
        }
        limpiarImagenesHuerfanas().catch(limpiezaError => console.error("No se pudo limpiar el GIF de fondo no guardado:", limpiezaError));
        console.error("No se pudo cambiar el GIF de fondo de la página:", error);
        window.alert("No se pudo procesar o guardar el GIF de fondo. Prueba con otro archivo.");
      } finally {
        if (objectUrl) URL.revokeObjectURL(objectUrl);
        input.value = "";
      }
    }

    async function quitarFondoPaginaEditor() {
      const pagina = paginaActualAbierta();
      if (!pagina || (!pagina.fondoImagen && !pagina.fondoGifKey)) return;
      if (!window.confirm("¿Quitar el fondo de esta página?")) return;
      const anteriores = {
        fondoImagen: pagina.fondoImagen,
        fondoGifKey: pagina.fondoGifKey,
        fondoTonoAutomatico: pagina.fondoTonoAutomatico,
        fondoLuminanciaPromedio: pagina.fondoLuminanciaPromedio,
        fondoParallax: pagina.fondoParallax
      };
      delete pagina.fondoImagen;
      delete pagina.fondoGifKey;
      delete pagina.fondoTonoAutomatico;
      delete pagina.fondoLuminanciaPromedio;
      delete pagina.fondoParallax;
      if (!guardarCarpetas()) {
        for (const [propiedad, valor] of Object.entries(anteriores)) {
          if (valor === undefined) delete pagina[propiedad];
          else pagina[propiedad] = valor;
        }
        return;
      }
      document.getElementById("editor-fondo-quitar").disabled = true;
      document.getElementById("editor-fondo-parallax-activo").checked = true;
      await aplicarFondoPaginaEditor(pagina);
    }

    function renombrarPagina(materia, pagina, tituloElemento) {
      const input = document.createElement("input");
      input.className = "pagina-renombrar";
      input.type = "text";
      input.maxLength = 120;
      input.setAttribute("aria-label", "Nuevo título de la página");
      input.value = tituloTarjetaPagina(pagina);
      const fila = tituloElemento.closest("[data-pagina-fila]");
      if (fila) fila.draggable = false;
      tituloElemento.replaceWith(input);
      const terminar = guardar => {
        if (!input.isConnected) return;
        if (guardar) {
          pagina.cardTitle = input.value.trim();
          pagina.titulo = pagina.cardTitle;
          pagina.actualizado = Date.now();
          guardarCarpetas();
        }
        renderPaginas(materia);
      };
      input.addEventListener("keydown", event => {
        if (event.key === "Enter") {
          event.preventDefault();
          input.blur();
        } else if (event.key === "Escape") {
          event.preventDefault();
          terminar(false);
        }
      });
      input.addEventListener("blur", () => terminar(true), { once: true });
      input.focus();
      input.select();
    }

    function crearPagina(carpetaId, materiaId, cuatrimestre = "", unidad = "") {
      const carpeta = carpetas.find(item => item.id === carpetaId);
      const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaId);
      if (!esCarpetaDeEstudio(carpeta) || !materia) return;
      const cuatrimestresValidos = new Set(["primer-cuatrimestre", "segundo-cuatrimestre"]);
      const unidadesValidas = new Set(["Unidad 1", "Unidad 2", "Unidad 3", "Unidad 4"]);
      if (!cuatrimestresValidos.has(cuatrimestre) || !unidadesValidas.has(unidad)) return;
      const pagina = {
        id: makeId(),
        titulo: "Nueva página",
        cardTitle: "Nueva página",
        pageTitle: "",
        contenido: "",
        actualizado: Date.now(),
        portada: "",
        color: COLORES_PAGINA[materia.paginas.length % COLORES_PAGINA.length],
        cuatrimestre,
        unidad,
        pdf: null
      };
      materia.paginas.unshift(pagina);
      if (!guardarCarpetas()) {
        materia.paginas.shift();
        return;
      }
      abrirPagina(carpeta.id, pagina.id, materia.id);
    }

    async function abrirPagina(carpetaId, paginaId, materiaId = materiaActivaId, elementoGaleria = null) {
      if (vistaActual === "pagina") guardarPaginaAbierta();
      cerrarVistaPdfEditor();
      detenerParallaxFondoEditor();
      detenerCorrector();
      imagenEditorActiva = null;
      imagenEditorActivaId = "";
      tablaEditorActivaId = "";
      const carpeta = carpetas.find(item => item.id === carpetaId);
      const modoGaleria = esElementoGaleria(elementoGaleria) && elementoGaleria.id === paginaId && elementoGaleria.id === carpetaId;
      const materia = modoGaleria ? null : materiasDeCarpeta(carpeta).find(item => item.id === materiaId);
      const pagina = modoGaleria ? elementoGaleria : encontrarPagina(carpeta, paginaId, materiaId);
      if ((!modoGaleria && (!esCarpetaDeEstudio(carpeta) || !materia || !pagina)) || (modoGaleria && carpeta !== elementoGaleria)) return;
      carpetaActualId = carpetaId;
      materiaActivaId = materia?.id || null;
      elementoGaleriaActivoId = modoGaleria ? elementoGaleria.id : null;
      seccionActivaId = carpeta.seccionId || seccionActivaId;
      paginaActivaId = paginaId;
      vistaActual = "pagina";
      document.getElementById("search-wrap").hidden = false;
      actualizarNavegacion();
      view.innerHTML = `
        <section class="pagina-editor">
          <div class="pagina-editor-herramientas">
            ${modoGaleria ? `<button class="vista-volver" type="button" data-volver-galeria="${escapeHtml(carpeta.seccionId || "")}">← Volver a ${escapeHtml(secciones.find(item => item.id === carpeta.seccionId)?.titulo || "Galería")}</button>` : `<button class="vista-volver" type="button" data-abrir-carpeta="${escapeHtml(carpeta.id)}" data-abrir-materia="${escapeHtml(materia.id)}">← Volver a ${escapeHtml(materia.titulo)}</button>`}
            <span class="corrector-estado" id="corrector-estado" aria-live="polite" hidden></span>
          </div>
          <div class="lectura-panel lectura-panel-${pagina.fondoEstiloPanel === "solido" || pagina.fondoEstiloPanel === "sin-panel" ? pagina.fondoEstiloPanel : "vidrio"}" id="lectura-panel">
            <input id="editor-archivo-imagen" type="file" accept="image/*" hidden aria-label="Subir imagen">
            <div class="editor-disposicion" id="editor-disposicion">
              <div class="editor-bloque">
                <button class="editor-panel-toggle" id="editor-panel-toggle" type="button" aria-expanded="false" aria-controls="editor-panel">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" aria-hidden="true"><path d="M4 5.5A1.5 1.5 0 0 1 5.5 4H19v16H5.5A1.5 1.5 0 0 1 4 18.5z"/><path d="M4 6h15M8 9h7M8 12h7M8 15h5"/></svg>
                  <span>Editar</span>
                </button>
                <div class="editor-titulo-fila">
                  <input class="page-title" id="pagina-titulo" type="text" placeholder="Sin título" aria-label="Título de la página" value="${escapeHtml(pagina.pageTitle)}">
                </div>
              <div class="editor-menu-comandos" id="editor-menu-comandos" hidden></div>
              <div class="page-content editor-contenido" id="pagina-cuerpo" contenteditable="true" role="textbox" aria-label="Contenido de la página" aria-multiline="true" spellcheck="true" lang="es"></div>
              </div>
              <aside class="panel-edicion" id="editor-panel" role="complementary" aria-label="Panel de edición" aria-hidden="true" inert>
              <div class="panel-encabezado">
                <h2 class="panel-titulo">Propiedades del texto</h2>
                <button class="panel-cerrar" id="editor-panel-cerrar" type="button" aria-label="Cerrar panel de edición">×</button>
              </div>
              <section class="panel-seccion">
                <h3 class="panel-seccion-encabezado">
                  <button class="panel-seccion-boton" type="button" data-panel-acordeon="organizacion" aria-expanded="true" aria-controls="panel-organizacion-contenido">ORGANIZACIÓN</button>
                </h3>
                <div class="panel-seccion-contenido" id="panel-organizacion-contenido">
                  <label class="panel-campo">Nombre de la tarjeta
                    <input type="text" data-card-title value="${escapeHtml(pagina.cardTitle)}" maxlength="120" aria-label="Nombre visible en la tarjeta">
                  </label>
                  ${modoGaleria ? "" : `<label class="panel-campo">Cuatrimestre
                    <select data-page-term aria-label="Cuatrimestre de la página">
                      <option value="" ${pagina.cuatrimestre ? "" : "selected"}>Sin cuatrimestre</option>
                      <option value="primer-cuatrimestre" ${pagina.cuatrimestre === "primer-cuatrimestre" ? "selected" : ""}>Primer Cuatrimestre</option>
                      <option value="segundo-cuatrimestre" ${pagina.cuatrimestre === "segundo-cuatrimestre" ? "selected" : ""}>Segundo Cuatrimestre</option>
                    </select>
                  </label>
                  <label class="panel-campo">Unidad
                    <select data-page-unit aria-label="Unidad de la página">
                      <option value="" ${pagina.unidad ? "" : "selected"}>Sin unidad</option>
                      <option value="Unidad 1" ${pagina.unidad === "Unidad 1" ? "selected" : ""}>Unidad 1</option>
                      <option value="Unidad 2" ${pagina.unidad === "Unidad 2" ? "selected" : ""}>Unidad 2</option>
                      <option value="Unidad 3" ${pagina.unidad === "Unidad 3" ? "selected" : ""}>Unidad 3</option>
                      <option value="Unidad 4" ${pagina.unidad === "Unidad 4" ? "selected" : ""}>Unidad 4</option>
                    </select>
                  </label>
                  <p class="panel-ayuda">Elige una unidad. Las páginas del mismo cuatrimestre y unidad se agrupan automáticamente.</p>`}
                  <div class="panel-archivo">
                    <input type="file" accept="application/pdf,.pdf" data-page-pdf-file hidden aria-label="Elegir un archivo PDF">
                    <button class="panel-archivo-boton" type="button" data-page-pdf-upload>Subir PDF</button>
                    <p class="panel-ayuda" data-page-pdf-status aria-live="polite">${pagina.pdf ? `${escapeHtml(pagina.pdf.title || pagina.pdf.fileName)}${pagina.pdf.author ? ` · ${escapeHtml(pagina.pdf.author)}` : ""}` : "No hay un PDF adjunto."}</p>
                    <button class="panel-archivo-boton" type="button" data-page-pdf-remove ${pagina.pdf ? "" : "hidden"}>Quitar PDF</button>
                  </div>
                </div>
              </section>
              <section class="panel-seccion">
                <h3 class="panel-seccion-encabezado">
                  <button class="panel-seccion-boton" type="button" data-panel-acordeon="texto" aria-expanded="true" aria-controls="panel-texto-contenido">TEXTO</button>
                </h3>
                <div class="panel-seccion-contenido" id="panel-texto-contenido">
                  <label class="panel-campo">Fuente
                    <select data-editor-font aria-label="Fuente del texto">
                      <option value="Arial">Sans</option><option value="Georgia">Serif</option><option value="Consolas">Monoespaciada</option><option value="Comic Sans MS">Manuscrita</option>
                    </select>
                  </label>
                  <label class="panel-campo">Tamaño de letra
                    <select data-editor-font-size aria-label="Tamaño de letra">
                      <option value="2">Pequeño</option><option value="3" selected>Normal</option><option value="4">Mediano</option><option value="5">Grande</option><option value="6">Muy grande</option>
                    </select>
                  </label>
                  <div class="panel-grupo panel-grupo-inline" role="group" aria-label="Formato de texto">
                    <button class="panel-control" type="button" data-editor-command="bold" aria-label="Negrita" aria-pressed="false"><strong>B</strong></button>
                    <button class="panel-control" type="button" data-editor-command="italic" aria-label="Cursiva" aria-pressed="false"><em>I</em></button>
                    <button class="panel-control" type="button" data-editor-command="underline" aria-label="Subrayado" aria-pressed="false"><u>U</u></button>
                    <button class="panel-control" type="button" data-editor-command="strikeThrough" aria-label="Tachado" aria-pressed="false"><s>S</s></button>
                  </div>
                  <div class="panel-grupo">
                    <label class="panel-campo">Color del texto<input type="color" data-editor-color value="#17171A" aria-label="Color del texto seleccionado"></label>
                    <label class="panel-campo">Resaltado<input type="color" data-editor-highlight value="#FFF176" aria-label="Color de resaltado"></label>
                  </div>
                  <div class="panel-grupo panel-grupo-inline" role="group" aria-label="Alineación del párrafo">
                    <button class="panel-control" type="button" data-editor-command="justifyLeft" aria-label="Alinear a la izquierda" aria-pressed="false">≡</button>
                    <button class="panel-control" type="button" data-editor-command="justifyCenter" aria-label="Centrar" aria-pressed="false">☰</button>
                    <button class="panel-control" type="button" data-editor-command="justifyRight" aria-label="Alinear a la derecha" aria-pressed="false">≡</button>
                    <button class="panel-control" type="button" data-editor-command="justifyFull" aria-label="Justificar" aria-pressed="false">☷</button>
                  </div>
                  <label class="panel-campo">Estilo de párrafo
                    <select data-editor-heading aria-label="Estilo de párrafo">
                      <option value="paragraph">Texto normal</option><option value="h1">Título</option><option value="h2">Subtítulo</option><option value="h3">Subtítulo pequeño</option><option value="blockquote">Cita</option>
                    </select>
                  </label>
                  <label class="panel-campo">Espacio del bloque seleccionado
                    <select data-editor-spacing aria-label="Espacio antes y después del bloque seleccionado">
                      <option value="normal">Normal</option><option value="chico">Chico</option><option value="grande">Grande</option>
                    </select>
                  </label>
                  <section class="editor-fondo-vector" aria-labelledby="editor-fondo-vector-titulo">
                    <h4 id="editor-fondo-vector-titulo">Fondo</h4>
                    <div class="editor-fondo-vector-galeria" data-editor-vector-gallery role="group" aria-label="Elegir un fondo vectorial"></div>
                    <input type="file" accept=".svg,image/svg+xml" data-editor-vector-file hidden aria-label="Subir un fondo SVG">
                    <button class="panel-control" type="button" data-editor-vector-upload>Subir un SVG propio</button>
                    <div class="editor-fondo-vector-ajustes">
                      <label class="panel-campo">Color del vector<input type="color" data-editor-vector-color value="#64748B" aria-label="Color del vector"></label>
                      <label class="panel-campo">Opacidad: <output data-editor-vector-opacity-value>45%</output>
                        <input type="range" min="0" max="100" step="1" value="45" data-editor-vector-opacity aria-label="Opacidad del vector">
                      </label>
                      <label class="panel-campo">Tamaño
                        <select data-editor-vector-size aria-label="Tamaño del fondo vectorial">
                          <option value="cover">Cubrir</option><option value="repeat">Repetir</option><option value="scale">Escala en %</option>
                        </select>
                      </label>
                      <label class="panel-campo">Escala: <output data-editor-vector-scale-value>100%</output>
                        <input type="range" min="25" max="250" step="1" value="100" data-editor-vector-scale aria-label="Escala del vector">
                      </label>
                      <label class="panel-campo">Posición
                        <select data-editor-vector-position aria-label="Posición del fondo vectorial">
                          <option value="center">Centrado</option><option value="top">Arriba</option><option value="bottom">Abajo</option><option value="left">Izquierda</option><option value="right">Derecha</option>
                        </select>
                      </label>
                      <label class="panel-campo">Suavizar: <output data-editor-vector-soften-value>45%</output>
                        <input type="range" min="0" max="80" step="1" value="45" data-editor-vector-soften aria-label="Capa para suavizar el fondo">
                      </label>
                    </div>
                    <div class="editor-fondo-vector-acciones">
                      <button class="panel-control" type="button" data-editor-vector-remove>Quitar fondo</button>
                    </div>
                    <p class="panel-ayuda" data-editor-vector-help>Elige un vector para mostrar estos ajustes.</p>
                  </section>
                  <div class="panel-grupo" role="group" aria-label="Listas">
                    <button class="panel-control" type="button" data-editor-command="ul" aria-label="Lista con viñetas">• Viñetas</button>
                    <button class="panel-control" type="button" data-editor-command="ol" aria-label="Lista numerada">1. Numerada</button>
                    <button class="panel-control" type="button" data-editor-command="task" aria-label="Lista de tareas">☑ Tareas</button>
                  </div>
                  <div class="panel-grupo" role="group" aria-label="Insertar contenido">
                    <button class="panel-control" type="button" data-editor-command="image" aria-label="Insertar imagen a lo ancho del texto">▧ Imagen a lo ancho</button>
                  </div>
                  <div class="panel-historial" role="group" aria-label="Historial de cambios">
                    <button class="panel-control" type="button" data-editor-command="undo" aria-label="Deshacer">↶ Deshacer</button>
                    <button class="panel-control" type="button" data-editor-command="redo" aria-label="Rehacer">↷ Rehacer</button>
                  </div>
                </div>
              </section>
              <section class="panel-seccion">
                <h3 class="panel-seccion-encabezado">
                  <button class="panel-seccion-boton" type="button" data-panel-acordeon="pagina" aria-expanded="false" aria-controls="panel-pagina-contenido">PÁGINA</button>
                </h3>
                <div class="panel-seccion-contenido" id="panel-pagina-contenido" hidden>
                  <div class="panel-archivo">
                    <label class="panel-campo">Opacidad del panel: <output id="editor-panel-opacidad-valor">${Number.isFinite(pagina.fondoOpacidadPanel) ? Math.max(40, Math.min(100, Math.round(pagina.fondoOpacidadPanel))) : 85}%</output>
                      <input id="editor-panel-opacidad" type="range" min="40" max="100" step="1" value="${Number.isFinite(pagina.fondoOpacidadPanel) ? Math.max(40, Math.min(100, Math.round(pagina.fondoOpacidadPanel))) : 85}" aria-label="Opacidad del panel">
                    </label>
                    <label class="panel-campo">Tono del panel
                      <select id="editor-panel-tono" aria-label="Tono del panel">
                        <option value="claro" ${pagina.fondoTonoPanel === "claro" ? "selected" : ""}>Claro</option>
                        <option value="oscuro" ${pagina.fondoTonoPanel === "oscuro" ? "selected" : ""}>Oscuro</option>
                        <option value="automatico" ${pagina.fondoTonoPanel === "oscuro" || pagina.fondoTonoPanel === "claro" ? "" : "selected"}>Automático</option>
                      </select>
                    </label>
                    <label class="panel-campo">Estilo del panel de lectura
                      <select id="editor-panel-texto-estilo" aria-label="Estilo del panel de lectura">
                        <option value="vidrio" ${pagina.fondoEstiloPanel === "solido" || pagina.fondoEstiloPanel === "sin-panel" ? "" : "selected"}>Vidrio</option>
                        <option value="solido" ${pagina.fondoEstiloPanel === "solido" ? "selected" : ""}>Sólido</option>
                        <option value="sin-panel" ${pagina.fondoEstiloPanel === "sin-panel" ? "selected" : ""}>Sin panel</option>
                      </select>
                    </label>
                    <input id="editor-fondo-archivo" type="file" accept="image/*" hidden aria-label="Elegir imagen de fondo de la página">
                    <button class="panel-archivo-boton" id="editor-fondo-elegir" type="button" aria-label="Elegir imagen de fondo">Elegir imagen de fondo</button>
                    <input id="editor-fondo-gif-archivo" type="file" accept="image/gif" hidden aria-label="Elegir GIF de fondo">
                    <button class="panel-archivo-boton" id="editor-fondo-gif-elegir" type="button">Elegir GIF de fondo</button>
                    <button class="panel-archivo-boton" id="editor-fondo-quitar" type="button" aria-label="Quitar imagen de fondo" ${pagina.fondoImagen || pagina.fondoGifKey ? "" : "disabled"}>Quitar imagen de fondo</button>
                    <label class="panel-parallax"><input id="editor-fondo-parallax-activo" type="checkbox" ${pagina.fondoParallax === false ? "" : "checked"}>Activar efecto parallax al desplazarse</label>
                    <p class="panel-ayuda">La imagen se reduce antes de guardarse. El fondo pertenece solo a esta página.</p>
                  </div>
                </div>
              </section>
            </aside>
          </div>
        </div>
        <section class="pagina-pdf-adjunto" data-page-pdf-viewer ${pagina.pdf ? "" : "hidden"} aria-label="Documento PDF adjunto">
          <div class="pagina-pdf-encabezado">
            <div>
              <h2 data-page-pdf-title>${escapeHtml(pagina.pdf?.title || pagina.pdf?.fileName || "")}</h2>
              <p class="pagina-pdf-metadatos" data-page-pdf-author>${pagina.pdf?.author ? `Autor: ${escapeHtml(pagina.pdf.author)}` : ""}</p>
            </div>
            <div>
              <button class="pagina-pdf-descargar" type="button" data-page-pdf-download="${escapeHtml(pagina.pdf?.key || "")}" ${pagina.pdf ? "" : "hidden"}>Descargar PDF</button>
              <button class="pagina-pdf-abrir" type="button" data-page-pdf-open aria-expanded="false">Abrir PDF</button>
            </div>
          </div>
          <iframe class="pagina-pdf-marco" data-page-pdf-frame title="Visor del documento PDF" loading="lazy" hidden></iframe>
        </section>
          <div class="editor-dialog" id="editor-dialog-tabla" role="dialog" aria-modal="true" aria-labelledby="editor-dialog-tabla-titulo" hidden>
            <form class="editor-dialog-contenido" id="editor-tabla-form">
              <h2 id="editor-dialog-tabla-titulo">Insertar tabla</h2>
              <label>Filas (1 a 20)<input id="editor-tabla-filas" type="number" min="1" max="20" value="3" required></label>
              <label>Columnas (1 a 20)<input id="editor-tabla-columnas" type="number" min="1" max="20" value="3" required></label>
              <div class="editor-dialog-acciones"><button type="button" data-editor-cancelar-tabla>Cancelar</button><button type="submit">Insertar tabla</button></div>
            </form>
          </div>
          <div class="corrector-globito" id="corrector-globito" hidden><button class="corrector-sugerencia" type="button"></button></div>
        </section>`;
      const title = document.getElementById("pagina-titulo");
      const editor = document.getElementById("pagina-cuerpo");
      if (pagina.contenidoFormato === "html") editor.innerHTML = sanitizarContenidoEditor(pagina.contenido);
      else if (pagina.contenido) editor.innerHTML = pagina.contenido.split(/\r\n?|\n/).filter(linea => linea.trim() !== "").map(linea =>
        `<p>${escapeHtml(linea)}</p>`
      ).join("");
      else editor.textContent = "";
      await hidratarImagenesEditor(editor);
      if (!editor.isConnected || vistaActual !== "pagina" || paginaActivaId !== paginaId) return;
      await aplicarFondoPaginaEditor(pagina);
      await asegurarTonoAutomaticoFondo(pagina);
      instalarControlesBloquesEditor(editor);
      title.addEventListener("input", () => {
        pagina.pageTitle = title.value;
        actualizarNavegacion();
        programarGuardadoPagina();
      });
      const cardTitleInput = document.querySelector("[data-card-title]");
      cardTitleInput.addEventListener("input", () => {
        pagina.cardTitle = cardTitleInput.value.trim();
        pagina.titulo = pagina.cardTitle;
        programarGuardadoPagina();
      });
      const termInput = document.querySelector("[data-page-term]");
      const unitInput = document.querySelector("[data-page-unit]");
      const guardarOrganizacion = () => {
        pagina.cuatrimestre = termInput.value;
        pagina.unidad = unitInput.value;
        programarGuardadoPagina();
      };
      termInput?.addEventListener("change", guardarOrganizacion);
      unitInput?.addEventListener("change", guardarOrganizacion);
      const pdfInput = document.querySelector("[data-page-pdf-file]");
      document.querySelector("[data-page-pdf-upload]").addEventListener("click", () => pdfInput.click());
      pdfInput.addEventListener("change", event => cambiarPdfPaginaEditor(event.currentTarget, pagina));
      document.querySelector("[data-page-pdf-open]").addEventListener("click", event => alternarVistaPdfEditor(event.currentTarget, pagina));
      document.querySelector("[data-page-pdf-viewer] [data-page-pdf-download]").addEventListener("click", event => descargarPdfPaginaEditor(event.currentTarget));
      document.querySelector("[data-page-pdf-remove]").addEventListener("click", () => quitarPdfPaginaEditor(pagina));
      editor.addEventListener("input", event => {
        if (marcadorCursorPendiente) marcadorCursorPendiente = false;
        else if (!event.inputType.startsWith("format")) limpiarMarcadorCursorEditor(editor);
        guardarRangoActualEditor(editor);
        actualizarPlaceholderParrafoEditor(editor);
        actualizarEstadoControlesTexto(editor);
        desenvolverErroresCorrector(editor);
        programarGuardadoPagina();
        programarRevisionCorrector(editor, pagina.id);
      });
      editor.addEventListener("keyup", () => {
        guardarRangoActualEditor(editor);
        actualizarPlaceholderParrafoEditor(editor);
        actualizarEstadoControlesTexto(editor);
      });
      editor.addEventListener("mouseup", () => {
        guardarRangoActualEditor(editor);
        actualizarPlaceholderParrafoEditor(editor);
        actualizarEstadoControlesTexto(editor);
      });
      editor.addEventListener("focus", () => actualizarPlaceholderParrafoEditor(editor));
      editor.addEventListener("blur", () => actualizarPlaceholderParrafoEditor(editor));
      editor.addEventListener("keydown", event => {
        if ((event.ctrlKey || event.metaKey) && ["b", "i", "u", "z", "y"].includes(event.key.toLowerCase())) {
          event.preventDefault();
          const key = event.key.toLowerCase();
          const command = key === "z" ? (event.shiftKey ? "redo" : "undo") : key === "y" ? "redo" : ({ b: "bold", i: "italic", u: "underline" })[key];
          document.execCommand(command);
          editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "formatText" }));
          actualizarEstadoControlesTexto(editor);
          return;
        }
        if (event.key === "Escape") {
          window.clearTimeout(temporizadorCerrarComandos);
          document.getElementById("editor-menu-comandos").hidden = true;
          if (panelEditorAbierto) {
            panelEditorAbierto = false;
            actualizarPanelEdicion();
            document.getElementById("editor-panel-toggle").focus();
          }
        }
        if (event.key === "Backspace" && !event.ctrlKey && !event.metaKey && !event.altKey) {
          const selection = window.getSelection();
          if (selection?.isCollapsed && selection.rangeCount && editor.contains(selection.anchorNode)) {
            const node = selection.anchorNode.nodeType === Node.ELEMENT_NODE ? selection.anchorNode : selection.anchorNode.parentElement;
            const block = node?.closest("p");
            const previousBlock = block?.previousElementSibling;
            if (block && previousBlock && /^H[1-3]$/.test(previousBlock.tagName)) {
              const beforeCaret = document.createRange();
              beforeCaret.selectNodeContents(block);
              beforeCaret.setEnd(selection.anchorNode, selection.anchorOffset);
              if (!beforeCaret.toString().replace(/\u200B/g, "").trim()) event.preventDefault();
            }
          }
        }
        if (event.key === "ArrowDown" || event.key === "ArrowUp") moverSeleccionComando(event);
        if (event.key === "Enter" && !document.getElementById("editor-menu-comandos").hidden && document.querySelector("#editor-menu-comandos [data-slash-command]")) {
          event.preventDefault();
          ejecutarComandoMenu(editor);
        }
      });
      editor.addEventListener("paste", event => {
        event.preventDefault();
        const items = Array.from(event.clipboardData?.items || []);
        const imageItem = items.find(item => item.type.startsWith("image/"));
        if (imageItem) {
          insertarArchivoImagen(editor, imageItem.getAsFile());
          return;
        }
        const plainText = event.clipboardData?.getData("text/plain") || "";
        if (plainText) {
          const blocks = plainText
            .replace(/\r\n?/g, "\n")
            .split(/\n\s*\n+/)
            .map(block => block.split("\n").map(line => line.trim()).filter(Boolean))
            .filter(block => block.length);
          const html = [];
          let listItems = [];
          const flushList = () => {
            if (!listItems.length) return;
            html.push(`<ul>${listItems.map(item => `<li>${escapeHtml(item)}</li>`).join("")}</ul>`);
            listItems = [];
          };
          blocks.forEach(block => {
            block.forEach(line => {
              const listItem = line.match(/^(?:- |• )(.*)$/);
              if (listItem) {
                listItems.push(listItem[1]);
                return;
              }
              flushList();
              const boldTitle = line.match(/^\*\*(.+)\*\*$/);
              const numberedTitle = line.length <= 60 && /^\d+\.\s+/.test(line);
              html.push(boldTitle
                ? `<h2>${escapeHtml(boldTitle[1].trim())}</h2>`
                : numberedTitle
                  ? `<h2>${escapeHtml(line)}</h2>`
                  : `<p>${escapeHtml(line)}</p>`);
            });
            flushList();
          });
          document.execCommand("insertHTML", false, html.join(""));
        }
        editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertFromPaste" }));
      });
      enlazarControlesEditor(editor);
      editor.addEventListener("pointermove", event => {
        const match = buscarErrorCorrectorEnPunto(event.clientX, event.clientY);
        if (match) mostrarSugerenciaCorrector(match);
      });
      editor.addEventListener("pointerout", event => {
        if (!sugerenciaCorrectorActual) return;
        if (event.relatedTarget?.closest?.("#corrector-globito")) return;
        programarOcultarSugerencia();
      });
      editor.addEventListener("click", event => {
        const imageMenuButton = event.target.closest("[data-editor-abrir-controles-imagen]");
        const clickedImage = event.target.closest(".editor-imagen-marco img");
        const figureToEdit = imageMenuButton?.closest(".editor-imagen") || clickedImage?.closest(".editor-imagen");
        if (figureToEdit?.classList.contains("editor-imagen-controles-ocultos")) {
          event.preventDefault();
          event.stopPropagation();
          figureToEdit.classList.remove("editor-imagen-controles-ocultos");
          figureToEdit.dataset.imageAlignmentPreview = figureToEdit.dataset.imageAlignment || "izquierda";
          const alignment = figureToEdit.querySelector("[data-editor-image-alignment]");
          if (alignment) alignment.value = figureToEdit.dataset.imageAlignmentPreview;
          actualizarEstadoEdicionImagen(figureToEdit);
          return;
        }
        const applyMoveButton = event.target.closest("[data-editor-aplicar-movimiento]");
        if (applyMoveButton) {
          event.preventDefault();
          event.stopPropagation();
          const figure = applyMoveButton.closest(".editor-imagen");
          if (!figure) return;
          const alignment = figure.querySelector("[data-editor-image-alignment]");
          if (!alignment) return;
          figure.dataset.imageAlignment = alignment.value;
          delete figure.dataset.imageAlignmentPreview;
          figure.classList.add("editor-imagen-controles-ocultos");
          actualizarEstadoEdicionImagen(figure);
          editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertFromDrop" }));
          return;
        }
        const deleteBlock = event.target.closest("[data-editor-borrar-imagen], [data-editor-borrar-tabla]");
        if (deleteBlock) {
          event.preventDefault();
          event.stopPropagation();
          if (!window.confirm(deleteBlock.hasAttribute("data-editor-borrar-imagen") ? "¿Quitar esta imagen?" : "¿Quitar esta tabla?")) return;
          const block = deleteBlock.hasAttribute("data-editor-borrar-imagen")
            ? editor.querySelector(`.editor-imagen[data-image-id="${CSS.escape(deleteBlock.dataset.editorBorrarImagen)}"]`)
            : editor.querySelector(`.editor-tabla-bloque[data-table-id="${CSS.escape(deleteBlock.dataset.editorBorrarTabla)}"]`);
          if (!block || !editor.contains(block)) return;
          block.remove();
          imagenEditorActiva = null;
          imagenEditorActivaId = "";
          tablaEditorActivaId = "";
          editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "deleteContent" }));
          return;
        }
        if (event.target.matches(".editor-tareas input[type=checkbox]")) editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertReplacementText" }));
        const image = event.target.closest(".editor-imagen img");
        imagenEditorActiva = image || null;
        imagenEditorActivaId = image?.closest(".editor-imagen")?.dataset.imageId || "";
        const table = event.target.closest(".editor-tabla-bloque");
        tablaEditorActivaId = table?.dataset.tableId || "";
        if (image) {
          const range = document.createRange();
          range.selectNode(image);
          window.getSelection().removeAllRanges();
          window.getSelection().addRange(range);
          actualizarEstadoControlesTexto(editor);
        }
      });
      editor.addEventListener("dragstart", event => {
        const figure = event.target.closest(".editor-imagen");
        if (!figure) return;
        event.dataTransfer.setData("text/editor-image", figure.dataset.imageKey || "");
        event.dataTransfer.effectAllowed = "move";
        imagenEditorActiva = figure.querySelector("img");
        imagenEditorActivaId = figure.dataset.imageId || "";
      });
      editor.addEventListener("dragover", event => {
        if (event.dataTransfer.types.includes("Files") || event.dataTransfer.types.includes("text/editor-image")) event.preventDefault();
      });
      editor.addEventListener("drop", event => {
        const file = Array.from(event.dataTransfer.files || []).find(item => item.type.startsWith("image/"));
        if (file) {
          event.preventDefault();
          colocarCursorEnPunto(editor, event.clientX, event.clientY);
          insertarArchivoImagen(editor, file);
          return;
        }
        const key = event.dataTransfer.getData("text/editor-image");
        if (!key) return;
        event.preventDefault();
        const figure = editor.querySelector(`.editor-imagen[data-image-key="${CSS.escape(key)}"]`);
        if (!figure) return;
        colocarCursorEnPunto(editor, event.clientX, event.clientY);
        ponerEnCursor(editor, figure);
        editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertFromDrop" }));
      });
      editor.addEventListener("keyup", () => {
        const selection = window.getSelection();
        const anchor = selection?.anchorNode?.nodeType === Node.ELEMENT_NODE ? selection.anchorNode : selection?.anchorNode?.parentElement;
        const figure = anchor?.closest(".editor-imagen");
        const tableBlock = anchor?.closest(".editor-tabla-bloque");
        if (figure) { imagenEditorActivaId = figure.dataset.imageId || ""; imagenEditorActiva = figure.querySelector("img"); }
        if (tableBlock) tablaEditorActivaId = tableBlock.dataset.tableId || "";
      });
      const tooltip = document.getElementById("corrector-globito");
      tooltip.addEventListener("pointerenter", () => window.clearTimeout(temporizadorOcultarSugerencia));
      tooltip.addEventListener("pointerleave", programarOcultarSugerencia);
      tooltip.querySelector("button").addEventListener("click", () => aplicarSugerenciaCorrector(editor));
      title.focus();
    }

    function guardarPaginaAbierta() {
      window.clearTimeout(temporizadorGuardadoPagina);
      if (vistaActual !== "pagina") return;
      const pagina = paginaActualAbierta();
      const title = document.getElementById("pagina-titulo");
      const cardTitle = document.querySelector("[data-card-title]");
      const editor = document.getElementById("pagina-cuerpo");
      if (!pagina || !title || !editor) return;
      const termInput = document.querySelector("[data-page-term]");
      const unitInput = document.querySelector("[data-page-unit]");
      pagina.pageTitle = title.value;
      if (cardTitle) {
        pagina.cardTitle = cardTitle.value.trim();
        pagina.titulo = pagina.cardTitle;
      }
      if (termInput && ["", "primer-cuatrimestre", "segundo-cuatrimestre"].includes(termInput.value)) {
        pagina.cuatrimestre = termInput.value;
      }
      if (unitInput && ["", "Unidad 1", "Unidad 2", "Unidad 3", "Unidad 4"].includes(unitInput.value)) {
        pagina.unidad = unitInput.value;
      }
      pagina.contenido = serializarContenidoEditor(editor);
      pagina.contenidoFormato = "html";
      pagina.actualizado = Date.now();
      guardarCarpetas();
    }

    function programarGuardadoPagina() {
      window.clearTimeout(temporizadorGuardadoPagina);
      temporizadorGuardadoPagina = window.setTimeout(guardarPaginaAbierta, 400);
    }

    function sanitizarContenidoEditor(html) {
      const documento = new DOMParser().parseFromString(String(html), "text/html");
      const fuentesSeguras = [
        { nombre: "Arial", coincidencias: ["arial", "helvetica", "sans-serif"] },
        { nombre: "Georgia", coincidencias: ["georgia", "times new roman", "serif"] },
        { nombre: "Consolas", coincidencias: ["consolas", "monospace"] },
        { nombre: "Comic Sans MS", coincidencias: ["comic sans", "segoe print", "cursive"] }
      ];
      const colorSeguro = value => {
        const color = String(value || "").trim();
        return /^#[0-9a-f]{3,8}$/i.test(color) || /^rgba?\([\d.%\s,]+\)$/i.test(color) ? color : "";
      };
      const fuenteSegura = value => {
        const normalized = String(value || "").toLocaleLowerCase("en").replace(/["']/g, "");
        return fuentesSeguras.find(item => item.coincidencias.some(name => normalized.includes(name)))?.nombre || "";
      };
      documento.body.querySelectorAll("font").forEach(font => {
        const span = documento.createElement("span");
        while (font.firstChild) span.append(font.firstChild);
        span.style.fontFamily = fuenteSegura(font.getAttribute("face"));
        const tamanos = { "1": "10px", "2": "12px", "3": "14px", "4": "18px", "5": "24px", "6": "32px", "7": "48px" };
        span.style.fontSize = tamanos[font.getAttribute("size")] || "";
        span.style.color = colorSeguro(font.getAttribute("color"));
        font.replaceWith(span);
      });
      const permitidas = new Set(["P", "DIV", "BR", "STRONG", "B", "EM", "I", "U", "S", "DEL", "H1", "H2", "H3", "UL", "OL", "LI", "BLOCKQUOTE", "PRE", "CODE", "HR", "FIGURE", "FIGCAPTION", "IMG", "TABLE", "THEAD", "TBODY", "TR", "TD", "TH", "INPUT", "SPAN"]);
      const peligrosas = new Set(["SCRIPT", "STYLE", "IFRAME", "OBJECT", "EMBED", "SVG", "MATH"]);
      const limpiar = node => {
        Array.from(node.childNodes).forEach(child => {
          if (child.nodeType !== Node.ELEMENT_NODE) return;
          if (peligrosas.has(child.tagName)) { child.remove(); return; }
          limpiar(child);
          if (!permitidas.has(child.tagName)) { child.replaceWith(...child.childNodes); return; }
          const tag = child.tagName;
          if (["STRONG", "B", "EM", "I", "U", "S", "DEL", "SPAN"].includes(tag) && !child.textContent && !child.children.length) { child.remove(); return; }
          const tableWrapper = tag === "DIV" && child.classList.contains("editor-tabla-bloque");
          const imageFrame = tag === "DIV" && child.classList.contains("editor-imagen-marco");
          if (["P", "DIV"].includes(tag) && !tableWrapper && !imageFrame && !child.textContent && !child.children.length) { child.remove(); return; }
          const key = child.dataset.imageKey || "";
          const imageId = child.dataset.imageId || "";
          const imageCrop = child.dataset.imageCrop === "true";
          const imagePosition = child.dataset.imagePosition || "50% 50%";
          const imageAlignment = child.dataset.imageAlignment ||
            (Number(child.dataset.imageX) >= 75 ? "derecha" : "izquierda");
          const tableId = child.dataset.tableId || "";
          const source = child.getAttribute("src") || "";
          const alt = child.getAttribute("alt") || "Imagen insertada";
          const width = child.getAttribute("width") || "";
          const rawStyleWidth = child.style?.width || "";
          const checked = child.hasAttribute("checked");
          const inputType = child.getAttribute("type") || "";
          const taskList = tag === "UL" && child.classList.contains("editor-tareas");
          const colSpan = child.getAttribute("colspan") || "";
          const rowSpan = child.getAttribute("rowspan") || "";
          const fontFamily = fuenteSegura(child.style?.fontFamily);
          const fontSize = child.style?.fontSize || "";
          const textColor = colorSeguro(child.style?.color);
          const highlightColor = colorSeguro(child.style?.backgroundColor);
          const textAlign = child.style?.textAlign || "";
          const blockSpacing = child.dataset.editorEspaciado || "";
          Array.from(child.attributes).forEach(attribute => child.removeAttribute(attribute.name));
          if (tag === "FIGURE") {
            child.className = "editor-imagen";
            if (key && /^[a-z0-9-]+$/i.test(key)) child.dataset.imageKey = key;
            if (/^[a-z0-9-]+$/i.test(imageId)) child.dataset.imageId = imageId;
            child.dataset.imagePosition = /^(?:100|[1-9]?\d)% (?:100|[1-9]?\d)%$/.test(imagePosition) ? imagePosition : "50% 50%";
            if (imageCrop) child.dataset.imageCrop = "true";
            if (["izquierda", "centro", "derecha"].includes(imageAlignment)) child.dataset.imageAlignment = imageAlignment;
            const safeWidth = /^(?:\d{1,4}(?:\.\d{1,2})?)(?:px|%)$/.test(rawStyleWidth) ? rawStyleWidth : "";
            child.style.width = safeWidth === "96%" ? "60%" : safeWidth || "60%";
            child.setAttribute("draggable", "false");
          } else if (tableWrapper) {
            child.className = "editor-tabla-bloque";
            if (/^[a-z0-9-]+$/i.test(tableId)) child.dataset.tableId = tableId;
          } else if (imageFrame) {
            child.className = "editor-imagen-marco";
          } else if (tag === "IMG") {
            const validKey = /^[a-z0-9-]+$/i.test(key);
            const safeSource = validKey ? "" : source;
            if (!validKey && !/^data:image\/jpeg;base64,[a-z0-9+/=]+$/i.test(safeSource)) {
              child.remove();
              return;
            }
            if (safeSource) child.setAttribute("src", safeSource);
            child.setAttribute("alt", alt);
            if (validKey) child.dataset.imageKey = key;
            if (/^\d{1,4}$/.test(width)) child.setAttribute("width", width);
          } else if (tag === "INPUT") {
            if (inputType !== "checkbox") { child.remove(); return; }
            child.setAttribute("type", "checkbox");
            child.setAttribute("contenteditable", "false");
            if (checked) child.setAttribute("checked", "");
          } else if (["SPAN", "STRONG", "B", "EM", "I", "U", "S", "DEL"].includes(tag)) {
            if (fontFamily) child.style.fontFamily = fontFamily;
            const normalizedFontSize = ({ "xx-small": "9px", "x-small": "10px", small: "12px", medium: "14px", large: "18px", "x-large": "24px", "xx-large": "32px" })[fontSize] || fontSize;
            if (/^(?:[8-9]|[1-6]\d|7[0-2])px$/.test(normalizedFontSize)) child.style.fontSize = normalizedFontSize;
            if (textColor) child.style.color = textColor;
            if (highlightColor) child.style.backgroundColor = highlightColor;
          } else if (["P", "DIV", "H1", "H2", "H3", "BLOCKQUOTE"].includes(tag)) {
            if (["left", "center", "right", "justify"].includes(textAlign)) child.style.textAlign = textAlign;
            if (["P", "H1", "H2", "H3"].includes(tag) && ["chico", "grande"].includes(blockSpacing)) {
              child.dataset.editorEspaciado = blockSpacing;
            }
          } else if (tag === "TD" || tag === "TH") {
            if (/^(?:[1-9]|1\d|20)$/.test(colSpan)) child.setAttribute("colspan", colSpan);
            if (/^(?:[1-9]|1\d|20)$/.test(rowSpan)) child.setAttribute("rowspan", rowSpan);
          } else if (taskList) {
            child.className = "editor-tareas";
          }
        });
      };
      limpiar(documento.body);
      const bloquesContenido = new Set(["P", "H1", "H2", "H3", "BLOCKQUOTE", "UL", "OL", "PRE", "HR", "FIGURE", "TABLE"]);
      const normalizarBloques = parent => {
        let child = parent.firstChild;
        while (child) {
          const next = child.nextSibling;
          if (child.nodeType === Node.TEXT_NODE && parent === documento.body) {
            if (child.textContent.trim()) {
              const paragraph = documento.createElement("p");
              paragraph.textContent = child.textContent;
              child.replaceWith(paragraph);
            } else child.remove();
          } else if (child.nodeType === Node.ELEMENT_NODE) {
            const isWrapper = child.matches(".editor-tabla-bloque, .editor-imagen-marco");
            if (parent === documento.body && child.tagName === "BR") child.remove();
            else if (parent === documento.body && child.tagName === "DIV" && !isWrapper) {
              const hasBlockChildren = Array.from(child.children).some(element => bloquesContenido.has(element.tagName));
              if (hasBlockChildren) child.replaceWith(...Array.from(child.childNodes));
              else {
                const paragraph = documento.createElement("p");
                paragraph.style.textAlign = child.style.textAlign;
                if (["chico", "grande"].includes(child.dataset.editorEspaciado)) {
                  paragraph.dataset.editorEspaciado = child.dataset.editorEspaciado;
                }
                paragraph.replaceChildren(...Array.from(child.childNodes));
                child.replaceWith(paragraph);
              }
            } else {
              normalizarBloques(child);
              if (["P", "H1", "H2", "H3"].includes(child.tagName)) {
                child.querySelectorAll("br").forEach(lineBreak => {
                  if (!lineBreak.previousSibling || lineBreak.previousSibling.nodeName === "BR") lineBreak.remove();
                });
                const hasMeaningfulContent = child.textContent.replace(/\u200B/g, "").trim() ||
                  child.querySelector("img, table, input, figure");
                if (!hasMeaningfulContent) child.remove();
              }
            }
          }
          child = next;
        }
        if (parent === documento.body && parent.querySelector(":scope > div:not(.editor-tabla-bloque):not(.editor-imagen-marco)")) {
          normalizarBloques(parent);
        }
      };
      normalizarBloques(documento.body);
      return documento.body.innerHTML;
    }

    function serializarContenidoEditor(editor) {
      const clon = editor.cloneNode(true);
      clon.querySelectorAll("[data-editor-ui]").forEach(element => element.remove());
      const walker = document.createTreeWalker(clon, NodeFilter.SHOW_TEXT);
      let textNode;
      while ((textNode = walker.nextNode())) textNode.nodeValue = textNode.nodeValue.replace(/\u200B/g, "");
      clon.querySelectorAll(".editor-imagen").forEach(figure => {
        const image = figure.querySelector("img");
        const key = figure.dataset.imageKey || image?.dataset.imageKey;
        if (image && key && /^[a-z0-9-]+$/i.test(key)) {
          figure.dataset.imageKey = key;
          image.dataset.imageKey = key;
          image.removeAttribute("src");
        }
      });
      clon.querySelectorAll("input[type=checkbox]").forEach(input => {
        if (input.checked) input.setAttribute("checked", "");
        else input.removeAttribute("checked");
      });
      return sanitizarContenidoEditor(clon.innerHTML);
    }

    async function hidratarImagenesEditor(editor) {
      const imagenes = Array.from(editor.querySelectorAll("img[data-image-key], img[src^=\"indexeddb:\"]"));
      for (const image of imagenes) {
        const key = image.dataset.imageKey || image.getAttribute("src").slice("indexeddb:".length);
        try {
          const blob = await leerImagenDeBaseDatos(key);
          if (!blob) throw new Error(`No se encontró la imagen guardada (${key}).`);
          image.src = URL.createObjectURL(blob);
          image.dataset.imageKey = key;
          image.closest(".editor-imagen")?.setAttribute("data-image-key", key);
        } catch (error) {
          console.error("No se pudo restaurar una imagen de la página:", error);
          const status = document.getElementById("corrector-estado");
          if (status) {
            status.textContent = "No se pudo cargar una imagen guardada. El resto de la página sigue disponible.";
            status.hidden = false;
          }
        }
      }
    }

    function instalarControlesBloquesEditor(editor) {
      const imageIds = new Set();
      editor.querySelectorAll(".editor-imagen").forEach(figure => {
        let key = figure.dataset.imageKey || figure.querySelector("img")?.dataset.imageKey || "";
        if (!/^[a-z0-9-]+$/i.test(key)) key = makeId();
        figure.dataset.imageKey = key;
        let imageId = figure.dataset.imageId || "";
        if (!/^[a-z0-9-]+$/i.test(imageId) || imageIds.has(imageId)) imageId = makeId();
        imageIds.add(imageId);
        figure.dataset.imageId = imageId;
        const savedImagePosition = /^(?:100|[1-9]?\d)% (?:100|[1-9]?\d)%$/.test(figure.dataset.imagePosition || "")
          ? figure.dataset.imagePosition
          : "50% 50%";
        figure.dataset.imagePosition = savedImagePosition;
        if (!["izquierda", "centro", "derecha"].includes(figure.dataset.imageAlignment)) {
          figure.dataset.imageAlignment = Number(figure.dataset.imageX) >= 75 ? "derecha" : "izquierda";
        }
        figure.setAttribute("draggable", "false");
        figure.classList.add("editor-imagen-controles-ocultos");
        let imageMenuButton = figure.querySelector("[data-editor-abrir-controles-imagen]");
        if (!imageMenuButton) {
          imageMenuButton = document.createElement("button");
          imageMenuButton.type = "button";
          imageMenuButton.className = "editor-imagen-menu";
          imageMenuButton.dataset.editorUi = "true";
          imageMenuButton.dataset.editorAbrirControlesImagen = "";
          imageMenuButton.contentEditable = "false";
          imageMenuButton.textContent = "⋯";
          figure.append(imageMenuButton);
        }
        imageMenuButton.setAttribute("aria-label", "Abrir controles de imagen");
        imageMenuButton.title = "Editar imagen";
        delete figure.dataset.imageAlignmentPreview;
        const image = figure.querySelector("img");
        if (image) {
          image.dataset.imageKey = key;
          image.draggable = false;
          const position = /^(?:100|[1-9]?\d)% (?:100|[1-9]?\d)%$/.test(figure.dataset.imagePosition || "")
            ? figure.dataset.imagePosition
            : "50% 50%";
          actualizarEstiloEncuadreImagen(image, position, figure.dataset.imageCrop === "true");
          let frame = image.closest(".editor-imagen-marco");
          if (!frame) {
            frame = document.createElement("div");
            frame.className = "editor-imagen-marco";
            image.before(frame);
            frame.append(image);
          }
        }
        let button = figure.querySelector("[data-editor-borrar-imagen]");
        if (!button) {
          button = document.createElement("button");
          button.type = "button";
          button.className = "editor-borrar-bloque";
          button.dataset.editorUi = "true";
          button.contentEditable = "false";
          figure.append(button);
        }
        button.dataset.editorBorrarImagen = imageId;
        button.setAttribute("aria-label", "Quitar esta imagen");
        button.textContent = "Quitar";
        let topBar = figure.querySelector(".editor-imagen-barra-superior");
        if (!topBar) {
          topBar = document.createElement("div");
          topBar.className = "editor-imagen-barra editor-imagen-barra-superior";
          topBar.dataset.editorUi = "true";
          topBar.contentEditable = "false";
          figure.prepend(topBar);
        }
        topBar.replaceChildren();
        const applyButton = document.createElement("button");
        applyButton.type = "button";
        applyButton.className = "editor-imagen-aplicar";
        applyButton.dataset.editorAplicarMovimiento = imageId;
        applyButton.textContent = "Aplicar cambio";
        applyButton.hidden = true;
        const removeButton = button;
        topBar.append(applyButton, removeButton);
        const alignment = document.createElement("select");
        alignment.className = "editor-imagen-alineacion";
        alignment.dataset.editorImageAlignment = "";
        alignment.setAttribute("aria-label", "Alineación de la imagen respecto al texto");
        [["izquierda", "Izquierda"], ["centro", "Centro"], ["derecha", "Derecha"]].forEach(([value, label]) => {
          const option = document.createElement("option");
          option.value = value;
          option.textContent = label;
          alignment.append(option);
        });
        alignment.value = figure.dataset.imageAlignment;
        alignment.addEventListener("change", () => {
          figure.dataset.imageAlignmentPreview = alignment.value;
          actualizarEstadoEdicionImagen(figure);
        });
        topBar.append(alignment);
        applyButton.hidden = true;
      });
      const tableIds = new Set();
      editor.querySelectorAll("table").forEach(table => {
        let wrapper = table.closest(".editor-tabla-bloque");
        if (!wrapper) {
          wrapper = document.createElement("div");
          wrapper.className = "editor-tabla-bloque";
          table.before(wrapper);
          wrapper.append(table);
        }
        let tableId = wrapper.dataset.tableId || "";
        if (!/^[a-z0-9-]+$/i.test(tableId) || tableIds.has(tableId)) tableId = makeId();
        tableIds.add(tableId);
        wrapper.dataset.tableId = tableId;
        let button = wrapper.querySelector("[data-editor-borrar-tabla]");
        if (!button) {
          button = document.createElement("button");
          button.type = "button";
          button.className = "editor-borrar-bloque";
          button.dataset.editorUi = "true";
          button.contentEditable = "false";
          wrapper.prepend(button);
        }
        button.dataset.editorBorrarTabla = tableId;
        button.setAttribute("aria-label", "Quitar esta tabla");
        button.textContent = "Quitar tabla";
      });
    }

    function puntoEnTexto(container, offset) {
      const walker = document.createTreeWalker(container, NodeFilter.SHOW_TEXT);
      let node;
      let remaining = Math.max(0, offset);
      let last = null;
      while ((node = walker.nextNode())) {
        last = node;
        if (remaining <= node.length) return { node, offset: remaining };
        remaining -= node.length;
      }
      if (!last) {
        last = document.createTextNode("");
        container.append(last);
      }
      return { node: last, offset: last.length };
    }

    function ponerEnCursor(editor, node) {
      editor.focus();
      const selection = window.getSelection();
      const range = rangoEditorGuardado && editor.contains(rangoEditorGuardado.commonAncestorContainer)
        ? rangoEditorGuardado.cloneRange()
        : selection?.rangeCount && editor.contains(selection.anchorNode) ? selection.getRangeAt(0).cloneRange() : null;
      const esTablaEditor = node.nodeName === "DIV" && node.classList.contains("editor-tabla-bloque");
      const esBloque = ["FIGURE", "TABLE", "HR", "UL", "OL"].includes(node.nodeName) || esTablaEditor;
      if (!range) {
        editor.append(node);
        return;
      }
      range.deleteContents();
      range.collapse(true);
      const anchor = range.startContainer.nodeType === Node.ELEMENT_NODE ? range.startContainer : range.startContainer.parentElement;
      const bloque = anchor?.closest("p,h1,h2,h3,blockquote,li,div");
      const dentroDeCelda = anchor?.closest("td,th");
      if (esBloque && bloque && bloque !== editor && !dentroDeCelda && editor.contains(bloque)) {
        const posterior = document.createRange();
        posterior.selectNodeContents(bloque);
        posterior.setStart(range.startContainer, range.startOffset);
        const contenidoPosterior = posterior.extractContents();
        const bloquePosterior = bloque.cloneNode(false);
        bloquePosterior.append(contenidoPosterior);
        const tieneContenidoPosterior = bloquePosterior.textContent.replace(/\u200B/g, "").length > 0
          || Boolean(bloquePosterior.querySelector("br,img,table,hr,input,figure,ul,ol"));
        const insercion = document.createRange();
        insercion.setStartAfter(bloque);
        insercion.insertNode(node);
        if (tieneContenidoPosterior) node.after(bloquePosterior);
        const table = node.nodeName === "TABLE" ? node : node.querySelector?.("table");
        if (table?.rows.length && table.rows[0].cells.length) {
          range.selectNodeContents(table.rows[0].cells[0]);
          range.collapse(true);
        } else if (tieneContenidoPosterior) {
          range.setStart(bloquePosterior, 0);
          range.collapse(true);
        } else {
          range.setStartAfter(node);
          range.collapse(true);
        }
      } else {
        range.insertNode(node);
        range.setStartAfter(node);
        range.collapse(true);
      }
      selection.removeAllRanges();
      selection.addRange(range);
      rangoEditorGuardado = range.cloneRange();
    }

    function guardarRangoActualEditor(editor) {
      const selection = window.getSelection();
      if (selection?.rangeCount && editor.contains(selection.anchorNode)) {
        rangoEditorGuardado = selection.getRangeAt(0).cloneRange();
      }
    }

    function actualizarPlaceholderParrafoEditor(editor) {
      editor.querySelectorAll("[data-placeholder]").forEach(bloque => bloque.removeAttribute("data-placeholder"));
      if (document.activeElement !== editor) return;
      const selection = window.getSelection();
      if (!selection?.isCollapsed || !selection.anchorNode || !editor.contains(selection.anchorNode)) return;
      const nodo = selection.anchorNode.nodeType === Node.ELEMENT_NODE
        ? selection.anchorNode
        : selection.anchorNode.parentElement;
      const bloque = nodo?.closest("p, div");
      if (!bloque || bloque === editor || !editor.contains(bloque)) return;
      const texto = (bloque.textContent || "").replace(/\u200B/g, "").trim();
      if (texto || bloque.querySelector("img, video, audio, iframe, table, hr, input, figure")) return;
      bloque.setAttribute("data-placeholder", "Presioná '/' para ver los comandos");
    }

    document.addEventListener("selectionchange", () => {
      const editor = document.getElementById("pagina-cuerpo");
      if (editor) actualizarPlaceholderParrafoEditor(editor);
    });

    function colocarCursorEnPunto(editor, x, y) {
      let range = document.caretRangeFromPoint?.(x, y) || null;
      if (!range && document.caretPositionFromPoint) {
        const point = document.caretPositionFromPoint(x, y);
        if (point) {
          range = document.createRange();
          range.setStart(point.offsetNode, point.offset);
          range.collapse(true);
        }
      }
      if (!range || !editor.contains(range.startContainer)) return;
      const selection = window.getSelection();
      selection.removeAllRanges();
      selection.addRange(range);
      rangoEditorGuardado = range.cloneRange();
    }

    function actualizarEstiloEncuadreImagen(image, position, crop) {
      if (!image) return;
      image.style.objectPosition = position;
      image.style.objectFit = crop ? "cover" : "";
      image.style.height = crop ? "100%" : "";
    }

    function actualizarEstadoEdicionImagen(figure) {
      const applyButton = figure.querySelector("[data-editor-aplicar-movimiento]");
      if (applyButton) applyButton.hidden = figure.classList.contains("editor-imagen-controles-ocultos");
    }

    async function comprimirImagen(archivo) {
      const bitmap = await createImageBitmap(archivo);
      try {
        const scale = Math.min(1, 2400 / Math.max(bitmap.width, bitmap.height));
        if (scale === 1) return archivo;
        const canvas = document.createElement("canvas");
        canvas.width = Math.max(1, Math.round(bitmap.width * scale));
        canvas.height = Math.max(1, Math.round(bitmap.height * scale));
        const context = canvas.getContext("2d");
        if (!context) throw new Error("No se pudo preparar la imagen.");
        context.imageSmoothingEnabled = true;
        context.imageSmoothingQuality = "high";
        context.drawImage(bitmap, 0, 0, canvas.width, canvas.height);
        const imageType = archivo.type === "image/png" ? "image/png" : "image/jpeg";
        return await new Promise((resolve, reject) => {
          canvas.toBlob(blob => blob ? resolve(blob) : reject(new Error("No se pudo comprimir la imagen.")), imageType, imageType === "image/jpeg" ? 0.92 : undefined);
        });
      } finally {
        bitmap.close();
      }
    }

    async function insertarArchivoImagen(editor, archivo) {
      if (!archivo || !archivo.type.startsWith("image/")) return;
      try {
        const blob = await comprimirImagen(archivo);
        const key = makeId();
        await guardarImagenEnBaseDatos(key, blob);
        const figure = document.createElement("figure");
        figure.className = "editor-imagen";
        figure.dataset.imageKey = key;
        figure.dataset.imageId = makeId();
        figure.style.width = "60%";
        figure.setAttribute("draggable", "false");
        const image = document.createElement("img");
        image.src = URL.createObjectURL(blob);
        image.dataset.imageKey = key;
        image.alt = archivo.name || "Imagen insertada";
        const caption = document.createElement("figcaption");
        caption.contentEditable = "true";
        caption.textContent = "Escribe un pie de foto (opcional)";
        figure.append(image, caption);
        ponerEnCursor(editor, figure);
        instalarControlesBloquesEditor(editor);
        editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertFromPaste" }));
      } catch (error) {
        console.error("No se pudo insertar la imagen:", error);
        window.alert("No se pudo guardar la imagen. Revisa el espacio disponible y vuelve a intentarlo.");
      }
    }

    function insertarTabla(editor) {
      editorTablaPendiente = editor;
      const dialog = document.getElementById("editor-dialog-tabla");
      dialog.hidden = false;
      document.getElementById("editor-tabla-filas").focus();
    }

    function confirmarInsercionTabla(event) {
      event.preventDefault();
      const editor = editorTablaPendiente;
      if (!editor?.isConnected) return;
      const filas = Number(document.getElementById("editor-tabla-filas").value);
      const columnas = Number(document.getElementById("editor-tabla-columnas").value);
      if (!Number.isInteger(filas) || !Number.isInteger(columnas) || filas < 1 || filas > 20 || columnas < 1 || columnas > 20) {
        window.alert("Elige entre 1 y 20 filas y columnas.");
        return;
      }
      const table = document.createElement("table");
      const wrapper = document.createElement("div");
      wrapper.className = "editor-tabla-bloque";
      wrapper.dataset.tableId = makeId();
      const body = document.createElement("tbody");
      for (let rowIndex = 0; rowIndex < filas; rowIndex++) {
        const row = body.insertRow();
        for (let columnIndex = 0; columnIndex < columnas; columnIndex++) row.insertCell().append(document.createElement("br"));
      }
      table.append(body);
      wrapper.append(table);
      ponerEnCursor(editor, wrapper);
      instalarControlesBloquesEditor(editor);
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertFromPaste" }));
      document.getElementById("editor-dialog-tabla").hidden = true;
      editorTablaPendiente = null;
    }

    function celdaActivaEditor(editor) {
      const range = rangoEditorGuardado;
      const node = range?.commonAncestorContainer;
      const element = node?.nodeType === Node.ELEMENT_NODE ? node : node?.parentElement;
      const cell = element?.closest("td, th");
      return cell && editor.contains(cell) ? cell : null;
    }

    function modificarTabla(editor, action) {
      const cell = celdaActivaEditor(editor);
      const row = cell?.closest("tr");
      const table = cell?.closest("table");
      if (!cell || !row || !table) return;
      const index = cell.cellIndex;
      if (action === "row-add") {
        const newRow = row.parentElement.insertRow(row.sectionRowIndex + 1);
        Array.from(row.cells).forEach(() => newRow.insertCell().append(document.createElement("br")));
      }
      if (action === "row-remove" && row.parentElement.rows.length > 1) row.remove();
      if (action === "column-add") Array.from(table.rows).forEach(current => current.insertCell(index + 1).append(document.createElement("br")));
      if (action === "column-remove" && row.cells.length > 1) Array.from(table.rows).forEach(current => current.deleteCell(index));
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertReplacementText" }));
    }

    function quitarImagenEditor(editor, imageId) {
      if (!imageId || !window.confirm("¿Quitar esta imagen?")) return false;
      const figure = editor.querySelector(`.editor-imagen[data-image-id="${CSS.escape(imageId)}"]`);
      if (!figure || !editor.contains(figure)) return false;
      figure.remove();
      imagenEditorActiva = null;
      imagenEditorActivaId = "";
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "deleteContent" }));
      return true;
    }

    function quitarTablaEditor(editor, tableId) {
      if (!tableId || !window.confirm("¿Quitar esta tabla?")) return false;
      const wrapper = editor.querySelector(`.editor-tabla-bloque[data-table-id="${CSS.escape(tableId)}"]`);
      if (!wrapper || !editor.contains(wrapper)) return false;
      wrapper.remove();
      tablaEditorActivaId = "";
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "deleteContent" }));
      return true;
    }

    function aplicarComandoEditor(editor, command) {
      editor.focus();
      if (rangoEditorGuardado && editor.contains(rangoEditorGuardado.commonAncestorContainer)) {
        const selection = window.getSelection();
        selection.removeAllRanges();
        selection.addRange(rangoEditorGuardado.cloneRange());
      }
      if (["bold", "italic", "underline", "strikeThrough", "justifyLeft", "justifyCenter", "justifyRight", "justifyFull", "undo", "redo"].includes(command)) document.execCommand(command);
      else if (["h1", "h2", "h3", "blockquote"].includes(command)) aplicarFormatoBloque(editor, command);
      else if (command === "paragraph") aplicarFormatoBloque(editor, "p");
      else if (command === "ul" || command === "ol") document.execCommand(command === "ul" ? "insertUnorderedList" : "insertOrderedList");
      else if (command === "task") {
        const list = document.createElement("ul");
        list.className = "editor-tareas";
        const item = document.createElement("li");
        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.contentEditable = "false";
        const text = document.createElement("span");
        text.textContent = " Tarea";
        item.append(checkbox, text);
        list.append(item);
        ponerEnCursor(editor, list);
      } else if (command === "hr") ponerEnCursor(editor, document.createElement("hr"));
      else if (command === "image") document.getElementById("editor-archivo-imagen").click();
      else if (command === "table") insertarTabla(editor);
      else if (command.startsWith("row-") || command.startsWith("column-")) modificarTabla(editor, command);
      else if (command === "image-remove") quitarImagenEditor(editor, imagenEditorActivaId);
      else if (command === "table-remove") quitarTablaEditor(editor, tablaEditorActivaId);
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "formatText" }));
      guardarRangoActualEditor(editor);
      actualizarEstadoControlesTexto(editor);
    }

    function aplicarComandoConValorEditor(editor, command, value) {
      editor.focus();
      if (rangoEditorGuardado && editor.contains(rangoEditorGuardado.commonAncestorContainer)) {
        const selection = window.getSelection();
        selection.removeAllRanges();
        selection.addRange(rangoEditorGuardado.cloneRange());
      }
      document.execCommand("styleWithCSS", false, true);
      document.execCommand(command, false, value);
      document.execCommand("styleWithCSS", false, false);
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "formatText" }));
      guardarRangoActualEditor(editor);
      actualizarEstadoControlesTexto(editor);
    }

    function actualizarEstadoControlesTexto(editor) {
      const panel = document.getElementById("editor-panel");
      const selection = window.getSelection();
      if (!panel || !selection?.rangeCount || !editor.contains(selection.anchorNode)) return;
      panel.querySelectorAll("[data-editor-command][aria-pressed]").forEach(button => {
        const command = button.dataset.editorCommand;
        let active = false;
        try { active = document.queryCommandState(command); }
        catch (error) { console.error(`No se pudo consultar el formato ${command}:`, error); }
        button.setAttribute("aria-pressed", String(active));
      });
      const node = selection.anchorNode.nodeType === Node.ELEMENT_NODE ? selection.anchorNode : selection.anchorNode.parentElement;
      const block = node?.closest("h1,h2,h3,blockquote,p");
      const heading = panel.querySelector("[data-editor-heading]");
      if (heading && block) heading.value = ({ H1: "h1", H2: "h2", H3: "h3", BLOCKQUOTE: "blockquote" })[block.tagName] || "paragraph";
      const spacing = panel.querySelector("[data-editor-spacing]");
      if (spacing && block) spacing.value = ["chico", "grande"].includes(block.dataset.editorEspaciado) ? block.dataset.editorEspaciado : "normal";
    }

    function aplicarEspaciadoBloqueEditor(editor, spacingValue) {
      const selection = window.getSelection();
      if (!selection?.rangeCount || !editor.contains(selection.anchorNode)) return;
      const node = selection.anchorNode.nodeType === Node.ELEMENT_NODE ? selection.anchorNode : selection.anchorNode.parentElement;
      const block = node?.closest("p,h1,h2,h3");
      if (!block || !editor.contains(block)) return;
      if (["chico", "grande"].includes(spacingValue)) block.dataset.editorEspaciado = spacingValue;
      else delete block.dataset.editorEspaciado;
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "formatText" }));
      actualizarEstadoControlesTexto(editor);
    }

    function aplicarFormatoBloque(editor, tagName) {
      const selection = window.getSelection();
      if (!selection?.rangeCount || !editor.contains(selection.anchorNode)) return;
      const node = selection.anchorNode.nodeType === Node.ELEMENT_NODE ? selection.anchorNode : selection.anchorNode.parentElement;
      const block = node?.closest("p,div,h1,h2,h3,blockquote");
      if (!block || block === editor || !editor.contains(block)) {
        document.execCommand("formatBlock", false, tagName);
        return;
      }
      convertirBloqueEditor(editor, block, tagName);
    }

    function convertirBloqueEditor(editor, block, tagName) {
      const selection = window.getSelection();
      const replacement = document.createElement(tagName);
      while (block.firstChild) replacement.append(block.firstChild);
      const necesitaCursorVacio = !replacement.textContent && !replacement.querySelector("img,table,input");
      const marcador = necesitaCursorVacio ? document.createTextNode("\u200B") : null;
      if (marcador) {
        replacement.replaceChildren(marcador);
        marcadorCursorPendiente = true;
      }
      block.replaceWith(replacement);
      const range = document.createRange();
      if (marcador) range.setStart(marcador, marcador.length);
      else {
        range.selectNodeContents(replacement);
        range.collapse(false);
      }
      selection.removeAllRanges();
      selection.addRange(range);
      guardarRangoActualEditor(editor);
    }

    function limpiarMarcadorCursorEditor(editor) {
      const walker = document.createTreeWalker(editor, NodeFilter.SHOW_TEXT);
      const selection = window.getSelection();
      const anchor = selection?.rangeCount ? { node: selection.anchorNode, offset: selection.anchorOffset } : null;
      const focus = selection?.rangeCount ? { node: selection.focusNode, offset: selection.focusOffset } : null;
      let node;
      while ((node = walker.nextNode())) {
        const value = node.nodeValue || "";
        if (!value.includes("\u200B")) continue;
        if (anchor?.node === node) anchor.offset = value.slice(0, anchor.offset).replace(/\u200B/g, "").length;
        if (focus?.node === node) focus.offset = value.slice(0, focus.offset).replace(/\u200B/g, "").length;
        node.nodeValue = value.replace(/\u200B/g, "");
      }
      if (anchor && focus && editor.contains(anchor.node) && editor.contains(focus.node)) {
        if (typeof selection.setBaseAndExtent === "function") selection.setBaseAndExtent(anchor.node, anchor.offset, focus.node, focus.offset);
        else {
          const range = document.createRange();
          range.setStart(anchor.node, anchor.offset);
          range.setEnd(focus.node, focus.offset);
          selection.removeAllRanges();
          selection.addRange(range);
        }
      }
    }

    function sincronizarControlesFondoVector(panel, pagina) {
      const settings = estadoFondoVectorial(pagina);
      const gallery = panel.querySelector("[data-editor-vector-gallery]");
      gallery.querySelectorAll("[data-editor-vector-id]").forEach(button => {
        button.setAttribute("aria-pressed", String(button.dataset.editorVectorId === settings?.id));
      });
      const values = {
        "[data-editor-vector-color]": settings?.color || "#64748B",
        "[data-editor-vector-opacity]": String(settings?.opacity ?? 45),
        "[data-editor-vector-size]": settings?.size || "cover",
        "[data-editor-vector-scale]": String(settings?.scale ?? 100),
        "[data-editor-vector-position]": settings?.position || "center",
        "[data-editor-vector-soften]": String(settings?.soften ?? 45)
      };
      Object.entries(values).forEach(([selector, value]) => {
        const control = panel.querySelector(selector);
        control.value = value;
        control.disabled = !settings || (selector === "[data-editor-vector-scale]" && settings.size !== "scale");
      });
      panel.querySelector("[data-editor-vector-remove]").disabled = !settings;
      panel.querySelector("[data-editor-vector-opacity-value]").value = `${settings?.opacity ?? 45}%`;
      panel.querySelector("[data-editor-vector-scale-value]").value = `${settings?.scale ?? 100}%`;
      panel.querySelector("[data-editor-vector-soften-value]").value = `${settings?.soften ?? 45}%`;
      panel.querySelector("[data-editor-vector-help]").textContent = settings
        ? "Vista previa activa; los cambios se guardan en esta página."
        : "Elige un vector para mostrar estos ajustes.";
    }

    function configurarFondoVectorEditor(panel, pagina) {
      const gallery = panel.querySelector("[data-editor-vector-gallery]");
      gallery.replaceChildren();
      const fondos = fondosVectorialesEditor();
      fondos.forEach(fondo => {
        const button = document.createElement("button");
        button.type = "button";
        button.className = "editor-fondo-vector-opcion";
        button.dataset.editorVectorId = fondo.id;
        button.setAttribute("aria-label", `Usar fondo ${fondo.nombre}`);
        button.title = fondo.nombre;
        const svg = svgFondoConAjustes(fondo.svg, "#64748B", 100);
        button.style.backgroundImage = `url("data:image/svg+xml;charset=utf-8,${encodeURIComponent(svg)}")`;
        button.addEventListener("click", () => {
          const controls = panel.querySelector(".editor-fondo-vector-ajustes");
          const previous = pagina.fondoVectorial;
          pagina.fondoVectorial = {
            id: fondo.id,
            svg: sanitizarSvgFondo(fondo.svg),
            color: controls.querySelector("[data-editor-vector-color]").value,
            opacity: Number(controls.querySelector("[data-editor-vector-opacity]").value),
            size: controls.querySelector("[data-editor-vector-size]").value,
            scale: Number(controls.querySelector("[data-editor-vector-scale]").value),
            position: controls.querySelector("[data-editor-vector-position]").value,
            soften: Number(controls.querySelector("[data-editor-vector-soften]").value)
          };
          aplicarFondoVectorialEditor(pagina);
          sincronizarControlesFondoVector(panel, pagina);
          if (!guardarCarpetas()) {
            if (previous === undefined) delete pagina.fondoVectorial;
            else pagina.fondoVectorial = previous;
            aplicarFondoVectorialEditor(pagina);
            sincronizarControlesFondoVector(panel, pagina);
          }
        });
        gallery.append(button);
      });
      sincronizarControlesFondoVector(panel, pagina);
      const actualizarDesdeControles = persistir => {
        if (!pagina.fondoVectorial) return;
        const previous = pagina.fondoVectorial;
        const controls = panel.querySelector(".editor-fondo-vector-ajustes");
        pagina.fondoVectorial = {
          ...previous,
          color: controls.querySelector("[data-editor-vector-color]").value,
          opacity: Number(controls.querySelector("[data-editor-vector-opacity]").value),
          size: controls.querySelector("[data-editor-vector-size]").value,
          scale: Number(controls.querySelector("[data-editor-vector-scale]").value),
          position: controls.querySelector("[data-editor-vector-position]").value,
          soften: Number(controls.querySelector("[data-editor-vector-soften]").value)
        };
        aplicarFondoVectorialEditor(pagina);
        sincronizarControlesFondoVector(panel, pagina);
        if (persistir && !guardarCarpetas()) {
          pagina.fondoVectorial = previous;
          aplicarFondoVectorialEditor(pagina);
          sincronizarControlesFondoVector(panel, pagina);
        }
      };
      panel.querySelectorAll("[data-editor-vector-color], [data-editor-vector-opacity], [data-editor-vector-size], [data-editor-vector-scale], [data-editor-vector-position], [data-editor-vector-soften]").forEach(control => {
        control.addEventListener("input", () => actualizarDesdeControles(false));
        control.addEventListener("change", () => actualizarDesdeControles(true));
      });
      panel.querySelector("[data-editor-vector-upload]").addEventListener("click", () => panel.querySelector("[data-editor-vector-file]").click());
      panel.querySelector("[data-editor-vector-file]").addEventListener("change", async event => {
        const input = event.currentTarget;
        const file = input.files?.[0];
        if (!file) return;
        try {
          const sanitizedSvg = sanitizarSvgFondo(await file.text());
          const controls = panel.querySelector(".editor-fondo-vector-ajustes");
          const previous = pagina.fondoVectorial;
          controls.querySelector("[data-editor-vector-opacity]").value = "100";
          controls.querySelector("[data-editor-vector-soften]").value = "0";
          pagina.fondoVectorial = {
            id: "custom",
            svg: sanitizedSvg,
            color: controls.querySelector("[data-editor-vector-color]").value,
            opacity: 100,
            size: controls.querySelector("[data-editor-vector-size]").value,
            scale: Number(controls.querySelector("[data-editor-vector-scale]").value),
            position: controls.querySelector("[data-editor-vector-position]").value,
            soften: 0
          };
          aplicarFondoVectorialEditor(pagina);
          sincronizarControlesFondoVector(panel, pagina);
          const status = document.getElementById("corrector-estado");
          if (status) {
            status.textContent = "SVG propio cargado y guardado.";
            status.hidden = false;
          }
          if (!guardarCarpetas()) {
            if (previous === undefined) delete pagina.fondoVectorial;
            else pagina.fondoVectorial = previous;
            aplicarFondoVectorialEditor(pagina);
            sincronizarControlesFondoVector(panel, pagina);
            if (status) status.textContent = "El SVG se cargó, pero no se pudo guardar. Libera espacio y vuelve a intentarlo.";
          }
        } catch (error) {
          console.error("No se pudo cargar el SVG del fondo:", error);
          const status = document.getElementById("corrector-estado");
          if (status) {
            status.textContent = error instanceof Error ? error.message : "No se pudo leer el archivo SVG.";
            status.hidden = false;
          }
        } finally {
          input.value = "";
        }
      });
      panel.querySelector("[data-editor-vector-remove]").addEventListener("click", () => {
        const previous = pagina.fondoVectorial;
        delete pagina.fondoVectorial;
        aplicarFondoVectorialEditor(pagina);
        sincronizarControlesFondoVector(panel, pagina);
        if (!guardarCarpetas()) {
          pagina.fondoVectorial = previous;
          aplicarFondoVectorialEditor(pagina);
          sincronizarControlesFondoVector(panel, pagina);
        }
      });
    }

    function enlazarControlesEditor(editor) {
      configurarPanelEdicion();
      const panel = document.getElementById("editor-panel");
      const pagina = paginaActualAbierta();
      if (!pagina) throw new Error("No se encontró la página activa para configurar el fondo vectorial.");
      configurarFondoVectorEditor(panel, pagina);
      let opacidadPanelGuardada = Number.isFinite(paginaActualAbierta()?.fondoOpacidadPanel)
        ? paginaActualAbierta().fondoOpacidadPanel
        : 85;
      panel.addEventListener("mousedown", event => {
        const control = event.target.closest("[data-editor-command], [data-editor-font], [data-editor-font-size], [data-editor-heading], [data-editor-spacing], [data-editor-color], [data-editor-highlight]");
        if (!control) return;
        guardarRangoActualEditor(editor);
        if (control.matches("button")) event.preventDefault();
      });
      panel.addEventListener("click", event => {
        const button = event.target.closest("[data-editor-command]");
        if (button) aplicarComandoEditor(editor, button.dataset.editorCommand);
      });
      panel.querySelector("[data-editor-font]").addEventListener("change", event => aplicarComandoConValorEditor(editor, "fontName", event.target.value));
      panel.querySelector("[data-editor-font-size]").addEventListener("change", event => aplicarComandoConValorEditor(editor, "fontSize", event.target.value));
      panel.querySelector("[data-editor-heading]").addEventListener("change", event => aplicarComandoEditor(editor, event.target.value));
      panel.querySelector("[data-editor-spacing]").addEventListener("change", event => aplicarEspaciadoBloqueEditor(editor, event.target.value));
      panel.querySelector("[data-editor-color]").addEventListener("input", event => aplicarComandoConValorEditor(editor, "foreColor", event.target.value));
      panel.querySelector("[data-editor-highlight]").addEventListener("input", event => aplicarComandoConValorEditor(editor, "hiliteColor", event.target.value));
      const controlOpacidadPanel = document.getElementById("editor-panel-opacidad");
      const valorOpacidadPanel = document.getElementById("editor-panel-opacidad-valor");
      controlOpacidadPanel.addEventListener("input", event => {
        const opacidad = Math.max(40, Math.min(100, Number(event.currentTarget.value)));
        const pagina = paginaActualAbierta();
        if (!pagina) return;
        pagina.fondoOpacidadPanel = opacidad;
        document.getElementById("lectura-panel").style.setProperty("--lectura-opacidad-panel", `${opacidad}%`);
        valorOpacidadPanel.value = `${opacidad}%`;
      });
      controlOpacidadPanel.addEventListener("change", event => {
        const pagina = paginaActualAbierta();
        if (!pagina) return;
        const opacidad = Math.max(40, Math.min(100, Number(event.currentTarget.value)));
        pagina.fondoOpacidadPanel = opacidad;
        if (!guardarCarpetas()) {
          pagina.fondoOpacidadPanel = opacidadPanelGuardada;
          event.currentTarget.value = String(opacidadPanelGuardada);
          valorOpacidadPanel.value = `${opacidadPanelGuardada}%`;
          document.getElementById("lectura-panel").style.setProperty("--lectura-opacidad-panel", `${opacidadPanelGuardada}%`);
          return;
        }
        opacidadPanelGuardada = opacidad;
      });
      document.getElementById("editor-panel-tono").addEventListener("change", async event => {
        const pagina = paginaActualAbierta();
        if (!pagina) return;
        const tonoAnterior = pagina.fondoTonoPanel;
        pagina.fondoTonoPanel = ["claro", "oscuro"].includes(event.currentTarget.value)
          ? event.currentTarget.value
          : "automatico";
        aplicarEstiloPanelLectura(pagina);
        if (pagina.fondoTonoPanel === "automatico") {
          await asegurarTonoAutomaticoFondo(pagina);
        }
        if (!guardarCarpetas()) {
          if (tonoAnterior === undefined) delete pagina.fondoTonoPanel;
          else pagina.fondoTonoPanel = tonoAnterior;
          aplicarEstiloPanelLectura(pagina);
        }
      });
      document.getElementById("editor-panel-texto-estilo").addEventListener("change", event => {
        const pagina = paginaActualAbierta();
        const readingPanel = document.getElementById("lectura-panel");
        if (!pagina || !readingPanel) return;
        const anterior = pagina.fondoEstiloPanel;
        const estilo = ["vidrio", "solido", "sin-panel"].includes(event.currentTarget.value)
          ? event.currentTarget.value
          : "vidrio";
        pagina.fondoEstiloPanel = estilo;
        readingPanel.classList.remove("lectura-panel-vidrio", "lectura-panel-solido", "lectura-panel-sin-panel");
        readingPanel.classList.add(`lectura-panel-${estilo}`);
        if (!guardarCarpetas()) {
          if (anterior === undefined) delete pagina.fondoEstiloPanel;
          else pagina.fondoEstiloPanel = anterior;
          const estiloAnterior = ["vidrio", "solido", "sin-panel"].includes(anterior) ? anterior : "vidrio";
          event.currentTarget.value = estiloAnterior;
          readingPanel.classList.remove("lectura-panel-vidrio", "lectura-panel-solido", "lectura-panel-sin-panel");
          readingPanel.classList.add(`lectura-panel-${estiloAnterior}`);
        }
      });
      document.getElementById("editor-fondo-elegir").addEventListener("mousedown", event => {
        event.preventDefault();
        guardarRangoActualEditor(editor);
      });
      document.getElementById("editor-fondo-elegir").addEventListener("click", () => document.getElementById("editor-fondo-archivo").click());
      document.getElementById("editor-fondo-archivo").addEventListener("change", event => cambiarFondoPaginaEditor(event.currentTarget));
      document.getElementById("editor-fondo-gif-elegir").addEventListener("mousedown", event => {
        event.preventDefault();
        guardarRangoActualEditor(editor);
      });
      document.getElementById("editor-fondo-gif-elegir").addEventListener("click", () => document.getElementById("editor-fondo-gif-archivo").click());
      document.getElementById("editor-fondo-gif-archivo").addEventListener("change", event => cambiarFondoGifEditor(event.currentTarget));
      document.getElementById("editor-fondo-quitar").addEventListener("click", quitarFondoPaginaEditor);
      document.getElementById("editor-fondo-parallax-activo").addEventListener("change", async event => {
        const pagina = paginaActualAbierta();
        if (!pagina) return;
        const anterior = pagina.fondoParallax;
        pagina.fondoParallax = event.currentTarget.checked;
        if (!guardarCarpetas()) {
          pagina.fondoParallax = anterior;
          event.currentTarget.checked = anterior !== false;
        }
        await aplicarFondoPaginaEditor(pagina);
      });
      document.getElementById("editor-panel-toggle").addEventListener("mousedown", () => guardarRangoActualEditor(editor));
      document.getElementById("editor-panel-cerrar").addEventListener("mousedown", () => guardarRangoActualEditor(editor));
      document.getElementById("editor-tabla-form").addEventListener("submit", confirmarInsercionTabla);
      document.querySelector("[data-editor-cancelar-tabla]").addEventListener("click", () => {
        document.getElementById("editor-dialog-tabla").hidden = true;
        editorTablaPendiente = null;
        editor.focus();
      });
      document.getElementById("editor-archivo-imagen").addEventListener("change", event => {
        insertarArchivoImagen(editor, event.target.files?.[0]);
        event.target.value = "";
      });
      const menu = document.getElementById("editor-menu-comandos");
      menu.addEventListener("mousedown", event => event.preventDefault());
      menu.addEventListener("click", event => {
        const button = event.target.closest("[data-slash-command]");
        if (!button) return;
        ejecutarComandoSlash(editor, button.dataset.slashCommand);
      });
      editor.addEventListener("input", () => {
        indiceComandoActivo = 0;
        actualizarMenuComandos(editor);
      });
      editor.addEventListener("pointerup", () => {
        editor.querySelectorAll(".editor-imagen").forEach(figure => {
          if (getComputedStyle(figure).resize === "both") figure.style.width = `${Math.round(figure.getBoundingClientRect().width)}px`;
        });
      });
    }

    function puntoSeleccion(editor) {
      const selection = window.getSelection();
      if (!selection?.rangeCount || !editor.contains(selection.anchorNode)) return null;
      return { node: selection.anchorNode, offset: selection.anchorOffset, range: selection.getRangeAt(0) };
    }

    function obtenerPrefijoComandos(editor) {
      const punto = puntoSeleccion(editor);
      if (!punto) return null;
      const node = punto.node.nodeType === Node.ELEMENT_NODE ? punto.node : punto.node.parentElement;
      const bloque = node?.closest("p,div,h1,h2,h3,li") || editor;
      const range = document.createRange();
      range.selectNodeContents(bloque);
      range.setEnd(punto.node, punto.offset);
      const antes = range.toString();
      const match = antes.match(/(?:^|[ \t])\/([^\n]*)$/);
      if (!match) return null;
      const inicio = match.index + (match[0].startsWith("/") ? 0 : 1);
      return { bloque, query: match[1], inicio, punto };
    }

    function posicionarMenuComandos(menu, punto) {
      const rect = punto.range.getBoundingClientRect();
      const margen = 8;
      const separacion = 6;
      menu.style.maxHeight = "";
      const barraSuperior = document.querySelector(".topbar")?.getBoundingClientRect();
      const limiteSuperior = barraSuperior && barraSuperior.bottom > 0 && barraSuperior.top < window.innerHeight
        ? Math.max(margen, barraSuperior.bottom + margen)
        : margen;
      const limiteInferior = window.innerHeight - margen;
      const alturaPreferida = menu.offsetHeight;
      const espacioAbajo = Math.max(0, limiteInferior - rect.bottom - separacion);
      const espacioArriba = Math.max(0, rect.top - separacion - limiteSuperior);
      const cabeAbajo = alturaPreferida <= espacioAbajo;
      const cabeArriba = alturaPreferida <= espacioArriba;
      const abrirArriba = !cabeAbajo && (cabeArriba || espacioArriba > espacioAbajo);
      const espacioDisponible = abrirArriba ? espacioArriba : espacioAbajo;
      menu.style.maxHeight = `${Math.max(0, Math.min(330, espacioDisponible))}px`;
      const altoMenu = menu.offsetHeight;
      const topDeseado = abrirArriba ? rect.top - altoMenu - separacion : rect.bottom + separacion;
      const top = Math.max(limiteSuperior, Math.min(limiteInferior - altoMenu, topDeseado));
      const left = Math.max(margen, Math.min(window.innerWidth - menu.offsetWidth - margen, rect.left || rect.right || margen));
      const contenedor = menu.offsetParent || menu.parentElement;
      const contenedorRect = contenedor.getBoundingClientRect();
      menu.style.left = `${left - contenedorRect.left + contenedor.scrollLeft - contenedor.clientLeft}px`;
      menu.style.top = `${top - contenedorRect.top + contenedor.scrollTop - contenedor.clientTop}px`;
    }

    function actualizarMenuComandos(editor) {
      const menu = document.getElementById("editor-menu-comandos");
      window.clearTimeout(temporizadorCerrarComandos);
      const prefijo = obtenerPrefijoComandos(editor);
      if (!prefijo) { menu.hidden = true; menu.replaceChildren(); return; }
      const normalizarBusqueda = value => String(value || "").normalize("NFD").replace(/[\u0300-\u036f]/g, "").toLocaleLowerCase("es").trim();
      const query = normalizarBusqueda(prefijo.query);
      const opciones = COMANDOS_EDITOR.filter(item =>
        normalizarBusqueda(`${item.label} ${item.description} ${item.keywords}`).includes(query)
      );
      const encabezado = `<div class="editor-comandos-encabezado"><span class="editor-comandos-slash">/</span>${query ? "" : '<span class="editor-comandos-busqueda">Escribí para buscar</span>'}</div>`;
      const pie = '<div class="editor-comandos-pie"><span>Cerrar menú</span><span class="editor-comandos-pie-tecla">esc</span></div>';
      if (!opciones.length) {
        menu.innerHTML = `${encabezado}<div class="editor-comandos-lista"><div class="editor-comandos-sin-resultados" role="status">Sin resultados</div></div>${pie}`;
        menu.hidden = false;
        posicionarMenuComandos(menu, prefijo.punto);
        menu.dataset.opciones = "";
        temporizadorCerrarComandos = window.setTimeout(() => {
          menu.hidden = true;
          menu.replaceChildren();
        }, 850);
        return;
      }
      indiceComandoActivo = Math.min(indiceComandoActivo, opciones.length - 1);
      const grupos = Array.from(new Set(opciones.map(item => item.group)));
      const contenido = grupos.map(group => {
        const items = opciones.filter(item => item.group === group);
        return `<section class="editor-comandos-seccion" aria-label="${escapeHtml(group)}"><h3 class="editor-comandos-seccion-titulo">${escapeHtml(group)}</h3>${items.map(item => {
          const index = opciones.indexOf(item);
          return `<button type="button" role="option" data-slash-command="${escapeHtml(item.id)}" aria-selected="${index === indiceComandoActivo}"><span class="editor-comando-icono" aria-hidden="true">${item.icon}</span><span class="editor-comando-nombre">${escapeHtml(item.label)}</span>${item.shortcut ? `<span class="editor-comando-atajo">${escapeHtml(item.shortcut)}</span>` : ""}</button>`;
        }).join("")}</section>`;
      }).join("");
      menu.innerHTML = `${encabezado}<div class="editor-comandos-lista" role="listbox" aria-label="Comandos del editor">${contenido}</div>${pie}`;
      menu.hidden = false;
      posicionarMenuComandos(menu, prefijo.punto);
      menu.dataset.opciones = opciones.map(item => item.id).join(",");
    }

    function quitarConsultaComandos(editor) {
      const prefijo = obtenerPrefijoComandos(editor);
      if (!prefijo) return;
      const range = document.createRange();
      const start = puntoEnTexto(prefijo.bloque, prefijo.inicio);
      range.setStart(start.node, start.offset);
      range.setEnd(prefijo.punto.node, prefijo.punto.offset);
      range.deleteContents();
      prefijo.bloque.querySelectorAll("strong,b,em,i,u,s,del,span").forEach(node => {
        if (!node.textContent && !node.children.length) node.remove();
      });
      range.collapse(true);
      const selection = window.getSelection();
      selection.removeAllRanges();
      selection.addRange(range);
      rangoEditorGuardado = range.cloneRange();
    }

    function moverSeleccionComando(event) {
      const menu = document.getElementById("editor-menu-comandos");
      if (menu.hidden) return;
      const options = Array.from(menu.querySelectorAll("[data-slash-command]"));
      if (!options.length) return;
      event.preventDefault();
      indiceComandoActivo = (indiceComandoActivo + (event.key === "ArrowDown" ? 1 : -1) + options.length) % options.length;
      options.forEach((option, index) => option.setAttribute("aria-selected", String(index === indiceComandoActivo)));
      options[indiceComandoActivo].scrollIntoView({ block: "nearest" });
    }

    function ejecutarComandoMenu(editor) {
      const menu = document.getElementById("editor-menu-comandos");
      const options = Array.from(menu.querySelectorAll("[data-slash-command]"));
      const command = options[indiceComandoActivo]?.dataset.slashCommand;
      if (!command) return;
      ejecutarComandoSlash(editor, command);
    }

    function agruparTextoSueltoEditor(editor) {
      const etiquetasBloque = new Set(["BLOCKQUOTE", "DIV", "FIGURE", "H1", "H2", "H3", "HR", "OL", "P", "PRE", "TABLE", "UL"]);
      let grupo = [];
      const envolverGrupo = () => {
        if (!grupo.length) return;
        const parrafo = document.createElement("p");
        grupo[0].before(parrafo);
        grupo.forEach(node => parrafo.append(node));
        grupo = [];
      };
      Array.from(editor.childNodes).forEach(node => {
        if (node.nodeType === Node.ELEMENT_NODE && etiquetasBloque.has(node.tagName)) {
          envolverGrupo();
          return;
        }
        grupo.push(node);
      });
      envolverGrupo();
    }

    function ejecutarComandoSlash(editor, command) {
      const menu = document.getElementById("editor-menu-comandos");
      if (obtenerPrefijoComandos(editor)?.bloque === editor) agruparTextoSueltoEditor(editor);
      const bloqueSlash = obtenerPrefijoComandos(editor)?.bloque;
      window.clearTimeout(temporizadorCerrarComandos);
      quitarConsultaComandos(editor);
      menu.hidden = true;
      if (["h1", "h2", "h3", "paragraph"].includes(command) && bloqueSlash && bloqueSlash !== editor && editor.contains(bloqueSlash)) {
        convertirBloqueEditor(editor, bloqueSlash, command === "paragraph" ? "p" : command);
        editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "formatText" }));
        return;
      }
      if (command === "code-block" && bloqueSlash && bloqueSlash !== editor && editor.contains(bloqueSlash)) {
        convertirBloqueEditor(editor, bloqueSlash, "pre");
        editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "formatText" }));
        return;
      }
      aplicarComandoEditor(editor, command);
    }

    function textoPlanoEditor(editor) {
      return mapearTextoEditor(editor).text;
    }

    function mapearTextoEditor(editor) {
      const partes = [];
      const nodos = [];
      let longitud = 0;
      const bloques = new Set(["ADDRESS", "BLOCKQUOTE", "DIV", "H1", "H2", "H3", "LI", "OL", "P", "PRE", "TABLE", "TR", "UL"]);
      const recorrer = node => {
        if (node.nodeType === Node.TEXT_NODE) {
          const text = node.nodeValue || "";
          nodos.push({ node, start: longitud, end: longitud + text.length });
          partes.push(text);
          longitud += text.length;
          return;
        }
        if (node.nodeType !== Node.ELEMENT_NODE) return;
        const esBloque = bloques.has(node.tagName);
        if (esBloque && longitud && partes[partes.length - 1] !== "\n") {
          partes.push("\n");
          longitud++;
        }
        if (node.tagName === "BR") { partes.push("\n"); longitud++; return; }
        Array.from(node.childNodes).forEach(recorrer);
      };
      Array.from(editor.childNodes).forEach(recorrer);
      return { text: partes.join(""), nodos };
    }

    function rangoTextoEditor(editor, start, end) {
      const { nodos } = mapearTextoEditor(editor);
      const buscarPunto = (offset, esFinal) => {
        const segmento = nodos.find(item => esFinal
          ? offset > item.start && offset <= item.end
          : offset >= item.start && offset < item.end);
        if (segmento) return { node: segmento.node, offset: offset - segmento.start };
        const borde = nodos.find(item => offset === item.start || offset === item.end);
        return borde ? { node: borde.node, offset: offset - borde.start } : null;
      };
      const inicio = buscarPunto(start, false);
      const fin = buscarPunto(end, true);
      if (!inicio || !fin) return null;
      const range = document.createRange();
      range.setStart(inicio.node, inicio.offset);
      range.setEnd(fin.node, fin.offset);
      return range;
    }

    function buscarErrorCorrectorEnPunto(x, y) {
      return sugerenciasCorrector.find(match =>
        Array.from(match.range.getClientRects()).some(rect => x >= rect.left && x <= rect.right && y >= rect.top && y <= rect.bottom)
      ) || null;
    }

    function aplicarMarcasCorrector(editor, matches) {
      desenvolverErroresCorrector(editor);
      const text = textoPlanoEditor(editor);
      const rangos = matches
        .filter(match => Number.isInteger(match.offset) && Number.isInteger(match.length) && match.length > 0 && Array.isArray(match.replacements) && match.replacements.length && typeof match.replacements[0].value === "string")
        .map(match => ({ inicio: match.offset, fin: match.offset + match.length, sugerencia: match.replacements[0].value }))
        .filter(match => match.inicio >= 0 && match.fin <= text.length)
        .map(match => ({ ...match, range: rangoTextoEditor(editor, match.inicio, match.fin) }))
        .filter(match => match.range);
      sugerenciasCorrector = rangos;
      if (window.CSS?.highlights && window.Highlight) {
        CSS.highlights.set("corrector-errors", new Highlight(...rangos.map(match => match.range)));
      }
    }

    function detenerCorrector() {
      window.clearTimeout(temporizadorCorrector);
      window.clearTimeout(temporizadorOcultarSugerencia);
      if (solicitudCorrector) {
        solicitudCorrector.abort();
        solicitudCorrector = null;
      }
      if (window.CSS?.highlights) CSS.highlights.delete("corrector-errors");
      sugerenciasCorrector = [];
      const tooltip = document.getElementById("corrector-globito");
      if (tooltip) tooltip.hidden = true;
      sugerenciaCorrectorActual = null;
    }

    function programarRevisionCorrector(editor, paginaId) {
      window.clearTimeout(temporizadorCorrector);
      if (solicitudCorrector) {
        solicitudCorrector.abort();
        solicitudCorrector = null;
      }
      const status = document.getElementById("corrector-estado");
      if (status) {
        status.hidden = true;
        status.textContent = "";
      }
      if (!correctorLanguageToolActivo) return;
      temporizadorCorrector = window.setTimeout(() => revisarConLanguageTool(editor, paginaId), 800);
    }

    function desenvolverErroresCorrector(editor) {
      if (window.CSS?.highlights) CSS.highlights.delete("corrector-errors");
      sugerenciasCorrector = [];
      const tooltip = document.getElementById("corrector-globito");
      if (tooltip) tooltip.hidden = true;
      sugerenciaCorrectorActual = null;
    }

    async function revisarConLanguageTool(editor, paginaId) {
      const text = textoPlanoEditor(editor);
      const status = document.getElementById("corrector-estado");
      if (!text.trim() || !status) return;
      const controller = new AbortController();
      solicitudCorrector = controller;
      status.textContent = "Revisando ortografía…";
      try {
        const response = await fetch("https://api.languagetool.org/v2/check", {
          method: "POST",
          headers: { "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8" },
          body: new URLSearchParams({ text, language: "es" }),
          signal: controller.signal
        });
        if (!response.ok) throw new Error(`LanguageTool respondió con estado ${response.status}.`);
        const result = await response.json();
        if (vistaActual !== "pagina" || paginaActivaId !== paginaId || document.getElementById("pagina-cuerpo") !== editor) return;
        const matches = Array.isArray(result.matches) ? result.matches : [];
        aplicarMarcasCorrector(editor, matches);
        status.hidden = matches.length === 0;
        status.textContent = matches.length ? "LanguageTool encontró posibles errores." : "";
      } catch (error) {
        if (error.name === "AbortError") return;
        console.error("No se pudo revisar el texto con LanguageTool:", error);
        if (document.getElementById("corrector-estado") === status) {
          status.textContent = "LanguageTool no está disponible; puedes seguir escribiendo con el corrector del navegador.";
          status.hidden = false;
        }
      } finally {
        if (solicitudCorrector === controller) solicitudCorrector = null;
      }
    }

    function mostrarSugerenciaCorrector(mark) {
      const value = mark.sugerencia;
      const tooltip = document.getElementById("corrector-globito");
      if (!value || !tooltip) return;
      window.clearTimeout(temporizadorOcultarSugerencia);
      const button = tooltip.querySelector("button");
      button.textContent = `Cambiar por «${value}»`;
      sugerenciaCorrectorActual = mark;
      tooltip.hidden = false;
      tooltip.style.visibility = "hidden";
      const rect = mark.range.getBoundingClientRect();
      const left = Math.max(12, Math.min(window.innerWidth - tooltip.offsetWidth - 12, rect.left + rect.width / 2 - tooltip.offsetWidth / 2));
      tooltip.style.left = `${left}px`;
      tooltip.style.top = `${Math.max(8, rect.top - tooltip.offsetHeight - 8)}px`;
      tooltip.style.visibility = "visible";
    }

    function programarOcultarSugerencia() {
      temporizadorOcultarSugerencia = window.setTimeout(() => {
        const tooltip = document.getElementById("corrector-globito");
        if (tooltip) tooltip.hidden = true;
      }, 180);
    }

    function aplicarSugerenciaCorrector(editor) {
      const button = document.querySelector("#corrector-globito .corrector-sugerencia");
      const tooltip = document.getElementById("corrector-globito");
      const mark = sugerenciaCorrectorActual;
      if (!button || !tooltip || !mark) return;
      const replacement = mark.sugerencia;
      const range = mark.range;
      range.deleteContents();
      const text = document.createTextNode(replacement);
      range.insertNode(text);
      range.setStartAfter(text);
      range.collapse(true);
      const selection = window.getSelection();
      selection.removeAllRanges();
      selection.addRange(range);
      tooltip.hidden = true;
      sugerenciaCorrectorActual = null;
      editor.dispatchEvent(new InputEvent("input", { bubbles: true, inputType: "insertReplacementText", data: replacement }));
    }

    function renderEnlaces(carpeta) {
      const list = document.getElementById("vista-enlaces");
      list.innerHTML = carpeta.enlaces.map(enlace => `<li class="paginas-etiqueta-enlace"><a href="${escapeHtml(enlace.url)}" target="_blank" rel="noopener noreferrer">${escapeHtml(enlace.titulo)}</a><button class="paginas-quitar-enlace" type="button" data-borrar-enlace="${escapeHtml(enlace.id)}" aria-label="Eliminar enlace ${escapeHtml(enlace.titulo)}">×</button></li>`).join("");
      list.onclick = event => {
        const button = event.target.closest("[data-borrar-enlace]");
        if (!button) return;
        const enlacesAnteriores = carpeta.enlaces;
        carpeta.enlaces = carpeta.enlaces.filter(enlace => enlace.id !== button.dataset.borrarEnlace);
        if (!guardarCarpetas()) carpeta.enlaces = enlacesAnteriores;
        renderEnlaces(carpeta);
      };
    }

    function abrirConfiguracion() {
      if (vistaActual === "pagina") guardarPaginaAbierta();
      elementoGaleriaActivoId = null;
      cerrarVistaPdfEditor();
      detenerCorrector();
      vistaActual = "configuracion";
      carpetaActualId = null;
      materiaActivaId = null;
      paginaActivaId = null;
      document.getElementById("search-wrap").hidden = true;
      actualizarNavegacion();
      renderSidebar();
      const activeSection = seccionActiva();
      const addLabel = activeSection?.tipo === "juegos" ? "juego" : activeSection?.tipo === "notebooklm" ? "cuaderno" : activeSection?.id === "mis-carpetas" ? "carpeta" : "elemento";
      view.innerHTML = `<section class="config-contenido"><h1 class="config-titulo">Configuración</h1><p class="config-descripcion">Administra tus secciones, carpetas y portadas.</p><div class="corrector-opcion"><label><input id="corrector-preferencia" type="checkbox" ${correctorLanguageToolActivo ? "checked" : ""}><span>Activar corrección con LanguageTool</span></label><p class="corrector-aviso-config">Si la activas, el texto de las páginas se enviará a un servidor externo (LanguageTool) para analizar la ortografía. Por defecto está desactivada; puedes seguir usando el corrector del navegador.</p></div><section class="secciones-ajustes"><h2>Secciones</h2><p>Renombra, elimina y cambia el orden de las secciones del menú lateral.</p><div class="secciones-ajustes-lista" id="secciones-ajustes-lista"></div><button class="config-agregar" id="config-agregar-seccion" type="button">+ Nueva sección</button></section><section class="orden-seccion" aria-labelledby="orden-titulo"><div class="orden-encabezado"><div><h2 class="orden-titulo" id="orden-titulo">Orden de las carpetas y elementos</h2><p class="orden-ayuda">Arrastra desde ⋮⋮ o usa las flechas. El orden también cambia en las grillas.</p></div><div class="orden-acciones"><button class="orden-accion" id="orden-alfabetico" type="button">Ordenar A-Z</button><button class="orden-accion" id="orden-deshacer" type="button" ${ultimoOrdenAnterior ? "" : "disabled"}>Deshacer último cambio</button></div></div><div class="orden-lista" id="orden-lista"></div><p class="orden-aviso" id="orden-aviso" role="status"></p><span class="orden-anuncio" id="orden-anuncio" aria-live="polite"></span></section><div class="config-lista" id="config-lista"></div><button class="config-agregar" type="button" id="config-agregar">+ Agregar ${escapeHtml(addLabel)}</button></section>`;
      renderSeccionesConfiguracion();
      renderListaConfiguracion();
      document.getElementById("corrector-preferencia").addEventListener("change", event => {
        if (!guardarPreferenciaCorrector(event.target.checked)) event.target.checked = !event.target.checked;
      });
      document.getElementById("orden-alfabetico").addEventListener("click", ordenarCarpetasAlfabeticamente);
      document.getElementById("orden-deshacer").addEventListener("click", deshacerOrdenCarpetas);
      document.getElementById("config-agregar-seccion").addEventListener("click", () => crearSeccion(false));
      document.getElementById("config-agregar").addEventListener("click", () => {
        crearElementoSeccion(seccionActivaId, "", false);
        renderListaConfiguracion();
        renderSidebar();
      });
    }

    function capturarPosicionesOrden(contenedor) {
      return new Map(Array.from(contenedor.querySelectorAll(".orden-bloque"), bloque => [
        bloque.dataset.ordenId,
        bloque.getBoundingClientRect()
      ]));
    }

    function animarPosicionesOrden(contenedor, posiciones) {
      for (const bloque of contenedor.querySelectorAll(".orden-bloque")) {
        const anterior = posiciones.get(bloque.dataset.ordenId);
        if (!anterior) continue;
        const actual = bloque.getBoundingClientRect();
        const x = anterior.left - actual.left;
        const y = anterior.top - actual.top;
        if (!x && !y) continue;
        bloque.style.transition = "none";
        bloque.style.transform = `translate(${x}px, ${y}px)`;
        requestAnimationFrame(() => {
          bloque.style.transition = "";
          bloque.style.transform = "";
        });
      }
    }

    function anunciarOrdenCarpeta(carpeta) {
      const posicion = carpetas.findIndex(item => item.id === carpeta.id) + 1;
      const anuncio = document.getElementById("orden-anuncio");
      if (anuncio) anuncio.textContent = `${carpeta.titulo} movida a la posición ${posicion} de ${carpetas.length}.`;
    }

    function guardarNuevoOrdenCarpetas(ids, anuncioId = null) {
      const ordenAnterior = [...carpetas];
      const porId = new Map(carpetas.map(carpeta => [carpeta.id, carpeta]));
      if (ids.length !== carpetas.length || ids.some(id => !porId.has(id))) return false;
      if (ids.every((id, index) => id === carpetas[index].id)) return true;
      carpetas.splice(0, carpetas.length, ...ids.map(id => porId.get(id)));
      if (!guardarCarpetas()) {
        carpetas.splice(0, carpetas.length, ...ordenAnterior);
        return false;
      }
      ultimoOrdenAnterior = ordenAnterior.map(carpeta => carpeta.id);
      actualizarOrdenReflejado();
      if (anuncioId) {
        const movida = carpetas.find(carpeta => carpeta.id === anuncioId);
        if (movida) anunciarOrdenCarpeta(movida);
      }
      return true;
    }

    function actualizarOrdenReflejado() {
      renderSidebar();
      if (document.getElementById("carpetas-grilla")) renderCarpetas();
      if (document.getElementById("config-lista")) renderListaConfiguracion();
      else renderOrdenCarpetas();
    }

    function moverCarpetaA(id, indiceDestino) {
      const desde = carpetas.findIndex(carpeta => carpeta.id === id);
      if (desde < 0 || carpetas.length < 2) return;
      const destino = Math.max(0, Math.min(carpetas.length - 1, indiceDestino));
      if (desde === destino) return;
      const orden = carpetas.map(carpeta => carpeta.id);
      const [idMovido] = orden.splice(desde, 1);
      orden.splice(destino, 0, idMovido);
      if (guardarNuevoOrdenCarpetas(orden, idMovido)) {
        const bloque = Array.from(document.querySelectorAll("[data-orden-id]"))
          .find(item => item.dataset.ordenId === idMovido);
        bloque?.focus();
      }
    }

    function ordenarCarpetasAlfabeticamente() {
      if (carpetas.length < 2) return;
      if (!window.confirm("¿Ordenar todas las carpetas alfabéticamente por título?")) return;
      const orden = [...carpetas]
        .sort((a, b) => a.titulo.localeCompare(b.titulo, "es", { sensitivity: "base" }))
        .map(carpeta => carpeta.id);
      guardarNuevoOrdenCarpetas(orden);
    }

    function deshacerOrdenCarpetas() {
      if (!ultimoOrdenAnterior) return;
      const idsActuales = new Set(carpetas.map(carpeta => carpeta.id));
      const orden = ultimoOrdenAnterior.filter(id => idsActuales.has(id));
      carpetas.forEach(carpeta => {
        if (!orden.includes(carpeta.id)) orden.push(carpeta.id);
      });
      ultimoOrdenAnterior = null;
      if (!guardarNuevoOrdenCarpetas(orden)) {
        ultimoOrdenAnterior = orden;
        return;
      }
      ultimoOrdenAnterior = null;
      actualizarOrdenReflejado();
    }

    function renderOrdenCarpetas() {
      const section = document.querySelector(".orden-seccion");
      const list = document.getElementById("orden-lista");
      if (!section || !list) return;
      const undo = document.getElementById("orden-deshacer");
      if (undo) undo.disabled = !ultimoOrdenAnterior;
      if (carpetas.length < 2) {
        list.innerHTML = `<p class="orden-vacio">${carpetas.length ? "Agrega otra carpeta para poder cambiar el orden." : "Todavía no hay carpetas para ordenar."}</p>`;
        section.querySelector("#orden-alfabetico").disabled = true;
        if (undo) undo.disabled = true;
        return;
      }
      section.querySelector("#orden-alfabetico").disabled = false;
      list.innerHTML = carpetas.map((carpeta, index) => {
        const id = escapeHtml(carpeta.id);
        const titulo = escapeHtml(carpeta.titulo || "Sin título");
        const miniatura = carpeta.portada
          ? `<img src="${escapeHtml(carpeta.portada)}" alt="">`
          : `<span aria-hidden="true">${escapeHtml((carpeta.titulo || "·").slice(0, 1).toLocaleUpperCase("es"))}</span>`;
        return `<article class="orden-bloque" data-orden-id="${id}" tabindex="0" aria-label="${titulo}, posición ${index + 1} de ${carpetas.length}">
          <button class="orden-mango" type="button" data-orden-arrastre="${id}" aria-label="Arrastrar ${titulo}" title="Arrastrar para reordenar">⋮⋮</button>
          <span class="orden-miniatura" style="background-color:${escapeHtml(carpeta.color)}">${miniatura}</span>
          <span class="orden-nombre">${titulo}</span>
          <span class="orden-movimientos">
            <button class="orden-mover" type="button" data-orden-mover="${id}" data-direccion="-1" aria-label="Subir ${titulo}" title="Subir" ${index === 0 ? "disabled" : ""}>↑</button>
            <button class="orden-mover" type="button" data-orden-mover="${id}" data-direccion="1" aria-label="Bajar ${titulo}" title="Bajar" ${index === carpetas.length - 1 ? "disabled" : ""}>↓</button>
            <button class="orden-mover" type="button" data-orden-extremo="${id}" data-extremo="inicio" aria-label="Mover ${titulo} al principio" title="Al principio">⇈</button>
            <button class="orden-mover" type="button" data-orden-extremo="${id}" data-extremo="final" aria-label="Mover ${titulo} al final" title="Al final">⇊</button>
          </span>
        </article>`;
      }).join("");

      list.onclick = event => {
        const mover = event.target.closest("[data-orden-mover]");
        if (mover) {
          const index = carpetas.findIndex(carpeta => carpeta.id === mover.dataset.ordenMover);
          moverCarpetaA(mover.dataset.ordenMover, index + Number(mover.dataset.direccion));
          return;
        }
        const extremo = event.target.closest("[data-orden-extremo]");
        if (extremo) {
          const destino = extremo.dataset.extremo === "inicio" ? 0 : carpetas.length - 1;
          moverCarpetaA(extremo.dataset.ordenExtremo, destino);
        }
      };
      list.onkeydown = event => {
        if (!event.altKey || !["ArrowUp", "ArrowDown"].includes(event.key)) return;
        const bloque = event.target.closest("[data-orden-id]");
        if (!bloque) return;
        event.preventDefault();
        const indice = carpetas.findIndex(carpeta => carpeta.id === bloque.dataset.ordenId);
        moverCarpetaA(bloque.dataset.ordenId, indice + (event.key === "ArrowUp" ? -1 : 1));
      };
      list.onpointerdown = event => {
        const mango = event.target.closest("[data-orden-arrastre]");
        if (!mango || !event.isPrimary || event.button !== 0) return;
        const bloque = mango.closest("[data-orden-id]");
        if (!bloque) return;
        event.preventDefault();
        iniciarArrastreOrden(event, list, bloque, mango);
      };
    }

    function iniciarArrastreOrden(eventInicial, list, bloque, mango) {
      if (estadoArrastreOrden) return;
      const bounds = bloque.getBoundingClientRect();
      const placeholder = document.createElement("div");
      placeholder.className = "orden-placeholder";
      placeholder.setAttribute("aria-hidden", "true");
      const posiciones = capturarPosicionesOrden(list);
      bloque.replaceWith(placeholder);
      animarPosicionesOrden(list, posiciones);
      bloque.classList.add("orden-arrastrado");
      Object.assign(bloque.style, {
        position: "fixed",
        left: `${bounds.left}px`,
        top: `${bounds.top}px`,
        width: `${bounds.width}px`,
        height: `${bounds.height}px`,
        margin: "0",
        pointerEvents: "none",
        transform: "translate3d(0, 0, 0)"
      });
      document.body.append(bloque);
      const offsetX = eventInicial.clientX - bounds.left;
      const offsetY = eventInicial.clientY - bounds.top;
      const pointerId = eventInicial.pointerId;
      const mover = event => {
        if (!estadoArrastreOrden || event.pointerId !== pointerId) return;
        event.preventDefault();
        bloque.style.transform = `translate3d(${event.clientX - offsetX - bounds.left}px, ${event.clientY - offsetY - bounds.top}px, 0)`;
        const candidatos = Array.from(list.querySelectorAll("[data-orden-id]"));
        if (!candidatos.length) return;
        const objetivo = candidatos.reduce((cercano, actual) => {
          const a = actual.getBoundingClientRect();
          const c = cercano.getBoundingClientRect();
          const distanciaActual = Math.hypot(event.clientX - (a.left + a.width / 2), event.clientY - (a.top + a.height / 2));
          const distanciaCercano = Math.hypot(event.clientX - (c.left + c.width / 2), event.clientY - (c.top + c.height / 2));
          return distanciaActual < distanciaCercano ? actual : cercano;
        });
        const objetivoRect = objetivo.getBoundingClientRect();
        const mismoRenglon = Math.abs((objetivoRect.top + objetivoRect.height / 2) - (event.clientY)) < objetivoRect.height * 0.55;
        const despues = mismoRenglon
          ? event.clientX > objetivoRect.left + objetivoRect.width / 2
          : event.clientY > objetivoRect.top + objetivoRect.height / 2;
        const posicionesAntes = capturarPosicionesOrden(list);
        if (despues) objetivo.after(placeholder);
        else objetivo.before(placeholder);
        animarPosicionesOrden(list, posicionesAntes);
      };
      const terminar = event => {
        if (!estadoArrastreOrden || event.pointerId !== pointerId) return;
        document.removeEventListener("pointermove", mover);
        document.removeEventListener("pointerup", terminar);
        document.removeEventListener("pointercancel", cancelar);
        const indiceDestino = Array.from(list.children)
          .slice(0, Array.from(list.children).indexOf(placeholder))
          .filter(child => child.matches("[data-orden-id]")).length;
        const ids = carpetas.map(carpeta => carpeta.id);
        const idMovido = bloque.dataset.ordenId;
        const desde = ids.indexOf(idMovido);
        if (desde >= 0) {
          ids.splice(desde, 1);
          ids.splice(Math.min(indiceDestino, ids.length), 0, idMovido);
        }
        bloque.remove();
        placeholder.remove();
        estadoArrastreOrden = null;
        const posiciones = capturarPosicionesOrden(list);
        const cambioReal = desde >= 0 && ids.some((id, index) => id !== carpetas[index]?.id);
        if (cambioReal && guardarNuevoOrdenCarpetas(ids, idMovido)) {
          const nuevaLista = document.getElementById("orden-lista");
          if (nuevaLista) animarPosicionesOrden(nuevaLista, posiciones);
        } else {
          renderOrdenCarpetas();
        }
      };
      const cancelar = () => {
        document.removeEventListener("pointermove", mover);
        document.removeEventListener("pointerup", terminar);
        document.removeEventListener("pointercancel", cancelar);
        bloque.remove();
        placeholder.remove();
        estadoArrastreOrden = null;
        renderOrdenCarpetas();
      };
      estadoArrastreOrden = { pointerId, bloque, placeholder };
      document.addEventListener("pointermove", mover, { passive: false });
      document.addEventListener("pointerup", terminar);
      document.addEventListener("pointercancel", cancelar);
      mango.setPointerCapture?.(pointerId);
    }

    function renderListaConfiguracion() {
      const list = document.getElementById("config-lista");
      list.innerHTML = carpetas.length ? carpetas.map(carpeta => {
        const colorValido = typeof carpeta.color === "string" && /^#[0-9a-f]{6}$/i.test(carpeta.color);
        const color = colorValido ? carpeta.color : COLORES_CARPETA[0];
        return `<article class="config-tarjeta" data-config-id="${escapeHtml(carpeta.id)}">
          <div class="config-campos">
            <label class="config-campo"><span>Título</span><input type="text" data-campo="titulo" value="${escapeHtml(carpeta.titulo)}" maxlength="100"></label>
            <label class="config-campo"><span>Texto al pasar el mouse</span><input type="text" data-campo="textoHover" value="${escapeHtml(carpeta.textoHover)}" maxlength="120"></label>
              ${secciones.find(section => section.id === carpeta.seccionId)?.tipo === "notebooklm" ? `<label class="config-campo"><span>Enlace de NotebookLM</span><input type="url" data-campo="enlaceExterno" value="${escapeHtml(carpeta.enlaceExterno || "")}" placeholder="https://notebooklm.google.com/..." maxlength="2000"></label>` : ""}
            <label class="config-campo"><span>Imagen de portada</span><input type="file" data-portada accept="image/*">${carpeta.portada ? '<button class="config-eliminar" type="button" data-quitar-portada>Quitar imagen y usar color</button>' : ""}</label>
            <label class="config-campo config-color-campo"><span>Color de fondo si no hay imagen</span><input type="color" data-color value="${escapeHtml(color)}"></label>
            <div class="config-color-muestras" role="group" aria-label="Colores sugeridos para ${escapeHtml(carpeta.titulo || "la carpeta")}">
              ${COLORES_CARPETA.map(muestra => `<button class="config-color-muestra" type="button" data-color-muestra="${muestra}" style="--muestra-color:${muestra}" aria-label="Usar color ${muestra}" aria-pressed="${muestra.toLowerCase() === color.toLowerCase()}"></button>`).join("")}
            </div>
            <button class="config-eliminar" type="button" data-eliminar-carpeta>Eliminar carpeta</button>
          </div>
          <div class="config-vista-previa" style="background-color:${escapeHtml(color)}">${carpeta.portada ? `<img src="${escapeHtml(carpeta.portada)}" alt="Vista previa de ${escapeHtml(carpeta.titulo)}">` : `<span>${escapeHtml(carpeta.titulo || "Vista previa")}</span>`}</div>
        </article>`;
      }).join("") : '<p class="config-vacio">No hay carpetas. Agrega una para empezar.</p>';
      list.oninput = event => {
        const field = event.target.closest("[data-campo]");
        const color = event.target.closest("[data-color]");
        const card = event.target.closest("[data-config-id]");
        if (!card) return;
        const carpeta = carpetas.find(item => item.id === card.dataset.configId);
        if (!carpeta) return;
        if (field) carpeta[field.dataset.campo] = field.value;
        if (color) carpeta.color = color.value;
        if (field || color) {
          guardarCarpetas();
          const preview = card.querySelector(".config-vista-previa");
          preview.style.backgroundColor = carpeta.color;
          card.querySelectorAll("[data-color-muestra]").forEach(muestra => {
            muestra.setAttribute("aria-pressed", String(muestra.dataset.colorMuestra.toLowerCase() === carpeta.color.toLowerCase()));
          });
          const title = card.querySelector('[data-campo="titulo"]');
          if (!carpeta.portada) preview.innerHTML = `<span>${escapeHtml(title.value || "Vista previa")}</span>`;
          renderSidebar();
          actualizarNavegacion();
          renderOrdenCarpetas();
        }
      };
      list.onchange = async event => {
        const input = event.target.closest("[data-portada]");
        if (!input || !input.files || !input.files[0]) return;
        const file = input.files[0];
        const card = input.closest("[data-config-id]");
        const carpeta = carpetas.find(item => item.id === card.dataset.configId);
        if (!file.type.startsWith("image/")) { window.alert("Elige un archivo de imagen."); input.value = ""; return; }
        if (file.size > 15 * 1024 * 1024) { window.alert("La imagen debe ocupar menos de 15 MB."); input.value = ""; return; }
        try {
          const bitmap = await createImageBitmap(file);
          const scale = Math.min(1, 1200 / bitmap.width, 1200 / bitmap.height);
          const canvas = document.createElement("canvas");
          canvas.width = Math.max(1, Math.round(bitmap.width * scale));
          canvas.height = Math.max(1, Math.round(bitmap.height * scale));
          const context = canvas.getContext("2d");
          if (!context) throw new Error("No se pudo preparar la imagen.");
          context.drawImage(bitmap, 0, 0, canvas.width, canvas.height);
          bitmap.close();
          const previous = carpeta.portada;
          carpeta.portada = canvas.toDataURL("image/jpeg", 0.78);
          if (carpeta.portada.length > 1400000) { carpeta.portada = previous; window.alert("La imagen sigue siendo demasiado grande; intenta con otra."); return; }
          if (!guardarCarpetas()) { carpeta.portada = previous; return; }
          renderListaConfiguracion();
          renderSidebar();
        } catch (error) {
          console.error("No se pudo procesar la imagen:", error);
          window.alert("No se pudo abrir la imagen. Prueba con otro archivo.");
        } finally { input.value = ""; }
      };
      list.onclick = event => {
        const muestra = event.target.closest("[data-color-muestra]");
        if (muestra) {
          const card = muestra.closest("[data-config-id]");
          const carpeta = carpetas.find(item => item.id === card?.dataset.configId);
          if (!card || !carpeta) return;
          const colorAnterior = carpeta.color;
          const color = muestra.dataset.colorMuestra;
          carpeta.color = color;
          if (!guardarCarpetas()) {
            carpeta.color = colorAnterior;
            return;
          }
          card.querySelector('[data-color]').value = color;
          card.querySelector(".config-vista-previa").style.backgroundColor = color;
          card.querySelectorAll("[data-color-muestra]").forEach(boton => {
            boton.setAttribute("aria-pressed", String(boton.dataset.colorMuestra === color));
          });
          renderSidebar();
          renderOrdenCarpetas();
          return;
        }
        const removeCover = event.target.closest("[data-quitar-portada]");
        if (removeCover) {
          const card = removeCover.closest("[data-config-id]");
          const carpeta = carpetas.find(item => item.id === card.dataset.configId);
          if (!carpeta) return;
          carpeta.portada = "";
          guardarCarpetas();
          renderListaConfiguracion();
          return;
        }
        const button = event.target.closest("[data-eliminar-carpeta]");
        if (!button) return;
        const card = button.closest("[data-config-id]");
        const index = carpetas.findIndex(item => item.id === card.dataset.configId);
        if (index < 0 || !window.confirm(`¿Eliminar la carpeta «${carpetas[index].titulo}» y sus notas?`)) return;
        carpetas.splice(index, 1);
        guardarCarpetas();
        renderListaConfiguracion();
        renderSidebar();
      };
      renderOrdenCarpetas();
    }

    function pedirDatosDialogo(titulo, campos) {
      const dialog = document.getElementById("dialog-interaccion");
      const form = document.getElementById("dialog-interaccion-form");
      form.innerHTML = `<h2 id="dialog-interaccion-titulo">${escapeHtml(titulo)}</h2>${campos.map((field, index) => `<label><span>${escapeHtml(field.etiqueta)}</span><input id="dialog-interaccion-campo-${index}" name="${escapeHtml(field.nombre)}" type="${field.tipo || "text"}" value="${escapeHtml(field.valor || "")}" placeholder="${escapeHtml(field.placeholder || "")}" ${field.requerido === false ? "" : "required"} maxlength="${field.maxlength || 2000}"></label>`).join("")}<div class="editor-dialog-acciones"><button type="button" data-dialog-cancelar>Cancelar</button><button type="submit">Crear</button></div>`;
      return new Promise(resolve => {
        let finalizado = false;
        const cerrar = value => {
          if (finalizado) return;
          finalizado = true;
          dialog.hidden = true;
          form.onsubmit = null;
          form.onkeydown = null;
          dialog.onclick = null;
          resolve(value);
        };
        form.onsubmit = event => {
          event.preventDefault();
          cerrar(Object.fromEntries(new FormData(form).entries()));
        };
        form.onclick = event => {
          if (event.target.closest("[data-dialog-cancelar]")) cerrar(null);
        };
        form.onkeydown = event => {
          if (event.key === "Escape") {
            event.preventDefault();
            cerrar(null);
          }
        };
        dialog.onclick = event => {
          if (event.target === dialog) cerrar(null);
        };
        dialog.hidden = false;
        form.querySelector("input")?.focus();
      });
    }

    function crearCarpeta() {
      crearElementoSeccion(seccionActivaId);
    }

    async function crearMateria(carpetaId) {
      const carpeta = carpetas.find(item => item.id === carpetaId);
      if (!esCarpetaDeEstudio(carpeta)) return;
      const datos = await pedirDatosDialogo("Crear materia", [
        { nombre: "titulo", etiqueta: "Nombre de la materia", placeholder: "Por ejemplo, Biología", maxlength: 100 }
      ]);
      const titulo = String(datos?.titulo || "").trim();
      if (!titulo) return;
      const materia = {
        id: makeId(),
        titulo,
        portada: "",
        color: COLORES_CARPETA[materiasDeCarpeta(carpeta).length % COLORES_CARPETA.length],
        paginas: []
      };
      carpeta.materias.push(materia);
      if (!guardarCarpetas()) {
        carpeta.materias.pop();
        return;
      }
      abrirCarpeta(carpeta.id);
    }

    async function crearElementoSeccion(sectionId, tituloInicial = "", abrirElemento = true, nombrePersonalizado = "") {
      const section = secciones.find(item => item.id === sectionId);
      if (!section) return;
      const tipoNombre = nombrePersonalizado || (section.tipo === "juegos" ? "juego" : section.tipo === "notebooklm" ? "cuaderno" : section.id === "mis-carpetas" ? "carpeta" : "elemento");
      const etiquetaNombre = section.id === "mis-carpetas"
        ? (nombrePersonalizado === "materia" ? "Nombre de la materia" : "Nombre de la carpeta")
        : "Título";
      const datos = await pedirDatosDialogo(`Crear ${tipoNombre}`, [
        { nombre: "titulo", etiqueta: etiquetaNombre, valor: tituloInicial, placeholder: `Nombre del ${tipoNombre}`, maxlength: 100 },
        ...(section.tipo === "notebooklm" ? [{ nombre: "url", etiqueta: "Enlace de NotebookLM", tipo: "url", placeholder: "https://notebooklm.google.com/..." }] : [])
      ]);
      if (!datos) return;
      const titulo = String(datos.titulo || "").trim();
      if (!titulo) return;
      let enlaceExterno = "";
      if (section.tipo === "notebooklm") {
        try {
          const url = new URL(datos.url);
          if (!["https:", "http:"].includes(url.protocol)) throw new Error("El enlace debe usar HTTP o HTTPS.");
          enlaceExterno = url.href;
        } catch (error) {
          console.error("El enlace del cuaderno no es válido:", error);
          window.alert("Escribe un enlace válido que empiece con http:// o https://.");
          return;
        }
      }
      const carpeta = {
        id: makeId(),
        titulo: titulo.trim(),
        seccionId: section.id,
        enlaceExterno,
        portada: "",
        textoHover: section.tipo === "juegos" ? "Abrir juego" : section.tipo === "notebooklm" ? "Abrir cuaderno" : section.id === "mis-carpetas" ? "Abrir carpeta" : "Abrir elemento",
        color: COLORES_CARPETA[carpetas.filter(item => item.seccionId === section.id).length % COLORES_CARPETA.length],
        materias: [],
        paginas: [],
        enlaces: []
      };
      carpetas.push(carpeta);
      if (!guardarCarpetas()) {
        carpetas.pop();
        return;
      }
      renderSidebar();
      if (!abrirElemento) {
        if (document.getElementById("config-lista")) renderListaConfiguracion();
        return;
      }
      paginaActual = 1;
      if (section.tipo === "notebooklm") renderHome(section.id);
      else if (esCarpetaDeEstudio(carpeta)) abrirCarpeta(carpeta.id);
      else abrirElementoGaleria(carpeta.id);
    }

    async function crearSeccion(abrirSeccion = true) {
      const datos = await pedirDatosDialogo("Crear sección", [
        { nombre: "titulo", etiqueta: "Nombre de la sección", placeholder: "Por ejemplo, Mis películas", maxlength: 100 }
      ]);
      if (!datos) return;
      const titulo = String(datos.titulo || "").trim();
      if (!titulo) return;
      const seccion = { id: makeId(), titulo, tipo: "carpetas" };
      secciones.push(seccion);
      if (!guardarSecciones()) {
        secciones.pop();
        return;
      }
      renderSidebar();
      if (abrirSeccion) {
        seccionActivaId = seccion.id;
        renderHome(seccion.id);
      }
      else renderSeccionesConfiguracion();
    }

    function moverSeccion(id, direccion) {
      const index = secciones.findIndex(item => item.id === id);
      moverSeccionA(id, index + direccion);
    }

    function moverSeccionA(id, destinoSolicitado) {
      const index = secciones.findIndex(item => item.id === id);
      const destino = Math.max(0, Math.min(secciones.length - 1, destinoSolicitado));
      if (index < 0 || destino === index) return;
      const [seccion] = secciones.splice(index, 1);
      secciones.splice(destino, 0, seccion);
      if (!guardarSecciones()) {
        secciones.splice(destino, 1);
        secciones.splice(index, 0, seccion);
        return;
      }
      renderSidebar();
      renderSeccionesConfiguracion();
      if (vistaActual === "inicio") renderHome(inicioDashboard ? null : seccionActivaId);
    }

    function renderSeccionesConfiguracion() {
      const list = document.getElementById("secciones-ajustes-lista");
      if (!list) return;
      list.innerHTML = secciones.map((section, index) => `<div class="secciones-ajustes-item" data-config-seccion="${escapeHtml(section.id)}">
        <button class="secciones-ajustes-handle" type="button" data-seccion-arrastrar aria-label="Arrastrar sección ${escapeHtml(section.titulo)}" title="Arrastra para cambiar el orden">⋮⋮</button>
        <input type="text" data-seccion-titulo value="${escapeHtml(section.titulo)}" maxlength="100" aria-label="Nombre de la sección">
        <button type="button" data-seccion-mover="-1" aria-label="Subir sección ${escapeHtml(section.titulo)}" ${index === 0 ? "disabled" : ""}>↑</button>
        <button type="button" data-seccion-mover="1" aria-label="Bajar sección ${escapeHtml(section.titulo)}" ${index === secciones.length - 1 ? "disabled" : ""}>↓</button>
        <button type="button" data-seccion-eliminar>Eliminar</button>
      </div>`).join("") || '<p class="secciones-ajustes-vacia">Aún no hay secciones.</p>';
      list.onchange = event => {
        const field = event.target.closest("[data-seccion-titulo]");
        if (!field) return;
        const item = field.closest("[data-config-seccion]");
        const section = secciones.find(entry => entry.id === item?.dataset.configSeccion);
        if (!section) return;
        const value = field.value.trim();
        if (!value) {
          field.value = section.titulo;
          window.alert("El nombre de la sección no puede quedar vacío.");
          return;
        }
        const previous = section.titulo;
        section.titulo = value;
        if (!guardarSecciones()) {
          section.titulo = previous;
          field.value = previous;
        }
        renderSidebar();
        actualizarNavegacion();
      };
      list.onclick = event => {
        const item = event.target.closest("[data-config-seccion]");
        const section = secciones.find(entry => entry.id === item?.dataset.configSeccion);
        if (!section) return;
        const move = event.target.closest("[data-seccion-mover]");
        if (move) {
          moverSeccion(section.id, Number(move.dataset.seccionMover));
          return;
        }
        if (!event.target.closest("[data-seccion-eliminar]")) return;
        if (secciones.length <= 1) {
          window.alert("Debe quedar al menos una sección en el menú.");
          return;
        }
        if (!window.confirm(`¿Eliminar la sección «${section.titulo}» y todos sus elementos?`)) return;
        const sectionIndex = secciones.indexOf(section);
        const carpetasAnteriores = [...carpetas];
        for (let index = carpetas.length - 1; index >= 0; index--) {
          if (carpetas[index].seccionId === section.id) carpetas.splice(index, 1);
        }
        secciones.splice(sectionIndex, 1);
        if (!guardarSecciones() || !guardarCarpetas()) {
          carpetas.splice(0, carpetas.length, ...carpetasAnteriores);
          secciones.splice(sectionIndex, 0, section);
          guardarSecciones();
          guardarCarpetas();
          return;
        }
        if (seccionActivaId === section.id) seccionActivaId = secciones[0]?.id || "";
        renderSidebar();
        renderSeccionesConfiguracion();
        renderListaConfiguracion();
        actualizarNavegacion();
      };
      list.onpointerdown = event => {
        const handle = event.target.closest("[data-seccion-arrastrar]");
        if (!handle || !event.isPrimary || event.button !== 0) return;
        const row = handle.closest("[data-config-seccion]");
        if (!row) return;
        iniciarArrastreSeccion(event, list, row);
      };
    }

    function iniciarArrastreSeccion(initialEvent, list, row) {
      initialEvent.preventDefault();
      const bounds = row.getBoundingClientRect();
      const placeholder = document.createElement("div");
      placeholder.className = "secciones-ajustes-placeholder";
      placeholder.setAttribute("aria-hidden", "true");
      placeholder.style.height = `${bounds.height}px`;
      row.before(placeholder);
      row.classList.add("seccion-arrastrada");
      row.style.position = "fixed";
      row.style.zIndex = "25";
      row.style.width = `${bounds.width}px`;
      row.style.left = `${bounds.left}px`;
      row.style.top = `${bounds.top}px`;
      const pointerId = initialEvent.pointerId;
      const mover = event => {
        if (event.pointerId !== pointerId) return;
        event.preventDefault();
        row.style.left = `${event.clientX - bounds.width / 2}px`;
        row.style.top = `${event.clientY - 22}px`;
        const objetivo = document.elementFromPoint(event.clientX, event.clientY)?.closest(".secciones-ajustes-item");
        if (!objetivo || objetivo === row || !list.contains(objetivo)) return;
        const rect = objetivo.getBoundingClientRect();
        if (event.clientY < rect.top + rect.height / 2) objetivo.before(placeholder);
        else objetivo.after(placeholder);
      };
      const terminar = event => {
        if (event.pointerId !== pointerId) return;
        limpiar();
        const destino = Array.from(list.children).slice(0, Array.from(list.children).indexOf(placeholder))
          .filter(child => child !== row && child.matches(".secciones-ajustes-item")).length;
        placeholder.remove();
        row.style.position = "";
        row.style.zIndex = "";
        row.style.width = "";
        row.style.left = "";
        row.style.top = "";
        row.classList.remove("seccion-arrastrada");
        moverSeccionA(row.dataset.configSeccion, destino);
      };
      const cancelar = event => {
        if (event.pointerId !== pointerId) return;
        limpiar();
        placeholder.remove();
        row.style.position = "";
        row.style.zIndex = "";
        row.style.width = "";
        row.style.left = "";
        row.style.top = "";
        row.classList.remove("seccion-arrastrada");
      };
      const limpiar = () => {
        document.removeEventListener("pointermove", mover);
        document.removeEventListener("pointerup", terminar);
        document.removeEventListener("pointercancel", cancelar);
      };
      document.addEventListener("pointermove", mover, { passive: false });
      document.addEventListener("pointerup", terminar);
      document.addEventListener("pointercancel", cancelar);
    }

    function buscarItemInicio(kind, id) {
      return todosLosItemsInicio().find(item => item.kind === kind && item.id === id);
    }

    function abrirItemInicio(item) {
      if (!item) return;
      if (item.kind === "folder") abrirCarpeta(item.folder.id);
      else if (item.kind === "materia") abrirMateria(item.folder.id, item.materia.id);
      else if (item.kind === "page") abrirPagina(item.folder.id, item.page.id, item.materia.id);
      else if (item.kind === "notebook" && item.element.enlaceExterno) {
        try {
          const url = new URL(item.element.enlaceExterno);
          if (!["https:", "http:"].includes(url.protocol)) throw new Error("El enlace no usa HTTP seguro.");
          window.open(url.href, "_blank", "noopener,noreferrer");
        } catch (error) {
          console.error("El enlace guardado del cuaderno no es válido:", error);
          window.alert("El enlace del cuaderno no es válido. Corrígelo desde Configuración.");
        }
      } else if (item.element) abrirElementoGaleria(item.element.id);
    }

    function eliminarSeccionDesdeInicio(section) {
      if (secciones.length <= 1) {
        window.alert("Debe quedar al menos una sección.");
        return;
      }
      const elements = itemsDeSeccionInicio(section.id);
      const count = section.id === "mis-carpetas"
        ? elements.filter(item => item.kind === "page").length
        : elements.filter(item => ["gallery", "notebook"].includes(item.kind)).length;
      if (!window.confirm(`¿Eliminar «${section.titulo}» y sus ${count} ${count === 1 ? "página" : "páginas"}? Esta acción no se puede deshacer.`)) return;
      const sectionIndex = secciones.indexOf(section);
      const anteriores = [...carpetas];
      for (let index = carpetas.length - 1; index >= 0; index--) {
        if (carpetas[index].seccionId === section.id) carpetas.splice(index, 1);
      }
      secciones.splice(sectionIndex, 1);
      if (!guardarSecciones() || !guardarCarpetas()) {
        carpetas.splice(0, carpetas.length, ...anteriores);
        secciones.splice(sectionIndex, 0, section);
        guardarSecciones();
        guardarCarpetas();
        return;
      }
      if (seccionActivaId === section.id) seccionActivaId = secciones[0]?.id || "";
      renderSidebar();
      renderHome(inicioDashboard ? null : seccionActivaId);
    }

    document.addEventListener("dblclick", event => {
      const title = event.target.closest(".pagina-nombre");
      if (!title) return;
      window.clearTimeout(temporizadorAbrirPagina);
      event.preventDefault();
      event.stopPropagation();
      const carpeta = carpetas.find(item => item.id === carpetaActualId);
      const fila = title.closest("[data-pagina-fila]");
      const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaActivaId);
      const pagina = materia?.paginas.find(item => item.id === fila?.dataset.paginaFila);
      if (materia && pagina) renombrarPagina(materia, pagina, title);
    });
    document.addEventListener("click", async event => {
      if (!event.target.closest(".pagina-nombre")) window.clearTimeout(temporizadorAbrirPagina);
      if (event.target.closest(".home-hover-card") && event.target.closest("[data-open-study], [data-open-materia], [data-home-open-page], [data-open-gallery], [data-open-notebook]")) {
        cerrarTarjetaFlotanteInicio();
      }
      if (event.target.closest("[data-home-dashboard]")) { renderHome(null); return; }
      const dashboardMore = event.target.closest("[data-dashboard-more]");
      if (dashboardMore) {
        const menu = view.querySelector(".dashboard-more-menu");
        if (menu) {
          menu.hidden = !menu.hidden;
          dashboardMore.setAttribute("aria-expanded", String(!menu.hidden));
        }
        return;
      }
      if (event.target.closest("[data-dashboard-favorites]")) {
        view.querySelector("#dashboard-favorites")?.scrollIntoView({ behavior: "smooth", block: "start" });
        return;
      }
      const dashboardTarget = event.target.closest("[data-dashboard-target]");
      if (dashboardTarget) {
        const targetId = dashboardTarget.dataset.dashboardTarget;
        const scrollToTarget = () => view.querySelector(`#${CSS.escape(targetId)}`)?.scrollIntoView({ behavior: "smooth", block: "center" });
        if (!inicioDashboard) {
          renderHome(null);
          window.setTimeout(scrollToTarget, 100);
        } else scrollToTarget();
        return;
      }
      if (event.target.closest("[data-dashboard-archive]")) {
        const archive = secciones.find(section => section.tipo === "notebooklm");
        if (archive) renderHome(archive.id);
        else window.alert("Todavía no hay una sección de cuadernos para mostrar.");
        return;
      }
      const dashboardInfo = event.target.closest("[data-dashboard-info]");
      if (dashboardInfo) {
        const isHelp = dashboardInfo.dataset.dashboardInfo === "help";
        view.insertAdjacentHTML("beforeend", `<div class="home-details-backdrop" data-dashboard-modal><section class="home-details" role="dialog" aria-modal="true" aria-labelledby="dashboard-info-title"><h2 id="dashboard-info-title">${isHelp ? "Ayuda para organizarte" : "Actualizaciones"}</h2><p>${isHelp ? "Usa Materias para abrir tus carpetas, marca páginas como favoritas desde el menú de opciones y agrega tareas con casillas dentro de tus apuntes." : "Esta portada reúne tus materias, páginas y tareas guardadas. Tus datos siguen almacenados en este dispositivo."}</p><button type="button" data-dashboard-modal-close>Entendido</button></section></div>`);
        return;
      }
      if (event.target.closest("[data-dashboard-modal-close]") || event.target.matches("[data-dashboard-modal]")) {
        event.target.closest("[data-dashboard-modal]")?.remove();
        return;
      }
      if (event.target.closest("[data-dashboard-open-settings]")) { abrirConfiguracion(); return;       }
      if (event.target.closest("[data-dashboard-new-subject]")) {
        const section = secciones.find(item => item.id === "mis-carpetas");
        if (section) await crearElementoSeccion(section.id, "", true, "materia");
        else window.alert("Crea primero una sección de carpetas para agregar materias.");
        return;
      }
      if (event.target.closest("[data-dashboard-share]")) {
        try {
          if (navigator.share) {
            await navigator.share({ title: "Mi espacio académico", url: location.href });
            saveState.textContent = "Compartido";
            return;
          } else if (navigator.clipboard?.writeText) {
            await navigator.clipboard.writeText(location.href);
          } else {
            const linkField = document.createElement("textarea");
            linkField.value = location.href;
            linkField.setAttribute("readonly", "");
            linkField.style.position = "fixed";
            linkField.style.opacity = "0";
            document.body.append(linkField);
            linkField.select();
            const copied = document.execCommand("copy");
            linkField.remove();
            if (!copied) throw new Error("Este navegador no permite compartir ni copiar el enlace.");
          }
          saveState.textContent = "Enlace copiado";
        } catch (error) {
          if (error.name === "AbortError") return;
          console.error("No se pudo compartir el espacio académico:", error);
          window.alert(error.message || "No se pudo compartir el enlace.");
        }
        return;
      }
      const addSectionToggle = event.target.closest("[data-home-add-section]");
      if (addSectionToggle) {
        const panel = document.getElementById("home-create-popover");
        panel.hidden = !panel.hidden;
        addSectionToggle.setAttribute("aria-expanded", String(!panel.hidden));
        if (!panel.hidden) document.getElementById("home-new-section-name").focus();
        return;
      }
      if (event.target.closest("[data-home-create-section-submit]")) {
        const nameInput = document.getElementById("home-new-section-name");
        const title = nameInput.value.trim();
        if (!title) { nameInput.focus(); return; }
        const section = { id: makeId(), titulo: title, tipo: "carpetas", icon: document.getElementById("home-new-section-icon").value };
        secciones.push(section);
        if (!guardarSecciones()) { secciones.pop(); return; }
        renderSidebar();
        renderHome(section.id);
        return;
      }
      const sectionMenuToggle = event.target.closest("[data-home-section-menu]");
      if (sectionMenuToggle) {
        const menu = document.getElementById(sectionMenuToggle.getAttribute("aria-controls"));
        if (!menu) return;
        document.querySelectorAll(".home-section-popover").forEach(item => { item.hidden = true; });
        document.querySelectorAll("[data-home-section-menu]").forEach(item => item.setAttribute("aria-expanded", "false"));
        menu.hidden = false;
        sectionMenuToggle.setAttribute("aria-expanded", "true");
        return;
      }
      const renameSectionButton = event.target.closest("[data-home-rename-section]");
      if (renameSectionButton) {
        const section = secciones.find(item => item.id === renameSectionButton.dataset.homeRenameSection);
        if (!section) return;
        const title = window.prompt("Nombre de la sección:", section.titulo);
        if (title === null) return;
        if (!title.trim()) { window.alert("El nombre no puede quedar vacío."); return; }
        const previous = section.titulo;
        section.titulo = title.trim();
        if (!guardarSecciones()) section.titulo = previous;
        renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const changeSectionIcon = event.target.closest("[data-home-change-icon]");
      if (changeSectionIcon) {
        const section = secciones.find(item => item.id === changeSectionIcon.dataset.homeChangeIcon);
        if (!section) return;
        const icon = window.prompt("Elige un ícono: 📚 🎮 🎬 📖 💡", section.icon || "📚");
        if (icon === null) return;
        if (!["📚", "🎮", "🎬", "📖", "💡"].includes(icon.trim())) { window.alert("Elige uno de los íconos sugeridos."); return; }
        const previous = section.icon;
        section.icon = icon.trim();
        if (!guardarSecciones()) section.icon = previous;
        renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const deleteSectionButton = event.target.closest("[data-home-delete-section]");
      if (deleteSectionButton) {
        const section = secciones.find(item => item.id === deleteSectionButton.dataset.homeDeleteSection);
        if (section) eliminarSeccionDesdeInicio(section);
        return;
      }
      const createItem = event.target.closest("[data-home-create]");
      if (createItem) {
        const sectionId = createItem.dataset.homeCreateSection || seccionActivaId;
        const folderId = createItem.dataset.homeCreateFolder;
        if (createItem.dataset.homeCreate === "Crear materia" && folderId) crearMateria(folderId);
        else if (createItem.dataset.homeCreate === "Agregar página" && folderId) {
          const folder = carpetas.find(item => item.id === folderId);
          const subject = materiasDeCarpeta(folder).find(item => item.id === createItem.dataset.homeCreateMateria) || materiasDeCarpeta(folder)[0];
          if (subject) abrirMateria(folderId, subject.id);
          else crearMateria(folderId);
        } else crearElementoSeccion(sectionId);
        return;
      }
      const homePageOpen = event.target.closest("[data-home-open-page]");
      if (homePageOpen) {
        cerrarTarjetaFlotanteInicio();
        abrirPagina(homePageOpen.dataset.homeFolder, homePageOpen.dataset.homeOpenPage, homePageOpen.dataset.homeMateria);
        return;
      }
      const openFeature = event.target.closest("[data-home-open-feature]");
      if (openFeature) {
        abrirItemInicio(buscarItemInicio(openFeature.dataset.homeOpenFeature, openFeature.dataset.homeFeatureId));
        return;
      }
      const quickOpen = event.target.closest("[data-home-card-open]");
      if (quickOpen) {
        cerrarTarjetaFlotanteInicio();
        abrirItemInicio(buscarItemInicio(quickOpen.dataset.homeCardOpen, quickOpen.dataset.homeCardId));
        return;
      }
      const quickMore = event.target.closest("[data-home-card-more]");
      if (quickMore) {
        const portal = quickMore.closest(".home-hover-card");
        const toggle = portal?.querySelector(".home-card-menu-toggle") ||
          quickMore.closest(".home-card-wrap")?.querySelector(".home-card-menu-toggle");
        const menu = toggle && document.getElementById(toggle.getAttribute("aria-controls"));
        if (toggle && menu) {
          const abrir = menu.hidden;
          document.querySelectorAll(".paginas-menu-opciones").forEach(item => { item.hidden = true; });
          document.querySelectorAll("[data-pagina-opciones], [data-elemento-opciones], [data-materia-opciones]").forEach(item => item.setAttribute("aria-expanded", "false"));
          menu.hidden = !abrir;
          toggle.setAttribute("aria-expanded", String(abrir));
        }
        return;
      }
      const rowScroll = event.target.closest("[data-home-scroll]");
      if (rowScroll) {
        const track = document.getElementById(rowScroll.dataset.homeTrack);
        if (track) track.scrollBy({ left: Number(rowScroll.dataset.homeScroll) * track.clientWidth * 0.85, behavior: "smooth" });
        return;
      }
      const showDetails = event.target.closest("[data-home-details]");
      if (showDetails) {
        const item = buscarItemInicio(showDetails.dataset.homeDetails, showDetails.dataset.homeFeatureId);
        if (!item) return;
        view.insertAdjacentHTML("beforeend", `<div class="home-details-backdrop" data-home-details-close><section class="home-details" role="dialog" aria-modal="true" aria-labelledby="home-details-title"><h2 id="home-details-title">${escapeHtml(item.title)}</h2><p>${escapeHtml(item.sectionLabel || nombreSeccionInicio(secciones.find(section => section.id === item.sectionId) || { id: "", titulo: "Carpetas" }))}</p>${item.updated ? `<p>Última edición: ${escapeHtml(new Intl.DateTimeFormat("es", { dateStyle: "medium" }).format(item.updated))}</p>` : ""}<button type="button" data-home-details-open="${item.kind}" data-home-feature-id="${escapeHtml(item.id)}">Abrir</button></section></div>`);
        return;
      }
      if (event.target.matches("[data-home-details-close]")) { event.target.remove(); return; }
      const detailsOpen = event.target.closest("[data-home-details-open]");
      if (detailsOpen) {
        const item = buscarItemInicio(detailsOpen.dataset.homeDetailsOpen, detailsOpen.dataset.homeFeatureId);
        view.querySelector(".home-details-backdrop")?.remove();
        abrirItemInicio(item);
        return;
      }
      const favoritePage = event.target.closest("[data-home-favorite-page]");
      const favoriteElement = event.target.closest("[data-home-favorite-element]");
      if (favoritePage || favoriteElement) {
        const id = favoritePage ? favoritePage.dataset.homeFavoritePage : favoriteElement.dataset.homeFavoriteElement;
        const item = favoritePage ? buscarItemInicio("page", id) : buscarItemInicio("gallery", id) || buscarItemInicio("notebook", id);
        if (!item) return;
        const target = item.kind === "page" ? item.page : item.element;
        const previous = target.favorito;
        target.favorito = !target.favorito;
        if (!guardarCarpetas()) target.favorito = previous;
        renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const editHomePage = event.target.closest("[data-home-edit-page]");
      if (editHomePage) { abrirPagina(editHomePage.dataset.homeFolder, editHomePage.dataset.homeEditPage, editHomePage.dataset.homeMateria); return; }
      const renameHomePage = event.target.closest("[data-home-rename-page]");
      if (renameHomePage) {
        const item = buscarItemInicio("page", renameHomePage.dataset.homeRenamePage);
        if (!item) return;
        const title = window.prompt("Título de la tarjeta:", tituloTarjetaPagina(item.page));
        if (title === null) return;
        if (!title.trim()) { window.alert("El título no puede quedar vacío."); return; }
        const oldTitle = item.page.cardTitle;
        const oldLegacyTitle = item.page.titulo;
        item.page.cardTitle = title.trim();
        item.page.titulo = title.trim();
        item.page.actualizado = Date.now();
        if (!guardarCarpetas()) { item.page.cardTitle = oldTitle; item.page.titulo = oldLegacyTitle; }
        renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const coverTrigger = event.target.closest("[data-home-cover-trigger]");
      if (coverTrigger) {
        const selector = `[data-home-page-cover="${CSS.escape(coverTrigger.dataset.homeCoverTrigger)}"]`;
        (document.querySelector(`.home-hover-card ${selector}`) || view.querySelector(selector))?.click();
        return;
      }
      const deleteHomePage = event.target.closest("[data-home-delete-page]");
      if (deleteHomePage) {
        const item = buscarItemInicio("page", deleteHomePage.dataset.homeDeletePage);
        if (!item || !window.confirm(`¿Eliminar la página «${item.title}»?`)) return;
        const pagesBefore = item.materia.paginas;
        item.materia.paginas = pagesBefore.filter(page => page.id !== item.page.id);
        if (!guardarCarpetas()) item.materia.paginas = pagesBefore;
        renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const sectionButton = event.target.closest("[data-seccion-id]");
      if (sectionButton) {
        renderHome(sectionButton.dataset.seccionId);
        return;
      }
      const volverGaleria = event.target.closest("[data-volver-galeria]");
      if (volverGaleria) {
        renderHome(volverGaleria.dataset.volverGaleria);
        return;
      }
      const openGalleryItem = event.target.closest("[data-open-gallery]");
      if (openGalleryItem) {
        abrirElementoGaleria(openGalleryItem.dataset.openGallery);
        return;
      }
      const searchResultPage = event.target.closest("[data-abrir-resultado-pagina]");
      if (searchResultPage) {
        abrirPagina(searchResultPage.dataset.abrirResultadoCarpeta, searchResultPage.dataset.abrirResultadoPagina, searchResultPage.dataset.abrirResultadoMateria);
        return;
      }
      const openMateria = event.target.closest("[data-open-materia]");
      if (openMateria) {
        const folder = openMateria.dataset.materiaFolder || carpetaActualId;
        abrirMateria(folder, openMateria.dataset.openMateria);
        return;
      }
      if (event.target.closest("[data-crear-materia]")) {
        crearMateria(carpetaActualId);
        return;
      }
      const materiaMenuToggle = event.target.closest("[data-materia-opciones]");
      if (materiaMenuToggle) {
        const menu = document.getElementById(materiaMenuToggle.getAttribute("aria-controls"));
        if (!menu) return;
        const abrir = menu.hidden;
        document.querySelectorAll(".paginas-menu-opciones").forEach(item => { item.hidden = true; });
        document.querySelectorAll("[data-materia-opciones]").forEach(item => item.setAttribute("aria-expanded", "false"));
        menu.hidden = !abrir;
        materiaMenuToggle.setAttribute("aria-expanded", String(abrir));
        return;
      }
      const cambiarPortadaMateria = event.target.closest("[data-cambiar-portada-materia]");
      if (cambiarPortadaMateria) {
        const selector = `[data-materia-portada="${CSS.escape(cambiarPortadaMateria.dataset.cambiarPortadaMateria)}"]`;
        (document.querySelector(`.home-hover-card ${selector}`) || view.querySelector(selector))?.click();
        return;
      }
      const renombrarMateria = event.target.closest("[data-renombrar-materia]");
      if (renombrarMateria) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === renombrarMateria.dataset.renombrarMateria);
        if (!materia) return;
        const datos = await pedirDatosDialogo("Cambiar título de la materia", [
          { nombre: "titulo", etiqueta: "Nombre de la materia", valor: materia.titulo, maxlength: 100 }
        ]);
        const titulo = String(datos?.titulo || "").trim();
        if (!titulo || titulo === materia.titulo) return;
        const anterior = materia.titulo;
        materia.titulo = titulo;
        if (!guardarCarpetas()) materia.titulo = anterior;
        abrirCarpeta(carpeta.id);
        return;
      }
      const quitarPortadaMateria = event.target.closest("[data-quitar-portada-materia]");
      if (quitarPortadaMateria) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === quitarPortadaMateria.dataset.quitarPortadaMateria);
        if (!materia || !materia.portada) return;
        const anterior = materia.portada;
        materia.portada = "";
        if (!guardarCarpetas()) materia.portada = anterior;
        abrirCarpeta(carpeta.id);
        return;
      }
      const eliminarMateria = event.target.closest("[data-eliminar-materia]");
      if (eliminarMateria) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        if (!carpeta) return;
        const index = carpeta.materias.findIndex(item => item.id === eliminarMateria.dataset.eliminarMateria);
        if (index < 0) return;
        const materia = carpeta.materias[index];
        if (!window.confirm(`¿Eliminar la materia «${materia.titulo}» y sus páginas?`)) return;
        carpeta.materias.splice(index, 1);
        if (!guardarCarpetas()) carpeta.materias.splice(index, 0, materia);
        else limpiarImagenesHuerfanas().catch(error => console.error("No se pudieron limpiar archivos de la materia eliminada:", error));
        abrirCarpeta(carpeta.id);
        return;
      }
      if (event.target.closest("[data-crear-elemento]")) {
        crearElementoSeccion(seccionActivaId);
        return;
      }
      const elementoMenuToggle = event.target.closest("[data-elemento-opciones]");
      if (elementoMenuToggle) {
        const menu = document.getElementById(elementoMenuToggle.getAttribute("aria-controls"));
        if (!menu) return;
        const abrir = menu.hidden;
        document.querySelectorAll(".paginas-menu-opciones").forEach(item => { item.hidden = true; });
        document.querySelectorAll("[data-pagina-opciones], [data-elemento-opciones]").forEach(item => item.setAttribute("aria-expanded", "false"));
        menu.hidden = !abrir;
        elementoMenuToggle.setAttribute("aria-expanded", String(abrir));
        return;
      }
      const cambiarPortadaElemento = event.target.closest("[data-cambiar-portada-elemento]");
      if (cambiarPortadaElemento) {
        const selector = `[data-elemento-portada="${CSS.escape(cambiarPortadaElemento.dataset.cambiarPortadaElemento)}"]`;
        (document.querySelector(`.home-hover-card ${selector}`) || view.querySelector(selector))?.click();
        return;
      }
      const renombrarElemento = event.target.closest("[data-renombrar-elemento]");
      if (renombrarElemento) {
        const elemento = carpetas.find(item => item.id === renombrarElemento.dataset.renombrarElemento);
        if (!elemento) return;
        const datos = await pedirDatosDialogo("Renombrar elemento", [
          { nombre: "titulo", etiqueta: "Título", valor: elemento.titulo, maxlength: 100 }
        ]);
        const titulo = String(datos?.titulo || "").trim();
        if (!titulo || titulo === elemento.titulo) return;
        const tituloAnterior = elemento.titulo;
        elemento.titulo = titulo;
        if (!guardarCarpetas()) {
          elemento.titulo = tituloAnterior;
          return;
        }
        renderHome(seccionActivaId);
        return;
      }
      const quitarPortadaElemento = event.target.closest("[data-quitar-portada-elemento]");
      if (quitarPortadaElemento) {
        const elemento = carpetas.find(item => item.id === quitarPortadaElemento.dataset.quitarPortadaElemento);
        if (!elemento || !elemento.portada) return;
        const portadaAnterior = elemento.portada;
        elemento.portada = "";
        if (!guardarCarpetas()) elemento.portada = portadaAnterior;
        renderHome(seccionActivaId);
        return;
      }
      const eliminarElemento = event.target.closest("[data-eliminar-elemento]");
      if (eliminarElemento) {
        const index = carpetas.findIndex(item => item.id === eliminarElemento.dataset.eliminarElemento);
        if (index < 0 || !window.confirm(`¿Eliminar «${carpetas[index].titulo || "Sin título"}» y todas sus páginas?`)) return;
        const [eliminado] = carpetas.splice(index, 1);
        if (!guardarCarpetas()) carpetas.splice(index, 0, eliminado);
        renderHome(seccionActivaId);
        return;
      }
      const notebookButton = event.target.closest("[data-open-notebook]");
      if (notebookButton) {
        const notebook = carpetas.find(item => item.id === notebookButton.dataset.openNotebook);
        if (!notebook?.enlaceExterno) {
          window.alert("Agrega el enlace de este cuaderno desde Configuración.");
          return;
        }
        try {
          const url = new URL(notebook.enlaceExterno);
          if (!["https:", "http:"].includes(url.protocol)) throw new Error("El enlace no usa HTTP seguro.");
          window.open(url.href, "_blank", "noopener,noreferrer");
        } catch (error) {
          console.error("El enlace guardado del cuaderno no es válido:", error);
          window.alert("El enlace del cuaderno no es válido. Corrígelo desde Configuración.");
        }
        return;
      }
      const menuToggle = event.target.closest("[data-pagina-opciones]");
      if (menuToggle) {
        const menu = document.getElementById(menuToggle.getAttribute("aria-controls"));
        if (!menu) return;
        const abrir = menu.hidden;
        document.querySelectorAll(".paginas-menu-opciones").forEach(item => { item.hidden = true; });
        document.querySelectorAll("[data-pagina-opciones], [data-elemento-opciones]").forEach(item => item.setAttribute("aria-expanded", "false"));
        menu.hidden = !abrir;
        menuToggle.setAttribute("aria-expanded", String(abrir));
        return;
      }
      const coverButton = event.target.closest("[data-cambiar-portada-pagina]");
      if (coverButton) {
        const id = coverButton.dataset.cambiarPortadaPagina;
        const input = Array.from(document.querySelectorAll(".home-hover-card [data-pagina-portada], #view [data-pagina-portada]"))
          .find(item => item.dataset.paginaPortada === id);
        input?.click();
        return;
      }
      const renameButton = event.target.closest("[data-renombrar-pagina]");
      if (renameButton) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaActivaId);
        const pagina = materia?.paginas.find(item => item.id === renameButton.dataset.renombrarPagina);
        if (!materia || !pagina) return;
        const titulo = window.prompt("Nuevo título de la tarjeta:", tituloTarjetaPagina(pagina));
        if (titulo === null) return;
        if (!titulo.trim()) {
          window.alert("El título de la página no puede quedar vacío.");
          return;
        }
        const tituloAnterior = pagina.cardTitle;
        const legacyTitle = pagina.titulo;
        const actualizadoAnterior = pagina.actualizado;
        pagina.cardTitle = titulo.trim();
        pagina.titulo = pagina.cardTitle;
        pagina.actualizado = Date.now();
        if (!guardarCarpetas()) {
          pagina.cardTitle = tituloAnterior;
          pagina.titulo = legacyTitle;
          pagina.actualizado = actualizadoAnterior;
        }
        renderPaginas(materia);
        return;
      }
      const removePageCover = event.target.closest("[data-quitar-portada-pagina]");
      if (removePageCover) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaActivaId);
        const pagina = materia?.paginas.find(item => item.id === removePageCover.dataset.quitarPortadaPagina);
        if (!materia || !pagina || !pagina.portada) return;
        const portadaAnterior = pagina.portada;
        pagina.portada = "";
        if (!guardarCarpetas()) pagina.portada = portadaAnterior;
        renderPaginas(materia);
        return;
      }
      const deletePageCard = event.target.closest("[data-eliminar-pagina-tarjeta]");
      if (deletePageCard) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaActivaId);
        const pagina = materia?.paginas.find(item => item.id === deletePageCard.dataset.eliminarPaginaTarjeta);
        if (!materia || !pagina || !window.confirm(`¿Eliminar la página «${tituloTarjetaPagina(pagina)}»?`)) return;
        const paginasAnteriores = materia.paginas;
        materia.paginas = materia.paginas.filter(item => item.id !== pagina.id);
        if (!guardarCarpetas()) materia.paginas = paginasAnteriores;
        renderPaginas(materia);
        return;
      }
      const openPage = event.target.closest("[data-abrir-pagina]");
      if (openPage) {
        if (event.target.closest(".pagina-renombrar")) return;
        const title = event.target.closest(".pagina-nombre");
        if (title) {
          window.clearTimeout(temporizadorAbrirPagina);
          if (event.detail > 1) return;
          const paginaId = openPage.dataset.abrirPagina;
          const carpetaId = carpetaActualId;
          temporizadorAbrirPagina = window.setTimeout(() => {
            if (vistaActual === "materia" && carpetaActualId === carpetaId) abrirPagina(carpetaId, paginaId);
          }, 550);
          return;
        }
        abrirPagina(carpetaActualId, openPage.dataset.abrirPagina);
        return;
      }
      if (event.target.closest("[data-crear-pagina]")) {
        const button = event.target.closest("[data-crear-pagina]");
        crearPagina(carpetaActualId, materiaActivaId, button.dataset.pageTerm || "", button.dataset.pageUnit || "");
        return;
      }
      const openMateriaFromPage = event.target.closest("[data-abrir-materia]");
      if (openMateriaFromPage) { abrirMateria(openMateriaFromPage.dataset.abrirCarpeta, openMateriaFromPage.dataset.abrirMateria); return; }
      const backToMaterias = event.target.closest("[data-volver-materias]");
      if (backToMaterias) { abrirCarpeta(backToMaterias.dataset.volverMaterias); return; }
      const deletePage = event.target.closest("[data-eliminar-pagina]");
      if (deletePage) {
        const carpeta = carpetas.find(item => item.id === carpetaActualId);
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaActivaId);
        const pagina = materia?.paginas.find(item => item.id === deletePage.dataset.eliminarPagina);
        if (!materia || !pagina || !window.confirm(`¿Eliminar la página «${tituloTarjetaPagina(pagina)}»?`)) return;
        const paginasAnteriores = materia.paginas;
        materia.paginas = materia.paginas.filter(item => item.id !== pagina.id);
        if (!guardarCarpetas()) materia.paginas = paginasAnteriores;
        renderPaginas(materia);
        return;
      }
      const open = event.target.closest("[data-open-study]");
      if (open) { abrirCarpeta(open.dataset.openStudy); return; }
      if (event.target.closest("[data-go-home]")) { renderHome(null); return; }
      if (event.target.closest("[data-volver]")) { renderHome(); return; }
    });
    document.addEventListener("change", async event => {
      const dashboardTask = event.target.closest("[data-dashboard-task-page]");
      if (dashboardTask) {
        const pageId = dashboardTask.dataset.dashboardTaskPage;
        const taskIndex = Number(dashboardTask.dataset.dashboardTaskIndex);
        const inputIndex = Number(dashboardTask.dataset.dashboardInputIndex);
        const task = tareasDashboardInicio().find(item => item.page.id === pageId && item.taskIndex === taskIndex);
        if (!task || !Number.isInteger(inputIndex) || inputIndex < 0) {
          window.alert("No se pudo actualizar esta tarea. Vuelve a abrir la página e inténtalo otra vez.");
          return;
        }
        const previous = task.page.contenido;
        task.page.contenido = alternarCheckboxGuardado(previous, inputIndex);
        if (task.page.contenido === previous) {
          window.alert("No se pudo encontrar la casilla guardada para esta tarea.");
          return;
        }
        if (!guardarCarpetas()) task.page.contenido = previous;
        renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const homePageCover = event.target.closest("[data-home-page-cover]");
      if (homePageCover?.files?.[0]) {
        const item = buscarItemInicio("page", homePageCover.dataset.homePageCover);
        if (!item) return;
        await cambiarPortadaPagina(item.materia, homePageCover);
        if (vistaActual === "inicio") renderHome(inicioDashboard ? null : seccionActivaId);
        return;
      }
      const materiaPortadaInput = event.target.closest("[data-materia-portada]");
      if (materiaPortadaInput?.files?.[0]) {
        await cambiarPortadaMateria(materiaPortadaInput);
        return;
      }
      const materiaColorInput = event.target.closest("[data-materia-color]");
      if (materiaColorInput) {
        const carpeta = carpetas.find(item => materiasDeCarpeta(item).some(materia => materia.id === materiaColorInput.dataset.materiaColor));
        const materia = materiasDeCarpeta(carpeta).find(item => item.id === materiaColorInput.dataset.materiaColor);
        if (!carpeta || !materia) return;
        const anterior = materia.color;
        materia.color = materiaColorInput.value;
        if (!guardarCarpetas()) materia.color = anterior;
        abrirCarpeta(carpeta.id);
        return;
      }
      const coverInput = event.target.closest("[data-elemento-portada]");
      if (coverInput?.files?.[0]) {
        const elemento = carpetas.find(item => item.id === coverInput.dataset.elementoPortada);
        const file = coverInput.files[0];
        if (!elemento) return;
        if (!file.type.startsWith("image/")) {
          window.alert("Elige un archivo de imagen.");
          coverInput.value = "";
          return;
        }
        if (file.size > 15 * 1024 * 1024) {
          window.alert("La imagen debe ocupar menos de 15 MB.");
          coverInput.value = "";
          return;
        }
        try {
          const bitmap = await createImageBitmap(file);
          const scale = Math.min(1, 1200 / bitmap.width, 1200 / bitmap.height);
          const canvas = document.createElement("canvas");
          canvas.width = Math.max(1, Math.round(bitmap.width * scale));
          canvas.height = Math.max(1, Math.round(bitmap.height * scale));
          const context = canvas.getContext("2d");
          if (!context) throw new Error("No se pudo preparar la imagen.");
          context.drawImage(bitmap, 0, 0, canvas.width, canvas.height);
          bitmap.close();
          const portadaAnterior = elemento.portada;
          elemento.portada = canvas.toDataURL("image/jpeg", 0.78);
          if (elemento.portada.length > 1400000) {
            elemento.portada = portadaAnterior;
            window.alert("La imagen sigue siendo demasiado grande; intenta con otra.");
            return;
          }
          if (!guardarCarpetas()) elemento.portada = portadaAnterior;
          renderHome(seccionActivaId);
        } catch (error) {
          console.error("No se pudo procesar la portada del elemento:", error);
          window.alert("No se pudo abrir la imagen. Prueba con otro archivo.");
        } finally {
          coverInput.value = "";
        }
        return;
      }
      const colorInput = event.target.closest("[data-elemento-color]");
      if (!colorInput) return;
      const elemento = carpetas.find(item => item.id === colorInput.dataset.elementoColor);
      if (!elemento) return;
      const colorAnterior = elemento.color;
      elemento.color = colorInput.value;
      if (!guardarCarpetas()) elemento.color = colorAnterior;
      renderHome(seccionActivaId);
    });
    document.getElementById("home-link").addEventListener("click", () => renderHome(null));
    document.getElementById("settings-link").addEventListener("click", abrirConfiguracion);
    document.getElementById("new-folder-nav").addEventListener("click", () => {
      if (inicioDashboard) {
        const folderWithSubject = carpetas.find(folder => esCarpetaDeEstudio(folder) && materiasDeCarpeta(folder).length);
        const folderWithoutSubject = carpetas.find(esCarpetaDeEstudio);
        if (folderWithSubject) {
          abrirMateria(folderWithSubject.id, materiasDeCarpeta(folderWithSubject)[0].id);
          return;
        }
        if (folderWithoutSubject) {
          crearMateria(folderWithoutSubject.id);
          return;
        }
        const studySection = secciones.find(section => section.id === "mis-carpetas");
        if (studySection) {
          crearElementoSeccion(studySection.id);
          return;
        }
      }
      crearCarpeta();
    });
    document.getElementById("new-section-nav").addEventListener("click", () => crearSeccion());
    document.getElementById("add-folder-icon").addEventListener("click", () => crearSeccion());
    themeToggle.addEventListener("click", () => {
      const theme = document.documentElement.dataset.theme === "dark" ? "light" : "dark";
      setTheme(theme);
      try { localStorage.setItem(THEME_KEY, theme); }
      catch (error) { console.error("No se pudo guardar la preferencia del tema:", error); saveState.textContent = "No se pudo guardar el tema"; }
    });
    search.addEventListener("input", () => {
      const query = search.value.trim();
      if (query) {
        renderBusquedaGlobal(search.value);
        return;
      }
      if (vistaActual !== "busqueda") return;
      const vistaAnterior = vistaBusquedaAnterior;
      const carpetaAnteriorId = carpetaBusquedaAnteriorId;
      const materiaAnteriorId = materiaBusquedaAnteriorId;
      const paginaAnteriorId = paginaBusquedaAnteriorId;
      const elementoGaleriaAnteriorId = elementoGaleriaBusquedaAnteriorId;
      const carpetaAnterior = carpetas.find(carpeta => carpeta.id === carpetaAnteriorId);
      if (vistaAnterior === "pagina" && elementoGaleriaAnteriorId && esElementoGaleria(carpetaAnterior)) abrirElementoGaleria(elementoGaleriaAnteriorId);
      else if (vistaAnterior === "pagina" && encontrarPagina(carpetaAnterior, paginaAnteriorId, materiaAnteriorId)) abrirPagina(carpetaAnteriorId, paginaAnteriorId, materiaAnteriorId);
      else if (vistaAnterior === "materia" && materiasDeCarpeta(carpetaAnterior).some(materia => materia.id === materiaAnteriorId)) abrirMateria(carpetaAnteriorId, materiaAnteriorId);
      else if (vistaAnterior === "carpeta" && carpetaAnterior) abrirCarpeta(carpetaAnteriorId);
      else renderHome(seccionActivaId);
    });

    renderSidebar();
    let ultimaPestanaInicio = null;
    try {
      const guardada = localStorage.getItem(HOME_TAB_KEY);
      ultimaPestanaInicio = guardada === "inicio" ? null : secciones.some(section => section.id === guardada) ? guardada : null;
    } catch (error) {
      console.error("No se pudo recuperar la última pestaña visitada:", error);
    }
    renderHome(ultimaPestanaInicio);
    configurarMenuLateral();
  </script>
  <script>
    (() => {
      "use strict";
      const TREE_KEY = "nido-page-tree-v1";
      const LEGACY_FOLDERS_KEY = "nido-study-folders-v1";
      const LEGACY_SECTIONS_KEY = "nido-content-sections-v1";
      const CARD_SIZE_KEY = "nido-tree-card-size-v1";
      const TREE_FOLDER_CLEANUP_KEY = "nido-requested-folder-cleanup-tree-v1";
      const PRIVATE_ROOT_ID = "nido-private-root";
      const root = document.getElementById("tree-app-root");
      const view = root.querySelector("[data-tree-view]");
      const menu = root.querySelector("[data-tree-slash-menu]");
      const saveStatus = root.querySelector(".tree-save-status");
      const searchFields = Array.from(root.querySelectorAll(".tree-search"));
      const slashCommands = [
        { id: "paragraph", label: "Texto", icon: "¶", match: "texto párrafo paragraph" },
        { id: "h1", label: "Título 1", icon: "H1", match: "titulo encabezado heading" },
        { id: "h2", label: "Título 2", icon: "H2", match: "titulo encabezado heading" },
        { id: "h3", label: "Título 3", icon: "H3", match: "titulo encabezado heading" },
        { id: "bullet", label: "Lista con viñetas", icon: "•", match: "lista viñetas puntos" },
        { id: "numbered", label: "Lista numerada", icon: "1.", match: "lista numerada orden" },
        { id: "checklist", label: "Lista de tareas", icon: "☐", match: "lista tareas checklist casilla" },
        { id: "toggle", label: "Toggle", icon: "▸", match: "toggle desplegable colapsable" },
        { id: "quote", label: "Cita", icon: "❞", match: "cita quote" },
        { id: "code", label: "Código", icon: "</>", match: "codigo code bloque" },
        { id: "divider", label: "Divisor", icon: "―", match: "divisor separador linea" },
        { id: "image", label: "Imagen", icon: "🖼", match: "imagen foto fotografia picture image" },
        { id: "page", label: "Página", icon: "▤", match: "pagina subpagina sub página" }
      ];
      const iconByType = { folder: "📁", subject: "", page: "📄" };
      const CARD_SIZE_MIN = 160;
      const CARD_SIZE_MAX = 360;
      const CARD_SIZE_STEP = 40;
      let cardSize = 250;
      try {
        const savedCardSize = Number(localStorage.getItem(CARD_SIZE_KEY));
        if (Number.isFinite(savedCardSize) && savedCardSize >= CARD_SIZE_MIN && savedCardSize <= CARD_SIZE_MAX) cardSize = savedCardSize;
      } catch (error) {
        console.error("No se pudo recuperar el tamaño de las tarjetas:", error);
      }
      let nodes = [];
      let selectedId = "";
      let searchQuery = "";
      let searchOpen = false;
      let saveTimer = 0;
      let slashState = null;
      let slashIndex = 0;
      let draggingId = "";

      const id = () => globalThis.crypto?.randomUUID?.() || `node-${Date.now().toString(36)}-${Math.random().toString(36).slice(2, 9)}`;
      const escape = value => String(value ?? "").replace(/[&<>"']/g, char => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", "\"": "&quot;", "'": "&#39;" })[char]);
      const normalize = value => String(value ?? "").toLocaleLowerCase("es").normalize("NFD").replace(/[\u0300-\u036f]/g, "");
      const createNode = (type, title, parentId, order, extra = {}) => ({
        id: id(), parentId, type, title, icon: iconByType[type], content: type === "page" ? [] : [],
        order, expanded: true, favorite: false, createdAt: new Date().toISOString(),
        updatedAt: new Date().toISOString(), ...extra
      });
      const findNode = nodeId => nodes.find(node => node.id === nodeId) || null;
      const getChildren = nodeId => nodes.filter(node => node.parentId === nodeId).sort((a, b) => a.order - b.order);
      const getAncestors = nodeId => {
        const path = [];
        let current = findNode(nodeId);
        while (current) {
          path.unshift(current);
          current = current.parentId ? findNode(current.parentId) : null;
        }
        return path;
      };
      const getDescendants = nodeId => {
        const result = [];
        const visit = parentId => getChildren(parentId).forEach(child => {
          result.push(child);
          visit(child.id);
        });
        visit(nodeId);
        return result;
      };
      const isValidParent = (type, parent) => {
        if (!parent) return type === "folder";
        if (parent.type === "folder") return type === "folder" || type === "subject";
        if (parent.type === "subject" || parent.type === "page") return type === "page";
        return false;
      };
      const reindexChildren = parentId => getChildren(parentId).forEach((node, index) => { node.order = index; });
      const moveNode = (nodeId, newParentId, index) => {
        const node = findNode(nodeId);
        const newParent = newParentId ? findNode(newParentId) : null;
        if (!node || node.id === PRIVATE_ROOT_ID || (newParentId && !newParent) || !isValidParent(node.type, newParent)) return false;
        if (nodeId === newParentId || (newParentId && getDescendants(nodeId).some(descendant => descendant.id === newParentId))) return false;
        if (newParentId === PRIVATE_ROOT_ID && node.id === PRIVATE_ROOT_ID) return false;
        const oldParentId = node.parentId;
        const oldSiblings = getChildren(oldParentId).filter(item => item.id !== node.id);
        node.parentId = newParentId;
        const newSiblings = getChildren(newParentId).filter(item => item.id !== node.id);
        const insertion = Math.max(0, Math.min(Number.isInteger(index) ? index : newSiblings.length, newSiblings.length));
        newSiblings.splice(insertion, 0, node);
        newSiblings.forEach((sibling, order) => { sibling.order = order; });
        oldSiblings.forEach((sibling, order) => { sibling.order = order; });
        node.updatedAt = new Date().toISOString();
        persist();
        render();
        return true;
      };
      const newPrivateRoot = () => ({
        id: PRIVATE_ROOT_ID, parentId: null, type: "folder", title: "Privado", icon: "🔒",
        content: [], order: Number.MAX_SAFE_INTEGER, expanded: true, favorite: false,
        createdAt: new Date().toISOString(), updatedAt: new Date().toISOString()
      });
      const plainBlock = text => ({ id: id(), type: "paragraph", text: String(text || ""), html: "" });
      const sanitizeHtml = source => {
        const documentHtml = new DOMParser().parseFromString(String(source || ""), "text/html");
        documentHtml.querySelectorAll("script,style,iframe,object,embed,form").forEach(element => element.remove());
        documentHtml.querySelectorAll("*").forEach(element => {
          Array.from(element.attributes).forEach(attribute => {
            if (/^on/i.test(attribute.name) || (["href", "src"].includes(attribute.name.toLowerCase()) && /^\s*javascript:/i.test(attribute.value))) {
              element.removeAttribute(attribute.name);
            }
          });
        });
        documentHtml.body.querySelectorAll("*").forEach(element => {
          if (element.tagName === "IMG") return;
          ["style", "class", "color", "bgcolor", "face", "size", "align", "dir", "lang"].forEach(name => element.removeAttribute(name));
        });
        documentHtml.body.querySelectorAll("font").forEach(element => element.replaceWith(...element.childNodes));
        return documentHtml.body.innerHTML;
      };
      const legacyContentToBlocks = source => {
        const value = String(source || "");
        if (!value) return [];
        if (!/<[a-z][\s\S]*>/i.test(value)) return value.split(/\r?\n/).map(plainBlock);
        const parsed = new DOMParser().parseFromString(value, "text/html");
        const blocks = [];
        Array.from(parsed.body.childNodes).forEach(child => {
          if (child.nodeType === Node.TEXT_NODE && !child.textContent.trim()) return;
          if (child.nodeType === Node.TEXT_NODE) {
            blocks.push(plainBlock(child.textContent));
            return;
          }
          const element = child;
          const tag = element.tagName.toLowerCase();
          let type = "legacy";
          if (/^h[1-3]$/.test(tag)) type = tag;
          else if (tag === "p" || tag === "div") type = "paragraph";
          else if (tag === "blockquote") type = "quote";
          else if (tag === "pre") type = "code";
          else if (tag === "hr") type = "divider";
          else if (tag === "ul" || tag === "ol") {
            type = tag === "ol" ? "numbered" : element.classList.contains("editor-tareas") ? "checklist" : "bullet";
            const items = Array.from(element.children).filter(item => item.tagName === "LI");
            if (items.length) {
              items.forEach(item => {
                const copy = item.cloneNode(true);
                const checkbox = copy.querySelector("input[type='checkbox']");
                const checked = Boolean(checkbox?.checked || checkbox?.hasAttribute("checked"));
                copy.querySelectorAll("input[type='checkbox']").forEach(input => input.remove());
                blocks.push({ id: id(), type, text: item.textContent || "", html: sanitizeHtml(copy.innerHTML), checked });
              });
              return;
            }
          }
          blocks.push({ id: id(), type, text: element.textContent || "", html: sanitizeHtml(element.innerHTML), checked: false });
        });
        return blocks.length ? blocks : [plainBlock(parsed.body.textContent || "")];
      };
      const legacySections = () => {
        try {
          const value = JSON.parse(localStorage.getItem(LEGACY_SECTIONS_KEY) || "[]");
          return Array.isArray(value) ? value : [];
        } catch (error) {
          console.error("No se pudieron leer las secciones antiguas durante la migración:", error);
          return [];
        }
      };
      const migrateLegacy = legacy => {
        const imported = [];
        const sections = legacySections();
        let privateRoot = newPrivateRoot();
        const usedIds = new Set();
        const uniqueId = candidate => {
          let next = typeof candidate === "string" && candidate ? candidate : id();
          while (usedIds.has(next)) next = id();
          usedIds.add(next);
          return next;
        };
        const timestamp = value => {
          const date = Number.isFinite(Number(value)) && String(value).trim() ? new Date(Number(value)) : new Date(value || Date.now());
          return Number.isNaN(date.getTime()) ? new Date().toISOString() : date.toISOString();
        };
        const addPage = (page, parentId, order) => {
          const pageNode = {
            id: uniqueId(page.id),
            parentId, type: "page", title: String(page.pageTitle || page.cardTitle || page.titulo || "Sin título"),
            icon: typeof page.icon === "string" ? page.icon : "📄",
            content: legacyContentToBlocks(page.contenido || ""), order, expanded: true,
            favorite: Boolean(page.favorito), createdAt: timestamp(page.creado || page.createdAt),
            updatedAt: timestamp(page.actualizado || page.updatedAt)
          };
          imported.push(pageNode);
          const children = Array.isArray(page.subpaginas) ? page.subpaginas : Array.isArray(page.paginas) ? page.paginas : [];
          children.forEach((child, childIndex) => addPage(child, pageNode.id, childIndex));
          return pageNode;
        };
        (Array.isArray(legacy) ? legacy : []).forEach((folder, folderIndex) => {
          const section = sections.find(item => item.id === folder.seccionId);
          const isStudyFolder = !folder.seccionId || folder.seccionId === "mis-carpetas" || section?.tipo === "carpetas";
          const folderNode = {
            id: uniqueId(folder.id), parentId: null, type: "folder",
            title: String(folder.titulo || "Sin título"), icon: typeof folder.icon === "string" ? folder.icon : "📁",
            content: [], order: folderIndex, expanded: true, favorite: Boolean(folder.favorito),
            createdAt: timestamp(folder.creado || folder.createdAt), updatedAt: timestamp(folder.actualizado || folder.updatedAt)
          };
          if (!isStudyFolder && section) folderNode.title = `${section.titulo}: ${folderNode.title}`;
          imported.push(folderNode);
          const subjects = Array.isArray(folder.materias) ? folder.materias : [];
          subjects.forEach((subject, subjectIndex) => {
            const subjectNode = {
              id: uniqueId(subject.id), parentId: folderNode.id, type: "subject",
              title: String(subject.titulo || "Sin título"), icon: typeof subject.icon === "string" ? subject.icon : iconByType.subject,
              content: [], order: subjectIndex, expanded: true, favorite: Boolean(subject.favorito),
              createdAt: timestamp(subject.creado || subject.createdAt), updatedAt: timestamp(subject.actualizado || subject.updatedAt)
            };
            imported.push(subjectNode);
            (Array.isArray(subject.paginas) ? subject.paginas : []).forEach((page, pageIndex) => addPage(page, subjectNode.id, pageIndex));
          });
          if (!subjects.length) {
            const pages = Array.isArray(folder.paginas) ? folder.paginas : [];
            if (pages.length) {
              const subjectNode = {
                id: uniqueId(), parentId: folderNode.id, type: "subject",
                title: isStudyFolder ? "Mis apuntes" : "Elementos",
                icon: iconByType.subject, content: [], order: 0, expanded: true, favorite: false,
                createdAt: folderNode.createdAt, updatedAt: folderNode.updatedAt
              };
              imported.push(subjectNode);
              pages.forEach((page, pageIndex) => addPage(page, subjectNode.id, pageIndex));
            }
          }
        });
        privateRoot.order = imported.filter(node => node.parentId === null).length;
        imported.push(privateRoot);
        return imported;
      };
      const exampleTree = () => {
        const folder = createNode("folder", "Historia", null, 0, { icon: "📚", expanded: true });
        const subject = createNode("subject", "Pedagogía", folder.id, 0, { icon: "🎓", expanded: true });
        const notes = createNode("page", "Apuntes clase 1", subject.id, 0, { icon: "📝", expanded: true });
        const summary = createNode("page", "Resumen", notes.id, 0, {
          icon: "📑", expanded: true,
          content: [plainBlock("Apuntes de Historia de la Educación."), { id: id(), type: "h2", text: "Ideas principales", html: "" }, plainBlock("Escribe aquí tus ideas, preguntas y referencias de la clase.")]
        });
        const questions = createNode("page", "Dudas", summary.id, 0, { icon: "💭", content: [plainBlock("¿Qué conceptos quiero repasar?")] });
        return [folder, subject, notes, summary, questions, newPrivateRoot()];
      };
      const load = () => {
        let limpiezaArbolPendiente = false;
        try {
          limpiezaArbolPendiente = localStorage.getItem(TREE_FOLDER_CLEANUP_KEY) !== "done";
          const savedTree = localStorage.getItem(TREE_KEY);
          if (savedTree) {
            const parsed = JSON.parse(savedTree);
            if (!Array.isArray(parsed) || parsed.some(node =>
              !node || typeof node.id !== "string" || !(node.parentId === null || typeof node.parentId === "string") ||
              !["folder", "subject", "page"].includes(node.type) || typeof node.title !== "string"
            )) throw new Error("El árbol guardado tiene un formato inválido.");
            nodes = parsed.map((node, index) => ({
              id: node.id, parentId: node.parentId, type: node.type, title: node.title,
              icon: typeof node.icon === "string" ? node.icon : iconByType[node.type],
              content: node.type === "page" && Array.isArray(node.content) ? node.content : [],
              order: Number.isInteger(node.order) ? node.order : index,
              expanded: node.expanded !== false, favorite: Boolean(node.favorite),
              createdAt: node.createdAt || new Date().toISOString(), updatedAt: node.updatedAt || new Date().toISOString()
            }));
            nodes.forEach(node => {
              if (node.type !== "page") return;
              node.content = node.content.map(block => ({
                ...block,
                id: typeof block.id === "string" ? block.id : id(),
                type: typeof block.type === "string" ? block.type : "paragraph",
                text: typeof block.text === "string" ? block.text : "",
                html: typeof block.html === "string" ? block.html : ""
              }));
            });
            if (!nodes.some(node => node.id === PRIVATE_ROOT_ID)) nodes.push(newPrivateRoot());
          } else {
            const legacyValue = localStorage.getItem(LEGACY_FOLDERS_KEY);
            if (legacyValue !== null) {
              const legacy = JSON.parse(legacyValue);
              if (!Array.isArray(legacy)) throw new Error("Los datos antiguos no tienen un formato válido.");
              nodes = legacy.length ? migrateLegacy(legacy) : [newPrivateRoot()];
            } else {
              nodes = exampleTree();
            }
            localStorage.setItem(TREE_KEY, JSON.stringify(nodes));
          }
        } catch (error) {
          console.error("No se pudo cargar o migrar el árbol de apuntes:", error);
          saveStatus.textContent = "Error al cargar datos";
          if (!nodes.length) nodes = exampleTree();
        }
        const carpetasRaizAEliminar = limpiezaArbolPendiente ? nodes.filter(node =>
          node.id !== PRIVATE_ROOT_ID && node.type === "folder" && node.parentId === null && esCarpetaSolicitadaEliminar(node)
        ) : [];
        let carpetasSolicitadasEliminadas = false;
        if (carpetasRaizAEliminar.length) {
          const idsEliminados = new Set();
          carpetasRaizAEliminar.forEach(folder => {
            idsEliminados.add(folder.id);
            getDescendants(folder.id).forEach(descendant => idsEliminados.add(descendant.id));
          });
          nodes = nodes.filter(node => !idsEliminados.has(node.id));
          idsEliminados.forEach(nodeId => dropPageStyle(nodeId));
          carpetasSolicitadasEliminadas = true;
        }
        let limpiezaArbolGuardada = false;
        if (limpiezaArbolPendiente) {
          try {
            if (carpetasSolicitadasEliminadas) localStorage.setItem(TREE_KEY, JSON.stringify(nodes));
            localStorage.setItem(TREE_FOLDER_CLEANUP_KEY, "done");
            limpiezaArbolGuardada = true;
          } catch (error) {
            console.error("No se pudo guardar la eliminación de las carpetas solicitadas:", error);
            saveStatus.textContent = "No se pudo guardar la eliminación";
          }
        }
        if (!nodes.some(node => node.parentId === null && node.id !== PRIVATE_ROOT_ID) && !carpetasSolicitadasEliminadas) {
          const sample = exampleTree();
          nodes = [...sample.filter(node => node.id !== PRIVATE_ROOT_ID), ...nodes.filter(node => node.id === PRIVATE_ROOT_ID)];
        }
        let migratedSubjectIcons = false;
        nodes.forEach(node => {
          if (node.type === "subject" && ["📚", "▤"].includes(node.icon)) {
            node.icon = "";
            migratedSubjectIcons = true;
          }
        });
        if (nodes.some(node => !isValidParent(node.type, node.parentId ? findNode(node.parentId) : null))) {
          console.error("El árbol contiene relaciones no válidas; se conserva el contenido y se corrigen los nodos incompatibles a carpetas raíz.");
          nodes.forEach(node => {
            if (node.id === PRIVATE_ROOT_ID) {
              node.parentId = null;
              node.type = "folder";
              return;
            }
            const parent = node.parentId ? findNode(node.parentId) : null;
            if (node.type === "page" && parent?.type === "folder") {
              const subject = createNode("subject", "Mis apuntes", parent.id, getChildren(parent.id).length);
              nodes.push(subject);
              node.parentId = subject.id;
              return;
            }
            if (node.type === "subject" && !parent) {
              node.type = "folder";
              node.icon = node.icon || iconByType.folder;
              return;
            }
            node.parentId = null;
            if (node.type !== "folder") node.type = "folder";
          });
        }
        const startPage = nodes.find(node => node.type === "page" && node.title === "Dudas") || nodes.find(node => node.type === "page");
        selectedId = startPage?.id || nodes.find(node => node.parentId === null && node.id !== PRIVATE_ROOT_ID)?.id || PRIVATE_ROOT_ID;
        if (migratedSubjectIcons || (carpetasSolicitadasEliminadas && !limpiezaArbolGuardada)) persist();
      };

      const ICON_SIZE = 96;
      const GIF_INLINE_BYTES = 200 * 1024;
      const GIF_MAX_BYTES = 25 * 1024 * 1024;
      const readAsDataUrl = file => new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => resolve(String(reader.result));
        reader.onerror = () => reject(reader.error || new Error("No se pudo leer el archivo"));
        reader.readAsDataURL(file);
      });
      // Acepta webp, jpg, png y gif. Todo (menos el gif animado) se convierte a un cuadrado chico (webp, o png si el navegador no soporta webp).
      const imageFileToIcon = async originalFile => {
        let file = originalFile;
        if (!file.type) {
          const ext = (file.name.split(".").pop() || "").toLowerCase();
          const types = { webp: "image/webp", jpg: "image/jpeg", jpeg: "image/jpeg", png: "image/png", gif: "image/gif" };
          if (types[ext]) file = new File([file], file.name, { type: types[ext] });
        }
        if (!/^image\/(webp|jpe?g|png|gif)$/i.test(file.type)) throw new Error("Usá una imagen WEBP, JPG, PNG o GIF.");
        if (file.type === "image/gif") {
          if (file.size > GIF_MAX_BYTES) throw new Error(`El GIF pesa ${(file.size / 1024).toFixed(0)} KB y el máximo es 25 MB.`);
          if (file.size <= GIF_INLINE_BYTES) return readAsDataUrl(file);
          const key = `idb:${id()}`;
          try { await iconPut(key, file); } catch (error) { throw new Error("Tu navegador no deja guardar GIF grandes (¿modo privado?). Probá con uno de menos de 200 KB."); }
          const url = URL.createObjectURL(file);
          try { await loadImageElement(url); } catch (error) { iconDel(key); URL.revokeObjectURL(url); throw new Error("El navegador no pudo abrir ese GIF (puede estar dañado)."); }
          iconUrls.set(key, url);
          return key;
        }
        const source = await readAsDataUrl(file);
        const img = await loadImageElement(source);
        const side = Math.min(img.naturalWidth, img.naturalHeight);
        const canvas = document.createElement("canvas");
        canvas.width = ICON_SIZE; canvas.height = ICON_SIZE;
        const ctx = canvas.getContext("2d");
        ctx.drawImage(img, (img.naturalWidth - side) / 2, (img.naturalHeight - side) / 2, side, side, 0, 0, ICON_SIZE, ICON_SIZE);
        let out = canvas.toDataURL("image/webp", 0.9);
        if (!out.startsWith("data:image/webp")) out = canvas.toDataURL("image/png");
        return out;
      };
      const openIconDialog = node => {
        document.querySelector(".tree-icon-dialog")?.remove();
        let chosen = node.icon || iconByType[node.type];
        const dialog = document.createElement("div");
        dialog.className = "tree-icon-dialog";
        dialog.innerHTML = `<div class="tree-icon-box" role="dialog" aria-modal="true" aria-label="Cambiar ícono">
          <h3>Cambiar ícono</h3>
          <div class="tree-icon-row"><div class="tree-icon-preview" data-icon-preview></div>
            <button type="button" data-icon-pick>Subir imagen…</button>
            <button type="button" data-icon-pick-gif>Subir GIF animado…</button>
            <button type="button" data-icon-clear>Quitar imagen</button>
            <input type="file" data-icon-file accept="image/webp,image/jpeg,image/png,image/gif,.webp,.jpg,.jpeg,.png,.gif" hidden>
            <input type="file" data-icon-file-gif accept="image/gif,.gif" hidden></div>
          <div class="tree-icon-hint">Imágenes WEBP, JPG o PNG (se recortan a cuadrado) y GIF animados de hasta 25 MB. También podés usar un emoji.</div>
          <div class="tree-icon-msg" data-icon-msg role="alert"></div>
          <label>O escribí un emoji<input type="text" data-icon-text maxlength="8" autocomplete="off"></label>
          <div class="tree-icon-row" style="justify-content:flex-end"><button type="button" data-icon-cancel>Cancelar</button><button type="button" class="is-primary" data-icon-save>Guardar</button></div>
        </div>`;
        const preview = dialog.querySelector("[data-icon-preview]");
        const textInput = dialog.querySelector("[data-icon-text]");
        const fileInput = dialog.querySelector("[data-icon-file]");
        const gifInput = dialog.querySelector("[data-icon-file-gif]");
        const message = dialog.querySelector("[data-icon-msg]");
        const paint = () => {
          preview.replaceChildren();
          if (isImageIcon(chosen)) {
            textInput.value = "";
            const current = chosen;
            const fallback = () => { if (chosen !== current) return; chosen = iconByType[node.type]; message.textContent = "La imagen guardada ya no está disponible. Subí otra o elegí un emoji."; paint(); };
            const show = src => {
              const img = document.createElement("img");
              img.alt = "";
              img.addEventListener("error", fallback, { once: true });
              img.src = src;
              preview.replaceChildren(img);
            };
            const src = iconSrc(current);
            if (src) show(src);
            else if (isStoredIcon(current)) {
              preview.textContent = "…";
              iconGet(current).then(blob => {
                if (chosen !== current) return;
                if (!blob) { fallback(); return; }
                iconUrls.set(current, URL.createObjectURL(blob));
                show(iconUrls.get(current));
              }).catch(fallback);
            } else fallback();
          }
          else { preview.textContent = chosen; textInput.value = chosen; }
        };
        const close = () => { dialog.remove(); document.removeEventListener("keydown", onKey, true); };
        const onKey = event => { if (event.key === "Escape") { event.stopPropagation(); close(); } };
        document.addEventListener("keydown", onKey, true);
        dialog.addEventListener("click", async event => {
          if (event.target === dialog || event.target.closest("[data-icon-cancel]")) { close(); return; }
          if (event.target.closest("[data-icon-pick-gif]")) { gifInput.click(); return; }
          if (event.target.closest("[data-icon-pick]")) { fileInput.click(); return; }
          if (event.target.closest("[data-icon-clear]")) { chosen = iconByType[node.type]; message.textContent = ""; paint(); return; }
          if (event.target.closest("[data-icon-save]")) {
            const typed = textInput.value.trim();
            if (!isImageIcon(chosen) && typed) chosen = typed;
            if (isStoredIcon(node.icon) && node.icon !== chosen) iconDel(node.icon);
            node.icon = chosen || iconByType[node.type];
            node.updatedAt = new Date().toISOString();
            persist(); render(); close();
          }
        });
        textInput.addEventListener("input", () => { chosen = textInput.value.trim() || iconByType[node.type]; message.textContent = ""; if (isImageIcon(preview.firstChild?.src)) preview.textContent = chosen; else preview.textContent = chosen; });
        [fileInput, gifInput].forEach(input => input.addEventListener("change", async () => {
          const file = input.files && input.files[0];
          input.value = "";
          if (!file) return;
          try { message.textContent = "Procesando…"; chosen = await imageFileToIcon(file); message.textContent = ""; paint(); }
          catch (error) { console.error("Ícono:", error); message.textContent = `${error && error.message ? error.message : "No se pudo usar esa imagen."} [${file.name}, ${(file.size / 1024).toFixed(0)} KB, tipo: ${file.type || "desconocido"}]`; }
        }));
        paint();
        document.body.append(dialog);
        (isImageIcon(chosen) ? dialog.querySelector("[data-icon-pick]") : textInput).focus();
      };
      const persist = () => {
        window.clearTimeout(saveTimer);
        saveStatus.textContent = "Guardando…";
        saveTimer = window.setTimeout(() => {
          try {
            localStorage.setItem(TREE_KEY, JSON.stringify(nodes));
            saveStatus.textContent = "Guardado";
          } catch (error) {
            console.error("No se pudo guardar el árbol de apuntes:", error);
            saveStatus.textContent = "No se pudo guardar";
          }
        }, 250);
      };
      const contentText = node => (node.content || []).map(block => {
        if (block.text) return block.text;
        if (block.html) return new DOMParser().parseFromString(block.html, "text/html").body.textContent || "";
        return "";
      }).join(" ");
      const nodeMatches = (node, query) => normalize(`${node.title} ${contentText(node)}`).includes(normalize(query));
      const descendantMatches = (nodeId, query) => getChildren(nodeId).some(child => nodeMatches(child, query) || descendantMatches(child.id, query));
      const isStoredIcon = value => typeof value === "string" && /^idb:[\w-]+$/.test(value);
      const isImageIcon = value => isStoredIcon(value) || (typeof value === "string" && /^data:image\/(?:webp|png|jpeg|gif);base64,[a-z0-9+/=]+$/i.test(value));
      // GIF pesados se guardan en IndexedDB (mucho más espacio que localStorage)
      const iconUrls = new Map();
      const iconLoading = new Set();
      const iconDbOpen = () => new Promise((resolve, reject) => {
        const request = indexedDB.open("nido-tree-icons-v1", 1);
        request.onupgradeneeded = () => request.result.createObjectStore("icons");
        request.onsuccess = () => resolve(request.result);
        request.onerror = () => reject(request.error);
      });
      const iconDbRun = async (mode, work) => {
        const db = await iconDbOpen();
        return new Promise((resolve, reject) => {
          const tx = db.transaction("icons", mode);
          const req = work(tx.objectStore("icons"));
          tx.oncomplete = () => resolve(req && req.result);
          tx.onerror = () => reject(tx.error);
        });
      };
      const iconPut = (key, blob) => iconDbRun("readwrite", store => store.put(blob, key));
      const iconGet = key => iconDbRun("readonly", store => store.get(key));
      const iconDel = key => iconDbRun("readwrite", store => store.delete(key)).catch(() => {});
      const iconSrc = value => {
        if (!isStoredIcon(value)) return value;
        if (!iconUrls.has(value) && !iconLoading.has(value)) {
          iconLoading.add(value);
          iconGet(value).then(blob => { if (blob) { iconUrls.set(value, URL.createObjectURL(blob)); render(); } }).catch(() => {});
        }
        return iconUrls.get(value) || "";
      };
      const nodeIcon = node => {
        if (!isImageIcon(node.icon)) return escape(node.icon || iconByType[node.type]);
        const src = iconSrc(node.icon);
        return src ? `<img class="tree-icon-img" src="${escape(src)}" alt="" draggable="false">` : escape(iconByType[node.type]);
      };
      const hasChildren = node => getChildren(node.id).length > 0;
      const activeFolderId = () => {
        const path = getAncestors(selectedId);
        for (let index = path.length - 1; index >= 0; index -= 1) if (path[index].type === "folder") return path[index].id;
        return "";
      };
      const dotsIcon = '<svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><circle cx="5" cy="12" r="2"/><circle cx="12" cy="12" r="2"/><circle cx="19" cy="12" r="2"/></svg>';
      const nodeMenuItems = node => {
        const addLabel = node.type === "folder" ? "Nueva materia" : node.type === "subject" ? "Nueva página" : "Nueva subpágina";
        const nestedActions = node.type === "folder"
          ? `<button type="button" data-tree-nested-add="${escape(node.id)}" data-nested-type="subject">Nueva materia</button><button type="button" data-tree-nested-add="${escape(node.id)}" data-nested-type="folder">Nueva subcarpeta</button>`
          : `<button type="button" data-tree-nested-add="${escape(node.id)}" data-nested-type="page">${addLabel}</button>`;
        return `${nestedActions}<button type="button" data-tree-rename="${escape(node.id)}">Renombrar</button><button type="button" data-tree-icon="${escape(node.id)}">Cambiar ícono</button><button type="button" data-tree-favorite="${escape(node.id)}">${node.favorite ? "Quitar favorito" : "Agregar a favoritos"}</button><button type="button" data-tree-duplicate="${escape(node.id)}">Duplicar</button><button type="button" data-tree-move="${escape(node.id)}">Mover a…</button><button type="button" data-tree-delete="${escape(node.id)}">Eliminar</button>`;
      };
      const nodeMenuMarkup = node => `<button class="tree-row-action" type="button" data-tree-menu-toggle="${escape(node.id)}" aria-label="Opciones de ${escape(node.title)}" aria-expanded="false" aria-haspopup="true">${dotsIcon}</button><div class="tree-inline-menu" data-tree-menu="${escape(node.id)}" hidden>${nodeMenuItems(node)}</div>`;
      const cardMenuMarkup = node => `<button class="tree-card-dots" type="button" data-tree-menu-toggle="${escape(node.id)}" aria-label="Opciones de ${escape(node.title)}" aria-haspopup="true" aria-expanded="false"><svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><circle cx="12" cy="5" r="1.9"/><circle cx="12" cy="12" r="1.9"/><circle cx="12" cy="19" r="1.9"/></svg></button><div class="tree-inline-menu" data-tree-menu="${escape(node.id)}" hidden>${nodeMenuItems(node)}<button type="button" data-card-bg="${escape(node.id)}">🖼 Cambiar fondo</button><button type="button" data-card-nobg="${escape(node.id)}">✕ Quitar fondo</button><button type="button" data-tree-open="${escape(node.id)}">↗ Abrir</button></div>`;
      const renderNode = (node, depth = 0, query = "") => {
        const children = getChildren(node.id);
        const shownChildren = query ? children.filter(child => nodeMatches(child, query) || descendantMatches(child.id, query)) : children;
        const isSelected = selectedId === node.id || activeFolderId() === node.id;
        const isOpen = node.expanded || Boolean(query);
        const addLabel = node.type === "folder" ? "Nueva materia" : node.type === "subject" ? "Nueva página" : "Nueva subpágina";
        const disclosure = children.length
          ? `<button class="tree-disclosure" type="button" data-tree-toggle="${escape(node.id)}" aria-label="${isOpen ? "Contraer" : "Expandir"} ${escape(node.title)}" aria-expanded="${isOpen}"><svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m6 3 5 5-5 5"/></svg></button>`
          : `<span class="tree-disclosure" aria-hidden="true"></span>`;
        return `<div class="tree-node-group" data-tree-group="${escape(node.id)}">
          <div class="tree-node-row ${isSelected ? "is-selected" : ""}" data-tree-row="${escape(node.id)}" data-tree-depth="${depth}" draggable="${node.id === PRIVATE_ROOT_ID ? "false" : "true"}" style="padding-left:${Math.min(depth * 12, 96)}px">
            ${disclosure}<button class="tree-node-main" type="button" data-tree-open="${escape(node.id)}" aria-current="${isSelected ? "page" : "false"}"><span class="tree-node-icon" aria-hidden="true">${nodeIcon(node)}</span><span class="tree-node-name">${escape(node.title)}</span></button>
            ${node.id === PRIVATE_ROOT_ID ? "" : `<button class="tree-row-action" type="button" data-tree-add="${escape(node.id)}" aria-label="${addLabel} en ${escape(node.title)}" title="${addLabel}">+</button>${nodeMenuMarkup(node)}`}
          </div>
          ${shownChildren.length && isOpen ? `<div class="tree-children">${shownChildren.map(child => renderNode(child, depth + 1, query)).join("")}</div>` : ""}
        </div>`;
      };
      const sidebarSearchRender = () => {
        const privateRoot = findNode(PRIVATE_ROOT_ID);
        const privateChildren = privateRoot ? getChildren(privateRoot.id) : [];
        const regularRoots = getChildren(null).filter(node => node.id !== PRIVATE_ROOT_ID);
        const roots = searchQuery
          ? regularRoots.filter(node => nodeMatches(node, searchQuery) || descendantMatches(node.id, searchQuery))
          : regularRoots;
        root.querySelector("[data-tree-regular]").innerHTML = roots.map(node => renderNode(node, 0, searchQuery)).join("") || `<div class="tree-empty">Todavía no hay carpetas.</div>`;
        const favoriteNodes = nodes.filter(node => node.id !== PRIVATE_ROOT_ID && node.favorite && (!searchQuery || nodeMatches(node, searchQuery))).sort((a, b) => a.title.localeCompare(b.title, "es"));
        root.querySelector("[data-tree-favorites]").innerHTML = favoriteNodes.map(node => `<div class="tree-node-row ${selectedId === node.id ? "is-selected" : ""}"><span class="tree-disclosure" aria-hidden="true"></span><button class="tree-node-main" type="button" data-tree-open="${escape(node.id)}"><span class="tree-node-icon">${nodeIcon(node)}</span><span class="tree-node-name">${escape(node.title)}</span></button><button class="tree-row-action" type="button" data-tree-favorite="${escape(node.id)}" aria-label="Quitar ${escape(node.title)} de favoritos" title="Quitar de favoritos">★</button><button class="tree-row-action" type="button" data-tree-delete="${escape(node.id)}" aria-label="Eliminar ${escape(node.title)}" title="Eliminar">🗑</button>${node.id === PRIVATE_ROOT_ID ? "" : nodeMenuMarkup(node)}</div>`).join("") || `<div class="tree-empty">Sin favoritos todavía.</div>`;
        root.querySelector("[data-tree-private]").innerHTML = privateChildren.map(node => renderNode(node, 0, searchQuery)).join("");
        root.querySelector("[data-tree-private-empty]").hidden = privateChildren.length > 0;
      };
      const childKind = parent => parent?.type === "folder" ? "subject" : "page";
      const createChild = (parentId, type, title) => {
        const parent = parentId ? findNode(parentId) : null;
        if (!isValidParent(type, parent)) return null;
        const siblings = getChildren(parentId);
        const node = createNode(type, title || (type === "folder" ? "Nueva carpeta" : type === "subject" ? "Nueva materia" : "Nueva página"), parentId, siblings.length);
        nodes.push(node);
        if (parent) parent.expanded = true;
        selectedId = node.id;
        searchOpen = false;
        searchQuery = "";
        searchFields.forEach(field => { field.value = ""; });
        persist();
        render();
        if (node.type === "page") view.querySelector(".tree-title-input")?.focus();
        return node;
      };
      const imageLayout = (w, pos, wrap) => wrap === "none"
        ? { cls: "", blockStyle: "", figStyle: `width:${w}%;margin-left:${(100 - w) * pos / 100}%` }
        : { cls: ` is-float${wrap === "right" ? " is-float-right" : ""}`, blockStyle: `width:${w}%`, figStyle: "" };
      const imageWrap = block => (block.wrap === "left" || block.wrap === "right" ? block.wrap : "none");
      const renderBlock = (block, index) => {
        if (block.type === "image") {
          const w = Math.max(10, Math.min(100, Number(block.w) || 60));
          const pos = Math.max(0, Math.min(100, Number.isFinite(Number(block.pos)) ? Number(block.pos) : 50));
          const layout = imageLayout(w, pos, imageWrap(block));
          const ratio = block.iw && block.ih ? `aspect-ratio:${block.iw}/${block.ih};` : "";
          const bid = escape(block.id);
          const btn = (act, label, text) => `<button type="button" data-tree-img-act="${act}" data-tree-img-block="${bid}" aria-label="${label}" title="${label}">${text}</button>`;
          return `<div class="tree-block${layout.cls}" data-tree-block="${bid}" data-type="image" style="${layout.blockStyle}"><div class="tree-block-tools"><button type="button" data-tree-block-add="${bid}" aria-label="Insertar bloque">+</button></div><figure class="tree-img${block.editing ? " is-editing" : ""}" data-tree-img-box="${bid}" tabindex="0" style="${layout.figStyle}" aria-label="Imagen. Doble clic o Enter para editarla"><img data-tree-img="${escape(block.imgId || "")}" alt="${escape(block.alt || "Imagen")}" draggable="false" style="${ratio}"><div class="tree-img-controls" role="toolbar" aria-label="Controles de imagen">${btn("left", "Mover a la izquierda", "←")}${btn("right", "Mover a la derecha", "→")}${btn("up", "Subir (entre los bloques de texto)", "↑")}${btn("down", "Bajar (entre los bloques de texto)", "↓")}<span class="tree-img-sep"></span>${btn("smaller", "Achicar", "−")}${btn("bigger", "Agrandar", "+")}<span class="tree-img-sep"></span>${btn("align-left", "Imagen a la izquierda, el texto la rodea", "◧")}${btn("align-center", "Imagen centrada en su propia fila (sin texto al lado)", "▣")}${btn("align-right", "Imagen a la derecha, el texto la rodea", "◨")}<span class="tree-img-sep"></span>${btn("delete", "Eliminar imagen", "🗑")}<span class="tree-img-sep"></span>${btn("apply", "Aplicar cambio", "✓ Aplicar cambio")}</div>${btn("edit", "Editar imagen", "✎").replace("<button ", "<button class=\"tree-img-edit\" ")}</figure></div>`;
        }
        if (block.type === "pagelink") {
          const target = findNode(block.pageId);
          if (!target) return "";
          return `<div class="tree-block" data-tree-block="${escape(block.id)}" data-type="pagelink"><div class="tree-block-tools"><button type="button" data-tree-block-add="${escape(block.id)}" aria-label="Insertar bloque">+</button></div><button class="tree-pagelink" type="button" data-tree-open="${escape(target.id)}"><span class="tree-pagelink-icon" aria-hidden="true">${nodeIcon(target)}</span><span class="tree-pagelink-title">${escape(target.title || "Sin título")}</span></button></div>`;
        }
        const type = slashCommands.some(command => command.id === block.type) || block.type === "legacy" ? block.type : "paragraph";
        const marker = type === "bullet" ? `<span class="tree-block-marker" aria-hidden="true">•</span>`
          : type === "numbered" ? `<span class="tree-block-marker" aria-hidden="true">${index + 1}.</span>`
          : type === "checklist" ? `<span class="tree-block-marker"><input type="checkbox" data-tree-check="${escape(block.id)}" aria-label="Marcar tarea" ${block.checked ? "checked" : ""}></span>`
          : type === "toggle" ? `<span class="tree-block-marker"><button type="button" data-tree-toggle-block="${escape(block.id)}" aria-expanded="${block.open !== false}">${block.open === false ? "▸" : "▾"}</button></span>`
          : `<span class="tree-block-marker" aria-hidden="true"></span>`;
        const editorTag = type === "divider" ? `<hr>` : `<div class="tree-block-content" data-type="${escape(type)}" data-tree-block-editor="${escape(block.id)}" data-placeholder="${index === 0 && !block.html ? "Escribe algo o presiona '/' para ver los comandos" : ""}" contenteditable="true" role="textbox" aria-label="${escape(slashCommands.find(command => command.id === type)?.label || "Bloque de texto")}" spellcheck="true">${block.html ? sanitizeHtml(block.html) : escape(block.text || "")}</div>`;
        const toggleBody = type === "toggle" ? `<div class="tree-toggle-body" data-tree-toggle-body="${escape(block.id)}" contenteditable="true" ${block.open === false ? "hidden" : ""}>${escape(block.body || "")}</div>` : "";
        return `<div class="tree-block" data-tree-block="${escape(block.id)}" data-type="${escape(type)}"><div class="tree-block-tools"><button type="button" data-tree-block-add="${escape(block.id)}" aria-label="Insertar bloque">+</button></div>${marker}${editorTag}${toggleBody}</div>`;
      };
      const breadcrumb = node => {
        const path = getAncestors(node.id);
        return `<nav class="tree-breadcrumb" aria-label="Ruta">${path.map((part, index) => index === path.length - 1
          ? `<strong>${escape(part.title)}</strong>`
          : `<button type="button" data-tree-open="${escape(part.id)}">${escape(part.title)}</button><span aria-hidden="true">/</span>`).join("")}</nav>`;
      };
      const childLinks = (node, list) => {
        const children = list || getChildren(node.id);
        if (!children.length) return `<div class="tree-empty">${node.type === "folder" ? "Esta carpeta todavía no tiene materias." : node.type === "subject" ? "Esta materia todavía no tiene páginas." : "Esta página todavía no tiene subpáginas."}</div>`;
        return `<div class="tree-child-list" aria-label="${node.type === "folder" ? "Materias y subcarpetas" : "Páginas"}">${children.map(child => `<div class="tree-child-item"><button class="tree-child-link" type="button" data-tree-open="${escape(child.id)}"><span aria-hidden="true">${nodeIcon(child)}</span><span>${escape(child.title)}</span><small>${child.type === "folder" ? "Carpeta" : child.type === "subject" ? "Materia" : "Página"}</small></button>${nodeMenuMarkup(child).replace('class="tree-row-action"', 'class="tree-row-action tree-child-menu-toggle"')}</div>`).join("")}</div>`;
      };
      const openNode = nodeId => {
        const node = findNode(nodeId);
        if (!node) return;
        selectedId = node.id;
        searchOpen = false;
        searchQuery = "";
        searchFields.forEach(field => { field.value = ""; });
        render();
        root.querySelector(".tree-main").scrollTo({ top: 0, behavior: "smooth" });
        setSidebarOpen(false);
      };
      const renderSearchResults = () => {
        const results = nodes.filter(node => node.id !== PRIVATE_ROOT_ID && nodeMatches(node, searchQuery)).slice(0, 60);
        return `<section class="tree-search-results" aria-label="Resultados de búsqueda">${results.length ? results.map(node => `<button class="tree-search-result" type="button" data-tree-open="${escape(node.id)}"><strong>${escape(node.title)}</strong><small>${getAncestors(node.id).map(item => item.title).map(escape).join(" / ")}</small></button>`).join("") : `<div class="tree-search-empty">No hay resultados para “${escape(searchQuery)}”.</div>`}</section>`;
      };
      const cardHue = text => { let h = 0; for (const ch of String(text)) h = (h * 31 + ch.charCodeAt(0)) % 360; return h; };
      const cardGrid = (node, children) => {
        const cards = children.map(child => {
          const hue = cardHue(child.title);
          const count = getChildren(child.id).length;
          const sub = child.type === "folder" ? `${count} ${count === 1 ? "elemento" : "elementos"}` : `${count} ${count === 1 ? "página" : "páginas"}`;
          return `<div class="tree-card" data-tree-card="${escape(child.id)}" data-card-name="${escape(normalize(child.title))}">
            <button class="tree-card-face" type="button" data-tree-open="${escape(child.id)}" style="--h:${hue}" aria-label="Abrir ${escape(child.title)}">
              <span class="tree-card-shade"></span>
              <span class="tree-card-body"><span class="tree-card-icon" aria-hidden="true">${nodeIcon(child)}</span><strong class="tree-card-title">${escape(child.title)}</strong><small class="tree-card-sub">${sub}</small></span>
            </button>
            ${cardMenuMarkup(child)}
          </div>`;
        }).join("");
        const addLabel = node.type === "folder" ? "Nueva materia" : "Nueva página";
        const add = `<div class="tree-card tree-card-add" data-card-keep><button class="tree-card-face" type="button" data-tree-add-child="${escape(node.id)}"><span class="tree-card-body"><span class="tree-card-plus" aria-hidden="true">+</span><strong class="tree-card-title">${addLabel}</strong></span></button></div>`;
        const filter = children.length ? `<label class="tree-card-filter"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><circle cx="11" cy="11" r="7"/><path d="m20 20-3.5-3.5"/></svg><input type="search" data-tree-card-filter placeholder="Filtrar ${node.type === "folder" ? "materias" : "páginas"} por nombre…" aria-label="Filtrar ${node.type === "folder" ? "materias" : "páginas"}" autocomplete="off"><span class="tree-card-count" data-tree-card-count>${children.length}</span></label>` : "";
        const itemLabel = node.type === "folder" ? "materias" : "páginas";
        const sizeControls = `<div class="tree-card-size-controls" aria-label="Tamaño de las ${itemLabel}"><span>Tamaño de ${itemLabel}</span><button type="button" data-tree-card-size-step="-1" aria-label="Reducir tamaño" ${cardSize <= CARD_SIZE_MIN ? "disabled" : ""}>−</button><output data-tree-card-size>${cardSize} px</output><button type="button" data-tree-card-size-step="1" aria-label="Aumentar tamaño" ${cardSize >= CARD_SIZE_MAX ? "disabled" : ""}>+</button></div>`;
        return `<div class="tree-card-toolbar">${filter}${sizeControls}</div><div class="tree-card-grid" data-tree-card-grid style="--tree-card-size:${cardSize}px">${cards}${add}</div><p class="tree-card-none" data-tree-card-none hidden>No hay ${itemLabel} con ese nombre.</p>`;
      };
      const paintCard = id => {
        const face = view.querySelector(`[data-tree-card="${CSS.escape(id)}"] .tree-card-face`);
        if (!face) return;
        const url = getPageStyle(id).bg ? bgUrls.get(id) : "";
        face.classList.toggle("has-bg", Boolean(url));
        face.style.backgroundImage = url ? `url("${url}")` : "";
      };
      const hydrateCards = () => {
        view.querySelectorAll("[data-tree-card]").forEach(card => {
          const id = card.dataset.treeCard;
          if (!getPageStyle(id).bg) return;
          if (bgUrls.has(id)) { paintCard(id); return; }
          getBackground(id).then(blob => {
            if (!blob) return;
            return blobToDataUrl(blob).then(dataUrl => { bgUrls.set(id, dataUrl); paintCard(id); });
          }).catch(error => console.error("No se pudo cargar el fondo:", error));
        });
      };
      const closeCardMenus = () => closeInlineMenus();
      const render = () => {
        sidebarSearchRender();
        const node = findNode(selectedId);
        if (!node) return;
        root.querySelector("[data-tree-mobile-title]").textContent = node.title;
        if (node.type === "page") {
          node.content = node.content.filter(block => block.type !== "pagelink" || findNode(block.pageId)?.parentId === node.id);
          if (!node.content.length || ["pagelink", "image"].includes(node.content[node.content.length - 1].type)) node.content.push(plainBlock(""));
        }
        let body = "";
        if (searchOpen && searchQuery) {
          body = `<div class="tree-breadcrumb"><button type="button" data-tree-home>Espacio</button><span aria-hidden="true">/</span><strong>Buscar</strong></div><div class="tree-heading"><span class="tree-heading-icon">⌕</span><h1 class="tree-title-input">Resultados para “${escape(searchQuery)}”</h1></div>${renderSearchResults()}`;
        } else if (node.type === "page") {
          const content = Array.isArray(node.content) ? node.content : [];
          body = `${breadcrumb(node)}<div class="tree-heading"><button class="tree-heading-icon tree-icon-button" type="button" data-tree-change-icon="${escape(node.id)}" aria-label="Cambiar ícono">${nodeIcon(node)}</button><input class="tree-title-input" data-tree-title="${escape(node.id)}" value="${escape(node.title)}" aria-label="Título de la página"></div><div class="tree-editor" data-tree-editor="${escape(node.id)}">${content.map(renderBlock).join("") || renderBlock(plainBlock(""), 0)}</div>`;
        } else {
          const label = node.type === "folder" ? "Carpeta" : "Materia";
          const children = getChildren(node.id);
          const createType = node.type === "folder" ? "subject" : "page";
          body = `${breadcrumb(node)}<div class="tree-heading">${node.icon ? `<button class="tree-heading-icon tree-icon-button" type="button" data-tree-change-icon="${escape(node.id)}" aria-label="Cambiar ícono">${nodeIcon(node)}</button>` : ""}<input class="tree-title-input" data-tree-title="${escape(node.id)}" value="${escape(node.title)}" aria-label="Nombre de ${label.toLocaleLowerCase("es")}"></div><p class="tree-view-subtitle">${label}${node.type === "folder" ? ` · ${children.filter(child => child.type === "subject").length} materias` : ` · ${children.length} páginas`}</p><div class="tree-view-actions"><button class="tree-content-action" type="button" data-tree-add-child="${escape(node.id)}"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M12 5v14M5 12h14"/></svg>${createType === "subject" ? "Nueva materia" : "Nueva página"}</button><button class="tree-content-action" type="button" data-tree-favorite="${escape(node.id)}">${node.favorite ? "★ En favoritos" : "☆ Favorito"}</button></div>${node.type === "folder" ? cardGrid(node, children) : childLinks(node)}`;
        }
        view.classList.toggle("is-grid", !searchOpen && node.type === "folder");
        view.innerHTML = body;
        syncToolbar(node);
        hydrateCards();
        hydrateImages();
      };
      /* ===== Personalización: tema, favorito, fondo (WebP) y panel de texto ===== */
      const THEME_KEY = "nido-tree-theme-v1";
      const STYLE_KEY = "nido-tree-page-style-v1";
      const treeMain = root.querySelector(".tree-main");
      const toolbar = root.querySelector("[data-tree-toolbar]");
      const settingsPanel = toolbar.querySelector("[data-tree-settings]");
      const favButton = toolbar.querySelector("[data-tool-fav]");
      const iconButton = toolbar.querySelector("[data-tool-icon]");
      const settingsToggle = toolbar.querySelector("[data-tool-settings]");
      const fileInput = toolbar.querySelector("[data-set-file]");
      const rangeA = toolbar.querySelector("[data-set-a]");
      const rangeB = toolbar.querySelector("[data-set-b]");
      const outA = toolbar.querySelector("[data-set-out-a]");
      const outB = toolbar.querySelector("[data-set-out-b]");
      const bgHint = toolbar.querySelector("[data-set-hint]");
      const fontSelect = toolbar.querySelector("[data-set-font]");
      const sizeRange = toolbar.querySelector("[data-set-size]");
      const outS = toolbar.querySelector("[data-set-out-s]");
      const TEXT_FONTS = {
        default: "",
        sans: '"Segoe UI", Inter, system-ui, Arial, sans-serif',
        serif: 'Georgia, "Times New Roman", serif',
        libro: '"Palatino Linotype", Palatino, "Book Antiqua", Garamond, Georgia, serif',
        redonda: '"Trebuchet MS", Verdana, sans-serif',
        mono: 'ui-monospace, "SF Mono", Consolas, "Courier New", monospace',
        manuscrita: '"Segoe Print", "Bradley Hand", "Comic Sans MS", cursive'
      };
      const HINT_DEFAULT = bgHint.textContent;
      const PRESETS = { clear: { a: 0, b: 0 }, glass: { a: 30, b: 8 }, solid: { a: 100, b: 0 } };
      const bgUrls = new Map();
      let pageStyles = {};
      try { pageStyles = JSON.parse(localStorage.getItem(STYLE_KEY)) || {}; } catch (error) { pageStyles = {}; }
      const getPageStyle = nodeId => ({ a: 0, b: 0, bg: false, f: "default", s: 16, fx: "none", fi: 70, fs: 1, tx: "none", ts: 1, tc: "#00e5ff", ...(pageStyles[nodeId] || {}) });
      const setPageStyle = (nodeId, patch) => {
        pageStyles[nodeId] = { ...getPageStyle(nodeId), ...patch };
        try { localStorage.setItem(STYLE_KEY, JSON.stringify(pageStyles)); } catch (error) { console.error("No se pudo guardar el estilo:", error); }
      };
      const openBgDb = () => new Promise((resolve, reject) => {
        const request = indexedDB.open("nido-tree-bg-v1", 1);
        request.onupgradeneeded = () => request.result.createObjectStore("bg");
        request.onsuccess = () => resolve(request.result);
        request.onerror = () => reject(request.error);
      });
      const bgRun = (mode, work) => openBgDb().then(db => new Promise((resolve, reject) => {
        const tx = db.transaction("bg", mode);
        const result = work(tx.objectStore("bg"));
        tx.oncomplete = () => resolve(result && "result" in result ? result.result : undefined);
        tx.onerror = () => reject(tx.error);
      }));
      const bgPut = (nodeId, blob) => bgRun("readwrite", store => store.put(blob, nodeId));
      const bgGet = nodeId => bgRun("readonly", store => store.get(nodeId));
      const bgDelete = nodeId => bgRun("readwrite", store => store.delete(nodeId));
      const dropPageStyle = nodeId => {
        delete pageStyles[nodeId];
        try { localStorage.setItem(STYLE_KEY, JSON.stringify(pageStyles)); } catch (error) { /* sin espacio */ }
        const url = bgUrls.get(nodeId);
        if (url) URL.revokeObjectURL(url);
        bgUrls.delete(nodeId);
        bgDelete(nodeId).catch(() => {});
        try { localStorage.removeItem(BG_LS_PREFIX + nodeId); } catch (error) { /* sin acceso */ }
      };
      const formatKb = bytes => bytes >= 1048576 ? `${(bytes / 1048576).toFixed(1)} MB` : `${Math.max(1, Math.round(bytes / 1024))} KB`;
      const MAX_SIDE = 1920;
      const isWebpFile = file => file.type === "image/webp" || /\.webp$/i.test(file.name || "");
      const isImageFile = file => file.type.startsWith("image/") || /\.(jpe?g|png|webp|gif|bmp|avif)$/i.test(file.name || "");
      const blobToDataUrl = blob => new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => resolve(reader.result);
        reader.onerror = () => reject(reader.error || new Error("No se pudo leer el archivo"));
        reader.readAsDataURL(blob);
      });
      const dataUrlToBlob = dataUrl => {
        const [head, body] = dataUrl.split(",");
        const mime = (head.match(/data:([^;]+)/) || [])[1] || "image/webp";
        const binary = atob(body);
        const bytes = new Uint8Array(binary.length);
        for (let i = 0; i < binary.length; i += 1) bytes[i] = binary.charCodeAt(i);
        return new Blob([bytes], { type: mime });
      };
      const loadImageElement = src => new Promise((resolve, reject) => {
        const img = new Image();
        img.onload = () => resolve(img);
        img.onerror = () => reject(new Error("el navegador no pudo decodificar la imagen"));
        img.src = src;
      });
      const decodeImage = async file => {
        if (window.createImageBitmap) {
          try { return await createImageBitmap(file, { imageOrientation: "from-image" }); } catch (error) { /* probar con <img> */ }
          try { return await createImageBitmap(file); } catch (error) { /* probar con <img> */ }
        }
        const objectUrl = URL.createObjectURL(file);
        try { return await loadImageElement(objectUrl); } catch (error) { /* probar con data URL */ } finally { URL.revokeObjectURL(objectUrl); }
        return loadImageElement(await blobToDataUrl(file));
      };
      const canvasToBlob = (canvas, type, quality) => new Promise(resolve => canvas.toBlob(resolve, type, quality));
      const imageToWebp = async (file, quality = 0.82) => {
        const source = await decodeImage(file);
        const srcWidth = source.naturalWidth || source.width;
        const srcHeight = source.naturalHeight || source.height;
        if (!srcWidth || !srcHeight) throw new Error("la imagen está vacía");
        const scale = Math.min(1, MAX_SIDE / Math.max(srcWidth, srcHeight));
        const canvas = document.createElement("canvas");
        canvas.width = Math.max(1, Math.round(srcWidth * scale));
        canvas.height = Math.max(1, Math.round(srcHeight * scale));
        canvas.getContext("2d").drawImage(source, 0, 0, canvas.width, canvas.height);
        if (source.close) source.close();
        let blob = await canvasToBlob(canvas, "image/webp", quality);
        if (!blob || blob.type !== "image/webp") blob = await canvasToBlob(canvas, "image/jpeg", Math.max(quality, 0.85));
        if (!blob) throw new Error("no se pudo generar la imagen convertida");
        return { blob, width: canvas.width, height: canvas.height };
      };
      const sniffFormat = async file => {
        try {
          const b = new Uint8Array(await file.slice(0, 16).arrayBuffer());
          const ascii = Array.from(b).map(n => (n >= 32 && n < 127 ? String.fromCharCode(n) : ".")).join("");
          if (ascii.startsWith("RIFF") && ascii.slice(8, 12) === "WEBP") return "WebP";
          if (b[0] === 0xff && b[1] === 0xd8) return "JPEG";
          if (b[0] === 0x89 && ascii.slice(1, 4) === "PNG") return "PNG";
          if (ascii.startsWith("GIF8")) return "GIF";
          if (ascii.slice(4, 8) === "ftyp") return `contenedor ${ascii.slice(8, 12).trim()} (AVIF/HEIC), no es WebP`;
          return `desconocido (${ascii.slice(0, 12)})`;
        } catch (error) { return "no se pudo leer"; }
      };
      const BG_LS_PREFIX = "nido-tree-bg-ls-";
      const putBackground = async (nodeId, blob) => {
        try { await bgPut(nodeId, blob); return "db"; } catch (error) { console.warn("IndexedDB no disponible:", error); }
        try { localStorage.setItem(BG_LS_PREFIX + nodeId, await blobToDataUrl(blob)); return "local"; } catch (error) { console.warn("localStorage lleno o bloqueado:", error); }
        return "memory";
      };
      const getBackground = async nodeId => {
        try { const blob = await bgGet(nodeId); if (blob) return blob; } catch (error) { /* probar localStorage */ }
        try { const data = localStorage.getItem(BG_LS_PREFIX + nodeId); if (data) return dataUrlToBlob(data); } catch (error) { /* sin fondo */ }
        return null;
      };
      const applyPageStyle = () => {
        const style = getPageStyle(selectedId);
        if (window.__treeApplyFx) window.__treeApplyFx(style);
        treeMain.style.setProperty("--panel-a", String(style.a / 100));
        treeMain.style.setProperty("--panel-blur", `${style.b}px`);
        const url = style.bg ? bgUrls.get(selectedId) : "";
        treeMain.style.backgroundImage = url ? `url("${url}")` : "";
        rangeA.value = String(style.a);
        rangeB.value = String(style.b);
        outA.textContent = `${style.a}%`;
        outB.textContent = `${style.b}px`;
        const fontCss = TEXT_FONTS[style.f] || "";
        if (fontCss) treeMain.style.setProperty("--page-font", fontCss); else treeMain.style.removeProperty("--page-font");
        if (Number(style.s) !== 16) treeMain.style.setProperty("--page-size", `${style.s}px`); else treeMain.style.removeProperty("--page-size");
        fontSelect.value = TEXT_FONTS[style.f] !== undefined ? style.f : "default";
        sizeRange.value = String(style.s);
        outS.textContent = `${style.s}px`;
        toolbar.querySelectorAll("[data-set-preset]").forEach(button => {
          const preset = PRESETS[button.dataset.setPreset];
          button.classList.toggle("is-active", preset.a === style.a && preset.b === style.b);
        });
        if (style.bg && !bgUrls.has(selectedId)) {
          const requested = selectedId;
          getBackground(requested).then(blob => {
            if (!blob) return;
            return blobToDataUrl(blob).then(dataUrl => {
              bgUrls.set(requested, dataUrl);
              if (selectedId === requested) applyPageStyle();
            });
          }).catch(error => console.error("No se pudo cargar el fondo:", error));
        }
      };
      const syncToolbar = node => {
        favButton.dataset.treeFavorite = node.id;
        iconButton.dataset.treeChangeIcon = node.id;
        favButton.classList.toggle("is-active", Boolean(node.favorite));
        favButton.setAttribute("aria-pressed", String(Boolean(node.favorite)));
        favButton.title = node.favorite ? "Quitar de favoritos" : "Agregar a favoritos";
        applyPageStyle();
      };
      const applyTheme = theme => {
        root.dataset.theme = theme;
        document.documentElement.style.colorScheme = theme;
        document.body.style.background = theme === "light" ? "#ffffff" : "#191919";
        try { localStorage.setItem(THEME_KEY, theme); } catch (error) { /* sin espacio */ }
      };
      let savedTheme = "dark";
      try { savedTheme = localStorage.getItem(THEME_KEY) === "light" ? "light" : "dark"; } catch (error) { savedTheme = "dark"; }
      applyTheme(savedTheme);
      const themeBtn = toolbar.querySelector("[data-tool-theme]");
      themeBtn.addEventListener("click", () => { if (!root.dataset.viewmode || root.dataset.viewmode === "default") applyTheme(root.dataset.theme === "light" ? "dark" : "light"); });
      const modesPanel = toolbar.querySelector("[data-tree-modes]");
      const modesToggle = toolbar.querySelector("[data-tool-modes]");
      const VIEWMODE_KEY = "nido-tree-viewmode-v1";
      const MODE_BODY_BG = { default: null, netflix: "#141414", enfoque: "#f4ecd8", biblioteca: "#13201b", pastel: "#fdf3f7", medianoche: "#0b1020", contraste: "#ffffff" };
      const MODE_SCHEME = { netflix: "dark", enfoque: "light", biblioteca: "dark", pastel: "light", medianoche: "dark", contraste: "light" };
      const applyViewMode = mode => {
        if (!(mode in MODE_BODY_BG)) mode = "default";
        if (mode === "default") delete root.dataset.viewmode; else root.dataset.viewmode = mode;
        if (mode === "default") {
          applyTheme(root.dataset.theme === "light" ? "light" : "dark");
        } else {
          document.documentElement.style.colorScheme = MODE_SCHEME[mode];
          document.body.style.background = MODE_BODY_BG[mode];
        }
        themeBtn.disabled = mode !== "default";
        themeBtn.title = mode === "default" ? "Modo claro / oscuro" : "El modo de vista define sus propios colores";
        modesPanel.querySelectorAll("[data-mode]").forEach(opt => {
          const on = opt.dataset.mode === mode;
          opt.classList.toggle("is-active", on);
          opt.setAttribute("aria-checked", String(on));
        });
        try { localStorage.setItem(VIEWMODE_KEY, mode); } catch (error) { /* sin espacio */ }
      };
      let savedMode = "default";
      try { savedMode = localStorage.getItem(VIEWMODE_KEY) || "default"; } catch (error) { savedMode = "default"; }
      applyViewMode(savedMode);
      const closeModes = () => { modesPanel.hidden = true; modesToggle.setAttribute("aria-expanded", "false"); };
      modesToggle.addEventListener("click", () => {
        const open = modesPanel.hidden;
        settingsPanel.hidden = true; settingsToggle.setAttribute("aria-expanded", "false");
        modesPanel.hidden = !open;
        modesToggle.setAttribute("aria-expanded", String(open));
      });
      modesPanel.querySelectorAll("[data-mode]").forEach(opt => opt.addEventListener("click", () => applyViewMode(opt.dataset.mode)));
      document.addEventListener("click", event => { if (!modesPanel.hidden && !toolbar.contains(event.target)) closeModes(); });
      document.addEventListener("keydown", event => { if (event.key === "Escape" && !modesPanel.hidden) { closeModes(); modesToggle.focus(); } });
      settingsToggle.addEventListener("click", () => {
        if (!modesPanel.hidden) closeModes();
        settingsPanel.hidden = !settingsPanel.hidden;
        settingsToggle.setAttribute("aria-expanded", String(!settingsPanel.hidden));
        if (!settingsPanel.hidden) bgHint.textContent = HINT_DEFAULT;
      });
      document.addEventListener("click", event => {
        if (!settingsPanel.hidden && !toolbar.contains(event.target)) { settingsPanel.hidden = true; settingsToggle.setAttribute("aria-expanded", "false"); }
      });
      document.addEventListener("keydown", event => {
        if (event.key === "Escape" && !settingsPanel.hidden) { settingsPanel.hidden = true; settingsToggle.setAttribute("aria-expanded", "false"); settingsToggle.focus(); }
      });
      fontSelect.addEventListener("change", () => { setPageStyle(selectedId, { f: fontSelect.value }); applyPageStyle(); });

      /* ===== Efectos de fondo y animaciones de texto ===== */
      const fxToggle = toolbar.querySelector("[data-tool-fx]");
      const txToggle = toolbar.querySelector("[data-tool-tx]");
      const fxPanel = toolbar.querySelector("[data-tree-fx]");
      const txPanel = toolbar.querySelector("[data-tree-tx]");
      const FX_IDS = ["none", "shine", "aurora", "rainbow", "party", "disco", "stars", "frame", "confetti"];
      const TX_IDS = ["none", "neon", "lights", "fade", "rainbow", "party", "gold", "wave", "glitch", "pulse"];
      const CONFETTI_COLORS = ["#ff3b6b", "#ff9d00", "#ffe600", "#36e08a", "#27c1ff", "#8a5cff", "#ff2bd6"];
      const fxLayer = document.createElement("div");
      fxLayer.className = "tree-fx-layer";
      fxLayer.setAttribute("aria-hidden", "true");
      treeMain.prepend(fxLayer);
      const placeFx = () => {
        const box = treeMain.getBoundingClientRect();
        Object.assign(fxLayer.style, { left: `${box.left}px`, top: `${box.top}px`, width: `${box.width}px`, height: `${box.height}px` });
      };
      if (window.ResizeObserver) new ResizeObserver(placeFx).observe(treeMain);
      window.addEventListener("resize", placeFx);
      const buildConfetti = on => {
        fxLayer.replaceChildren();
        if (!on) return;
        for (let n = 0; n < 40; n += 1) {
          const piece = document.createElement("i");
          piece.style.setProperty("--x", `${(Math.random() * 100).toFixed(1)}%`);
          piece.style.setProperty("--d", `${(4 + Math.random() * 5).toFixed(2)}s`);
          piece.style.setProperty("--w", `${(-Math.random() * 8).toFixed(2)}s`);
          piece.style.setProperty("--c", CONFETTI_COLORS[n % CONFETTI_COLORS.length]);
          fxLayer.append(piece);
        }
      };
      const fxOutI = fxPanel.querySelector("[data-fx-out-i]");
      const fxOutS = fxPanel.querySelector("[data-fx-out-s]");
      const fxRangeI = fxPanel.querySelector("[data-fx-i]");
      const fxRangeS = fxPanel.querySelector("[data-fx-s]");
      const txOutS = txPanel.querySelector("[data-tx-out-s]");
      const txRangeS = txPanel.querySelector("[data-tx-s]");
      const txColor = txPanel.querySelector("[data-tx-color]");
      const SLOW = 0.1; // Mantiene las animaciones lentas incluso al aumentar la velocidad.
      let lastFx = "none";
      const applyFx = style => {
        placeFx();
        const fx = FX_IDS.includes(style.fx) ? style.fx : "none";
        const tx = TX_IDS.includes(style.tx) ? style.tx : "none";
        const fi = Math.max(10, Math.min(100, Number(style.fi) || 70));
        const fs = Math.max(0.1, Math.min(3, Number(style.fs) || 1));
        const ts = Math.max(0.1, Math.min(3, Number(style.ts) || 1));
        const tc = /^#[0-9a-f]{6}$/i.test(style.tc || "") ? style.tc : "#00e5ff";
        fxLayer.dataset.fx = fx;
        fxLayer.style.setProperty("--fx-i", String(fi / 100));
        fxLayer.style.setProperty("--fx-s", String(fs * SLOW));
        if (fx !== lastFx) { buildConfetti(fx === "confetti"); lastFx = fx; }
        if (tx === "none") treeMain.removeAttribute("data-tx"); else treeMain.dataset.tx = tx;
        treeMain.style.setProperty("--tx-s", String(ts * SLOW));
        treeMain.style.setProperty("--tx-c", tc);
        fxPanel.querySelectorAll("[data-fx-opt]").forEach(button => {
          const on = button.dataset.fxOpt === fx;
          button.classList.toggle("is-active", on);
          button.setAttribute("aria-pressed", String(on));
        });
        txPanel.querySelectorAll("[data-tx-opt]").forEach(button => {
          const on = button.dataset.txOpt === tx;
          button.classList.toggle("is-active", on);
          button.setAttribute("aria-pressed", String(on));
        });
        fxRangeI.value = String(fi); fxOutI.textContent = `${fi}%`;
        fxRangeS.value = String(fs); fxOutS.textContent = `${fs.toFixed(1)}x · ciclo ≈ ${Math.round(2 / (fs * SLOW))} s`;
        txRangeS.value = String(ts); txOutS.textContent = `${ts.toFixed(1)}x · ciclo ≈ ${Math.round(1.5 / (ts * SLOW))} s`;
        txColor.value = tc;
        fxToggle.classList.toggle("is-active", fx !== "none");
        txToggle.classList.toggle("is-active", tx !== "none");
      };
      window.__treeApplyFx = applyFx;
      const closeFxPanels = () => {
        fxPanel.hidden = true; fxToggle.setAttribute("aria-expanded", "false");
        txPanel.hidden = true; txToggle.setAttribute("aria-expanded", "false");
      };
      const togglePanel = (panel, button) => {
        const open = panel.hidden;
        if (!modesPanel.hidden) closeModes();
        settingsPanel.hidden = true; settingsToggle.setAttribute("aria-expanded", "false");
        closeFxPanels();
        panel.hidden = !open;
        button.setAttribute("aria-expanded", String(open));
      };
      fxToggle.addEventListener("click", () => togglePanel(fxPanel, fxToggle));
      txToggle.addEventListener("click", () => togglePanel(txPanel, txToggle));
      modesToggle.addEventListener("click", closeFxPanels);
      settingsToggle.addEventListener("click", closeFxPanels);
      document.addEventListener("click", event => { if ((!fxPanel.hidden || !txPanel.hidden) && !toolbar.contains(event.target)) closeFxPanels(); });
      document.addEventListener("keydown", event => { if (event.key === "Escape" && (!fxPanel.hidden || !txPanel.hidden)) { closeFxPanels(); } });
      fxPanel.querySelectorAll("[data-fx-opt]").forEach(button => button.addEventListener("click", () => { setPageStyle(selectedId, { fx: button.dataset.fxOpt }); applyPageStyle(); }));
      txPanel.querySelectorAll("[data-tx-opt]").forEach(button => button.addEventListener("click", () => { setPageStyle(selectedId, { tx: button.dataset.txOpt }); applyPageStyle(); }));
      fxRangeI.addEventListener("input", () => { setPageStyle(selectedId, { fi: Number(fxRangeI.value) }); applyPageStyle(); });
      fxRangeS.addEventListener("input", () => { setPageStyle(selectedId, { fs: Number(fxRangeS.value) }); applyPageStyle(); });
      txRangeS.addEventListener("input", () => { setPageStyle(selectedId, { ts: Number(txRangeS.value) }); applyPageStyle(); });
      txColor.addEventListener("input", () => { setPageStyle(selectedId, { tc: txColor.value }); applyPageStyle(); });
      try { applyFx(getPageStyle(selectedId)); } catch (error) { /* se aplica en el primer render */ }
      sizeRange.addEventListener("input", () => { setPageStyle(selectedId, { s: Number(sizeRange.value) }); applyPageStyle(); });
      toolbar.querySelector("[data-set-text-reset]").addEventListener("click", () => { setPageStyle(selectedId, { f: "default", s: 16 }); applyPageStyle(); });
      rangeA.addEventListener("input", () => { setPageStyle(selectedId, { a: Number(rangeA.value) }); applyPageStyle(); });
      rangeB.addEventListener("input", () => { setPageStyle(selectedId, { b: Number(rangeB.value) }); applyPageStyle(); });
      toolbar.querySelectorAll("[data-set-preset]").forEach(button => button.addEventListener("click", () => {
        setPageStyle(selectedId, PRESETS[button.dataset.setPreset]);
        applyPageStyle();
      }));
      toolbar.querySelector("[data-set-pick]").addEventListener("click", () => fileInput.click());
      toolbar.querySelector("[data-set-remove]").addEventListener("click", () => {
        const target = selectedId;
        const url = bgUrls.get(target);
        if (url) URL.revokeObjectURL(url);
        bgUrls.delete(target);
        bgDelete(target).catch(() => {});
        try { localStorage.removeItem(BG_LS_PREFIX + target); } catch (error) { /* sin acceso */ }
        setPageStyle(target, { bg: false });
        applyPageStyle();
        bgHint.textContent = "Fondo quitado.";
      });
      fileInput.addEventListener("change", async () => {
        const file = fileInput.files && fileInput.files[0];
        fileInput.value = "";
        if (!file) return;
        if (!isImageFile(file)) { bgHint.textContent = "Ese archivo no es una imagen (usa JPG, PNG o WebP)."; return; }
        bgHint.textContent = "Convirtiendo a WebP…";
        const target = selectedId;
        try {
          let converted;
          try {
            converted = await imageToWebp(file);
          } catch (decodeError) {
            throw new Error(`no se pudo decodificar el archivo (formato real: ${await sniffFormat(file)}; ${decodeError.message})`);
          }
          const { blob, width, height } = converted;
          const dataUrl = await blobToDataUrl(blob);
          await loadImageElement(dataUrl).catch(() => { throw new Error("la imagen convertida no se puede mostrar como fondo en este navegador"); });
          const where = await putBackground(target, blob);
          bgUrls.set(target, dataUrl);
          const current = getPageStyle(target);
          setPageStyle(target, { bg: true, ...(current.a === 0 && current.b === 0 ? PRESETS.glass : {}) });
          if (selectedId === target) applyPageStyle();
          const detail = `${blob.type === "image/webp" ? "Convertida a WebP" : "Guardada como " + blob.type} · ${width}×${height} · ${formatKb(blob.size)} (original ${formatKb(file.size)}). Aplicada como fondo de esta página.`;
          bgHint.textContent = where === "memory" ? `${detail} Ojo: tu navegador bloqueó el almacenamiento, el fondo se perderá al recargar.` : detail;
        } catch (error) {
          console.error(error);
          bgHint.textContent = `No se pudo usar la imagen: ${error && error.message ? error.message : "error desconocido"}.`;
        }
      });
      const closeSlash = () => {
        menu.hidden = true;
        menu.replaceChildren();
        slashState = null;
        slashIndex = 0;
      };
      const filteredCommands = query => {
        const normalized = normalize(query.replace(/^\//, "").trim());
        return slashCommands.filter(command => !normalized || normalize(`${command.label} ${command.match}`).includes(normalized));
      };
      const showSlash = (blockEditor, query) => {
        slashState = { editor: blockEditor, query };
        const commands = filteredCommands(query);
        if (!commands.length) {
          closeSlash();
          return;
        }
        slashIndex = Math.min(slashIndex, commands.length - 1);
        menu.innerHTML = commands.map((command, index) => `<button type="button" role="option" data-tree-command="${command.id}" aria-selected="${index === slashIndex}"><span>${command.icon}</span><span>${command.label}</span><small>${command.id === "page" ? "/página" : ""}</small></button>`).join("");
        menu.hidden = false;
        const rect = blockEditor.getBoundingClientRect();
        const menuHeight = menu.getBoundingClientRect().height;
        menu.style.left = `${Math.max(8, Math.min(rect.left, innerWidth - 268))}px`;
        menu.style.top = `${Math.max(8, Math.min(rect.bottom + 4, innerHeight - menuHeight - 8))}px`;
      };
      const addInlinePage = (page, index) => {
        const child = createNode("page", "Nueva página", page.id, getChildren(page.id).length);
        nodes.push(child);
        page.expanded = true;
        const link = { id: id(), type: "pagelink", pageId: child.id, text: "", html: "" };
        page.content.splice(index, 0, link);
        return link;
      };
      const focusAfterLink = (page, link) => {
        const linkIndex = page.content.findIndex(item => item.id === link.id);
        let next = page.content[linkIndex + 1];
        if (!next) { next = plainBlock(""); page.content.push(next); }
        persist();
        render();
        view.querySelector(`[data-tree-block-editor="${CSS.escape(next.id)}"]`)?.focus();
      };
      /* ===== Imágenes dentro de la página ===== */
      const IMG_PREFIX = "img-";
      const imgUrls = new Map();
      const imageFileInput = toolbar.querySelector("[data-tree-image-file]");
      let imageTarget = null;
      let lastBlockId = "";
      const setImg = (img, url) => {
        img.onerror = () => {
          if (img.dataset.treeImgRetry) { img.alt = "Imagen no disponible"; return; }
          img.dataset.treeImgRetry = "1";
          const key = img.dataset.treeImg;
          imgUrls.delete(key);
          getBackground(IMG_PREFIX + key).then(blob => (blob ? blobToDataUrl(blob) : "")).then(fresh => {
            if (fresh) { imgUrls.set(key, fresh); img.src = fresh; } else img.alt = "Imagen no disponible";
          }).catch(() => { img.alt = "Imagen no disponible"; });
        };
        img.src = url;
      };
      const hydrateImages = () => {
        view.querySelectorAll("img[data-tree-img]").forEach(img => {
          const key = img.dataset.treeImg;
          if (!key) return;
          if (imgUrls.has(key)) { setImg(img, imgUrls.get(key)); return; }
          getBackground(IMG_PREFIX + key).then(blob => {
            if (!blob) { img.alt = "Imagen no disponible"; return; }
            return blobToDataUrl(blob).then(url => {
              imgUrls.set(key, url);
              view.querySelectorAll("img[data-tree-img]").forEach(other => { if (other.dataset.treeImg === key) setImg(other, url); });
            });
          }).catch(error => console.error("No se pudo cargar la imagen:", error));
        });
      };
      const imageStillUsed = key => nodes.some(node => (node.content || []).some(block => block.type === "image" && block.imgId === key));
      const releaseImage = key => {
        if (!key || imageStillUsed(key)) return;
        imgUrls.delete(key);
        bgDelete(IMG_PREFIX + key).catch(() => {});
        try { localStorage.removeItem(BG_LS_PREFIX + IMG_PREFIX + key); } catch (error) { /* sin acceso */ }
      };
      const requestImage = (pageId, afterBlockId, replaceBlockId) => {
        const page = findNode(pageId);
        if (!page || page.type !== "page") { window.alert("Abrí una página para insertar una imagen."); return; }
        imageTarget = { pageId, afterBlockId, replaceBlockId };
        imageFileInput.click();
      };
      const focusImage = blockId => {
        const box = view.querySelector(`[data-tree-img-box="${CSS.escape(blockId)}"]`);
        if (box) { box.focus({ preventScroll: true }); box.scrollIntoView({ block: "nearest" }); }
      };
      imageFileInput.addEventListener("change", async () => {
        const file = imageFileInput.files && imageFileInput.files[0];
        imageFileInput.value = "";
        const target = imageTarget;
        imageTarget = null;
        if (!file || !target) return;
        if (!isImageFile(file)) { window.alert("Ese archivo no es una imagen (usá JPG, PNG o WebP)."); return; }
        const page = findNode(target.pageId);
        if (!page) return;
        try {
          const { blob, width, height } = await imageToWebp(file, 0.92);
          const key = id();
          const where = await putBackground(IMG_PREFIX + key, blob);
          imgUrls.set(key, await blobToDataUrl(blob));
          if (where === "memory") window.alert("Tu navegador bloqueó el almacenamiento: la imagen se va a perder al recargar la página.");
          const block = { id: id(), type: "image", imgId: key, w: 40, pos: 50, iw: width, ih: height, wrap: "left", editing: true, alt: file.name.replace(/\.[^.]+$/, ""), text: "", html: "" };
          let at = page.content.findIndex(item => item.id === (target.replaceBlockId || target.afterBlockId));
          if (target.replaceBlockId && at >= 0) page.content.splice(at, 1, block);
          else page.content.splice(at < 0 ? page.content.length : at + 1, 0, block);
          const index = page.content.findIndex(item => item.id === block.id);
          const next = page.content[index + 1];
          if (!next || next.type === "image") page.content.splice(index + 1, 0, plainBlock(""));
          page.updatedAt = new Date().toISOString();
          persist();
          if (selectedId === page.id) { render(); focusImage(block.id); }
        } catch (error) {
          console.error(error);
          window.alert(`No se pudo usar la imagen: ${error && error.message ? error.message : "error desconocido"}.`);
        }
      });
      const imageAction = (blockId, act) => {
        const page = findNode(selectedId);
        if (!page || page.type !== "page") return;
        const index = page.content.findIndex(item => item.id === blockId);
        const block = page.content[index];
        if (!block || block.type !== "image") return;
        const w = Math.max(10, Math.min(100, Number(block.w) || 60));
        let pos = Math.max(0, Math.min(100, Number.isFinite(Number(block.pos)) ? Number(block.pos) : 50));
        let width = w;
        let wrap = imageWrap(block);
        if (act === "up" || act === "down") {
          const swap = index + (act === "up" ? -1 : 1);
          if (swap < 0 || swap >= page.content.length) return;
          [page.content[index], page.content[swap]] = [page.content[swap], page.content[index]];
          page.updatedAt = new Date().toISOString();
          persist();
          render();
          focusImage(blockId);
          return;
        }
        if (act === "apply" || act === "edit") {
          block.editing = act === "edit";
          page.updatedAt = new Date().toISOString();
          persist();
          render();
          focusImage(blockId);
          return;
        }
        if (act === "delete") {
          page.content.splice(index, 1);
          page.updatedAt = new Date().toISOString();
          releaseImage(block.imgId);
          persist();
          render();
          return;
        }
        if (act === "left") { if (wrap !== "none") wrap = "left"; else pos = Math.max(0, pos - 5); }
        else if (act === "right") { if (wrap !== "none") wrap = "right"; else pos = Math.min(100, pos + 5); }
        else if (act === "align-left") wrap = "left";
        else if (act === "align-right") wrap = "right";
        else if (act === "align-center") { wrap = "none"; pos = 50; }
        else if (act === "smaller") width = Math.max(10, w - 5);
        else if (act === "bigger") width = Math.min(100, w + 5);
        else return;
        block.w = width;
        block.pos = pos;
        block.wrap = wrap;
        page.updatedAt = new Date().toISOString();
        const wrapper = view.querySelector(`[data-tree-block="${CSS.escape(blockId)}"]`);
        const box = view.querySelector(`[data-tree-img-box="${CSS.escape(blockId)}"]`);
        const layout = imageLayout(width, pos, wrap);
        if (wrapper) { wrapper.className = `tree-block${layout.cls}`; wrapper.setAttribute("style", layout.blockStyle); }
        if (box) box.setAttribute("style", layout.figStyle);
        persist();
      };
      root.addEventListener("focusin", event => {
        const editor = event.target.closest && event.target.closest("[data-tree-block-editor]");
        if (editor) lastBlockId = editor.dataset.treeBlockEditor;
      });
      root.addEventListener("keydown", event => {
        const box = event.target.closest && event.target.closest("[data-tree-img-box]");
        if (!box || event.target !== box) return;
        const blockId = box.dataset.treeImgBox;
        if (!box.classList.contains("is-editing")) {
          if (event.key === "Enter") { event.preventDefault(); imageAction(blockId, "edit"); }
          return;
        }
        if (event.key === "Enter") { event.preventDefault(); imageAction(blockId, "apply"); return; }
        const keys = { ArrowLeft: "left", ArrowRight: "right", ArrowUp: "up", ArrowDown: "down", "+": "bigger", "=": "bigger", "-": "smaller", Delete: "delete", Backspace: "delete" };
        const act = keys[event.key];
        if (!act) return;
        event.preventDefault();
        imageAction(blockId, act);
      });
      root.addEventListener("dblclick", event => {
        const box = event.target.closest && event.target.closest("[data-tree-img-box]");
        if (box && !box.classList.contains("is-editing")) imageAction(box.dataset.treeImgBox, "edit");
      });
      toolbar.querySelector("[data-tool-image]").addEventListener("click", () => {
        const page = findNode(selectedId);
        if (!page || page.type !== "page") { window.alert("Abrí una página para insertar una imagen."); return; }
        const last = page.content.find(item => item.id === lastBlockId) ? lastBlockId : (page.content[page.content.length - 1]?.id || "");
        const lastBlock = page.content.find(item => item.id === last);
        const empty = lastBlock && lastBlock.type === "paragraph" && !(lastBlock.text || "").trim();
        requestImage(page.id, last, empty ? last : "");
      });
      const insertCommand = commandId => {
        if (!slashState) return;
        const state = slashState;
        const page = findNode(selectedId);
        const block = page?.content.find(item => item.id === state.editor.dataset.treeBlockEditor);
        if (!block || !page) { closeSlash(); return; }
        const currentText = state.editor.innerText || "";
        const slashMatch = currentText.match(/\/[^\s]*$/);
        if (commandId === "page") {
          if (slashMatch) state.editor.textContent = currentText.slice(0, slashMatch.index).trimEnd();
          const remaining = state.editor.innerText.replace(/\u00a0/g, " ").trim();
          closeSlash();
          const at = page.content.findIndex(item => item.id === block.id);
          let link;
          if (remaining) {
            block.text = state.editor.innerText;
            block.html = "";
            link = addInlinePage(page, at + 1);
          } else {
            page.content.splice(at, 1);
            link = addInlinePage(page, at);
          }
          page.updatedAt = new Date().toISOString();
          focusAfterLink(page, link);
          return;
        }
        if (commandId === "image") {
          if (slashMatch) state.editor.textContent = currentText.slice(0, slashMatch.index).trimEnd();
          const remaining = state.editor.innerText.replace(/\u00a0/g, " ").trim();
          block.text = state.editor.innerText || "";
          block.html = "";
          closeSlash();
          persist();
          requestImage(page.id, block.id, remaining ? "" : block.id);
          return;
        }
        const command = slashCommands.find(item => item.id === commandId);
        if (!command) return;
        if (slashMatch) state.editor.textContent = currentText.slice(0, slashMatch.index).trimEnd();
        block.type = command.id;
        block.text = state.editor.innerText || "";
        block.html = "";
        if (command.id === "divider") block.text = "";
        closeSlash();
        persist();
        render();
        view.querySelector(`[data-tree-block-editor="${CSS.escape(block.id)}"]`)?.focus();
      };
      const updateBlockFromEditor = editor => {
        const page = findNode(selectedId);
        if (!page || page.type !== "page") return;
        const block = page.content.find(item => item.id === editor.dataset.treeBlockEditor);
        if (!block) return;
        block.text = editor.innerText.replace(/\u00a0/g, " ");
        block.html = block.type === "paragraph" || block.type === "legacy" || /^h[1-3]$/.test(block.type)
          ? sanitizeHtml(editor.innerHTML)
          : "";
        page.updatedAt = new Date().toISOString();
        persist();
      };
      const splitBlock = editor => {
        const page = findNode(selectedId);
        const block = page?.content.find(item => item.id === editor.dataset.treeBlockEditor);
        if (!page || !block) return;
        const selection = getSelection();
        if (!selection?.rangeCount) return;
        const range = selection.getRangeAt(0);
        if (!editor.contains(range.startContainer)) return;
        const beforeRange = document.createRange();
        beforeRange.selectNodeContents(editor);
        beforeRange.setEnd(range.startContainer, range.startOffset);
        const before = beforeRange.toString();
        const after = (editor.innerText || "").slice(before.length);
        const oldIndex = page.content.findIndex(item => item.id === block.id);
        block.text = before;
        block.html = "";
        const next = { id: id(), type: "paragraph", text: after, html: "" };
        page.content.splice(oldIndex + 1, 0, next);
        persist();
        render();
        const nextEditor = view.querySelector(`[data-tree-block-editor="${CSS.escape(next.id)}"]`);
        nextEditor?.focus();
        if (nextEditor) {
          const caret = document.createRange();
          caret.selectNodeContents(nextEditor);
          caret.collapse(true);
          getSelection().removeAllRanges();
          getSelection().addRange(caret);
        }
      };
      const addBlockAfter = blockId => {
        const page = findNode(selectedId);
        if (!page || page.type !== "page") return;
        const index = page.content.findIndex(block => block.id === blockId);
        const block = { id: id(), type: "paragraph", text: "", html: "" };
        page.content.splice(index < 0 ? page.content.length : index + 1, 0, block);
        persist();
        render();
        view.querySelector(`[data-tree-block-editor="${CSS.escape(block.id)}"]`)?.focus();
      };
      /* Diálogos propios: window.prompt / confirm se bloquean en muchos navegadores (sobre todo con archivos locales). */
      const treeDialog = (title, bodyHtml, buttons) => new Promise(resolve => {
        document.querySelector(".tree-ask-dialog")?.remove();
        const dialog = document.createElement("div");
        dialog.className = "tree-icon-dialog tree-ask-dialog";
        dialog.innerHTML = `<form class="tree-icon-box" role="dialog" aria-modal="true" aria-label="${escape(title)}"><h3>${escape(title)}</h3>${bodyHtml}<div class="tree-icon-row" style="justify-content:flex-end">${buttons}</div></form>`;
        const finish = value => { dialog.remove(); document.removeEventListener("keydown", onKey, true); resolve(value); };
        const onKey = event => { if (event.key === "Escape") { event.stopPropagation(); finish(null); } };
        document.addEventListener("keydown", onKey, true);
        dialog.addEventListener("mousedown", event => { if (event.target === dialog) finish(null); });
        dialog.querySelector("[data-ask-cancel]").addEventListener("click", () => finish(null));
        dialog.querySelector("form").addEventListener("submit", event => {
          event.preventDefault();
          const field = dialog.querySelector("[data-ask-field]");
          finish(field ? field.value : true);
        });
        document.body.appendChild(dialog);
        const focusTarget = dialog.querySelector("[data-ask-field]") || dialog.querySelector("[data-ask-ok]");
        focusTarget.focus();
        if (focusTarget.select) focusTarget.select();
      });
      const treeAsk = (title, value) => treeDialog(title, `<label>Nombre<input type="text" data-ask-field maxlength="120" autocomplete="off" value="${escape(value)}"></label>`, `<button type="button" data-ask-cancel>Cancelar</button><button type="submit" class="is-primary" data-ask-ok>Guardar</button>`);
      const treeConfirm = (title, message, okLabel) => treeDialog(title, `<div class="tree-icon-hint" style="font-size:13px;color:#bbb">${escape(message)}</div>`, `<button type="button" data-ask-cancel>Cancelar</button><button type="submit" class="is-primary" data-ask-ok>${escape(okLabel || "Aceptar")}</button>`).then(result => result === true);
      const treeNotice = message => treeDialog("Aviso", `<div class="tree-icon-hint" style="font-size:13px;color:#bbb">${escape(message)}</div>`, `<button type="submit" class="is-primary" data-ask-ok>Entendido</button>`).catch(() => {});
      const removeNode = nodeId => {
        const node = findNode(nodeId);
        if (!node || node.id === PRIVATE_ROOT_ID) return;
        const descendants = getDescendants(nodeId);
        treeConfirm("Eliminar", descendants.length ? `¿Eliminar “${node.title}” y sus ${descendants.length} elementos dentro? Esta acción no se puede deshacer.` : `¿Eliminar “${node.title}”? Esta acción no se puede deshacer.`, "Eliminar").then(ok => {
          if (!ok || !findNode(nodeId)) return;
          const removedIds = new Set([nodeId, ...descendants.map(item => item.id)]);
          const removedImages = nodes.filter(item => removedIds.has(item.id)).flatMap(item => (item.content || []).filter(block => block.type === "image").map(block => block.imgId));
          nodes = nodes.filter(item => !removedIds.has(item.id));
          removedImages.forEach(releaseImage);
          removedIds.forEach(dropPageStyle);
          reindexChildren(node.parentId);
          if (removedIds.has(selectedId) || !findNode(selectedId)) selectedId = node.parentId && findNode(node.parentId) ? node.parentId : (getChildren(null).find(item => item.id !== PRIVATE_ROOT_ID)?.id || PRIVATE_ROOT_ID);
          persist();
          render();
        });
      };
      const duplicateNode = sourceId => {
        const source = findNode(sourceId);
        if (!source) return;
        const parent = source.parentId ? findNode(source.parentId) : null;
        if (!isValidParent(source.type, parent)) return;
        const cloneTree = (original, parentId, order) => {
          const cloned = { ...original, id: id(), parentId, order, title: original === source ? `${original.title} (copia)` : original.title, content: (original.content || []).map(block => ({ ...block, id: id() })), createdAt: new Date().toISOString(), updatedAt: new Date().toISOString() };
          nodes.push(cloned);
          getChildren(original.id).forEach((child, index) => cloneTree(child, cloned.id, index));
          return cloned;
        };
        const copy = cloneTree(source, source.parentId, getChildren(source.parentId).length);
        if (parent) parent.expanded = true;
        selectedId = copy.id;
        persist();
        render();
      };
      const moveByPrompt = nodeId => {
        const node = findNode(nodeId);
        if (!node) return;
        const blocked = new Set([nodeId, ...getDescendants(nodeId).map(child => child.id)]);
        const pathOf = item => getAncestors(item.id).map(part => part.title).join(" / ");
        const targets = nodes.filter(item => !blocked.has(item.id) && isValidParent(node.type, item));
        const rootOption = node.type === "folder" ? `<option value="">(Raíz del espacio)</option>` : "";
        const list = targets.map(item => `<option value="${escape(item.id)}"${item.id === node.parentId ? " selected" : ""}>${escape(item.id === PRIVATE_ROOT_ID ? "🔒 Privado" : pathOf(item))}</option>`).join("");
        if (!rootOption && !list) { treeNotice("No hay un destino válido para mover este elemento."); return; }
        treeDialog(`Mover “${node.title}” a…`, `<label>Destino<select data-ask-field style="padding:8px 10px;border:1px solid #444;border-radius:6px;background:#1c1c1c;color:#eee;font-size:14px">${rootOption}${list}</select></label>`, `<button type="button" data-ask-cancel>Cancelar</button><button type="submit" class="is-primary" data-ask-ok>Mover</button>`).then(destination => {
          if (destination === null) return;
          const parentId = destination === true || destination === "" ? null : destination;
          if (parentId === node.parentId || (parentId === null && !node.parentId)) return;
          if (!moveNode(nodeId, parentId, getChildren(parentId).length)) { treeNotice("Ese movimiento no está permitido por las reglas de jerarquía."); return; }
          const newParent = parentId ? findNode(parentId) : null;
          if (newParent) newParent.expanded = true;
          persist();
          render();
        });
      };
      const search = query => {
        searchQuery = query.trim();
        searchOpen = Boolean(searchQuery);
        render();
      };
      const setSidebarOpen = open => {
        const sidebar = root.querySelector(".tree-sidebar");
        root.classList.toggle("is-sidebar-open", open);
        sidebar.classList.toggle("is-open", open);
        root.querySelector(".tree-mobile-overlay").classList.toggle("is-open", open);
        root.querySelector("[data-tree-toggle-sidebar]")?.setAttribute("aria-expanded", String(open));
        root.querySelector("[data-tree-toggle-sidebar]")?.setAttribute("aria-label", open ? "Cerrar árbol de páginas" : "Abrir árbol de páginas");
      };
      window.addEventListener("resize", () => {
        if (!matchMedia("(max-width: 760px)").matches) setSidebarOpen(false);
      });

      let cardTarget = "";
      const cardFile = document.createElement("input");
      cardFile.type = "file"; cardFile.accept = "image/*"; cardFile.hidden = true;
      root.appendChild(cardFile);
      cardFile.addEventListener("change", async () => {
        const file = cardFile.files && cardFile.files[0];
        cardFile.value = "";
        const target = cardTarget;
        if (!file || !target) return;
        if (!isImageFile(file)) { alert("Ese archivo no es una imagen (usa JPG, PNG o WebP)."); return; }
        try {
          const { blob } = await imageToWebp(file);
          const dataUrl = await blobToDataUrl(blob);
          await putBackground(target, blob);
          bgUrls.set(target, dataUrl);
          const current = getPageStyle(target);
          setPageStyle(target, { bg: true, ...(current.a === 0 && current.b === 0 ? PRESETS.glass : {}) });
          paintCard(target);
          if (selectedId === target) applyPageStyle();
        } catch (error) {
          console.error(error);
          alert(`No se pudo usar la imagen: ${error && error.message ? error.message : "error desconocido"}.`);
        }
      });
      view.addEventListener("input", event => {
        const field = event.target.closest("[data-tree-card-filter]");
        if (!field) return;
        const query = normalize(field.value.trim());
        let shown = 0;
        view.querySelectorAll("[data-tree-card]").forEach(card => {
          const match = !query || card.dataset.cardName.includes(query);
          card.hidden = !match;
          if (match) shown += 1;
        });
        const keep = view.querySelector("[data-card-keep]");
        if (keep) keep.hidden = Boolean(query);
        const total = view.querySelectorAll("[data-tree-card]").length;
        view.querySelector("[data-tree-card-count]").textContent = query ? `${shown}/${total}` : String(total);
        view.querySelector("[data-tree-card-none]").hidden = shown > 0 || !query;
      });
      document.addEventListener("keydown", event => { if (event.key === "Escape") closeCardMenus(); });

      load();
      document.body.classList.add("tree-app-active");
      render();
      window.__treeApp = { get nodes() { return nodes; }, getChildren, getAncestors, getDescendants, moveNode };

      function closeInlineMenus() {
        root.querySelectorAll(".tree-inline-menu.is-floating").forEach(menuNode => menuNode.remove());
        root.querySelectorAll(".tree-inline-menu").forEach(menuNode => { menuNode.hidden = true; });
        root.querySelectorAll("[data-tree-menu-toggle]").forEach(button => button.setAttribute("aria-expanded", "false"));
        root.querySelectorAll(".tree-card.is-menu-open").forEach(card => card.classList.remove("is-menu-open"));
      }
      document.addEventListener("keydown", event => { if (event.key === "Escape") closeInlineMenus(); });
      root.addEventListener("click", event => {
        const action = event.target.closest("button");
        if (!action) return;
        if (action.closest(".tree-inline-menu")) closeInlineMenus();
        if (action.hasAttribute("data-tree-close-sidebar")) {
          setSidebarOpen(false);
          return;
        }
        if (action.hasAttribute("data-tree-toggle-sidebar")) {
          const isOpen = !root.querySelector(".tree-sidebar").classList.contains("is-open");
          setSidebarOpen(isOpen);
          action.setAttribute("aria-expanded", String(isOpen));
          return;
        }
        if (action.dataset.treeCardSizeStep) {
          const step = Number(action.dataset.treeCardSizeStep);
          cardSize = Math.max(CARD_SIZE_MIN, Math.min(CARD_SIZE_MAX, cardSize + step * CARD_SIZE_STEP));
          const cardGridElement = view.querySelector("[data-tree-card-grid]");
          if (cardGridElement) cardGridElement.style.setProperty("--tree-card-size", `${cardSize}px`);
          const sizeOutput = view.querySelector("[data-tree-card-size]");
          if (sizeOutput) sizeOutput.textContent = `${cardSize} px`;
          view.querySelectorAll("[data-tree-card-size-step]").forEach(button => {
            const buttonStep = Number(button.dataset.treeCardSizeStep);
            button.disabled = buttonStep < 0 ? cardSize <= CARD_SIZE_MIN : cardSize >= CARD_SIZE_MAX;
          });
          try {
            localStorage.setItem(CARD_SIZE_KEY, String(cardSize));
          } catch (error) {
            console.error("No se pudo guardar el tamaño de las tarjetas:", error);
            saveStatus.textContent = "No se pudo guardar el tamaño";
          }
          return;
        }
        const nestedAdd = action.dataset.treeNestedAdd;
        if (nestedAdd) {
          const parent = findNode(nestedAdd);
          const type = action.dataset.nestedType;
          closeCardMenus();
          if (parent && (type === "folder" || type === "subject" || type === "page")) createChild(parent.id, type);
          return;
        }
        if (action.dataset.treeImgAct) { event.preventDefault(); imageAction(action.dataset.treeImgBlock, action.dataset.treeImgAct); return; }
        if (action.dataset.cardBg) { cardTarget = action.dataset.cardBg; closeCardMenus(); cardFile.click(); return; }
        if (action.dataset.cardNobg) {
          const target = action.dataset.cardNobg;
          closeCardMenus();
          bgUrls.delete(target);
          bgDelete(target).catch(() => {});
          try { localStorage.removeItem(BG_LS_PREFIX + target); } catch (error) { /* sin acceso */ }
          setPageStyle(target, { bg: false });
          paintCard(target);
          if (selectedId === target) applyPageStyle();
          return;
        }
        if (action.hasAttribute("data-tree-home")) { selectedId = getChildren(null).find(node => node.id !== PRIVATE_ROOT_ID)?.id || PRIVATE_ROOT_ID; searchOpen = false; render(); return; }
        const command = action.dataset.treeCommand;
        if (command) { insertCommand(command); return; }
        const toggleBlock = action.dataset.treeToggleBlock;
        if (toggleBlock) {
          const page = findNode(selectedId);
          const block = page?.content.find(item => item.id === toggleBlock);
          if (block) { block.open = block.open === false; persist(); render(); }
          return;
        }
        const open = action.dataset.treeOpen;
        if (open) { openNode(open); return; }
        const disclosure = action.dataset.treeToggle;
        if (disclosure) {
          const node = findNode(disclosure);
          if (node) { node.expanded = !node.expanded; persist(); render(); }
          return;
        }
        const addChild = action.dataset.treeAdd ?? action.dataset.treeAddChild;
        if (addChild) {
          const parent = findNode(addChild);
          if (parent && parent.type === "page" && action.dataset.treeAddChild !== undefined) {
            focusAfterLink(parent, addInlinePage(parent, parent.content.length));
            return;
          }
          if (parent) createChild(parent.id, childKind(parent));
          return;
        }
        if (action.hasAttribute("data-tree-create-root")) { createChild(null, "folder"); return; }
        if (action.hasAttribute("data-tree-create-private")) { createChild(PRIVATE_ROOT_ID, "folder"); return; }
        if (action.hasAttribute("data-tree-create-current")) {
          const current = findNode(selectedId);
          if (current) createChild(current.id, childKind(current));
          else createChild(null, "folder");
          return;
        }
        const addBlockPage = action.dataset.treeAddBlock;
        if (addBlockPage) {
          const page = findNode(addBlockPage);
          if (page) {
            const block = plainBlock("");
            page.content.push(block);
            persist();
            render();
            view.querySelector(`[data-tree-block-editor="${CSS.escape(block.id)}"]`)?.focus();
          }
          return;
        }
        const addAfter = action.dataset.treeBlockAdd;
        if (addAfter) { addBlockAfter(addAfter); return; }
        const menuToggle = action.dataset.treeMenuToggle;
        if (menuToggle) {
          event.preventDefault();
          const source = action.nextElementSibling && action.nextElementSibling.classList.contains("tree-inline-menu") ? action.nextElementSibling : action.parentElement?.querySelector(".tree-inline-menu");
          const wasOpen = action.getAttribute("aria-expanded") === "true";
          closeInlineMenus();
          if (source && !wasOpen) {
            /* El menú se copia al contenedor principal: así ningún panel con transform, overflow o z-index lo recorta o lo deja sin clic. */
            const menuNode = source.cloneNode(true);
            menuNode.hidden = false;
            menuNode.classList.add("is-floating");
            root.appendChild(menuNode);
            const box = action.getBoundingClientRect();
            const menuWidth = menuNode.offsetWidth || 200;
            const menuHeight = menuNode.offsetHeight || 220;
            const left = Math.min(Math.max(8, box.right - menuWidth), window.innerWidth - menuWidth - 8);
            let top = box.bottom + 4;
            if (top + menuHeight > window.innerHeight - 8) top = Math.max(8, box.top - menuHeight - 4);
            menuNode.style.left = `${left}px`;
            menuNode.style.top = `${top}px`;
            action.setAttribute("aria-expanded", "true");
            action.closest(".tree-card")?.classList.add("is-menu-open");
          }
          return;
        }
        const rename = action.dataset.treeRename;
        if (rename) {
          const node = findNode(rename);
          if (node) treeAsk("Renombrar", node.title).then(title => {
            if (typeof title === "string" && title.trim() && findNode(rename)) { node.title = title.trim(); node.updatedAt = new Date().toISOString(); persist(); render(); }
          });
          return;
        }
        const changeIcon = action.dataset.treeIcon || action.dataset.treeChangeIcon;
        if (changeIcon) {
          const node = findNode(changeIcon);
          if (node) openIconDialog(node);
          return;
        }
        const favorite = action.dataset.treeFavorite;
        if (favorite) {
          const node = findNode(favorite);
          if (node) { node.favorite = !node.favorite; persist(); render(); }
          return;
        }
        const duplicate = action.dataset.treeDuplicate;
        if (duplicate) { duplicateNode(duplicate); return; }
        const move = action.dataset.treeMove;
        if (move) { moveByPrompt(move); return; }
        const remove = action.dataset.treeDelete;
        if (remove) { removeNode(remove); return; }
      });
      root.addEventListener("input", event => {
        const editor = event.target.closest("[data-tree-block-editor]");
        if (editor) {
          updateBlockFromEditor(editor);
          const value = editor.innerText || "";
          if (/^\/[^\s]*$/.test(value)) showSlash(editor, value);
          else if (slashState?.editor === editor) closeSlash();
          return;
        }
        const toggleBody = event.target.closest("[data-tree-toggle-body]");
        if (toggleBody) {
          const page = findNode(selectedId);
          const block = page?.content.find(item => item.id === toggleBody.dataset.treeToggleBody);
          if (block) { block.body = toggleBody.innerText; persist(); }
          return;
        }
        const searchField = event.target.closest(".tree-search");
        if (searchField) {
          searchFields.forEach(field => { if (field !== searchField) field.value = searchField.value; });
          search(searchField.value);
          return;
        }
      });
      root.addEventListener("change", event => {
        const checkbox = event.target.closest("[data-tree-check]");
        if (checkbox) {
          const page = findNode(selectedId);
          const block = page?.content.find(item => item.id === checkbox.dataset.treeCheck);
          if (block) { block.checked = checkbox.checked; persist(); }
          return;
        }
        const title = event.target.closest("[data-tree-title]");
        if (title) {
          const node = findNode(title.dataset.treeTitle);
          if (node && title.value.trim() && node.title !== title.value.trim()) { node.title = title.value.trim(); node.updatedAt = new Date().toISOString(); persist(); render(); }
        }
      });
      root.addEventListener("input", event => {
        const title = event.target.closest("[data-tree-title]");
        if (!title) return;
        const node = findNode(title.dataset.treeTitle);
        const value = title.value.trim();
        if (!node || !value) return;
        node.title = value;
        node.updatedAt = new Date().toISOString();
        persist();
        sidebarSearchRender();
        const crumb = view.querySelector(".tree-breadcrumb strong");
        if (crumb) crumb.textContent = value;
        const mobileTitle = root.querySelector("[data-tree-mobile-title]");
        if (mobileTitle) mobileTitle.textContent = value;
      });
      const caretAtStart = editor => {
        const selection = getSelection();
        if (!selection || !selection.rangeCount || !selection.isCollapsed) return false;
        const range = selection.getRangeAt(0);
        if (!editor.contains(range.startContainer)) return false;
        const before = document.createRange();
        before.selectNodeContents(editor);
        before.setEnd(range.startContainer, range.startOffset);
        return before.toString().length === 0;
      };
      const placeCaretAt = (editor, offset) => {
        editor.focus();
        const range = document.createRange();
        const walker = document.createTreeWalker(editor, NodeFilter.SHOW_TEXT);
        let remaining = offset;
        let node;
        let placed = false;
        while (offset != null && (node = walker.nextNode())) {
          if (remaining <= node.length) { range.setStart(node, remaining); range.collapse(true); placed = true; break; }
          remaining -= node.length;
        }
        if (!placed) { range.selectNodeContents(editor); range.collapse(offset == null ? false : true); }
        const selection = getSelection();
        selection.removeAllRanges();
        selection.addRange(range);
      };
      const backspaceAtStart = (editor, event) => {
        const page = findNode(selectedId);
        if (!page || page.type !== "page") return;
        const index = page.content.findIndex(item => item.id === editor.dataset.treeBlockEditor);
        if (index < 0) return;
        const block = page.content[index];
        const focusBlock = (blockId, offset) => {
          const target = view.querySelector(`[data-tree-block-editor="${CSS.escape(blockId)}"]`);
          if (target) placeCaretAt(target, offset);
        };
        if (block.type !== "paragraph" && block.type !== "legacy") {
          event.preventDefault();
          block.type = "paragraph";
          updateBlockFromEditor(editor);
          persist();
          render();
          focusBlock(block.id, 0);
          return;
        }
        if (index === 0) return;
        event.preventDefault();
        updateBlockFromEditor(editor);
        const previous = page.content[index - 1];
        const currentText = block.text || "";
        if (previous.type === "divider") {
          page.content.splice(index - 1, 1);
          persist();
          render();
          focusBlock(block.id, 0);
          return;
        }
        if (previous.type === "image") {
          focusImage(previous.id);
          return;
        }
        if (previous.type === "pagelink") {
          if (!currentText) view.querySelector(`[data-tree-block="${CSS.escape(previous.id)}"] .tree-pagelink`)?.focus();
          return;
        }
        const previousEditor = view.querySelector(`[data-tree-block-editor="${CSS.escape(previous.id)}"]`);
        const joinOffset = previousEditor ? previousEditor.textContent.length : (previous.text || "").length;
        if (currentText) {
          const htmlOf = item => item.html || escape(item.text || "");
          const keepsHtml = /^(paragraph|legacy|h1|h2|h3)$/.test(previous.type);
          const mergedHtml = keepsHtml ? sanitizeHtml(htmlOf(previous) + htmlOf(block)) : "";
          previous.text = (previous.text || "") + currentText;
          previous.html = mergedHtml;
        }
        page.content.splice(index, 1);
        page.updatedAt = new Date().toISOString();
        persist();
        render();
        focusBlock(previous.id, joinOffset);
      };
      root.addEventListener("input", event => {
        const blockEditor = event.target.closest && event.target.closest("[data-tree-block-editor]");
        if (blockEditor && !blockEditor.textContent && blockEditor.firstChild) blockEditor.innerHTML = "";
      }, true);
      root.addEventListener("paste", event => {
        const editor = event.target.closest && event.target.closest("[data-tree-block-editor], [data-tree-toggle-body]");
        if (!editor) return;
        event.preventDefault();
        const raw = (event.clipboardData && event.clipboardData.getData("text/plain")) || "";
        const lines = raw.replace(/\r\n?/g, "\n").replace(/\u00a0/g, " ").split("\n").map(line => line.replace(/\s+$/, "")).filter(line => line.trim());
        if (!lines.length) return;
        const isBlock = editor.hasAttribute("data-tree-block-editor");
        if (lines.length === 1 || !isBlock) { document.execCommand("insertText", false, isBlock ? lines[0] : lines.join("\n")); return; }
        const page = findNode(selectedId);
        const block = page && page.content.find(item => item.id === editor.dataset.treeBlockEditor);
        const selection = getSelection();
        if (!page || !block || !selection || !selection.rangeCount) return;
        const range = selection.getRangeAt(0);
        if (!editor.contains(range.startContainer)) return;
        const beforeRange = document.createRange();
        beforeRange.selectNodeContents(editor);
        beforeRange.setEnd(range.startContainer, range.startOffset);
        const afterRange = document.createRange();
        afterRange.selectNodeContents(editor);
        afterRange.setStart(range.endContainer, range.endOffset);
        const before = beforeRange.toString().replace(/\u00a0/g, " ");
        const after = afterRange.toString().replace(/\u00a0/g, " ");
        const keepType = ["bullet", "numbered", "checklist"].includes(block.type) ? block.type : "paragraph";
        const index = page.content.findIndex(item => item.id === block.id);
        block.text = before + lines[0];
        block.html = "";
        const created = lines.slice(1).map((line, position, all) => ({ ...plainBlock(position === all.length - 1 ? line + after : line), type: keepType }));
        page.content.splice(index + 1, 0, ...created);
        page.updatedAt = new Date().toISOString();
        persist();
        render();
        const last = created[created.length - 1];
        const lastEditor = view.querySelector(`[data-tree-block-editor="${CSS.escape(last.id)}"]`);
        if (lastEditor) { placeCaretAt(lastEditor, lines[lines.length - 1].length); lastEditor.scrollIntoView({ block: "nearest" }); }
      });
      root.addEventListener("keydown", event => {
        if ((event.ctrlKey || event.metaKey) && event.key.toLocaleLowerCase() === "k") {
          event.preventDefault();
          root.querySelector(".tree-search").focus();
          return;
        }
        const editor = event.target.closest("[data-tree-block-editor]");
        if (editor && slashState?.editor === editor && !menu.hidden) {
          const commands = filteredCommands(slashState.query);
          if (event.key === "ArrowDown" || event.key === "ArrowUp") {
            event.preventDefault();
            slashIndex = (slashIndex + (event.key === "ArrowDown" ? 1 : commands.length - 1)) % commands.length;
            showSlash(editor, slashState.query);
          } else if (event.key === "Enter") {
            event.preventDefault();
            insertCommand(commands[slashIndex]?.id);
            return;
          } else if (event.key === "Escape") {
            event.preventDefault();
            closeSlash();
            return;
          }
        }
        if (editor && event.key === "Enter" && !event.shiftKey) {
          event.preventDefault();
          splitBlock(editor);
        }
        if (editor && event.key === "Backspace" && !event.ctrlKey && !event.metaKey && !event.altKey && caretAtStart(editor)) {
          backspaceAtStart(editor, event);
        }
        if (event.key === "Escape") {
          closeSlash();
          setSidebarOpen(false);
        }
      });
      root.addEventListener("dragstart", event => {
        const row = event.target.closest("[data-tree-row]");
        if (!row || row.getAttribute("draggable") !== "true") return;
        draggingId = row.dataset.treeRow;
        event.dataTransfer.setData("text/plain", draggingId);
        event.dataTransfer.effectAllowed = "move";
      });
      root.addEventListener("dragover", event => {
        const row = event.target.closest("[data-tree-row]");
        if (!row || !draggingId) return;
        event.preventDefault();
        row.classList.add("is-drop-target");
      });
      root.addEventListener("dragleave", event => event.target.closest("[data-tree-row]")?.classList.remove("is-drop-target"));
      root.addEventListener("drop", event => {
        const row = event.target.closest("[data-tree-row]");
        if (!row || !draggingId) return;
        event.preventDefault();
        row.classList.remove("is-drop-target");
        const target = findNode(row.dataset.treeRow);
        const dragged = findNode(draggingId);
        if (!target || !dragged || target.id === dragged.id) return;
        const bounds = row.getBoundingClientRect();
        const fraction = (event.clientY - bounds.top) / bounds.height;
        let parentId;
        let index;
        if (fraction < .28 || fraction > .72) {
          parentId = target.parentId;
          index = target.order + (fraction > .72 ? 1 : 0);
        } else {
          parentId = target.id;
          index = getChildren(target.id).length;
        }
        if (!moveNode(dragged.id, parentId, index)) {
          saveStatus.textContent = "Movimiento no permitido";
          window.setTimeout(() => { saveStatus.textContent = "Guardado"; }, 1800);
        }
        draggingId = "";
      });
      document.addEventListener("click", event => {
        if (root.contains(event.target)) {
          const menuToggle = event.target.closest("[data-tree-menu-toggle]");
          if (!menuToggle && !event.target.closest(".tree-inline-menu")) closeInlineMenus();
          if (!event.target.closest(".tree-slash-menu") && !event.target.closest("[data-tree-block-editor]")) closeSlash();
        }
      });
      window.addEventListener("resize", closeSlash);
      window.addEventListener("scroll", event => {
        closeSlash();
        if (event.target.closest?.(".tree-inline-menu")) return;
        closeInlineMenus();
      }, true);
    })();
  </script>
<!-- ===== Mini reproductor de audio (YouTube IFrame API) ===== -->
<style>
  #ytm { position: fixed; right: 16px; bottom: calc(16px + env(safe-area-inset-bottom, 0px)); z-index: 300; display: flex; flex-direction: column; align-items: flex-end; color: #f2f2f0; font: 13px/1.4 Inter, system-ui, -apple-system, "Segoe UI", sans-serif; }
  #ytm *, #ytm *::before, #ytm *::after { box-sizing: border-box; }
  #ytm button, #ytm input { font: inherit; color: inherit; }
  #ytm button { cursor: pointer; }
  #ytm [hidden] { display: none !important; }
  #ytm :focus-visible { outline: 2px solid #8a9cff; outline-offset: 2px; }

  /* El video queda fuera de pantalla (solo audio) salvo que se active "Video" */
  #ytm .ytm-video { position: fixed; left: -10000px; top: 0; width: 320px; height: 180px; overflow: hidden; border-radius: 10px; background: #000; }
  #ytm .ytm-video iframe { display: block; width: 100%; height: 100%; border: 0; }
  #ytm.ytm-open.ytm-video-on .ytm-video { position: static; width: min(320px, calc(100vw - 24px)); height: auto; aspect-ratio: 16 / 9; margin-bottom: 8px; box-shadow: 0 8px 22px rgb(0 0 0 / 35%); }

  /* Cápsula minimizada */
  #ytm .ytm-pill { display: flex; align-items: center; gap: 2px; padding: 4px; border-radius: 999px; background: rgb(28 28 30 / 92%); box-shadow: 0 4px 14px rgb(0 0 0 / 28%); backdrop-filter: blur(8px); opacity: .72; transition: opacity .15s ease; }
  #ytm .ytm-pill:hover, #ytm .ytm-pill:focus-within { opacity: 1; }
  #ytm.ytm-playing .ytm-pill { opacity: .95; }
  #ytm .ytm-round { display: inline-grid; width: 34px; height: 34px; flex: 0 0 auto; place-items: center; padding: 0; border: 0; border-radius: 50%; background: transparent; font-size: 15px; line-height: 1; }
  #ytm .ytm-round:hover { background: rgb(255 255 255 / 14%); }
  #ytm .ytm-pill-title { max-width: 130px; padding: 0 8px 0 2px; overflow: hidden; border: 0; background: transparent; font-size: 12px; text-align: left; text-overflow: ellipsis; white-space: nowrap; }

  /* Panel */
  #ytm .ytm-panel { width: min(320px, calc(100vw - 24px)); padding: 12px; border: 1px solid rgb(255 255 255 / 10%); border-radius: 14px; background: rgb(28 28 30 / 97%); box-shadow: 0 12px 32px rgb(0 0 0 / 40%); }
  #ytm .ytm-head { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
  #ytm .ytm-head strong { font-size: 12px; letter-spacing: .03em; }
  #ytm .ytm-head-actions { display: flex; gap: 2px; }
  #ytm .ytm-small { height: 26px; padding: 0 8px; border: 0; border-radius: 6px; background: transparent; color: #b8b8be; font-size: 12px; }
  #ytm .ytm-small:hover, #ytm .ytm-small[aria-pressed="true"] { background: rgb(255 255 255 / 12%); color: #fff; }
  #ytm .ytm-search { display: flex; gap: 6px; }
  #ytm .ytm-input { min-width: 0; flex: 1; height: 34px; padding: 0 10px; border: 1px solid #3a3a3f; border-radius: 8px; outline: 0; background: #262629; font-size: 12px; }
  #ytm .ytm-input:focus { border-color: #8a9cff; }
  #ytm .ytm-go { height: 34px; padding: 0 12px; border: 0; border-radius: 8px; background: #f2f2f0; color: #111; font-weight: 600; font-size: 12px; }
  #ytm .ytm-msg { margin: 8px 0 0; color: #b8b8be; font-size: 11.5px; }
  #ytm .ytm-msg.is-error { color: #f0a0a0; }
  #ytm .ytm-results { display: grid; gap: 2px; max-height: 190px; margin-top: 8px; overflow-y: auto; }
  #ytm .ytm-result { display: flex; align-items: center; gap: 8px; padding: 4px; border: 0; border-radius: 7px; background: transparent; text-align: left; }
  #ytm .ytm-result:hover { background: rgb(255 255 255 / 10%); }
  #ytm .ytm-result img { width: 60px; height: 34px; flex: 0 0 auto; border-radius: 4px; object-fit: cover; background: #111; }
  #ytm .ytm-result span { display: -webkit-box; overflow: hidden; font-size: 12px; -webkit-line-clamp: 2; -webkit-box-orient: vertical; }
  #ytm .ytm-now { margin: 12px 0 6px; overflow: hidden; font-size: 12.5px; font-weight: 600; text-overflow: ellipsis; white-space: nowrap; }
  #ytm .ytm-seek { display: flex; align-items: center; gap: 8px; color: #a0a0a6; font-size: 11px; font-variant-numeric: tabular-nums; }
  #ytm .ytm-seek input { min-width: 0; flex: 1; accent-color: #8a9cff; }
  #ytm .ytm-controls { display: flex; align-items: center; justify-content: space-between; margin-top: 8px; }
  #ytm .ytm-main { width: 42px; height: 42px; background: #f2f2f0; color: #111; font-size: 17px; }
  #ytm .ytm-main:hover { background: #fff; }
  #ytm .ytm-skip { width: auto; min-width: 40px; padding: 0 6px; border-radius: 8px; font-size: 12px; }
  #ytm .ytm-speed { display: flex; align-items: center; gap: 2px; }
  #ytm .ytm-speed output { min-width: 42px; text-align: center; font-size: 12px; font-variant-numeric: tabular-nums; }
  #ytm .ytm-speed .ytm-round { width: 28px; height: 28px; border-radius: 6px; font-size: 16px; }
  #ytm .ytm-settings { margin-top: 10px; padding-top: 10px; border-top: 1px solid #3a3a3f; }
  #ytm .ytm-settings label { display: block; margin-bottom: 6px; color: #b8b8be; font-size: 11.5px; }
  @media (prefers-reduced-motion: reduce) { #ytm .ytm-pill { transition: none; } }
</style>

<div id="ytm" role="region" aria-label="Reproductor de audio">
  <div class="ytm-video"><div id="ytm-player"></div></div>

  <div class="ytm-pill" data-ytm-pill>
    <button class="ytm-round" type="button" data-ytm-open aria-label="Abrir reproductor" title="Abrir reproductor">♪</button>
    <button class="ytm-round" type="button" data-ytm-toggle aria-label="Reproducir" title="Reproducir / pausar">▶</button>
    <button class="ytm-pill-title" type="button" data-ytm-pill-title hidden></button>
  </div>

  <div class="ytm-panel" data-ytm-panel hidden>
    <div class="ytm-head">
      <strong>♪ Audio de YouTube</strong>
      <div class="ytm-head-actions">
        <button class="ytm-small" type="button" data-ytm-video aria-pressed="false" title="Mostrar u ocultar el video">Video</button>
        <button class="ytm-small" type="button" data-ytm-gear aria-label="Configuración" title="Configurar búsqueda">⚙</button>
        <button class="ytm-small" type="button" data-ytm-close aria-label="Minimizar" title="Minimizar">▾</button>
      </div>
    </div>

    <form class="ytm-search" data-ytm-form autocomplete="off">
      <input class="ytm-input" type="text" data-ytm-input placeholder="Pegá un link o buscá un video…" aria-label="Link o búsqueda">
      <button class="ytm-go" type="submit">Ir</button>
    </form>
    <p class="ytm-msg is-error" data-ytm-file hidden>Abriste la página como archivo (file://). Así YouTube suele bloquear el reproductor (error 153) y las claves con restricción de sitio fallan. Abrila desde http://localhost o desde un sitio publicado.</p>
    <p class="ytm-msg" data-ytm-msg hidden></p>
    <div class="ytm-results" data-ytm-results hidden></div>

    <p class="ytm-now" data-ytm-now>Nada reproduciéndose</p>
    <div class="ytm-seek">
      <span data-ytm-cur>0:00</span>
      <input type="range" min="0" max="0" step="1" value="0" data-ytm-seek aria-label="Posición">
      <span data-ytm-dur>0:00</span>
    </div>

    <div class="ytm-controls">
      <button class="ytm-round ytm-skip" type="button" data-ytm-back title="Retroceder 10 segundos">−10s</button>
      <button class="ytm-round ytm-main" type="button" data-ytm-toggle aria-label="Reproducir" title="Reproducir / pausar">▶</button>
      <button class="ytm-round ytm-skip" type="button" data-ytm-fwd title="Adelantar 10 segundos">+10s</button>
      <div class="ytm-speed" title="Velocidad">
        <button class="ytm-round" type="button" data-ytm-slower aria-label="Más lento">−</button>
        <output data-ytm-rate>1×</output>
        <button class="ytm-round" type="button" data-ytm-faster aria-label="Más rápido">+</button>
      </div>
    </div>

    <div class="ytm-settings" data-ytm-settings hidden>
      <label for="ytm-key">Clave de YouTube Data API v3 (solo para buscar por nombre; pegar links funciona sin clave)</label>
      <div class="ytm-search">
        <input class="ytm-input" id="ytm-key" type="password" data-ytm-key placeholder="AIza…" autocomplete="off">
        <button class="ytm-go" type="button" data-ytm-savekey>Guardar</button>
      </div>
    </div>
  </div>
</div>

<script>
(() => {
  const root = document.getElementById("ytm");
  if (!root) return;
  const $ = selector => root.querySelector(selector);
  const $$ = selector => Array.from(root.querySelectorAll(selector));
  const store = {
    get(key, fallback) { try { const raw = localStorage.getItem("ytm." + key); return raw === null ? fallback : JSON.parse(raw); } catch (e) { return fallback; } },
    set(key, value) { try { localStorage.setItem("ytm." + key, JSON.stringify(value)); } catch (e) {} }
  };
  const RATES = [0.5, 0.75, 1, 1.25, 1.5, 1.75, 2];
  let player = null, playerPromise = null, rate = store.get("rate", 1), timer = null, dragging = false;
  let last = store.get("last", null);

  const els = {
    panel: $("[data-ytm-panel]"), pill: $("[data-ytm-pill]"), pillTitle: $("[data-ytm-pill-title]"),
    input: $("[data-ytm-input]"), msg: $("[data-ytm-msg]"), results: $("[data-ytm-results]"),
    now: $("[data-ytm-now]"), seek: $("[data-ytm-seek]"), cur: $("[data-ytm-cur]"), dur: $("[data-ytm-dur]"),
    rate: $("[data-ytm-rate]"), settings: $("[data-ytm-settings]"), key: $("[data-ytm-key]"), videoBtn: $("[data-ytm-video]")
  };

  const fmt = seconds => {
    seconds = Math.max(0, Math.floor(seconds || 0));
    const h = Math.floor(seconds / 3600), m = Math.floor(seconds % 3600 / 60), s = seconds % 60;
    return (h ? h + ":" + String(m).padStart(2, "0") : m) + ":" + String(s).padStart(2, "0");
  };
  const decode = text => { const t = document.createElement("textarea"); t.innerHTML = text; return t.value; };
  const message = (text, error) => { els.msg.textContent = text || ""; els.msg.hidden = !text; els.msg.classList.toggle("is-error", Boolean(error)); };

  const setOpen = open => {
    els.panel.hidden = !open;
    els.pill.hidden = open;
    root.classList.toggle("ytm-open", open);
    store.set("open", open);
    if (open) els.input.focus({ preventScroll: true });
  };
  const setVideo = on => {
    root.classList.toggle("ytm-video-on", on);
    els.videoBtn.setAttribute("aria-pressed", String(on));
    store.set("video", on);
  };
  const setTitle = title => {
    els.now.textContent = title || "Nada reproduciéndose";
    els.pillTitle.textContent = title || "";
    els.pillTitle.hidden = !title;
  };
  const setPlayingUI = playing => {
    root.classList.toggle("ytm-playing", playing);
    $$("[data-ytm-toggle]").forEach(button => {
      button.textContent = playing ? "⏸" : "▶";
      button.setAttribute("aria-label", playing ? "Pausar" : "Reproducir");
    });
  };
  const showRate = () => { els.rate.textContent = rate + "×"; };

  const loadApi = () => new Promise((resolve, reject) => {
    if (window.YT && window.YT.Player) return resolve();
    const previous = window.onYouTubeIframeAPIReady;
    window.onYouTubeIframeAPIReady = () => { if (typeof previous === "function") previous(); resolve(); };
    const script = document.createElement("script");
    script.src = "https://www.youtube.com/iframe_api";
    script.onerror = () => reject(new Error("No se pudo cargar la API de YouTube"));
    document.head.appendChild(script);
  });

  const ensurePlayer = () => {
    if (playerPromise) return playerPromise;
    playerPromise = loadApi().then(() => new Promise(resolve => {
      const vars = { playsinline: 1, controls: 0, rel: 0, modestbranding: 1, disablekb: 1, fs: 0 };
      if (location.protocol.startsWith("http")) vars.origin = location.origin;
      player = new YT.Player("ytm-player", {
        width: "100%", height: "100%", playerVars: vars,
        events: {
          onReady: () => resolve(player),
          onStateChange: onState,
          onError: onError
        }
      });
    })).catch(error => { playerPromise = null; message(error.message, true); throw error; });
    return playerPromise;
  };

  const onState = event => {
    const state = event.data;
    const playing = state === 1 || state === 3;
    setPlayingUI(playing);
    if (state === 1) {
      if (player.getPlaybackRate() !== rate) player.setPlaybackRate(rate);
      const data = player.getVideoData ? player.getVideoData() : null;
      if (data && data.title) { setTitle(data.title); last = { id: data.video_id, title: data.title }; store.set("last", last); }
      startTimer();
    } else if (state === 2 || state === 0 || state === 5) {
      tick();
      if (state !== 2) stopTimer();
    }
  };
  const onError = event => {
    const code = event.data;
    const text = code === 153 ? "Error 153: YouTube no pudo verificar desde dónde se abre la página. Abrila desde http://localhost o un sitio publicado, no con doble clic."
      : code === 101 || code === 150 ? "Este video no permite reproducirse fuera de YouTube. Probá con otro."
      : code === 100 ? "No se encontró el video (puede ser privado o estar borrado)."
      : code === 2 ? "Link de video no válido."
      : "El reproductor falló (código " + code + ").";
    message(text, true);
    setPlayingUI(false);
  };

  const tick = () => {
    if (!player || !player.getCurrentTime || dragging) return;
    const current = player.getCurrentTime() || 0, duration = player.getDuration() || 0;
    els.seek.max = Math.floor(duration);
    els.seek.value = Math.floor(current);
    els.cur.textContent = fmt(current);
    els.dur.textContent = fmt(duration);
  };
  const startTimer = () => { stopTimer(); tick(); timer = setInterval(tick, 500); };
  const stopTimer = () => { if (timer) clearInterval(timer); timer = null; };

  const playId = (id, title) => {
    message("");
    els.results.hidden = true;
    if (title) setTitle(title);
    return ensurePlayer().then(p => { p.loadVideoById(id); p.setPlaybackRate(rate); }).catch(() => {});
  };

  const extractId = text => {
    text = text.trim();
    if (/^[\w-]{11}$/.test(text)) return text;
    let url;
    try { url = new URL(/^https?:\/\//i.test(text) ? text : "https://" + text); } catch (e) { return null; }
    const host = url.hostname.replace(/^(www\.|m\.|music\.)/, "");
    if (host === "youtu.be") { const id = url.pathname.slice(1).split("/")[0]; return /^[\w-]{11}$/.test(id) ? id : null; }
    if (host === "youtube.com" || host === "youtube-nocookie.com") {
      const v = url.searchParams.get("v");
      if (v && /^[\w-]{11}$/.test(v)) return v;
      const match = url.pathname.match(/^\/(?:shorts|embed|live|v)\/([\w-]{11})/);
      if (match) return match[1];
    }
    return null;
  };

  const search = async query => {
    const key = store.get("key", "");
    if (!key) {
      message("Para buscar por nombre hace falta una clave de la API (⚙). Con un link pegado funciona sin clave.", true);
      els.settings.hidden = false;
      return;
    }
    message("Buscando…");
    els.results.hidden = true;
    try {
      const url = "https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&maxResults=6&q=" + encodeURIComponent(query) + "&key=" + encodeURIComponent(key);
      const response = await fetch(url);
      const data = await response.json();
      if (!response.ok) {
        const info = (data.error && data.error.errors && data.error.errors[0]) || {};
        const raw = (data.error && data.error.message) || "";
        const reason = info.reason || "";
        let hint = raw || "Error de búsqueda";
        if (reason === "keyInvalid" || /API key not valid/i.test(raw)) hint = "La clave no es válida. Copiala de nuevo, completa y sin espacios.";
        else if (reason === "accessNotConfigured" || /has not been used|is disabled/i.test(raw)) hint = "Falta activar “YouTube Data API v3” en tu proyecto de Google Cloud (Biblioteca de APIs → YouTube Data API v3 → Habilitar).";
        else if (reason === "quotaExceeded") hint = "Se acabó la cuota diaria de búsquedas. Pegá el link del video o probá mañana.";
        else if (/referer|referrer/i.test(raw) || reason === "ipRefererBlocked") hint = "La clave tiene restricción de sitio web y esta página no coincide. En Google Cloud → Credenciales, quitá la restricción o agregá http://localhost/*.";
        throw new Error(hint);
      }
      const items = (data.items || []).filter(item => item.id && item.id.videoId);
      if (!items.length) { message("Sin resultados."); return; }
      message("");
      els.results.replaceChildren(...items.map(item => {
        const button = document.createElement("button");
        button.type = "button";
        button.className = "ytm-result";
        const image = document.createElement("img");
        image.alt = "";
        image.loading = "lazy";
        image.src = (item.snippet.thumbnails.default || {}).url || "";
        const label = document.createElement("span");
        const title = decode(item.snippet.title);
        label.textContent = title;
        button.append(image, label);
        button.addEventListener("click", () => playId(item.id.videoId, title));
        return button;
      }));
      els.results.hidden = false;
    } catch (error) {
      message(error instanceof TypeError ? "No se pudo conectar con YouTube. Revisá tu conexión; si abriste la página como archivo (file://), probá desde http://localhost." : "No se pudo buscar: " + error.message, true);
    }
  };

  // Eventos
  $("[data-ytm-form]").addEventListener("submit", event => {
    event.preventDefault();
    const value = els.input.value.trim();
    if (!value) return;
    const id = extractId(value);
    if (id) { playId(id); els.input.value = ""; } else { search(value); }
  });
  $$("[data-ytm-toggle]").forEach(button => button.addEventListener("click", () => {
    if (!player || !player.getPlayerState || player.getPlayerState() === -1 || player.getPlayerState() === 5) {
      if (last && last.id) playId(last.id, last.title); else setOpen(true);
      return;
    }
    if (player.getPlayerState() === 1 || player.getPlayerState() === 3) player.pauseVideo(); else player.playVideo();
  }));
  $("[data-ytm-open]").addEventListener("click", () => setOpen(true));
  $("[data-ytm-pill-title]").addEventListener("click", () => setOpen(true));
  $("[data-ytm-close]").addEventListener("click", () => setOpen(false));
  $("[data-ytm-video]").addEventListener("click", () => setVideo(!root.classList.contains("ytm-video-on")));
  $("[data-ytm-gear]").addEventListener("click", () => { els.settings.hidden = !els.settings.hidden; });
  $("[data-ytm-savekey]").addEventListener("click", () => {
    store.set("key", els.key.value.trim());
    message(els.key.value.trim() ? "Clave guardada. Ya podés buscar por nombre." : "Clave eliminada.");
    els.settings.hidden = true;
  });
  $("[data-ytm-back]").addEventListener("click", () => { if (player && player.seekTo) { player.seekTo(Math.max(0, player.getCurrentTime() - 10), true); tick(); } });
  $("[data-ytm-fwd]").addEventListener("click", () => { if (player && player.seekTo) { player.seekTo(player.getCurrentTime() + 10, true); tick(); } });
  const changeRate = step => {
    let index = RATES.indexOf(rate);
    if (index < 0) index = RATES.findIndex(value => value >= rate);
    rate = RATES[Math.min(RATES.length - 1, Math.max(0, index + step))];
    store.set("rate", rate);
    if (player && player.setPlaybackRate) player.setPlaybackRate(rate);
    showRate();
  };
  $("[data-ytm-slower]").addEventListener("click", () => changeRate(-1));
  $("[data-ytm-faster]").addEventListener("click", () => changeRate(1));
  els.seek.addEventListener("input", () => { dragging = true; els.cur.textContent = fmt(els.seek.value); });
  els.seek.addEventListener("change", () => { if (player && player.seekTo) player.seekTo(Number(els.seek.value), true); dragging = false; tick(); });
  // Que los atajos de la app no se activen mientras se escribe en el reproductor
  root.addEventListener("keydown", event => { event.stopPropagation(); if (event.key === "Escape") setOpen(false); });

  // Estado inicial
  showRate();
  setVideo(store.get("video", false));
  if (last && last.title) setTitle(last.title);
  els.key.value = store.get("key", "");
  if (location.protocol === "file:") $("[data-ytm-file]").hidden = false;
  setOpen(store.get("open", false));
})();
</script>
<!-- ===== Fin mini reproductor ===== -->

</body>
</html>
