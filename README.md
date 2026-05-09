<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Network Masters: La Ruta de los Datos</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&family=Cinzel+Decorative:wght@400;700&family=IM+Fell+English:ital@0;1&family=MedievalSharp&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --fire: #D4690A;
    --fire-bright: #F5A623;
    --fire-dim: #8B3E05;
    --gold: #C8A96E;
    --gold-dim: #8B7040;
    --ash: #C0B89A;
    --ash-dim: #7A7060;
    --stone: #1A1614;
    --stone-dark: #0D0B0A;
    --stone-mid: #2A2420;
    --stone-light: #3A322C;
    --soul-blue: #4A8FD4;
    --soul-bright: #7FBFFF;
    --blood: #8B1A1A;
    --estus: #F5A623;
  }

  html, body {
    width: 100%; height: 100%;
    background: #000;
    font-family: 'IM Fell English', serif;
    color: var(--ash);
    overflow: hidden;
  }

  /* ===== SLIDESHOW CONTAINER ===== */
  #presentation {
    width: 100vw; height: 100vh;
    position: relative;
    overflow: hidden;
  }

  .slide {
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
    display: none;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: var(--stone-dark);
    padding: 40px;
    opacity: 0;
    transition: opacity 0.6s ease;
  }

  .slide.active {
    display: flex;
    opacity: 1;
  }

  /* Stone texture overlay */
  .slide::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
      repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px),
      repeating-linear-gradient(90deg, transparent, transparent 2px, rgba(0,0,0,0.02) 2px, rgba(0,0,0,0.02) 4px);
    pointer-events: none;
    z-index: 0;
  }

  .slide > * { position: relative; z-index: 1; }

  /* Vignette */
  .slide::after {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at center, transparent 50%, rgba(0,0,0,0.7) 100%);
    pointer-events: none;
    z-index: 0;
  }

  /* ===== TOP HUD ===== */
  #hud {
    position: fixed;
    top: 0; left: 0; right: 0;
    height: 56px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 24px;
    background: linear-gradient(180deg, rgba(0,0,0,0.9) 0%, transparent 100%);
    z-index: 100;
    pointer-events: none;
  }

  #hud-left {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  #slide-title-hud {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--ash-dim);
  }

  /* ===== SOUL COUNTER ===== */
  #soul-counter {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: 'Cinzel', serif;
    font-size: 14px;
    color: var(--soul-bright);
  }

  #soul-counter .soul-gem {
    width: 18px; height: 18px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, var(--soul-bright), var(--soul-blue));
    box-shadow: 0 0 8px var(--soul-blue);
    display: inline-block;
  }

  /* ===== BONFIRE PROGRESS ===== */
  #bonfire-bar {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .bonfire-node {
    width: 14px; height: 14px;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .bonfire-node .flame {
    width: 10px; height: 10px;
    background: var(--ash-dim);
    clip-path: polygon(50% 0%, 80% 40%, 100% 70%, 80% 100%, 20% 100%, 0% 70%, 20% 40%);
    transition: all 0.4s ease;
  }

  .bonfire-node.lit .flame {
    background: var(--fire-bright);
    box-shadow: 0 0 6px var(--fire), 0 0 12px var(--fire-dim);
    animation: flicker 1.5s infinite alternate;
  }

  @keyframes flicker {
    0% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.8; transform: scale(0.95) translateY(-1px); }
    100% { opacity: 1; transform: scale(1.05); }
  }

  /* ===== BOTTOM CONTROLS ===== */
  #controls {
    position: fixed;
    bottom: 0; left: 0; right: 0;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 16px;
    background: linear-gradient(0deg, rgba(0,0,0,0.9) 0%, transparent 100%);
    z-index: 100;
  }

  .ctrl-btn {
    background: transparent;
    border: 1px solid var(--gold-dim);
    color: var(--gold);
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 2px;
    padding: 8px 24px;
    cursor: pointer;
    text-transform: uppercase;
    transition: all 0.2s;
  }

  .ctrl-btn:hover {
    border-color: var(--gold);
    background: rgba(200, 169, 110, 0.1);
    box-shadow: 0 0 12px rgba(200, 169, 110, 0.3);
  }

  .ctrl-btn:disabled {
    opacity: 0.2;
    cursor: default;
  }

  #slide-num {
    font-family: 'Cinzel', serif;
    font-size: 12px;
    color: var(--ash-dim);
    min-width: 60px;
    text-align: center;
  }

  /* ===== FOG GATE TRANSITION ===== */
  #fog-gate {
    position: fixed;
    inset: 0;
    background: rgba(200, 169, 110, 0.05);
    backdrop-filter: blur(0px);
    opacity: 0;
    z-index: 90;
    pointer-events: none;
    transition: opacity 0.3s ease;
  }

  #fog-gate.active {
    opacity: 1;
  }

  /* ===== BONFIRE NOTIFICATION ===== */
  #bonfire-notif {
    position: fixed;
    bottom: 80px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    opacity: 0;
    text-align: center;
    transition: all 0.5s ease;
    z-index: 200;
    pointer-events: none;
  }

  #bonfire-notif.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }

  #bonfire-notif p.top {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 4px;
    color: var(--fire-bright);
    text-transform: uppercase;
    margin-bottom: 4px;
  }

  #bonfire-notif p.bottom {
    font-family: 'Cinzel Decorative', serif;
    font-size: 16px;
    color: var(--gold);
    letter-spacing: 2px;
  }

  /* ===== DECORATIVE DIVIDER ===== */
  .divider {
    width: 100%;
    display: flex;
    align-items: center;
    gap: 12px;
    margin: 16px 0;
  }

  .divider::before, .divider::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold-dim), transparent);
  }

  .divider-icon {
    color: var(--fire);
    font-size: 16px;
  }

  /* ===== SLIDE-SPECIFIC STYLES ===== */

  /* --- PORTADA --- */
  #slide-0 {
    background:
      radial-gradient(ellipse at 50% 60%, rgba(212, 105, 10, 0.08) 0%, transparent 60%),
      var(--stone-dark);
    text-align: center;
  }

  .title-eyebrow {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 5px;
    text-transform: uppercase;
    color: var(--fire);
    margin-bottom: 20px;
  }

  .main-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 46px;
    font-weight: 700;
    line-height: 1.1;
    color: var(--gold);
    text-shadow: 0 0 40px rgba(212, 105, 10, 0.4);
    margin-bottom: 8px;
    letter-spacing: 2px;
  }

  .main-subtitle {
    font-family: 'Cinzel', serif;
    font-size: 18px;
    color: var(--fire-bright);
    letter-spacing: 4px;
    margin-bottom: 32px;
  }

  .lore-text {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: 14px;
    color: var(--ash-dim);
    max-width: 520px;
    line-height: 1.8;
    margin: 0 auto 32px;
    border-left: 2px solid var(--fire-dim);
    padding-left: 16px;
    text-align: left;
  }

  .cover-meta {
    display: flex;
    gap: 32px;
    justify-content: center;
    margin-top: 24px;
  }

  .cover-meta-item {
    text-align: center;
  }

  .cover-meta-item .label {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 3px;
    color: var(--ash-dim);
    display: block;
  }

  .cover-meta-item .value {
    font-family: 'Cinzel', serif;
    font-size: 13px;
    color: var(--gold);
    margin-top: 4px;
    display: block;
  }

  /* Flame SVG on portada */
  .bonfire-svg {
    width: 80px;
    height: 100px;
    margin: 0 auto 24px;
    display: block;
  }

  /* Corner ornaments */
  .corner-tl, .corner-tr, .corner-bl, .corner-br {
    position: absolute;
    width: 60px; height: 60px;
  }

  .corner-tl { top: 20px; left: 20px; border-top: 2px solid var(--gold-dim); border-left: 2px solid var(--gold-dim); }
  .corner-tr { top: 20px; right: 20px; border-top: 2px solid var(--gold-dim); border-right: 2px solid var(--gold-dim); }
  .corner-bl { bottom: 20px; left: 20px; border-bottom: 2px solid var(--gold-dim); border-left: 2px solid var(--gold-dim); }
  .corner-br { bottom: 20px; right: 20px; border-bottom: 2px solid var(--gold-dim); border-right: 2px solid var(--gold-dim); }

  /* --- NIVEL SLIDES --- */
  .level-header {
    width: 100%;
    text-align: center;
    margin-bottom: 28px;
  }

  .level-tag {
    display: inline-block;
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: var(--fire);
    border: 1px solid var(--fire-dim);
    padding: 4px 16px;
    margin-bottom: 10px;
  }

  .level-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 28px;
    color: var(--gold);
    letter-spacing: 1px;
    line-height: 1.2;
  }

  /* Content grid */
  .content-grid {
    display: grid;
    gap: 16px;
    width: 100%;
    max-width: 960px;
  }

  .content-grid.cols-2 { grid-template-columns: 1fr 1fr; }
  .content-grid.cols-3 { grid-template-columns: 1fr 1fr 1fr; }
  .content-grid.cols-12 { grid-template-columns: 1.2fr 1fr; }

  /* Scroll tablet (info box) */
  .scroll-tablet {
    background: rgba(26, 22, 20, 0.9);
    border: 1px solid var(--gold-dim);
    padding: 20px 24px;
    position: relative;
  }

  .scroll-tablet::before {
    content: '';
    position: absolute;
    top: 4px; left: 4px; right: 4px; bottom: 4px;
    border: 1px solid rgba(200, 169, 110, 0.15);
    pointer-events: none;
  }

  .scroll-tablet h3 {
    font-family: 'Cinzel', serif;
    font-size: 13px;
    letter-spacing: 2px;
    color: var(--fire-bright);
    margin-bottom: 10px;
    text-transform: uppercase;
  }

  .scroll-tablet p, .scroll-tablet li {
    font-family: 'IM Fell English', serif;
    font-size: 14px;
    color: var(--ash);
    line-height: 1.7;
  }

  .scroll-tablet ul {
    list-style: none;
    padding: 0;
  }

  .scroll-tablet ul li::before {
    content: '›';
    color: var(--fire);
    margin-right: 8px;
    font-weight: bold;
  }

  /* Item description box (Dark Souls tooltip style) */
  .item-box {
    background: rgba(10, 8, 7, 0.95);
    border: 1px solid var(--gold-dim);
    border-top: 3px solid var(--fire);
    padding: 16px 20px;
    font-family: 'IM Fell English', serif;
  }

  .item-box .item-name {
    font-family: 'Cinzel', serif;
    font-size: 13px;
    color: var(--gold);
    letter-spacing: 1px;
    margin-bottom: 4px;
  }

  .item-box .item-type {
    font-size: 10px;
    color: var(--ash-dim);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 10px;
    font-style: italic;
  }

  .item-box .item-desc {
    font-size: 13px;
    color: var(--ash);
    line-height: 1.6;
    font-style: italic;
  }

  /* Stat row */
  .stat-row {
    display: flex;
    align-items: center;
    gap: 12px;
    margin: 6px 0;
  }

  .stat-label {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 1px;
    color: var(--ash-dim);
    min-width: 70px;
    text-transform: uppercase;
  }

  .stat-bar-wrap {
    flex: 1;
    height: 6px;
    background: rgba(255,255,255,0.05);
    border: 1px solid var(--stone-light);
  }

  .stat-bar-fill {
    height: 100%;
    background: var(--fire);
    transition: width 0.5s ease;
  }

  .stat-bar-fill.gold { background: var(--gold); }
  .stat-bar-fill.soul { background: var(--soul-blue); }

  .stat-val {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    color: var(--ash);
    min-width: 28px;
    text-align: right;
  }

  /* Boss battle (vs comparison) */
  .boss-arena {
    width: 100%;
    max-width: 960px;
  }

  .vs-header {
    text-align: center;
    margin-bottom: 20px;
  }

  .vs-header .boss-eyebrow {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 5px;
    color: var(--blood);
    text-transform: uppercase;
    margin-bottom: 6px;
  }

  .vs-header .boss-title {
    font-family: 'Cinzel Decorative', serif;
    font-size: 24px;
    color: var(--gold);
  }

  .vs-grid {
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    gap: 16px;
    align-items: start;
  }

  .player-card {
    background: rgba(15, 12, 10, 0.95);
    border: 1px solid var(--gold-dim);
    padding: 20px;
  }

  .player-card.p1 { border-top: 3px solid var(--fire); }
  .player-card.p2 { border-top: 3px solid var(--soul-blue); }

  .player-label {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 4px;
    text-transform: uppercase;
    margin-bottom: 6px;
  }

  .p1 .player-label { color: var(--fire); }
  .p2 .player-label { color: var(--soul-bright); }

  .player-name {
    font-family: 'Cinzel Decorative', serif;
    font-size: 18px;
    margin-bottom: 12px;
  }

  .p1 .player-name { color: var(--fire-bright); }
  .p2 .player-name { color: var(--soul-bright); }

  .vs-divider {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    padding-top: 48px;
  }

  .vs-text {
    font-family: 'Cinzel Decorative', serif;
    font-size: 22px;
    color: var(--gold);
    opacity: 0.8;
  }

  .compare-row {
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    gap: 8px;
    align-items: center;
    padding: 8px 0;
    border-bottom: 1px solid rgba(200, 169, 110, 0.1);
  }

  .compare-row .crit {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--ash-dim);
    text-transform: uppercase;
    text-align: center;
  }

  .compare-row .val {
    font-size: 13px;
    color: var(--ash);
    line-height: 1.4;
  }

  .compare-row .val.left { text-align: right; }
  .compare-row .val.right { text-align: left; }

  .compare-row .win { color: var(--fire-bright); font-style: italic; }
  .compare-row .win-blue { color: var(--soul-bright); font-style: italic; }

  /* IP address display */
  .ip-display {
    font-family: 'Cinzel', monospace;
    font-size: 28px;
    color: var(--soul-bright);
    letter-spacing: 4px;
    text-shadow: 0 0 20px rgba(127, 191, 255, 0.4);
    text-align: center;
    padding: 16px;
    border: 1px solid var(--soul-blue);
    margin: 8px 0;
  }

  .packet-flow {
    display: flex;
    align-items: center;
    gap: 8px;
    justify-content: center;
    flex-wrap: wrap;
    padding: 16px 0;
  }

  .pf-node {
    background: var(--stone-mid);
    border: 1px solid var(--gold-dim);
    padding: 8px 14px;
    font-family: 'Cinzel', serif;
    font-size: 11px;
    color: var(--gold);
    text-align: center;
    min-width: 80px;
  }

  .pf-arrow {
    color: var(--fire);
    font-size: 18px;
  }

  /* Routing table */
  .routing-table {
    width: 100%;
    border-collapse: collapse;
    font-family: 'IM Fell English', serif;
    font-size: 13px;
  }

  .routing-table th {
    background: rgba(212, 105, 10, 0.15);
    border: 1px solid var(--fire-dim);
    padding: 8px 12px;
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 2px;
    color: var(--fire-bright);
    text-transform: uppercase;
    text-align: left;
  }

  .routing-table td {
    border: 1px solid rgba(200, 169, 110, 0.2);
    padding: 7px 12px;
    color: var(--ash);
  }

  .routing-table tr:nth-child(even) td {
    background: rgba(255,255,255,0.02);
  }

  .routing-table tr:hover td {
    background: rgba(212, 105, 10, 0.05);
  }

  .routing-table .highlight td {
    color: var(--fire-bright);
    background: rgba(212, 105, 10, 0.08);
  }

  /* Cable diagram */
  .cable-diagram {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0;
    padding: 16px;
  }

  .device-box {
    background: var(--stone-mid);
    border: 1px solid var(--gold-dim);
    padding: 12px 16px;
    text-align: center;
    min-width: 100px;
  }

  .device-box .device-icon { font-size: 24px; margin-bottom: 4px; }
  .device-box .device-name {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 2px;
    color: var(--gold);
    text-transform: uppercase;
  }

  .cable-line {
    flex: 1;
    height: 3px;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 60px;
  }

  .cable-line::before {
    content: '';
    position: absolute;
    left: 0; right: 0;
    height: 3px;
    background: repeating-linear-gradient(90deg, var(--fire-dim) 0px, var(--fire-dim) 8px, transparent 8px, transparent 12px);
  }

  .cable-label {
    position: absolute;
    top: -20px;
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 1px;
    color: var(--fire);
    white-space: nowrap;
    background: var(--stone-dark);
    padding: 0 4px;
  }

  /* Conclusion */
  .conclusion-box {
    background: rgba(10, 8, 7, 0.95);
    border: 1px solid var(--gold);
    border-top: 3px solid var(--fire-bright);
    padding: 24px 28px;
    max-width: 700px;
    position: relative;
  }

  .conclusion-box h3 {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 3px;
    color: var(--fire-bright);
    text-transform: uppercase;
    margin-bottom: 12px;
  }

  .conclusion-box p {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: 15px;
    color: var(--ash);
    line-height: 1.9;
  }

  /* "YOU DIED" style text */
  .died-text {
    font-family: 'Cinzel Decorative', serif;
    font-size: 42px;
    color: var(--blood);
    letter-spacing: 6px;
    text-shadow: 0 0 40px rgba(139, 26, 26, 0.6);
    text-align: center;
    opacity: 0.15;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    pointer-events: none;
    white-space: nowrap;
  }

  /* Level tag colors */
  .level-tag.completed { color: var(--fire-bright); border-color: var(--fire); }

  /* Fade in animation */
  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .slide.active .level-header,
  .slide.active .content-grid,
  .slide.active .boss-arena,
  .slide.active .lore-text,
  .slide.active .conclusion-box {
    animation: fadeInUp 0.5s ease both;
  }

  .slide.active .level-header { animation-delay: 0.1s; }
  .slide.active .content-grid { animation-delay: 0.2s; }

  /* Large center icon for portada */
  .bonfire-art {
    font-size: 64px;
    margin-bottom: 8px;
    animation: glowPulse 2s infinite alternate;
  }

  @keyframes glowPulse {
    from { filter: drop-shadow(0 0 8px var(--fire-dim)); }
    to { filter: drop-shadow(0 0 24px var(--fire)); }
  }

  /* Tag badge */
  .tag-badge {
    display: inline-block;
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 2px 10px;
    border: 1px solid;
    margin: 2px;
  }

  .tag-badge.green { color: #7AB648; border-color: #4A7020; background: rgba(74, 112, 32, 0.15); }
  .tag-badge.red { color: #E05050; border-color: #802020; background: rgba(128, 32, 32, 0.15); }
  .tag-badge.blue { color: var(--soul-bright); border-color: var(--soul-blue); background: rgba(74, 143, 212, 0.15); }
  .tag-badge.orange { color: var(--fire-bright); border-color: var(--fire-dim); background: rgba(212, 105, 10, 0.15); }

  /* Tooltip lore */
  .lore-card {
    border-left: 2px solid var(--fire-dim);
    padding: 10px 14px;
    background: rgba(0,0,0,0.4);
    margin-bottom: 12px;
  }

  .lore-card p {
    font-style: italic;
    font-size: 12px;
    color: var(--ash-dim);
    line-height: 1.6;
  }

</style>
</head>
<body>

<!-- FOG GATE -->
<div id="fog-gate"></div>

<!-- BONFIRE NOTIFICATION -->
<div id="bonfire-notif">
  <p class="top">⬆ bonfire lit</p>
  <p class="bottom" id="notif-text">Nivel completado</p>
</div>

<!-- HUD -->
<div id="hud">
  <div id="hud-left">
    <div id="bonfire-bar"></div>
    <span id="slide-title-hud">Portada</span>
  </div>
  <div id="soul-counter">
    <div class="soul-gem"></div>
    <span id="soul-count">0</span>
    <span style="font-size:10px; color: var(--ash-dim); letter-spacing:1px;">ALMAS</span>
  </div>
</div>

<!-- ===================== SLIDES ===================== -->
<div id="presentation">

  <!-- SLIDE 0: PORTADA -->
  <div class="slide active" id="slide-0">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <p class="title-eyebrow">▶&ensp;Iniciar misión&ensp;◀</p>
    <div class="bonfire-art">🔥</div>
    <h1 class="main-title">Network Masters</h1>
    <h2 class="main-subtitle">La Ruta de los Datos</h2>

    <div class="divider"><span class="divider-icon">⬥</span></div>

    <p class="lore-text">
      "Una empresa internacional ha perdido la comunicación entre sus sucursales. Las rutas están perdidas,
      los paquetes no llegan. Tu equipo ha sido invocado para restaurar la conectividad.
      Solo los que dominen el routing podrán salvar la red."
    </p>

    <div class="cover-meta">
      <div class="cover-meta-item">
        <span class="label">Asignatura</span>
        <span class="value">Networking y Comunicaciones</span>
      </div>
      <div class="cover-meta-item">
        <span class="label">Actividad</span>
        <span class="value">ADA 17 — Routing</span>
      </div>
      <div class="cover-meta-item">
        <span class="label">Clase</span>
        <span class="value">6C</span>
      </div>
    </div>

    <p style="margin-top:28px; font-family:'Cinzel',serif; font-size:10px; letter-spacing:3px; color:var(--ash-dim);">
      Presiona [ AVANZAR ] para comenzar tu travesía
    </p>
  </div>

  <!-- SLIDE 1: NIVEL 1 — QUÉ ES EL ROUTING -->
  <div class="slide" id="slide-1">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="level-header">
      <div class="level-tag">🔥 Bonfire I — Primera Hoguera</div>
      <h2 class="level-title">¿Qué realiza el Routing?</h2>
    </div>

    <div class="content-grid cols-12" style="max-width:960px;">
      <div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>Definición</h3>
          <p>El <strong style="color:var(--fire-bright)">routing</strong> (enrutamiento) es el proceso mediante el cual los datos viajan de una red a otra hasta llegar a su destino. Funciona como un sistema de navegación para los paquetes de información.</p>
        </div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>¿Para qué sirve?</h3>
          <ul>
            <li>Determinar la <em>mejor ruta</em> para enviar datos</li>
            <li>Conectar diferentes redes entre sí</li>
            <li>Mantener la comunicación eficiente y estable</li>
            <li>Evitar congestión y rutas rotas</li>
          </ul>
        </div>
        <div class="scroll-tablet">
          <h3>¿Cómo conecta redes?</h3>
          <p>Los routers examinan la dirección IP de destino de cada paquete y consultan su tabla de enrutamiento para decidir por cuál interfaz reenviarlo, cruzando múltiples redes hasta alcanzar el host final.</p>
        </div>
      </div>
      <div>
        <div class="item-box" style="margin-bottom:14px;">
          <div class="item-name">Router</div>
          <div class="item-type">Dispositivo de red — Capa 3</div>
          <div class="item-desc">"Guardián de las rutas. Examina cada paquete que cruza sus puertas y decide el camino más sabio hacia la verdad del destino."</div>
        </div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>Dispositivos que hacen Routing</h3>
          <ul>
            <li><strong style="color:var(--gold)">Router</strong> — principal responsable del enrutamiento</li>
            <li><strong style="color:var(--gold)">Switch capa 3</strong> — routing entre VLANs</li>
            <li><strong style="color:var(--gold)">Firewall</strong> — routing con seguridad</li>
            <li><strong style="color:var(--gold)">PC con múltiples NICs</strong> — routing básico</li>
          </ul>
        </div>
        <div class="lore-card">
          <p>Ejemplo cotidiano: cuando envías un mensaje a un amigo, el paquete pasa por tu router doméstico → router de tu ISP → varios routers de Internet → router del ISP de tu amigo → su dispositivo.</p>
        </div>
        <div style="text-align:center; margin-top:10px;">
          <svg width="120" height="80" viewBox="0 0 120 80" style="display:inline-block;">
            <rect x="10" y="25" width="30" height="30" rx="3" fill="none" stroke="#8B3E05" stroke-width="1.5"/>
            <text x="25" y="46" text-anchor="middle" font-family="Cinzel,serif" font-size="9" fill="#C8A96E">PC</text>
            <rect x="45" y="20" width="30" height="40" rx="3" fill="none" stroke="#D4690A" stroke-width="1.5"/>
            <text x="60" y="44" text-anchor="middle" font-family="Cinzel,serif" font-size="8" fill="#F5A623">ROUTER</text>
            <rect x="80" y="25" width="30" height="30" rx="3" fill="none" stroke="#4A8FD4" stroke-width="1.5"/>
            <text x="95" y="43" text-anchor="middle" font-family="Cinzel,serif" font-size="8" fill="#7FBFFF">DESTINO</text>
            <line x1="40" y1="40" x2="45" y2="40" stroke="#8B3E05" stroke-width="1.5" stroke-dasharray="3,2"/>
            <line x1="75" y1="40" x2="80" y2="40" stroke="#D4690A" stroke-width="1.5" stroke-dasharray="3,2"/>
            <polygon points="43,37 47,40 43,43" fill="#D4690A"/>
            <polygon points="78,37 82,40 78,43" fill="#4A8FD4"/>
          </svg>
        </div>
      </div>
    </div>
  </div>

  <!-- SLIDE 2: NIVEL 2 — TIPOS DE ENRUTAMIENTO -->
  <div class="slide" id="slide-2">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="level-header">
      <div class="level-tag">🔥 Bonfire II — Hoguera del Trifurcado</div>
      <h2 class="level-title">Tipos de Enrutamiento</h2>
    </div>

    <div class="content-grid cols-3" style="max-width:980px;">

      <!-- ESTÁTICO -->
      <div>
        <div class="item-box" style="margin-bottom:10px;">
          <div class="item-name">Enrutamiento Estático</div>
          <div class="item-type">Configuración manual</div>
          <div class="item-desc">"Runas grabadas en piedra. Inmutables, predecibles. El administrador traza el camino y el paquete lo sigue sin cuestionarlo."</div>
        </div>
        <div class="scroll-tablet">
          <h3>Características</h3>
          <ul>
            <li>Rutas configuradas manualmente</li>
            <li>No cambia automáticamente</li>
            <li>Requiere intervención del admin</li>
          </ul>
          <div style="margin-top:10px;">
            <div class="stat-row">
              <span class="stat-label">Seguridad</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill gold" style="width:90%"></div></div>
              <span class="stat-val">Alta</span>
            </div>
            <div class="stat-row">
              <span class="stat-label">Escalabilidad</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill" style="width:20%"></div></div>
              <span class="stat-val">Baja</span>
            </div>
            <div class="stat-row">
              <span class="stat-label">Adaptación</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill" style="width:5%"></div></div>
              <span class="stat-val">Nula</span>
            </div>
          </div>
          <p style="margin-top:10px; font-size:12px;"><span class="tag-badge green">✓ Poco tráfico</span><span class="tag-badge red">✗ Redes grandes</span></p>
          <p style="margin-top:8px; font-size:12px; font-style:italic;">Uso: redes pequeñas, sucursales únicas, conexiones específicas.</p>
        </div>
      </div>

      <!-- DINÁMICO -->
      <div>
        <div class="item-box" style="margin-bottom:10px; border-top-color: var(--soul-blue);">
          <div class="item-name" style="color:var(--soul-bright);">Enrutamiento Dinámico</div>
          <div class="item-type">Protocolos automáticos</div>
          <div class="item-desc">"Los routers hablan entre sí, comparten el conocimiento de las rutas y se adaptan cuando algún camino cae en la oscuridad."</div>
        </div>
        <div class="scroll-tablet">
          <h3>Características</h3>
          <ul>
            <li>Usa protocolos: RIP, OSPF, EIGRP, BGP</li>
            <li>Se adapta automáticamente a fallos</li>
            <li>Los routers intercambian información</li>
          </ul>
          <div style="margin-top:10px;">
            <div class="stat-row">
              <span class="stat-label">Seguridad</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill gold" style="width:55%"></div></div>
              <span class="stat-val">Media</span>
            </div>
            <div class="stat-row">
              <span class="stat-label">Escalabilidad</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill soul" style="width:95%"></div></div>
              <span class="stat-val">Alta</span>
            </div>
            <div class="stat-row">
              <span class="stat-label">Adaptación</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill soul" style="width:90%"></div></div>
              <span class="stat-val">Alta</span>
            </div>
          </div>
          <p style="margin-top:10px; font-size:12px;"><span class="tag-badge blue">✓ Empresas</span><span class="tag-badge blue">✓ Internet</span></p>
          <p style="margin-top:8px; font-size:12px; font-style:italic;">Uso: redes corporativas, ISPs, Internet (BGP).</p>
        </div>
      </div>

      <!-- POR DEFECTO -->
      <div>
        <div class="item-box" style="margin-bottom:10px; border-top-color: var(--gold);">
          <div class="item-name" style="color:var(--gold);">Enrutamiento por Defecto</div>
          <div class="item-type">Ruta de último recurso</div>
          <div class="item-desc">"Cuando ningún mapa menciona el destino, el viajero toma la única puerta que siempre está abierta."</div>
        </div>
        <div class="scroll-tablet">
          <h3>Características</h3>
          <ul>
            <li>Ruta 0.0.0.0/0 — acepta cualquier destino</li>
            <li>Se usa cuando no hay ruta específica</li>
            <li>Apunta al gateway del ISP</li>
          </ul>
          <div style="margin-top:10px;">
            <div class="stat-row">
              <span class="stat-label">Uso CPU</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill gold" style="width:15%"></div></div>
              <span class="stat-val">Mínimo</span>
            </div>
            <div class="stat-row">
              <span class="stat-label">Configuración</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill gold" style="width:95%"></div></div>
              <span class="stat-val">Simple</span>
            </div>
            <div class="stat-row">
              <span class="stat-label">Flexibilidad</span>
              <div class="stat-bar-wrap"><div class="stat-bar-fill gold" style="width:40%"></div></div>
              <span class="stat-val">Media</span>
            </div>
          </div>
          <p style="margin-top:10px; font-size:12px;"><span class="tag-badge orange">✓ Hogares</span><span class="tag-badge orange">✓ Stubs</span></p>
          <p style="margin-top:8px; font-size:12px; font-style:italic;">Uso: redes domésticas, routers de borde hacia ISP.</p>
        </div>
      </div>
    </div>
  </div>

  <!-- SLIDE 3: NIVEL 3 — BOSS BATTLE -->
  <div class="slide" id="slide-3">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="boss-arena">
      <div class="vs-header">
        <p class="boss-eyebrow">⚔ Encuentro con el Señor del Camino ⚔</p>
        <h2 class="boss-title">Boss Battle: Estático vs Dinámico</h2>
        <div class="divider"><span class="divider-icon">☠</span></div>
      </div>

      <div class="vs-grid">
        <!-- PLAYER 1 -->
        <div class="player-card p1">
          <div class="player-label">Player 1</div>
          <div class="player-name">Enrutamiento<br>Estático</div>
          <div style="font-size:13px; color:var(--ash); line-height:1.6;">
            <p style="margin-bottom:8px; font-style:italic; color:var(--ash-dim);">"El Guerrero de Piedra. Predecible. Inquebrantable. Lento en adaptarse, pero jamás traicionado por un protocolo."</p>
          </div>
          <div style="margin-top:12px;">
            <div class="stat-row"><span class="stat-label">Seguridad</span><div class="stat-bar-wrap"><div class="stat-bar-fill gold" style="width:95%"></div></div><span class="stat-val">95</span></div>
            <div class="stat-row"><span class="stat-label">Velocidad Config</span><div class="stat-bar-wrap"><div class="stat-bar-fill" style="width:70%"></div></div><span class="stat-val">70</span></div>
            <div class="stat-row"><span class="stat-label">Adaptación</span><div class="stat-bar-wrap"><div class="stat-bar-fill" style="width:5%"></div></div><span class="stat-val">5</span></div>
            <div class="stat-row"><span class="stat-label">Escalabilidad</span><div class="stat-bar-wrap"><div class="stat-bar-fill" style="width:15%"></div></div><span class="stat-val">15</span></div>
          </div>
        </div>

        <!-- VS -->
        <div class="vs-divider"><span class="vs-text">VS</span></div>

        <!-- PLAYER 2 -->
        <div class="player-card p2">
          <div class="player-label">Player 2</div>
          <div class="player-name">Enrutamiento<br>Dinámico</div>
          <div style="font-size:13px; color:var(--ash); line-height:1.6;">
            <p style="margin-bottom:8px; font-style:italic; color:var(--ash-dim);">"El Espíritu Errante. Aprende, se adapta, sobrevive. Sus protocolos susurran rutas entre los routers del reino."</p>
          </div>
          <div style="margin-top:12px;">
            <div class="stat-row"><span class="stat-label">Seguridad</span><div class="stat-bar-wrap"><div class="stat-bar-fill soul" style="width:60%"></div></div><span class="stat-val">60</span></div>
            <div class="stat-row"><span class="stat-label">Velocidad Config</span><div class="stat-bar-wrap"><div class="stat-bar-fill soul" style="width:40%"></div></div><span class="stat-val">40</span></div>
            <div class="stat-row"><span class="stat-label">Adaptación</span><div class="stat-bar-wrap"><div class="stat-bar-fill soul" style="width:95%"></div></div><span class="stat-val">95</span></div>
            <div class="stat-row"><span class="stat-label">Escalabilidad</span><div class="stat-bar-wrap"><div class="stat-bar-fill soul" style="width:97%"></div></div><span class="stat-val">97</span></div>
          </div>
        </div>
      </div>

      <!-- Comparison rows -->
      <div style="margin-top:16px; background:rgba(10,8,7,0.8); border:1px solid var(--gold-dim); padding:12px 16px;">
        <div class="compare-row">
          <div class="val left win">Manual, rápida y simple</div>
          <div class="crit">Configuración</div>
          <div class="val right">Compleja, requiere planificación</div>
        </div>
        <div class="compare-row">
          <div class="val left">Redes pequeñas / sucursales</div>
          <div class="crit">Ideal para</div>
          <div class="val right win-blue">Redes medianas y grandes</div>
        </div>
        <div class="compare-row">
          <div class="val left">No se recupera solo</div>
          <div class="crit">Ante fallos</div>
          <div class="val right win-blue">Recalcula automáticamente</div>
        </div>
        <div class="compare-row" style="border:none;">
          <div class="val left win">Sin overhead de protocolos</div>
          <div class="crit">Uso de CPU</div>
          <div class="val right">Mayor procesamiento</div>
        </div>
      </div>
    </div>
  </div>

  <!-- SLIDE 4: NIVEL 4 — IP ROUTING -->
  <div class="slide" id="slide-4">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="level-header">
      <div class="level-tag">🔥 Bonfire IV — El Sello del Origen</div>
      <h2 class="level-title">¿Qué es el Enrutamiento IP?</h2>
    </div>

    <div class="content-grid cols-2" style="max-width:960px;">
      <div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>Dirección IP</h3>
          <p>Una dirección IP es un identificador único asignado a cada dispositivo en una red. Funciona como la dirección de tu hogar en el mundo digital.</p>
          <div class="ip-display" style="margin-top:12px;">192.168.1.10</div>
          <p style="font-size:12px; color:var(--ash-dim); margin-top:6px; text-align:center;">IPv4 — 4 octetos de 8 bits cada uno</p>
        </div>
        <div class="scroll-tablet">
          <h3>¿Qué es un paquete de datos?</h3>
          <ul>
            <li>Unidad básica de transmisión en redes</li>
            <li>Contiene: cabecera + datos + checksum</li>
            <li>La cabecera incluye IP origen e IP destino</li>
            <li>Se reensamblan en el destino</li>
          </ul>
        </div>
      </div>
      <div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>¿Cómo usa el Router las IPs?</h3>
          <ul>
            <li>Recibe el paquete por una interfaz</li>
            <li>Lee la IP destino del encabezado</li>
            <li>Consulta su tabla de enrutamiento</li>
            <li>Elige la mejor ruta (menor métrica)</li>
            <li>Reenvía por la interfaz correcta</li>
          </ul>
        </div>
        <div class="scroll-tablet">
          <h3>Flujo de un paquete</h3>
          <div class="packet-flow">
            <div class="pf-node">PC<br><small style="font-size:9px; color:var(--ash-dim);">192.168.1.10</small></div>
            <span class="pf-arrow">→</span>
            <div class="pf-node">ROUTER<br><small style="font-size:9px; color:var(--fire-dim);">Gateway</small></div>
            <span class="pf-arrow">→</span>
            <div class="pf-node">INTERNET<br><small style="font-size:9px; color:var(--soul-blue);">BGP hops</small></div>
            <span class="pf-arrow">→</span>
            <div class="pf-node">SERVIDOR<br><small style="font-size:9px; color:var(--ash-dim);">8.8.8.8</small></div>
          </div>
          <p style="font-size:12px; font-style:italic; color:var(--ash-dim); margin-top:8px;">Cada salto (hop) un router decide el siguiente tramo del camino.</p>
        </div>
        <div class="lore-card" style="margin-top:10px;">
          <p>"El router no lee el contenido del paquete, solo su destino. Como un mensajero que no abre la carta, solo la entrega."</p>
        </div>
      </div>
    </div>
  </div>

  <!-- SLIDE 5: NIVEL 5 — SWITCH-ROUTER -->
  <div class="slide" id="slide-5">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="level-header">
      <div class="level-tag">🔥 Bonfire V — El Puente y la Puerta</div>
      <h2 class="level-title">Conexión Switch – Router</h2>
    </div>

    <div class="content-grid cols-2" style="max-width:960px;">
      <div>
        <div class="item-box" style="margin-bottom:14px; border-top-color: var(--soul-blue);">
          <div class="item-name" style="color:var(--soul-bright);">Switch</div>
          <div class="item-type">Dispositivo de Capa 2 — LAN</div>
          <div class="item-desc">"El guardián interno. Conecta dispositivos dentro de la misma red usando direcciones MAC. No cruza fronteras, pero domina el territorio local."</div>
        </div>
        <div class="item-box" style="margin-bottom:14px;">
          <div class="item-name">Router</div>
          <div class="item-type">Dispositivo de Capa 3 — Interredes</div>
          <div class="item-desc">"El explorador de fronteras. Conecta redes distintas usando IPs. Decide si el paquete se queda en la LAN o cruza hacia el mundo exterior."</div>
        </div>
        <div class="scroll-tablet">
          <h3>Tipo de cable</h3>
          <ul>
            <li><strong style="color:var(--gold)">Par trenzado Cat5e/Cat6</strong> — conexión más común (RJ-45)</li>
            <li><strong style="color:var(--gold)">Cable directo</strong> — PC a Switch, Switch a Router</li>
            <li><strong style="color:var(--gold)">Cable cruzado</strong> — dispositivos del mismo tipo (aunque hoy el Auto-MDIX lo hace innecesario)</li>
          </ul>
        </div>
      </div>
      <div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>Ejemplo: Red LAN básica</h3>
          <div style="padding:16px 0;">
            <div class="cable-diagram" style="flex-wrap:wrap; gap:4px;">
              <div class="device-box">
                <div class="device-icon">💻</div>
                <div class="device-name">PC 1</div>
              </div>
              <div class="cable-line" style="min-width:30px;"><div class="cable-label">Cat6</div></div>
              <div class="device-box" style="border-color:var(--soul-blue);">
                <div class="device-icon">🔀</div>
                <div class="device-name" style="color:var(--soul-bright);">Switch</div>
              </div>
              <div class="cable-line" style="min-width:30px;"><div class="cable-label">Cat6</div></div>
              <div class="device-box" style="border-color:var(--fire);">
                <div class="device-icon">📡</div>
                <div class="device-name" style="color:var(--fire-bright);">Router</div>
              </div>
              <div class="cable-line" style="min-width:30px;"><div class="cable-label">WAN</div></div>
              <div class="device-box" style="border-color:var(--gold);">
                <div class="device-icon">🌐</div>
                <div class="device-name" style="color:var(--gold);">Internet</div>
              </div>
            </div>
            <div class="cable-diagram" style="justify-content:flex-start; padding-left:20px; gap:4px;">
              <div class="device-box">
                <div class="device-icon">💻</div>
                <div class="device-name">PC 2</div>
              </div>
              <div class="cable-line" style="min-width:30px;"></div>
              <div style="width:68px; text-align:center; font-family:'Cinzel',serif; font-size:9px; color:var(--ash-dim); padding-top:10px;">↑<br>Mismo Switch</div>
            </div>
          </div>
        </div>
        <div class="scroll-tablet">
          <h3>Paso a paso</h3>
          <ul>
            <li>PC se conecta a un puerto del switch (RJ-45)</li>
            <li>Switch conecta al router por su uplink port</li>
            <li>Router asigna IPs vía DHCP a los dispositivos</li>
            <li>El router enruta el tráfico LAN → WAN</li>
          </ul>
        </div>
      </div>
    </div>
  </div>

  <!-- SLIDE 6: NIVEL 6 — TABLA DE ENRUTAMIENTO -->
  <div class="slide" id="slide-6">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="level-header">
      <div class="level-tag">🔥 Bonfire VI — El Grimorio de Rutas</div>
      <h2 class="level-title">Tabla de Enrutamiento</h2>
    </div>

    <div class="content-grid cols-12" style="max-width:960px;">
      <div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>¿Qué es?</h3>
          <p>La tabla de enrutamiento es el mapa de rutas almacenado en un router. Contiene todas las redes que el router conoce y cómo alcanzarlas.</p>
        </div>
        <div class="scroll-tablet" style="margin-bottom:14px;">
          <h3>Información que contiene</h3>
          <ul>
            <li><strong style="color:var(--fire-bright)">Red destino</strong> — IP de la red objetivo</li>
            <li><strong style="color:var(--fire-bright)">Máscara</strong> — define el tamaño de la red</li>
            <li><strong style="color:var(--fire-bright)">Next Hop</strong> — siguiente router en el camino</li>
            <li><strong style="color:var(--fire-bright)">Interfaz</strong> — por dónde salir</li>
            <li><strong style="color:var(--fire-bright)">Métrica</strong> — costo de la ruta</li>
            <li><strong style="color:var(--fire-bright)">Origen</strong> — C=conectada, S=estática, O=OSPF</li>
          </ul>
        </div>
        <div class="lore-card">
          <p>Si no existe ruta para el destino: el paquete es descartado y se envía un mensaje ICMP "Destination Unreachable" al origen. <em>El mensajero retorna con las manos vacías.</em></p>
        </div>
      </div>
      <div>
        <div style="overflow-x:auto;">
          <table class="routing-table">
            <thead>
              <tr>
                <th>Origen</th>
                <th>Red Destino</th>
                <th>Máscara</th>
                <th>Next Hop</th>
                <th>Interfaz</th>
                <th>Métrica</th>
              </tr>
            </thead>
            <tbody>
              <tr class="highlight">
                <td>C</td>
                <td>192.168.1.0</td>
                <td>/24</td>
                <td>Directa</td>
                <td>Gi0/0</td>
                <td>0</td>
              </tr>
              <tr class="highlight">
                <td>C</td>
                <td>10.0.0.0</td>
                <td>/30</td>
                <td>Directa</td>
                <td>Gi0/1</td>
                <td>0</td>
              </tr>
              <tr>
                <td>S</td>
                <td>172.16.0.0</td>
                <td>/16</td>
                <td>10.0.0.2</td>
                <td>Gi0/1</td>
                <td>1</td>
              </tr>
              <tr>
                <td>O</td>
                <td>192.168.2.0</td>
                <td>/24</td>
                <td>10.0.0.2</td>
                <td>Gi0/1</td>
                <td>110</td>
              </tr>
              <tr>
                <td>S*</td>
                <td>0.0.0.0</td>
                <td>/0</td>
                <td>203.0.113.1</td>
                <td>Gi0/2</td>
                <td>1</td>
              </tr>
            </tbody>
          </table>
        </div>
        <p style="font-size:11px; color:var(--ash-dim); margin-top:8px; font-style:italic;">C=Conectada directamente &ensp;|&ensp; S=Estática &ensp;|&ensp; O=OSPF &ensp;|&ensp; S*=Ruta por defecto</p>
        <div class="scroll-tablet" style="margin-top:12px;">
          <h3>¿Cómo ayuda al router?</h3>
          <p>El router busca la entrada que coincide mejor con la IP destino del paquete (longest prefix match) y lo reenvía según esa entrada. Sin tabla de rutas, el router es ciego.</p>
        </div>
      </div>
    </div>
  </div>

  <!-- SLIDE 7: NIVEL FINAL — CONCLUSIÓN -->
  <div class="slide" id="slide-7">
    <div class="corner-tl"></div><div class="corner-tr"></div>
    <div class="corner-bl"></div><div class="corner-br"></div>

    <div class="level-header">
      <div class="level-tag" style="color:var(--gold); border-color:var(--gold);">👑 Nivel Final — El Señor de los Anillos de Red</div>
      <h2 class="level-title">Conclusión: La Victoria del Routing</h2>
    </div>

    <div style="max-width:800px; width:100%;">
      <div class="conclusion-box" style="margin-bottom:16px;">
        <h3>¿Por qué es importante el routing?</h3>
        <p>El routing es la columna vertebral de Internet. Sin él, los paquetes no sabrían a dónde ir. Cada mensaje, video, juego o llamada que hacemos depende de que cientos de routers tomen decisiones correctas en milisegundos. Sin routing, la red sería una isla sin conexión al mundo.</p>
      </div>

      <div class="conclusion-box" style="margin-bottom:16px; border-top-color: var(--soul-blue);">
        <h3>Tema más interesante</h3>
        <p>La <strong style="color:var(--fire-bright)">tabla de enrutamiento</strong> resultó fascinante: es el "cerebro" del router, el grimorio donde están escritas todas las rutas posibles. La forma en que un router elige la ruta con el prefijo más largo (longest prefix match) es un algoritmo elegante que opera en tiempo real sobre millones de entradas en Internet.</p>
      </div>

      <div class="conclusion-box" style="border-top-color: var(--gold);">
        <h3>Aplicación en redes reales</h3>
        <p>En redes empresariales, el enrutamiento dinámico con OSPF mantiene la conectividad entre sucursales aunque fallen enlaces. En Internet, BGP enruta tráfico entre los sistemas autónomos de todo el mundo. En redes domésticas, la ruta por defecto nos conecta al ISP. El routing está activo cada segundo que estamos en línea.</p>
      </div>

      <div style="margin-top:20px; text-align:center;">
        <div class="divider"><span class="divider-icon" style="font-size:20px;">🔥</span></div>
        <p style="font-family:'Cinzel Decorative',serif; font-size:20px; color:var(--gold); letter-spacing:3px; margin-top:12px;">
          MISIÓN COMPLETADA
        </p>
        <p style="font-family:'Cinzel',serif; font-size:10px; letter-spacing:4px; color:var(--fire); margin-top:6px;">
          RED RESTAURADA &ensp;·&ensp; EMPRESA RECONECTADA &ensp;·&ensp; ALMAS RECOLECTADAS
        </p>
      </div>
    </div>
  </div>

</div><!-- /presentation -->

<!-- CONTROLS -->
<div id="controls">
  <button class="ctrl-btn" id="btn-prev" onclick="navigate(-1)" disabled>◀ Retroceder</button>
  <span id="slide-num">1 / 8</span>
  <button class="ctrl-btn" id="btn-next" onclick="navigate(1)">Avanzar ▶</button>
</div>

<script>
const slides = document.querySelectorAll('.slide');
const total = slides.length;
let current = 0;

const bonfireNames = [
  'Portada', 'Bonfire I', 'Bonfire II', 'Boss Battle',
  'Bonfire IV', 'Bonfire V', 'Bonfire VI', 'Nivel Final'
];

const soulValues = [0, 500, 1200, 3000, 5000, 7500, 10000, 15000];

function buildBonfireBar() {
  const bar = document.getElementById('bonfire-bar');
  for (let i = 1; i < total; i++) {
    const node = document.createElement('div');
    node.className = 'bonfire-node';
    node.id = 'bf-' + i;
    node.innerHTML = '<div class="flame"></div>';
    bar.appendChild(node);
  }
}

function updateHUD(idx) {
  document.getElementById('slide-title-hud').textContent = bonfireNames[idx] || '';
  document.getElementById('slide-num').textContent = (idx + 1) + ' / ' + total;
  document.getElementById('soul-count').textContent = soulValues[idx].toLocaleString();

  // Lit bonfires up to current
  for (let i = 1; i < total; i++) {
    const node = document.getElementById('bf-' + i);
    if (node) {
      if (i <= idx) node.classList.add('lit');
      else node.classList.remove('lit');
    }
  }

  document.getElementById('btn-prev').disabled = idx === 0;
  document.getElementById('btn-next').disabled = idx === total - 1;
}

function showBonfireNotif(idx) {
  if (idx === 0) return;
  const notif = document.getElementById('bonfire-notif');
  const text = document.getElementById('notif-text');
  const msgs = ['', 'Routing Descubierto', 'Tipos Dominados', 'Boss Derrotado', 'IP Comprendida', 'Switch Conectado', 'Tabla Aprendida', 'Victoria Final'];
  text.textContent = msgs[idx] || bonfireNames[idx];
  notif.classList.add('show');
  setTimeout(() => notif.classList.remove('show'), 2200);
}

function navigate(dir) {
  const fog = document.getElementById('fog-gate');
  fog.classList.add('active');

  setTimeout(() => {
    slides[current].classList.remove('active');
    current = Math.max(0, Math.min(total - 1, current + dir));
    slides[current].classList.add('active');
    updateHUD(current);
    if (dir > 0) showBonfireNotif(current);
    fog.classList.remove('active');
  }, 280);
}

document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowRight' || e.key === 'ArrowDown' || e.key === ' ') {
    if (current < total - 1) navigate(1);
  }
  if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
    if (current > 0) navigate(-1);
  }
});

buildBonfireBar();
updateHUD(0);
</script>
</body>
</html>
