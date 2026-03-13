<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Crimen y Castigo — Reseña Interactiva</title>
<link href="https://fonts.googleapis.com/css2?family=IM+Fell+English:ital@0;1&family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=Cinzel:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --ink: #1a0a05;
    --parchment: #f5ede0;
    --blood: #8b1a1a;
    --gold: #c9a84c;
    --shadow: #3d1c0e;
    --muted: #7a5c4a;
    --accent1: #c94040;
    --accent2: #4090c9;
    --cream: #faf3e8;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background-color: var(--ink);
    color: var(--parchment);
    font-family: 'Cormorant Garamond', Georgia, serif;
    font-size: 18px;
    line-height: 1.75;
    overflow-x: hidden;
  }

  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 60px 40px;
    background:
      radial-gradient(ellipse at 50% 100%, rgba(139,26,26,0.35) 0%, transparent 70%),
      linear-gradient(180deg, #0a0402 0%, #1a0a05 60%, #0f0302 100%);
    overflow: hidden;
  }

  .hero::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
      repeating-linear-gradient(0deg, transparent, transparent 60px, rgba(201,168,76,0.03) 60px, rgba(201,168,76,0.03) 61px),
      repeating-linear-gradient(90deg, transparent, transparent 60px, rgba(201,168,76,0.03) 60px, rgba(201,168,76,0.03) 61px);
    pointer-events: none;
  }

  .hero-year {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 6px;
    color: var(--gold);
    opacity: 0.7;
    margin-bottom: 32px;
    animation: fadeUp 1s ease 0.2s both;
  }

  .hero-title {
    font-family: 'Cinzel', serif;
    font-size: clamp(42px, 8vw, 100px);
    font-weight: 700;
    line-height: 1.05;
    background: linear-gradient(135deg, var(--gold) 0%, #e8c97a 40%, var(--gold) 70%, #a07830 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: fadeUp 1s ease 0.4s both;
  }

  .hero-amp {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: 0.55em;
    display: block;
    -webkit-text-fill-color: rgba(201,168,76,0.5);
    letter-spacing: 2px;
    margin: 8px 0;
  }

  .hero-author {
    font-family: 'Cormorant Garamond', serif;
    font-style: italic;
    font-size: clamp(16px, 2.5vw, 22px);
    color: var(--muted);
    letter-spacing: 3px;
    margin-top: 24px;
    animation: fadeUp 1s ease 0.6s both;
  }

  .hero-divider {
    width: 120px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--gold), transparent);
    margin: 40px auto;
    animation: fadeUp 1s ease 0.8s both;
  }

  .hero-tagline {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: clamp(15px, 2vw, 20px);
    color: rgba(245,237,224,0.55);
    max-width: 600px;
    animation: fadeUp 1s ease 1s both;
  }

  .scroll-cta {
    position: absolute;
    bottom: 36px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    cursor: pointer;
    animation: fadeUp 1s ease 1.4s both;
    color: var(--gold);
    opacity: 0.6;
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 4px;
    transition: opacity 0.3s;
  }
  .scroll-cta:hover { opacity: 1; }
  .scroll-arrow {
    width: 20px; height: 20px;
    border-right: 1px solid var(--gold);
    border-bottom: 1px solid var(--gold);
    transform: rotate(45deg);
    animation: bounce 2s infinite;
  }

  @keyframes bounce {
    0%,100% { transform: rotate(45deg) translateY(0); }
    50% { transform: rotate(45deg) translateY(6px); }
  }
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .nav-tabs {
    position: sticky;
    top: 0;
    z-index: 100;
    display: flex;
    justify-content: center;
    background: rgba(10,4,2,0.97);
    border-bottom: 1px solid rgba(201,168,76,0.2);
    backdrop-filter: blur(12px);
    padding: 0 20px;
    flex-wrap: wrap;
  }

  .tab-btn {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 3px;
    color: var(--muted);
    background: none;
    border: none;
    padding: 20px 24px;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
    white-space: nowrap;
  }
  .tab-btn::after {
    content: '';
    position: absolute;
    bottom: 0; left: 50%; right: 50%;
    height: 2px;
    background: var(--gold);
    transition: all 0.3s;
  }
  .tab-btn:hover { color: var(--parchment); }
  .tab-btn.active { color: var(--gold); }
  .tab-btn.active::after { left: 15%; right: 15%; }

  .section { display: none; animation: sectionIn 0.5s ease both; }
  .section.active { display: block; }
  @keyframes sectionIn {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .synopsis-wrap {
    max-width: 860px;
    margin: 80px auto;
    padding: 0 40px;
  }

  .section-label {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 5px;
    color: var(--gold);
    opacity: 0.7;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .section-label::before, .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(201,168,76,0.3));
  }
  .section-label::after { background: linear-gradient(90deg, rgba(201,168,76,0.3), transparent); }

  .section-heading {
    font-family: 'IM Fell English', serif;
    font-size: clamp(32px, 5vw, 52px);
    font-weight: 400;
    font-style: italic;
    line-height: 1.2;
    margin-bottom: 32px;
    color: var(--cream);
  }

  .synopsis-text {
    font-size: 19px;
    color: rgba(245,237,224,0.82);
  }

  .data-strip {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1px;
    background: rgba(201,168,76,0.15);
    margin: 60px 0;
    border: 1px solid rgba(201,168,76,0.15);
  }
  .data-card {
    background: rgba(26,10,5,0.9);
    padding: 28px 20px;
    text-align: center;
  }
  .data-card .label {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 3px;
    color: var(--gold);
    opacity: 0.6;
    margin-bottom: 10px;
    display: block;
  }
  .data-card .value {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: 20px;
    color: var(--cream);
  }

  .source-panel {
    max-width: 900px;
    margin: 0 auto;
    padding: 60px 50px;
  }
  .source-panel-1 { background: linear-gradient(160deg, #1a0808 0%, #0f0404 100%); }
  .source-panel-2 { background: linear-gradient(160deg, #081018 0%, #040a10 100%); }

  .source-badge {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 32px;
    flex-wrap: wrap;
  }
  .source-num {
    font-family: 'Cinzel', serif;
    font-size: 11px;
    letter-spacing: 2px;
    padding: 4px 14px;
    border: 1px solid;
    border-radius: 20px;
  }
  .s1 .source-num { color: var(--accent1); border-color: var(--accent1); }
  .s2 .source-num { color: var(--accent2); border-color: var(--accent2); }
  .source-site {
    font-style: italic;
    font-size: 13px;
    color: var(--muted);
  }

  .source-title {
    font-family: 'IM Fell English', serif;
    font-size: clamp(22px, 3vw, 32px);
    line-height: 1.25;
    margin-bottom: 28px;
    color: var(--cream);
  }
  .source-body { font-size: 17px; line-height: 1.8; color: rgba(245,237,224,0.75); }
  .source-body p { margin-bottom: 20px; }

  .highlight-box {
    border-left: 3px solid;
    padding: 20px 24px;
    margin: 28px 0;
    border-radius: 0 8px 8px 0;
  }
  .s1 .highlight-box { border-color: var(--accent1); background: rgba(201,64,64,0.08); }
  .s2 .highlight-box { border-color: var(--accent2); background: rgba(64,144,201,0.08); }
  .highlight-box p {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: 19px;
    color: var(--parchment);
    margin: 0;
  }

  .tag-list { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 32px; }
  .tag {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 2px;
    padding: 5px 12px;
    border-radius: 20px;
  }
  .s1 .tag { background: rgba(201,64,64,0.12); color: #e07070; }
  .s2 .tag { background: rgba(64,144,201,0.12); color: #70b0e0; }

  .compare-wrap { max-width: 1100px; margin: 80px auto; padding: 0 40px; }

  .compare-table { width: 100%; border-collapse: collapse; margin-top: 48px; }
  .compare-table th {
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 3px;
    padding: 16px 20px;
    text-align: left;
    border-bottom: 1px solid rgba(201,168,76,0.3);
  }
  .compare-table th:nth-child(1) { color: var(--gold); opacity: 0.6; }
  .compare-table th:nth-child(2) { color: var(--accent1); }
  .compare-table th:nth-child(3) { color: var(--accent2); }
  .compare-table td {
    padding: 20px;
    vertical-align: top;
    border-bottom: 1px solid rgba(139,26,26,0.3);
    font-size: 15px;
    line-height: 1.7;
    color: rgba(245,237,224,0.85);
    background: rgba(80,10,10,0.6);
  }
  .compare-table tr:hover td { background: rgba(110,15,15,0.75); }
  .compare-table td:first-child {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--gold);
    opacity: 0.8;
    white-space: nowrap;
  }

  .verdict-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
    margin-top: 60px;
    background: rgba(201,168,76,0.1);
  }
  @media(max-width:600px) { .verdict-grid { grid-template-columns: 1fr; } }

  .verdict-card { padding: 44px 38px; }
  .verdict-card:first-child { background: rgba(201,64,64,0.07); }
  .verdict-card:last-child  { background: rgba(64,144,201,0.07); }

  .verdict-label {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 4px;
    margin-bottom: 16px;
    display: block;
  }
  .verdict-card:first-child .verdict-label { color: var(--accent1); }
  .verdict-card:last-child .verdict-label  { color: var(--accent2); }

  .verdict-text {
    font-family: 'IM Fell English', serif;
    font-style: italic;
    font-size: clamp(17px, 2vw, 22px);
    line-height: 1.55;
    color: var(--cream);
  }

  .chars-wrap { max-width: 1000px; margin: 80px auto; padding: 0 40px; }
  .chars-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 2px;
    background: rgba(201,168,76,0.1);
    margin-top: 48px;
  }
  .char-card {
    background: linear-gradient(160deg, #120808 0%, #0c0404 100%);
    padding: 36px 28px;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }
  .char-card::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 2px;
    background: var(--blood);
    transform: scaleX(0);
    transition: transform 0.3s;
  }
  .char-card:hover { background: linear-gradient(160deg, #1c0a0a 0%, #150606 100%); }
  .char-card:hover::after { transform: scaleX(1); }

  .char-initial {
    font-family: 'Cinzel', serif;
    font-size: 48px;
    color: rgba(139,26,26,0.3);
    line-height: 1;
    margin-bottom: 12px;
    transition: color 0.3s;
  }
  .char-card:hover .char-initial { color: rgba(139,26,26,0.6); }
  .char-name { font-family: 'IM Fell English', serif; font-size: 22px; color: var(--cream); margin-bottom: 8px; }
  .char-role {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 3px;
    color: var(--gold);
    opacity: 0.6;
    margin-bottom: 14px;
    display: block;
  }
  .char-desc { font-size: 15px; color: rgba(245,237,224,0.6); line-height: 1.7; }
  .char-detail {
    display: none;
    margin-top: 14px;
    padding-top: 14px;
    border-top: 1px solid rgba(201,168,76,0.15);
    font-size: 14px;
    color: rgba(245,237,224,0.5);
    line-height: 1.7;
    font-style: italic;
  }
  .char-card.open .char-detail { display: block; }
  .char-toggle {
    font-family: 'Cinzel', serif;
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--gold);
    opacity: 0.5;
    margin-top: 12px;
    display: block;
  }

  .themes-wrap { max-width: 900px; margin: 80px auto; padding: 0 40px 100px; }
  .theme-item {
    display: grid;
    grid-template-columns: 80px 1fr;
    gap: 28px;
    align-items: start;
    padding: 36px 0;
    border-bottom: 1px solid rgba(201,168,76,0.08);
    opacity: 0;
    transform: translateX(-20px);
    transition: all 0.5s;
  }
  .theme-item.visible { opacity: 1; transform: translateX(0); }
  .theme-num {
    font-family: 'Cinzel', serif;
    font-size: 48px;
    font-weight: 700;
    color: rgba(139,26,26,0.22);
    line-height: 1;
    text-align: right;
  }
  .theme-content h3 { font-family: 'IM Fell English', serif; font-size: 26px; color: var(--cream); margin-bottom: 10px; }
  .theme-content p { font-size: 16px; color: rgba(245,237,224,0.65); line-height: 1.8; }

  footer {
    text-align: center;
    padding: 60px 20px;
    border-top: 1px solid rgba(201,168,76,0.1);
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 3px;
    color: var(--muted);
    opacity: 0.6;
  }
  footer a { color: var(--gold); text-decoration: none; opacity: 0.7; }
  footer a:hover { opacity: 1; }

  @media(max-width:600px) {
    .source-panel { padding: 36px 24px; }
    .compare-wrap, .chars-wrap, .themes-wrap, .synopsis-wrap { padding: 0 20px; }
    .compare-table { font-size: 13px; }
    .compare-table td, .compare-table th { padding: 14px 10px; }
  }
</style>
</head>
<body>

<section class="hero">
  <div class="hero-year">FIÓDOR MIJÁILOVICH DOSTOIEVSKI · 1866</div>
  <h1 class="hero-title">
    Crimen
    <span class="hero-amp">y</span>
    Castigo
  </h1>
  <p class="hero-author">Reseña Interactiva · Dos Perspectivas</p>
  <p style="font-family:'Cormorant Garamond',serif;font-size:clamp(14px,1.8vw,18px);color:rgba(201,168,76,0.6);letter-spacing:2px;margin-top:10px;animation:fadeUp 1s ease 0.7s both;">por Roberto Monforte e Itzel Ku</p>
  <div class="hero-divider"></div>
  <p class="hero-tagline">«En los dilemas de Raskólnikov están la ansiedad, el pánico, la depresión, la esperanza, el amor, la muerte, el odio…»</p>
  <div class="scroll-cta" onclick="document.querySelector('.nav-tabs').scrollIntoView({behavior:'smooth'})">
    EXPLORAR
    <div class="scroll-arrow"></div>
  </div>
</section>

<nav class="nav-tabs">
  <button class="tab-btn active" onclick="showTab('sinopsis',this)">SINOPSIS</button>
  <button class="tab-btn" onclick="showTab('fuente1',this)">FUENTE I</button>
  <button class="tab-btn" onclick="showTab('fuente2',this)">FUENTE II</button>
  <button class="tab-btn" onclick="showTab('comparativa',this)">COMPARATIVA</button>
  <button class="tab-btn" onclick="showTab('personajes',this)">PERSONAJES</button>
  <button class="tab-btn" onclick="showTab('temas',this)">TEMAS</button>
</nav>

<!-- SINOPSIS -->
<section class="section active" id="sinopsis">
  <div class="synopsis-wrap">
    <div class="section-label">PRESENTACIÓN DE LA OBRA</div>
    <h2 class="section-heading">Una novela sobre el alma que se desgarra</h2>
    <p class="synopsis-text">
      <em>Crimen y castigo</em> (1866) es una de las novelas más influyentes de la literatura universal.
      Rodión Raskólnikov, exestudiante hundido en la miseria en San Petersburgo, planea asesinar a una
      usurera convencido de que su superioridad intelectual lo sitúa por encima de la moral convencional.
    </p>
    <br>
    <p class="synopsis-text">
      Pero el verdadero crimen no es el asesinato: es la guerra interior que lo consume. La paranoia,
      la culpa y la alienación son el castigo real, mucho antes de que la justicia formal intervenga.
      Dostoievski construye así una obra que es novela psicológica, ensayo filosófico y drama moral al mismo tiempo.
    </p>

    <div class="data-strip">
      <div class="data-card"><span class="label">PUBLICACIÓN</span><span class="value">1866</span></div>
      <div class="data-card"><span class="label">GÉNERO</span><span class="value">Novela psicológica</span></div>
      <div class="data-card"><span class="label">ESCENARIO</span><span class="value">San Petersburgo</span></div>
      <div class="data-card"><span class="label">CORRIENTE</span><span class="value">Realismo · Existencialismo</span></div>
      <div class="data-card"><span class="label">PROTAGONISTA</span><span class="value">Rodión Raskólnikov</span></div>
    </div>

    <div class="section-label">DOS LECTURAS</div>
    <h3 style="font-family:'IM Fell English',serif;font-size:26px;font-style:italic;color:var(--cream);margin-bottom:20px;">Una obra, dos miradas</h3>
    <p class="synopsis-text">
      La <strong style="color:var(--accent1)">Fuente 1</strong> (<em>Leer es vivir dos veces</em>) es una reseña apasionada y personal que destaca la atmósfera psicológica sofocante y los magistrales diálogos entre Raskólnikov y el inspector Porfiri.
    </p>
    <br>
    <p class="synopsis-text">
      La <strong style="color:var(--accent2)">Fuente 2</strong> (<em>Cultura Genial</em>) adopta un enfoque analítico y académico, situando la novela como ensayo filosófico sobre la moral, el individuo y la sociedad, e insertando a Dostoievski como pionero del existencialismo literario.
    </p>
    <br>
    <p class="synopsis-text" style="color:rgba(245,237,224,0.5);font-style:italic;">
      Navega por las pestañas para explorar cada fuente, su comparativa y los elementos clave de la obra.
    </p>
  </div>
</section>

<!-- FUENTE 1 -->
<section class="section s1" id="fuente1">
  <div class="source-panel source-panel-1">
    <div class="source-badge">
      <span class="source-num">FUENTE I</span>
      <span class="source-site">leeresvivirdosveces.com · Enero 2022</span>
    </div>
    <h2 class="source-title">«Lo que los rusos hicieron con la Literatura está a la altura de muy pocos»</h2>
    <div class="source-body">
      <p>
        Esta reseña, publicada en el blog literario <em>Leer es vivir dos veces</em>, parte de una premisa
        entusiasta: la literatura rusa del siglo XIX es incomparable. El reseñador sitúa <em>Crimen y castigo</em>
        junto a <em>Guerra y paz</em> y <em>Anna Karénina</em> como una de las novelas más influyentes
        e internacionales de la tradición rusa.
      </p>
      <div class="highlight-box">
        <p>El ambiente de asfixia que consigue generar Dostoievski en la mente del protagonista es el principal atractivo de la lectura.</p>
      </div>
      <p>
        Para esta fuente, el corazón de la novela es la tensión psicológica sostenida: la permanente
        incertidumbre entre librarse del castigo o confesar, los momentos en que Raskólnikov casi es
        detenido, y sobre todo el juego de inteligencias que mantiene con el inspector Porfiri.
        El reseñador considera esos diálogos —en especial los de los capítulos V de la tercera y cuarta
        parte— una de las cimas de la literatura universal, valoración que comparte con Stefan Zweig.
      </p>
      <p>
        La fuente destaca también el papel de Sonia: para Dostoievski, únicamente la fe puede sanar
        la depravación humana. Y subraya que el nombre de Raskólnikov, derivado del ruso «escisión»,
        no es casual: apunta a su separación de la sociedad y de sus propias emociones.
      </p>
      <div class="highlight-box">
        <p>Dos siglos después, nadie ha superado a Tolstói y Dostoievski, y seguimos leyendo sus novelas del siglo XIX para entender a la humanidad de hoy.</p>
      </div>
      <p>
        La reseña concluye con una reflexión sobre la vigencia atemporal de estos autores: lo que hicieron
        con la literatura solo es comparable, quizás, con Cervantes.
      </p>
    </div>
    <div class="tag-list">
      <span class="tag">PSICOLOGÍA</span>
      <span class="tag">TENSIÓN NARRATIVA</span>
      <span class="tag">PORFIRI VS. RASKÓLNIKOV</span>
      <span class="tag">FE Y REDENCIÓN</span>
      <span class="tag">LITERATURA RUSA</span>
      <span class="tag">RESEÑA PERSONAL</span>
    </div>
  </div>
</section>

<!-- FUENTE 2 -->
<section class="section s2" id="fuente2">
  <div class="source-panel source-panel-2">
    <div class="source-badge">
      <span class="source-num">FUENTE II</span>
      <span class="source-site">culturagenial.com · Análisis académico</span>
    </div>
    <h2 class="source-title">«Crimen y castigo» como novela-ensayo filosófico y social</h2>
    <div class="source-body">
      <p>
        <em>Cultura Genial</em> aborda la novela desde una perspectiva analítica y estructural,
        definiéndola no solo como obra de ficción sino como un ensayo filosófico sobre la moral y
        la relación del individuo con la sociedad. El mundo interno de los personajes tiene tanta
        importancia como el externo, lo que aproxima la obra a un tratado de psicología humana.
      </p>
      <div class="highlight-box">
        <p>La novela adquiere un tono de ensayo filosófico sobre la moral y la relación del individuo con la sociedad rusa: extremadamente pudorosa, católica, zarista y aristocrática.</p>
      </div>
      <p>
        La fuente plantea la pregunta central: ¿puede el asesinato de una persona despreciable ser
        moralmente justificable si el objetivo es superior? Raskólnikov cree en la existencia de hombres
        extraordinarios que trascienden la moral común, pero la novela desmonta esa teoría a través
        de sus consecuencias.
      </p>
      <p>
        Un aspecto peculiar es la interpretación del comportamiento de Raskólnikov tras el crimen:
        sus provocaciones ante el juez, su delirio y su incapacidad para aprovechar lo robado parecen
        indicar que, inconscientemente, el protagonista busca ser descubierto y castigado. El castigo
        no viene de fuera: es deseado desde dentro.
      </p>
      <div class="highlight-box">
        <p>Pareciera que Raskolnikov busca el castigo desde el primer segundo después del crimen, como si la confesión fuera la única salida posible a su delirio.</p>
      </div>
      <p>
        La fuente contextualiza la obra en la biografía del autor: Dostoievski estuvo preso en 1849,
        fue exiliado a Siberia y convivió nueve años con asesinos y criminales. Esa experiencia directa
        es la base semiautobiográfica que da autenticidad a la novela. Finalmente, sitúa a Dostoievski
        como pionero del existencialismo literario, anticipando la narrativa introspectiva del siglo XX.
      </p>
    </div>
    <div class="tag-list">
      <span class="tag">ENSAYO FILOSÓFICO</span>
      <span class="tag">MORAL Y SOCIEDAD</span>
      <span class="tag">EXISTENCIALISMO</span>
      <span class="tag">ANÁLISIS ESTRUCTURAL</span>
      <span class="tag">CONTEXTO BIOGRÁFICO</span>
      <span class="tag">NOVELA-ENSAYO</span>
    </div>
  </div>
</section>

<!-- COMPARATIVA -->
<section class="section" id="comparativa">
  <div class="compare-wrap">
    <div class="section-label">ANÁLISIS COMPARATIVO</div>
    <h2 class="section-heading">Dos miradas sobre la misma oscuridad</h2>

    <table class="compare-table">
      <thead>
        <tr>
          <th>DIMENSIÓN</th>
          <th>⬥ FUENTE I · Leer es vivir dos veces</th>
          <th>⬥ FUENTE II · Cultura Genial</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>ENFOQUE</td>
          <td>Reseña personal y apasionada. Voz subjetiva de un lector entusiasta de la literatura rusa.</td>
          <td>Análisis académico y estructural. Voz editorial que busca contextualizar y explicar la obra.</td>
        </tr>
        <tr>
          <td>ELEMENTO CENTRAL</td>
          <td>La atmósfera psicológica sofocante y la tensión de los diálogos entre Raskólnikov y Porfiri.</td>
          <td>La dimensión filosófica: la pregunta sobre si el crimen puede ser moral, y el individuo ante la sociedad.</td>
        </tr>
        <tr>
          <td>EL CASTIGO</td>
          <td>Es interno y psicológico antes que formal. La confesión final libera al protagonista de su alienación.</td>
          <td>El protagonista parece buscar activamente el castigo desde el primer momento; es una necesidad inconsciente.</td>
        </tr>
        <tr>
          <td>PERSONAJE DE SONIA</td>
          <td>Símbolo de la fe religiosa como único remedio a la depravación humana.</td>
          <td>Parte del núcleo de apoyo que también atormenta moralmente al protagonista en términos morales.</td>
        </tr>
        <tr>
          <td>CONTEXTO LITERARIO</td>
          <td>Sitúa la novela en la cumbre de la literatura rusa junto a Tolstói. Cita a Stefan Zweig.</td>
          <td>Enmarca la obra en el realismo y el existencialismo. Dostoievski como precursor del siglo XX.</td>
        </tr>
        <tr>
          <td>CONTEXTO BIOGRÁFICO</td>
          <td>No se desarrolla. El foco está en la experiencia lectora y el impacto de la obra.</td>
          <td>Central: el encarcelamiento y el exilio del autor nutren directamente la autenticidad de la novela.</td>
        </tr>
        <tr>
          <td>VIGENCIA</td>
          <td>Explícita: leemos el siglo XIX para entender la humanidad de hoy.</td>
          <td>Implícita: la novela anticipa las grandes corrientes narrativas del siglo XX.</td>
        </tr>
        <tr>
          <td>TONO</td>
          <td>Admirativo, íntimo, con juicios propios y exclamaciones entusiastas.</td>
          <td>Expositivo, ordenado temáticamente, sin valoraciones personales.</td>
        </tr>
      </tbody>
    </table>

    <div class="verdict-grid">
      <div class="verdict-card">
        <span class="verdict-label">FUENTE I · SÍNTESIS</span>
        <p class="verdict-text">
          Una reseña que vive la obra como experiencia: valora la tensión, el genio narrativo
          y la capacidad de Dostoievski para trazar el devenir del alma humana con precisión quirúrgica.
          Es la voz de alguien atrapado por el libro.
        </p>
      </div>
      <div class="verdict-card">
        <span class="verdict-label">FUENTE II · SÍNTESIS</span>
        <p class="verdict-text">
          Un análisis que disecciona la obra como artefacto filosófico y social: la entiende como
          un ensayo sobre la moral, el crimen y la conciencia, y sitúa a Dostoievski como
          pionero del existencialismo literario.
        </p>
      </div>
    </div>

    <div style="margin-top:60px;padding:40px;background:rgba(201,168,76,0.04);border:1px solid rgba(201,168,76,0.12);">
      <div class="section-label">PUNTO DE ENCUENTRO</div>
      <p style="font-family:'IM Fell English',serif;font-style:italic;font-size:22px;color:var(--cream);line-height:1.6;text-align:center;">
        Ambas fuentes coinciden en que <em>Crimen y castigo</em> es una obra maestra que trasciende
        su época, que la psicología del protagonista es su elemento más poderoso, y que el verdadero
        castigo de Raskólnikov es interior, mucho antes de que la justicia formal intervenga.
      </p>
    </div>
  </div>
</section>

<!-- PERSONAJES -->
<section class="section" id="personajes">
  <div class="chars-wrap">
    <div class="section-label">GALERÍA DE PERSONAJES</div>
    <h2 class="section-heading">Almas en conflicto</h2>
    <p style="color:rgba(245,237,224,0.45);font-style:italic;margin-top:8px;font-size:15px;">Haz clic en cada tarjeta para ampliar.</p>
    <div class="chars-grid">

      <div class="char-card" onclick="toggleChar(this)">
        <div class="char-initial">R</div>
        <div class="char-name">Rodión Raskólnikov</div>
        <span class="char-role">PROTAGONISTA</span>
        <p class="char-desc">Exestudiante empobrecido que cree en su superioridad intelectual y planea un asesinato para demostrarla.</p>
        <span class="char-toggle">+ VER MÁS</span>
        <div class="char-detail">Su nombre deriva del ruso «escisión», aludiendo a su separación de la sociedad y de sus propias emociones. Comete el crimen creyendo ser un Napoleón, pero la paranoia y la culpa lo consumen desde el primer instante.</div>
      </div>

      <div class="char-card" onclick="toggleChar(this)">
        <div class="char-initial">P</div>
        <div class="char-name">Porfiri Petrovich</div>
        <span class="char-role">INSPECTOR DE POLICÍA</span>
        <p class="char-desc">El antagonista intelectual de Raskólnikov. Lo tiene calado desde el principio y lo desafía con maestría psicológica.</p>
        <span class="char-toggle">+ VER MÁS</span>
        <div class="char-detail">Stefan Zweig consideró sus diálogos con Raskólnikov una de las cimas de la literatura universal. Porfiri nunca acusa directamente: juega, insinúa, observa. Es el espejo que refleja la culpa del protagonista.</div>
      </div>

      <div class="char-card" onclick="toggleChar(this)">
        <div class="char-initial">S</div>
        <div class="char-name">Sonia Marmeladova</div>
        <span class="char-role">FIGURA DE REDENCIÓN</span>
        <p class="char-desc">Joven en extrema miseria que conserva una fe religiosa inquebrantable. Representa la posibilidad de la salvación.</p>
        <span class="char-toggle">+ VER MÁS</span>
        <div class="char-detail">Para la Fuente 1, encarna la idea de que solo la fe cura la depravación. Para la Fuente 2, forma parte del núcleo afectivo que también ejerce presión moral sobre Raskólnikov. Lo acompañará hasta Siberia.</div>
      </div>

      <div class="char-card" onclick="toggleChar(this)">
        <div class="char-initial">A</div>
        <div class="char-name">Alena Ivanovna</div>
        <span class="char-role">LA USURERA · VÍCTIMA</span>
        <p class="char-desc">Prestamista a quien Raskólnikov considera moralmente despreciable y, por tanto, «prescindible» según su teoría.</p>
        <span class="char-toggle">+ VER MÁS</span>
        <div class="char-detail">Su figura encarna la pregunta filosófica central de la novela: ¿puede el asesinato de alguien considerado «vulgar» ser moralmente justificable? Dostoievski usa este personaje para cuestionar la teoría del hombre superior.</div>
      </div>

      <div class="char-card" onclick="toggleChar(this)">
        <div class="char-initial">D</div>
        <div class="char-name">Dunia Raskólnikova</div>
        <span class="char-role">HERMANA DEL PROTAGONISTA</span>
        <p class="char-desc">Su presencia en San Petersburgo intensifica la perturbación moral de Raskólnikov. La familia como espejo de la culpa.</p>
        <span class="char-toggle">+ VER MÁS</span>
        <div class="char-detail">La posibilidad de que su familia descubra el crimen es una tortura constante. Su amor por ella contrasta con la frialdad con la que el protagonista pretendía actuar. La sociedad más íntima también castiga.</div>
      </div>

      <div class="char-card" onclick="toggleChar(this)">
        <div class="char-initial">R</div>
        <div class="char-name">Razumikhin</div>
        <span class="char-role">AMIGO Y APOYO</span>
        <p class="char-desc">Compañero de estudios, representante de la razón práctica frente al idealismo distorsionado del protagonista.</p>
        <span class="char-toggle">+ VER MÁS</span>
        <div class="char-detail">Forma parte del núcleo de apoyo que rodea a Raskólnikov, aunque también contribuye a la tensión moral. Su pragmatismo contrasta con el nihilismo teórico del protagonista y ofrece un contrapeso narrativo esencial.</div>
      </div>

    </div>
  </div>
</section>

<!-- TEMAS -->
<section class="section" id="temas">
  <div class="themes-wrap">
    <div class="section-label">GRANDES TEMAS</div>
    <h2 class="section-heading">Los ejes que sostienen la obra</h2>

    <div class="theme-item"><div class="theme-num">01</div><div class="theme-content">
      <h3>El castigo psicológico</h3>
      <p>Antes de cualquier condena formal, Raskólnikov ya está siendo castigado por su propia mente. La paranoia, el delirio y la culpa son el verdadero castigo de la novela. Ambas fuentes coinciden en que este es el elemento más poderoso de la obra.</p>
    </div></div>

    <div class="theme-item"><div class="theme-num">02</div><div class="theme-content">
      <h3>El hombre superior y la moral</h3>
      <p>Raskólnikov cree que los hombres extraordinarios están por encima de las leyes morales. Esta teoría —que anticipa a Nietzsche— es el motor del crimen y la base filosófica que Dostoievski se propone demoler a lo largo de la novela.</p>
    </div></div>

    <div class="theme-item"><div class="theme-num">03</div><div class="theme-content">
      <h3>El individuo frente a la sociedad</h3>
      <p>La sociedad rusa —católica, zarista, aristocrática— ejerce una presión moral constante. Incluso cuando Raskólnikov rechaza intelectualmente la culpa, la mirada de los demás lo persigue y lo doblega desde dentro.</p>
    </div></div>

    <div class="theme-item"><div class="theme-num">04</div><div class="theme-content">
      <h3>La fe como redención</h3>
      <p>A través de Sonia, Dostoievski propone que solo la fe puede salvar al hombre de su propia destrucción. No la razón ni la ley: es la rendición espiritual lo que finalmente redime a Raskólnikov en el epílogo.</p>
    </div></div>

    <div class="theme-item"><div class="theme-num">05</div><div class="theme-content">
      <h3>El existencialismo avant la lettre</h3>
      <p>Los monólogos interiores y la introspección radical anticipan el existencialismo literario del siglo XX. Dostoievski fue un pionero de la narrativa de conciencia que luego dominaría en Joyce, Kafka y Camus.</p>
    </div></div>

    <div class="theme-item"><div class="theme-num">06</div><div class="theme-content">
      <h3>La tensión como técnica narrativa</h3>
      <p>Cada encuentro con Porfiri, cada conversación casual podría ser el momento de la caída. Esa incertidumbre sostenida es lo que convierte a Dostoievski en un maestro del suspense psicológico y hace la lectura adictiva.</p>
    </div></div>

  </div>
</section>

<footer>
  <p>Material interactivo elaborado a partir de las fuentes propuestas</p>
  <p style="margin-top:12px;">
    <a href="https://leeresvivirdosveces.com/2022/01/05/resena-de-crimen-y-castigo-de-fiodor-m-dostoievski/" target="_blank">FUENTE I</a>
    &nbsp;·&nbsp;
    <a href="https://www.culturagenial.com/es/libro-crimen-y-castigo-de-fiodor-dostoyevski/" target="_blank">FUENTE II</a>
  </p>
</footer>

<script>
  function showTab(id, btn) {
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    btn.classList.add('active');
    if (id === 'temas') setTimeout(observeThemes, 100);
    window.scrollTo({ top: document.querySelector('.nav-tabs').offsetTop - 10, behavior: 'smooth' });
  }

  function toggleChar(el) {
    el.classList.toggle('open');
    const toggle = el.querySelector('.char-toggle');
    if (toggle) toggle.textContent = el.classList.contains('open') ? '− CERRAR' : '+ VER MÁS';
  }

  function observeThemes() {
    const items = document.querySelectorAll('.theme-item');
    const obs = new IntersectionObserver((entries) => {
      entries.forEach((e, i) => {
        if (e.isIntersecting) setTimeout(() => e.target.classList.add('visible'), i * 120);
      });
    }, { threshold: 0.1 });
    items.forEach(item => obs.observe(item));
  }

  document.addEventListener('DOMContentLoaded', observeThemes);
</script>
</body>
</html>
