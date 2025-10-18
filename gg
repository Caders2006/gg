<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>INTRADEL - Trading Journal & AI Mentor</title>

<!-- Charts (used for BOTH real stats and homepage mockups) -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<!-- REAL AI - Transformers.js for client-side AI model -->
<script src="https://cdn.jsdelivr.net/npm/@xenova/transformers@2.17.1"></script>


<style>
  /* =========================
     UI Variant — "Carbon Glow Matrix"
     ========================= */
  :root{
    --bg-0:#060708;
    --bg-1:#080b10;
    --bg-2:#0a0f14;

    --text:#e8eef7;
    --muted:#9fb0c8;

    /* Default to Red & Black theme */
    --accent: #ff6b6b;
    --accent-strong: #ff8585;
    --accent-soft: #ffbaba;
    --accent-rgb: 255, 107, 107;
    --btn-glow: 0 10px 24px rgba(255,107,107,.22);

    --success:#2ddc90;
    --danger:#ff6b6b;

    --stroke-weak: rgba(255,255,255,0.08);
    --stroke-strong: rgba(255,255,255,0.16);
    --shadow-1: 0 10px 26px rgba(0,0,0,.34);
    --shadow-2: 0 22px 44px rgba(0,0,0,.45);
    
    --card-radius: 14px;
    --btn-radius: 12px;

    --header-height: 66px; /* Approximate height of header */
    --footer-height: 64px;
    --glass-blur: 12px;

    /* NEW: Variables for homepage color interaction demo */
    --mock-accent-color: var(--accent);
    --mock-success-color: var(--success);
    --mock-danger-color: var(--danger);
  }

  /* NEW: Light Theme */
  body.theme-light {
    --bg-0: #f5f7fa;
    --bg-1: #ffffff;
    --bg-2: #f0f2f5;
    --text: #1f2937;
    --muted: #6b7280;
    --stroke-weak: rgba(0,0,0,0.08);
    --stroke-strong: rgba(0,0,0,0.16);
    --shadow-1: 0 4px 12px rgba(0,0,0,.08);
    --shadow-2: 0 10px 25px rgba(0,0,0,.1);
    --btn-glow: 0 8px 20px rgba(var(--accent-rgb),.2);
  }
  body.theme-light .btn-primary { color: #fff; }
  body.theme-light .btn-secondary { color: var(--text); }
  body.theme-light .ai-chat-bubble.user { color: #fff; }
  body.theme-light .profile-role-badge { color: #fff; }
  body.theme-light .logo {
    background: linear-gradient(90deg, var(--accent), #1f2937);
    -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color:transparent;
  }
  body.theme-light body:before, body.theme-light body:after { display: none; }
  body.theme-light .fx-stripes { display: none; }
  body.theme-light #bgCanvas { opacity: 0.2; }
  body.theme-light header, body.theme-light footer {
    background: linear-gradient(180deg, rgba(255,255,255,.8), rgba(255,255,255,.7));
  }
  body.theme-light .modal .modal-content {
      background: linear-gradient(180deg, rgba(255,255,255,.98), rgba(248,250,252,.98));
  }
  body.theme-light select option{background:var(--bg-1);color:var(--text)}


  *{box-sizing:border-box;margin:0;padding:0}
  html,body{height:100%;}
  body{
    display: flex;
    flex-direction: column;
    font-family: ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Ubuntu, Cantarell, "Helvetica Neue", Arial, "Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol";
    color:var(--text); line-height:1.6; position:relative;

    background:
      radial-gradient(1100px 900px at 120% -10%, rgba(255,115,80,0.16), transparent 55%),
      radial-gradient(900px 700px at -15% 0%, rgba(var(--accent-rgb),0.22), transparent 58%),
      linear-gradient(180deg, var(--bg-1), var(--bg-2));
  }
  
  /* Scrolling behavior */
  #homePage { overflow-y: auto; scroll-behavior: smooth; }
  body:not(.page-home) { overflow: hidden; }
  
  /* Scrollbar Hiding Fix */
  #homePage, #dashboardPage .tab, body {
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
  }
  #homePage::-webkit-scrollbar, #dashboardPage .tab::-webkit-scrollbar, body::-webkit-scrollbar {
    display: none; /* Chrome, Safari, and Opera */
  }


  /* Matrix lines + scan overlay */
  body:before{
    content:"";
    position:fixed; inset:0; pointer-events:none; z-index:-6;
    background:
      linear-gradient(transparent 29px, rgba(255,255,255,0.03) 30px),
      linear-gradient(90deg, transparent 29px, rgba(255,255,255,0.03) 30px);
    background-size: 30px 30px, 30px 30px;
    mask-image: radial-gradient(1400px 900px at 50% 8%, rgba(0,0,0,.85), rgba(0,0,0,1) 65%);
  }
  body:after{
    content:"";
    position:fixed; inset:0; pointer-events:none; z-index:-5;
    background: linear-gradient( to bottom, rgba(255,255,255,0.04), rgba(255,255,255,0.02) );
  }

  /* Subtle animated diagonals */
  .fx-stripes{
    position:fixed; inset:-20% -20% -10% -20%; z-index:-4; pointer-events:none; overflow:hidden;
    background-image:
      linear-gradient(115deg, rgba(var(--accent-rgb),.12) 0%, transparent 12%, transparent 88%, rgba(255,115,80,.10) 100%);
    animation: sweep 18s ease-in-out infinite alternate;
    filter: blur(10px);
  }
  @keyframes sweep{
    0%{ transform: translate3d(-2%,0,0) }
    100%{ transform: translate3d(2%,0,0) }
  }

  /* Starfield canvas (kept) */
  #bgCanvas{position:fixed;inset:0;width:100%;height:100%;z-index:-7;pointer-events:none}

  /* Header with centered logo and nav groups on sides */
  header{
    position:sticky;top:0;z-index:1000;
    display:grid;grid-template-columns: 1fr auto 1fr auto; align-items:center;
    gap:1rem; padding:.9rem 1.25rem;
    background: linear-gradient(180deg, rgba(8,12,16,.66), rgba(8,12,16,.46));
    backdrop-filter: blur(var(--glass-blur)) saturate(140%);
    -webkit-backdrop-filter: blur(var(--glass-blur)) saturate(140%);
    border-bottom: 1px solid var(--stroke-weak);
    box-shadow: var(--shadow-1);
    height: var(--header-height);
    flex-shrink: 0;
  }
  .nav-left,.nav-right{
    display:flex; align-items:center; gap:.8rem; flex-wrap:wrap;
  }
  .nav-left{ justify-content:flex-start }
  .nav-right{ justify-content:flex-end }
  .logo{
    font-weight:1000;font-size:1.2rem; letter-spacing:.7px; cursor:pointer;
    background: linear-gradient(90deg, var(--accent-soft), #ffffff);
    -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color:transparent;
    text-shadow: 0 0 22px rgba(var(--accent-rgb),.25);
  }
  nav a{color:var(--text);opacity:.92;text-decoration:none;cursor:pointer;font-weight:800;position:relative}
  nav a:after{
    content:""; position:absolute; left:0; right:100%; bottom:-6px; height:2px;
    background:linear-gradient(90deg,var(--accent),transparent);
    transition: right .25s ease;
  }
  nav a:hover:after{ right:0; }

  .auth-buttons{display:flex;gap:.5rem;align-items:center; justify-content:flex-end}
  button{
    padding:.6rem 1rem;border-radius:var(--btn-radius);border:0;font-weight:900;cursor:pointer;
    letter-spacing:.25px; transition: transform .15s ease, box-shadow .2s ease, background .2s ease, border-color .2s ease;
  }
  .btn-primary{
    background: linear-gradient(135deg, var(--accent-strong), var(--accent));
    color:#071317; box-shadow: var(--btn-glow);
    border:1px solid rgba(255,255,255,.08);
  }
  .btn-primary:hover{ transform: translateY(-1px); box-shadow: 0 16px 32px rgba(var(--accent-rgb),.30); }
  .btn-secondary{
    background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.025));
    border:1px solid var(--stroke-weak); color:var(--text);
  }
  .btn-secondary:hover{ border-color: var(--accent); box-shadow: 0 0 0 3px rgba(var(--accent-rgb),.14) inset; }

  .user-chip{ display:flex; align-items:center; gap:.5rem; }
  #userAvatar{ width:28px;height:28px;border-radius:50%;object-fit:cover;border:1px solid var(--stroke-weak); display:none; }

  .container{max-width:1280px;margin:0 auto;padding:1.25rem}
  
    /* =================================
       HOMEPAGE 10000x STYLES
       ================================= */
    #homePage .container { max-width: 1100px; padding: 0 2rem; }
    #homePage .home-section { 
        padding: 5rem 0; 
        opacity: 0; 
        transform: translateY(30px); 
        transition: opacity 0.8s ease-out, transform 0.8s ease-out; 
    }
    #homePage .home-section.is-visible { opacity: 1; transform: translateY(0); }
    #homePage .section-header { text-align: center; margin-bottom: 3rem; }
    #homePage .section-header h2 { font-size: 2.8rem; line-height: 1.2; margin-bottom: .75rem; letter-spacing: -1px; }
    #homePage .section-header p { font-size: 1.1rem; color: var(--muted); max-width: 600px; margin: auto; }

    /* Feature Showcase Layout */
    #homePage .feature-showcase {
      display: grid;
      grid-template-columns: 1fr 1fr;
      align-items: center;
      gap: 3rem;
    }
    #homePage .feature-showcase .text-content { grid-column: 1; }
    #homePage .feature-showcase .mockup-content { grid-column: 2; perspective: 1500px; }
    #homePage .feature-showcase.reversed .text-content { grid-column: 2; }
    #homePage .feature-showcase.reversed .mockup-content { grid-column: 1; }
    #homePage .feature-showcase h3 { font-size: 2rem; margin-bottom: 1rem; }
    #homePage .feature-showcase p { color: var(--muted); margin-bottom: 1rem; }

    /* Generic Mockup Card Style */
    .mockup-card {
        border-radius: 18px;
        border: 1px solid rgba(255,255,255,0.1);
        background: linear-gradient(135deg, rgba(12,15,22,0.6), rgba(12,15,22,0.4));
        backdrop-filter: blur(16px);
        box-shadow: 0 40px 80px -20px rgba(0,0,0,0.5);
        padding: 1rem;
        transition: transform 0.4s ease, border-color .3s ease, box-shadow .3s ease;
        transform: rotateY(10deg) rotateX(5deg);
    }
    .reversed .mockup-card { transform: rotateY(-10deg) rotateX(5deg); }
    .mockup-card:hover { transform: rotateY(0) rotateX(0) scale(1.02); }

    /* Hero Section */
    .home-hero {
        padding: 6rem 0;
        text-align: center;
        position: relative;
    }
    .hero-text h1 {
        font-size: 4rem;
        font-weight: 1000;
        line-height: 1.1;
        letter-spacing: -2px;
        background: linear-gradient(90deg, #fff, var(--accent-soft));
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color: transparent;
        margin-bottom: 1rem;
    }
    .hero-text p { font-size: 1.25rem; color: var(--muted); margin-bottom: 2rem; max-width: 600px; margin-left: auto; margin-right: auto;}
    .hero-cta { display: flex; gap: .8rem; justify-content: center; }

    /* 3D Dashboard Card */
    #hero-3d-card {
        width: 80%;
        max-width: 700px;
        aspect-ratio: 16 / 10;
        margin: 3rem auto 0 auto;
    }
    #hero-3d-card-inner {
        border-radius: 12px;
        overflow: hidden;
        height: 100%;
        display: flex; flex-direction: column;
        background: #0c0f16;
    }
    .mock-header {
        padding: .5rem 1rem;
        background: rgba(255,255,255,0.05);
        display: flex;
        align-items: center;
        gap: .5rem;
        flex-shrink: 0;
    }
    .mock-dot { width: 10px; height: 10px; border-radius: 50%; }
    .mock-body { flex-grow: 1; padding: .75rem; display: grid; grid-template-columns: 1fr 2.5fr; gap: .75rem; overflow: hidden; }
    
    /* MOCKUP CONTENT - Scoped to prevent conflicts */
    .mock-sidebar { display: flex; flex-direction: column; gap: .5rem; }
    .mock-sidebar-item { padding: .5rem .75rem; border-radius: 6px; font-size: .8rem; font-weight: 700; color: var(--muted); transition: all .2s ease; }
    #homePage .mock-sidebar-item.active { 
        background: rgba(from var(--mock-accent-color) r g b / 0.15); 
        border: 1px solid rgba(from var(--mock-accent-color) r g b / 0.2);
        color: var(--mock-accent-color);
    }
    
    .mock-main { display: flex; flex-direction: column; gap: .75rem; }
    .mock-main h2 { font-size: 1.2rem; margin: 0; }
    .mock-stat-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: .5rem; }
    .mock-stat-card { background: rgba(255,255,255,0.03); border-radius: 6px; padding: .5rem; text-align: center; }
    .mock-stat-label { font-size: .6rem; color: var(--muted); margin-bottom: .2rem; }
    .mock-stat-value { font-size: .9rem; font-weight: 800; transition: color .2s ease; }
    #homePage .mock-stat-value.profit { color: var(--mock-success-color); }
    #homePage .mock-stat-value.loss { color: var(--mock-danger-color); }

    .mock-trades-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: .3rem; }
    .mock-trade-day { 
        aspect-ratio: 1 / 1; border-radius: 4px; 
        display: flex; flex-direction: column; justify-content: center; align-items: center; 
        font-size: .6rem; background: rgba(255,255,255,0.04); 
        transition: background .2s ease;
        font-weight: 700;
    }
    #homePage .mock-trade-day.profit { background-color: rgba(from var(--mock-success-color) r g b / 0.25); color: var(--mock-success-color); }
    #homePage .mock-trade-day.loss { background-color: rgba(from var(--mock-danger-color) r g b / 0.25); color: var(--mock-danger-color); }

    /* Journal Mockup (HTML-based) */
    #mockup-journal-card { padding: 1rem; }
    #homePage .mock-tag {
        background: rgba(255,255,255,.06);
        border:1px solid var(--stroke-weak);
        padding:.18rem .5rem; border-radius:999px;
        font-size:.78rem; display: inline-block;
        transition: color .3s, border-color .3s, background-color .3s;
    }
    #homePage .mock-pnl-tag {
        border-color: rgba(from var(--mock-success-color) r g b / 0.35);
        color: var(--mock-success-color);
    }

    /* Stats Mockup - GLITCH FIX: ADDING MIN-HEIGHT */
    #mockup-stats-card .charts-grid { 
        display: grid; 
        grid-template-columns: 1fr 1fr; 
        gap: 1rem; 
        align-items: center;
        min-height: 150px; /* This prevents layout shift on scroll */
    }
    #mockup-stats-card canvas { max-height: 150px; }

    /* AI Mockup */
    #mockup-ai-card { padding: 1.2rem; }
    .bento-ai-chat { font-family: monospace; font-size: .9em; }
    .bento-ai-chat .line { margin-bottom: .5rem; }
    .bento-ai-chat .line.user { color: var(--muted); }
    .bento-ai-chat .line.ai { color: var(--accent-soft); }
    #homePage .bento-ai-chat .line.ai { color: var(--mock-accent-color); }
    .bento-ai-chat .cursor { display: inline-block; width: 8px; height: 1em; background: var(--accent-soft); animation: blink .7s infinite; }
    #homePage .bento-ai-chat .cursor { background: var(--mock-accent-color); }
    @keyframes blink { 50% { opacity: 0; } }

    /* ZEN Mockup */
    .zen-pulse-mockup { position: relative; width: 150px; height: 150px; margin: 1rem auto; }
    .zen-pulse-mockup .zen-pulse {
        width: 100%; height: 100%; border-radius: 50%;
        background: rgba(from var(--mock-accent-color) r g b / 0.2);
        animation: pulse 4s ease-in-out infinite;
        transition: background .3s ease;
    }
    @keyframes pulse { 0%, 100% { transform: scale(0.8); opacity: 0.2; } 50% { transform: scale(1.2); opacity: 0.4; } }

    /* Color Customization Mockup */
    #mockup-color-card { border-color: var(--mock-accent-color); box-shadow: 0 40px 80px -20px rgba(from var(--mock-accent-color) r g b / 0.2); }
    .color-picker-mockup { display: flex; justify-content: space-around; padding: 1rem; }
    .color-swatch { text-align: center; font-size: .8em; }
    .color-swatch .swatch-box { 
        width: 50px; height: 50px; border-radius: 8px; margin: 0 auto .5rem auto; 
        border: 2px solid var(--stroke-strong); cursor: pointer;
        transition: transform .2s ease, background-color .3s ease, color .3s ease;
    }
    .color-swatch .swatch-box:hover { transform: scale(1.1); box-shadow: 0 0 15px -2px currentColor; }
    #swatch-accent-box { background-color: var(--mock-accent-color); color: var(--mock-accent-color); }
    #swatch-win-box { background-color: var(--mock-success-color); color: var(--mock-success-color); }
    #swatch-loss-box { background-color: var(--mock-danger-color); color: var(--mock-danger-color); }
    /* Hidden input for color picker - BUG FIX: NO LONGER OVERLAID */
    .color-swatch input[type="color"] { display: none; }

    /* Final CTA */
    #homePage .final-cta { text-align: center; }
    #homePage .final-cta h2 { font-size: 2.5rem; margin-bottom: .5rem; }
    #homePage .final-cta p { color: var(--muted); margin-bottom: 1.5rem; }


  /* =================================
       FEATURES & SUBSCRIPTIONS PAGES
     ================================= */
  .page .hero { text-align: center; padding: 3rem 1.5rem; }
  .page .hero h1 { font-size: 2.8rem; letter-spacing: -1px; }
  .page .hero p { font-size: 1.1rem; max-width: 700px; margin: 0.5rem auto 0 auto; color: var(--muted); }
  
  .features-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 1.5rem;
      margin-top: 2rem;
  }
  .feature-card {
      background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
      border: 1px solid var(--stroke-weak);
      padding: 1.5rem;
      border-radius: var(--card-radius);
      transition: transform .2s ease, border-color .2s ease, box-shadow .2s ease;
  }
  .feature-card:hover { transform: translateY(-5px); border-color: var(--stroke-strong); box-shadow: 0 14px 28px rgba(0,0,0,.25); }
  .feature-card .icon { font-size: 2rem; margin-bottom: 1rem; color: var(--accent-soft); }
  .feature-card h3 { font-size: 1.5rem; margin-bottom: 0.5rem; }
  .feature-card p { color: var(--muted); }

  .pricing-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 2rem;
      margin-top: 2rem;
      justify-content: center;
      max-width: 800px;
      margin-left: auto;
      margin-right: auto;
  }
  .pricing-card {
      background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.028));
      border: 1px solid var(--stroke-weak);
      padding: 2rem;
      border-radius: var(--card-radius);
      display: flex;
      flex-direction: column;
  }
  .pricing-card.recommended {
      border-color: var(--accent);
      box-shadow: 0 0 30px rgba(var(--accent-rgb), 0.15), 0 0 0 4px rgba(var(--accent-rgb), 0.1) inset;
  }
  .pricing-card h3 { font-size: 1.8rem; color: var(--accent-soft); }
  .pricing-card .price { font-size: 2.5rem; font-weight: 1000; margin: 0.5rem 0; }
  .pricing-card .price-note { color: var(--muted); margin-bottom: 1.5rem; }
  .pricing-card ul { list-style: none; padding: 0; margin: 0 0 2rem 0; }
  .pricing-card li { padding: 0.5rem 0; display: flex; align-items: center; gap: 0.75rem; border-bottom: 1px solid var(--stroke-weak); }
  .pricing-card li:before { content: '✓'; color: var(--success); font-weight: 800; }
  .pricing-card button { width: 100%; margin-top: auto; padding: 0.8rem 1rem; font-size: 1.05rem; }

  /* =================================
       END GENERIC PAGE STYLES
     ================================= */
  main {
    flex-grow: 1;
    overflow-y: auto;
    height: calc(100vh - var(--header-height));
  }
  .page{
    display:none;
    width:100%;
    height: 100%;
  }
  .page.active{display:block}

  /* DASHBOARD: two-column layout */
  #dashboardPage.active .container{
    display:flex; flex-direction:row; gap:1.5rem; align-items:flex-start;
    height: 100%; max-width: none;
    padding: 1.25rem;
  }
  #sidebar{
    display:flex; flex-direction:column; gap:.6rem;
    position:sticky; top: 0;
    width: 260px;
    flex-shrink: 0;
    height: calc(100vh - var(--header-height) - 1.25rem * 2);
    background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
    border:1px solid var(--stroke-weak);
    border-radius:12px; padding:.8rem;
  }
  #dashboardPage .tab{
    flex-grow: 1;
    height: calc(100vh - var(--header-height) - 1.25rem * 2);
    overflow-y: auto;
    padding-right: 0; /* space for scrollbar */
  }

  .nav-item{
    padding:.6rem 1rem; display: block; width: 100%; text-align: left;
    background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
    border:1px solid var(--stroke-weak);
    border-radius:10px; cursor:pointer; font-weight:900; font-size: 1rem; color:#e8e8e8; letter-spacing:.3px;
    transition: border-color .2s, transform .15s, box-shadow .2s, background .2s;
    position: relative;
    display: flex;
    align-items: center;
    gap: 0.8rem;
  }
  .nav-item .nav-icon {
    width: 20px;
    height: 20px;
    stroke-width: 2.5;
    opacity: 0.7;
  }
  .nav-item:hover{ transform: translateY(-2px); }
  .nav-item.active{
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(var(--accent-rgb),.15) inset, 0 10px 24px rgba(var(--accent-rgb),.10);
    background: linear-gradient(180deg, rgba(255,255,255,.07), rgba(255,255,255,.03));
  }
  .nav-item.active .nav-icon {
    opacity: 1;
    stroke: var(--accent-soft);
  }
  .notification-badge {
    position: absolute;
    top: 8px;
    right: 12px;
    background: var(--accent);
    color: #000;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    font-size: 0.8rem;
    font-weight: 900;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-left: auto;
  }


  /* Stat cards (kept) */
  .stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:1rem;margin-bottom:1rem}
  .stat-card{
    background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
    border:1px solid var(--stroke-weak);
    padding:1rem;border-radius:var(--card-radius);text-align:center;
    transition: transform .2s ease, border-color .2s ease, box-shadow .2s ease;
  }
  .stat-card:hover{ transform: translateY(-3px); border-color: var(--stroke-strong); box-shadow: 0 14px 28px rgba(0,0,0,.34); }
  .stat-label{color:var(--muted);font-size:.88rem;margin-bottom:.35rem}
  .stat-value{
    font-size:1.6rem;font-weight:1000;
  }
  .stat-value.profit{ color: var(--success); }
  .stat-value.loss{ color: var(--danger); }
  .stat-value:not(.profit):not(.loss) {
    background: linear-gradient(90deg, var(--accent-soft), var(--accent));
    -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color:transparent
  }

  .period-selector select{
    background: linear-gradient(180deg, rgba(255,255,255,.065), rgba(255,255,255,.035));
    border:1px solid var(--stroke-weak);
    color:var(--text);padding:.55rem .9rem;border-radius:.6rem;font-size:1rem;appearance:none;padding-right:2.4rem
  }
  select option{background:var(--bg-1);color:var(--text)}

  /* Calendar grid (kept) */
  .weekday-header{
    display:grid;grid-template-columns:repeat(7, minmax(110px,1fr));gap:.6rem;margin-bottom:.25rem;
    text-align:center;color:var(--muted);font-weight:1000; letter-spacing:.25px;
  }
  .weekday-header div{padding:.25rem 0}
  .trades-grid{
    display:grid;grid-template-columns:repeat(7, minmax(110px,1fr));gap:.6rem
  }
  .trade-day{
    padding:.85rem;border-radius:12px;text-align:center;border:1px solid var(--stroke-weak);
    background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
    transition: transform .18s, box-shadow .18s, border-color .18s, background .18s;
    cursor:pointer
  }
  .trade-day.weekend{
      background: transparent;
      border-color: transparent;
      cursor: default;
      pointer-events: none;
      opacity: 0.5;
  }
  .trade-day:not(.weekend):hover{
    transform:translateY(-4px);
    border-color:var(--stroke-strong);
    box-shadow: 0 12px 24px rgba(0,0,0,.32), 0 0 0 2px rgba(var(--accent-rgb),.10) inset;
    background: linear-gradient(180deg, rgba(255,255,255,.06), rgba(255,255,255,.03));
  }
  /* FULL FILL for win/loss days on dashboard (default; can be overridden dynamically) */
  .trade-day.profit{
    background: linear-gradient(180deg, rgba(45,220,144,.45), rgba(45,220,144,.25)) !important;
    border-color: rgba(45,220,144,.6) !important;
    box-shadow: 0 12px 28px rgba(45,220,144,.12);
  }
  .trade-day.loss{
    background: linear-gradient(180deg, rgba(255,107,107,.45), rgba(255,107,107,.25)) !important;
    border-color: rgba(255,107,107,.6) !important;
    box-shadow: 0 12px 28px rgba(255,107,107,.12);
  }
  .trade-day.placeholder{visibility:hidden;pointer-events:none;border:none}
  .day-number{font-weight:1000;margin-bottom:.25rem}
  .pnl{font-weight:1000}

  /* Journal list (stretched, non-overlapping) */
  .trades-log{display:grid;grid-template-columns:repeat(auto-fill,minmax(460px,1fr));gap:1rem;margin-top:1rem}
  @media (max-width: 920px){ .trades-log{grid-template-columns:repeat(auto-fill,minmax(380px,1fr));} }
  @media (max-width: 720px){ .trades-log{grid-template-columns:1fr;} }

  .trade-card{
    background: linear-gradient(180deg, rgba(255,255,255,.055), rgba(255,255,255,.03));
    border:1px solid var(--stroke-weak);
    padding:1rem;border-radius:14px; position:relative;
    transition: transform .18s ease, border-color .18s ease, box-shadow .18s ease;
  }
  .trade-card:hover{ transform: translateY(-3px); border-color: var(--stroke-strong); box-shadow: 0 14px 28px rgba(0,0,0,.34); }
  .trade-card .trade-actions{ position:static; display:flex; gap:.6rem; justify-content:flex-end; margin-top:.8rem }
  .trade-card p{overflow-wrap:anywhere}
  .trade-card img{display:block;max-width:100%;height:auto;border-radius:.5rem;border:1px solid var(--stroke-weak)}

  /* Chart cards */
  .chart-card{
    background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
    border:1px solid var(--stroke-weak);
    padding:.9rem;border-radius:14px; transition: transform .2s, border-color .2s, box-shadow .2s;
  }
  .chart-card:hover{ transform: translateY(-3px); border-color: var(--stroke-strong); box-shadow: 0 14px 28px rgba(0,0,0,.34); }
  .charts-grid{ display:grid; grid-template-columns:repeat(auto-fit,minmax(320px,1fr)); gap:1rem; margin-top:.75rem; }

  /* Modals (glass) */
  .modal{display:none;position:fixed;inset:0;background:rgba(4,6,12,0.55);backdrop-filter:blur(12px) saturate(140%);z-index:3000;align-items:center;justify-content:center}
  .modal .modal-content{
    background: linear-gradient(180deg, rgba(16,20,32,.96), rgba(12,16,24,.97));
    padding:1.5rem;border-radius:14px;border:1px solid var(--stroke-strong);max-width:640px;width:92%; box-shadow: var(--shadow-2);
  }
  .modal .modal-content h2 { margin-top: 0; }
  .modal .close{float:right;font-size:1.6rem;color:var(--accent-soft);cursor:pointer}

  input[type="text"],input[type="password"],input[type="email"],input[type="number"],textarea,select{
    background:linear-gradient(180deg, rgba(255,255,255,.055), rgba(255,255,255,.03));
    border:1px solid var(--stroke-weak); color:var(--text);
    padding:.6rem .75rem;border-radius:.6rem;width:100%;
    outline: 2px solid transparent; transition: outline-color .15s, border-color .15s, box-shadow .15s;
  }
  input:focus,textarea:focus,select:focus{
    outline-color: rgba(var(--accent-rgb),.22);
    border-color: var(--accent); box-shadow: 0 0 0 3px rgba(var(--accent-rgb),.14);
  }

  /* Improved Date Input */
  .date-input-wrapper { position: relative; }
  input[type="date"] {
      position: relative;
      padding-right: 2.5rem; /* space for icon */
  }
  input[type="date"]::-webkit-calendar-picker-indicator {
      position: absolute;
      top: 0; right: 0; bottom: 0; left: 0;
      width: 100%; height: 100%;
      padding: 0; margin: 0;
      color: transparent;
      background: transparent;
      cursor: pointer;
  }
  .date-input-wrapper::after {
      content: '';
      position: absolute;
      right: 12px;
      top: 50%;
      transform: translateY(-50%);
      width: 20px;
      height: 20px;
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%239fb0c8' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Crect x='3' y='4' width='18' height='18' rx='2' ry='2'%3E%3C/rect%3E%3Cline x1='16' y1='2' x2='16' y2='6'%3E%3C/line%3E%3Cline x1='8' y1='2' x2='8' y2='6'%3E%3C/line%3E%3Cline x1='3' y1='10' x2='21' y2='10'%3E%3C/line%3E%3C/svg%3E");
      background-size: contain;
      pointer-events: none;
      opacity: 0.7;
  }


  /* Meditation (kept) */
  .meditation-overlay{ position:fixed;inset:0;z-index:4995; display:flex;align-items:center;justify-content:center; backdrop-filter: blur(6px) brightness(.75) }
  .meditation-bg { position:absolute; inset:0; z-index:4996; background: linear-gradient(120deg, rgba(20,40,70,0.7), rgba(40,30,90,0.65), rgba(20,70,80,0.65)); background-size:400% 400%; animation: coolflow 14s ease-in-out infinite; filter: blur(18px) saturate(1.05); opacity:.95; }
  @keyframes coolflow { 0%{background-position:0% 50%} 50%{background-position:100% 50%} 100%{background-position:0% 50%} }
  .meditation-screen{position:relative;z-index:4998;display:flex;align-items:center;justify-content:center;flex-direction:column;color:#fff;padding:1rem}
  .meditation-panel{background:linear-gradient(90deg, rgba(255,255,255,0.06), rgba(255,255,255,0.04));padding:2rem;border-radius:1rem;max-width:720px;width:90%;text-align:center;border:1px solid rgba(255,255,255,0.10)}
  .meditation-instruction{font-size:1.05rem;margin-bottom:.8rem}
  .meditation-timer{font-weight:1000;font-size:1.4rem}

  body.in-meditation header, body.in-meditation main, body.in-meditation footer { filter: blur(3px) brightness(.7); pointer-events: none; user-select: none; }
  /* FIX: Removed pointer-events:none from overlay itself */

  #notification{
    position:fixed;top:18px;right:18px;background:linear-gradient(90deg, rgba(45,220,144,.95), rgba(0,188,212,.85));color:#041616;
    padding:.7rem 1rem;border-radius:.75rem;z-index:3001; font-weight:1000; letter-spacing:.2px; box-shadow: 0 10px 22px rgba(var(--accent-rgb),.18);
  }

  footer{
    height:var(--footer-height);
    display:flex;align-items:center;justify-content:center;
    background: linear-gradient(180deg, rgba(10,12,18,.92), rgba(8,10,16,.96));
    border-top:1px solid var(--stroke-weak);
    padding:0 1rem;color:#b9c9e0;z-index:1000;font-size:.95rem;
    flex-shrink: 0;
  }
  footer .inner{width:100%;max-width:1200px;display:flex;align-items:center;justify-content:center;gap:.5rem}
  footer a { color: var(--accent-soft); text-decoration: none; font-weight:1000; }

  /* Theme variants (kept) */
  body.theme-redblack { --accent: #ff6b6b; --accent-strong: #ff8585; --accent-soft: #ffbaba; --accent-rgb: 255, 107, 107; --btn-glow: 0 10px 24px rgba(255,107,107,.22); }
  body.theme-blueblack{ --accent:#00bcd4; --accent-strong:#19d1e8; --accent-soft:#9feaff; --accent-rgb: 0, 188, 212; --btn-glow: 0 10px 24px rgba(0,188,212,.22); }
  body.theme-greenblack{ --accent:#2cc97d; --accent-strong:#52e0a0; --accent-soft:#aef5d5; --accent-rgb: 44, 201, 125; --btn-glow: 0 10px 24px rgba(44,201,125,.22); }
  body.theme-purpleblack{ --accent:#a070ff; --accent-strong:#c49bff; --accent-soft:#e1d2ff; --accent-rgb: 160, 112, 255; --btn-glow: 0 10px 24px rgba(160,112,255,.22); }

  /* =================================
       ZEN PAGE OVERHAUL
     ================================= */
  #zenTab .zen-hero {
    padding: 2.5rem 1.5rem;
    border-radius: 18px;
    background: linear-gradient(135deg, rgba(var(--accent-rgb), 0.05), rgba(var(--accent-rgb), 0.15));
    border: 1px solid var(--stroke-weak);
    text-align: center;
  }
  #zenTab .zen-hero h1 { font-size: 2.5rem; color: var(--accent-soft); }
  #zenTab .zen-hero p { font-size: 1.1rem; color: var(--muted); max-width: 600px; margin: .5rem auto 0 auto; }
  
  .zen-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 1.5rem;
    margin-top: 1.5rem;
  }
  .zen-feature-card {
    background: linear-gradient(180deg, rgba(255,255,255,.04), rgba(255,255,255,.02));
    border: 1px solid var(--stroke-weak);
    padding: 1.5rem;
    border-radius: var(--card-radius);
  }
  .zen-feature-card h3 { color: var(--accent-soft); margin-bottom: 1rem; }

  /* Breathing Visualizer */
  .breathing-visualizer {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 200px;
    gap: 1rem;
  }
  #breathPacer {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 30px rgba(var(--accent-rgb), 0.3);
    transition: transform 3.8s ease-in-out;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .breathing-visualizer.active #breathPacer {
    animation: box-breath 16s ease-in-out infinite;
  }
  #breathInstruction { font-size: 1.1rem; font-weight: 700; }
  @keyframes box-breath {
    0%, 100% { transform: scale(0.6); opacity: 0.7; } /* Inhale Start */
    25% { transform: scale(1.0); opacity: 1; }      /* Inhale End / Hold Start */
    50% { transform: scale(1.0); opacity: 1; }      /* Hold End / Exhale Start */
    75% { transform: scale(0.6); opacity: 0.7; }      /* Exhale End / Hold Start */
  }

  /* Soundscapes */
  .soundscape-controls { display: flex; gap: 0.8rem; flex-wrap: wrap; margin-bottom: 1rem; }
  .sound-btn { font-size: 1.5rem; padding: 0.6rem; border-radius: 8px; cursor: pointer; background: rgba(255,255,255,0.05); border: 1px solid var(--stroke-weak); transition: all .2s; }
  .sound-btn:hover { background: rgba(255,255,255,0.1); border-color: var(--accent); }
  .sound-btn.active { background: rgba(var(--accent-rgb), 0.2); border-color: var(--accent); }
  #nowPlayingSound { color: var(--muted); }
  
  /* Meditation Options (re-styled) */
  .meditation-options { display: flex; gap: 1rem; }
  .meditation-option-card {
    flex: 1;
    text-align: center;
    padding: 1.5rem;
    border-radius: 12px;
    background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.025));
    border: 1px solid var(--stroke-weak);
    cursor: pointer;
    transition: transform .2s, box-shadow .2s, border-color .2s;
  }
  .meditation-option-card:hover { transform: translateY(-5px); border-color: var(--accent); box-shadow: 0 14px 28px rgba(0,0,0,.3); }
  .meditation-option-card h4 { font-size: 1.5rem; margin-bottom: .25rem; }
  .meditation-option-card p { color: var(--muted); font-size: 0.9rem; }

  /* Mindfulness Journal */
  #mindfulnessJournal { min-height: 120px; margin-bottom: 0.5rem; }
  
  /* =================================
       AI PAGE OVERHAUL
     ================================= */
  #aiTab {
      display: flex;
      flex-direction: column;
      height: 100%;
  }
  .ai-suggested-prompts {
      display: flex;
      flex-wrap: wrap;
      gap: 0.6rem;
      margin-bottom: 1rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid var(--stroke-weak);
  }
  .ai-prompt-btn {
      padding: .5rem .9rem;
      border-radius: 8px;
      font-size: 0.9rem;
  }
  #aiChatWindow {
      flex-grow: 1;
      overflow-y: auto;
      padding: 1rem;
      background: rgba(0,0,0,0.2);
      border-radius: 12px;
  }
  .ai-chat-bubble {
      max-width: 80%;
      padding: .8rem 1.1rem;
      border-radius: 18px;
      margin-bottom: 1rem;
      line-height: 1.5;
  }
  .ai-chat-bubble.user {
      background: linear-gradient(135deg, var(--accent-strong), var(--accent));
      color: #071317;
      border-bottom-right-radius: 4px;
      margin-left: auto;
  }
  .ai-chat-bubble.ai {
      background: linear-gradient(180deg, rgba(255,255,255,.07), rgba(255,255,255,.04));
      border: 1px solid var(--stroke-weak);
      border-bottom-left-radius: 4px;
      margin-right: auto;
      white-space: pre-wrap;
  }
  .ai-chat-bubble.ai ul { margin: .8rem 0; padding-left: 1.5rem; }
  .ai-chat-bubble.ai li { margin-bottom: .5rem; }
  
  .ai-thinking-indicator {
      display: flex;
      gap: 4px;
      align-items: center;
  }
  .ai-thinking-indicator span {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background-color: var(--accent-soft);
      animation: ai-pulse 1.4s infinite ease-in-out both;
  }
  .ai-thinking-indicator span:nth-child(1) { animation-delay: -0.32s; }
  .ai-thinking-indicator span:nth-child(2) { animation-delay: -0.16s; }
  @keyframes ai-pulse {
      0%, 80%, 100% { transform: scale(0); }
      40% { transform: scale(1.0); }
  }

  .ai-input-form {
      display: flex;
      gap: 0.8rem;
      margin-top: 1rem;
  }
  #aiQuestion { flex-grow: 1; }

/* =================================
       FORUM, FRIENDS & DM 100x REDESIGN
     ================================= */
  #forumTab, #friendsTab {
      display: flex;
      flex-direction: column;
      height: 100%;
      background: linear-gradient(180deg, rgba(var(--accent-rgb), 0.02), transparent 40%);
      padding: 1rem;
      border-radius: 12px;
  }
   #dmTab {
      display: flex;
      flex-direction: column;
      height: 100%;
      background: linear-gradient(180deg, rgba(var(--accent-rgb), 0.02), transparent 40%);
      border-radius: 12px;
  }
  .chat-container {
    flex-grow: 1;
    overflow-y: auto;
    padding: 1rem;
    display: flex;
    flex-direction: column;
  }
  .message-group {
      display: flex;
      margin-bottom: 1.25rem;
      gap: 12px;
      max-width: 85%;
  }
  .message-group.own {
      align-self: flex-end;
      flex-direction: row-reverse;
  }
  .chat-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid var(--stroke-strong);
      flex-shrink: 0;
      cursor: pointer;
      align-self: flex-start;
  }
  .message-content {
      display: flex;
      flex-direction: column;
      gap: 4px;
  }
  .own .message-content { align-items: flex-end; }
  .message-header {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 0.9em;
  }
  .message-header .username {
      font-weight: 800;
      color: var(--accent-soft);
      cursor: pointer;
  }
  .message-header .timestamp {
      color: var(--muted);
      font-size: 0.9em;
  }
  .message-bubble {
      padding: .8rem 1.2rem;
      border-radius: 20px;
      line-height: 1.5;
      background: linear-gradient(180deg, rgba(255,255,255,.07), rgba(255,255,255,.04));
      border: 1px solid var(--stroke-weak);
      white-space: pre-wrap;
      word-wrap: break-word;
      border-bottom-left-radius: 4px;
  }
  .own .message-bubble {
      background: linear-gradient(135deg, var(--accent), var(--accent-strong));
      color: #040808;
      border: none;
      border-bottom-right-radius: 4px;
      border-bottom-left-radius: 20px;
  }
  .message-bubble img {
      max-width: 300px;
      height: auto;
      border-radius: 12px;
      margin-top: 8px;
      display: block;
      border: 1px solid rgba(0,0,0,0.2);
  }
  .chat-input-form {
      display: flex;
      gap: 0.8rem;
      margin-top: 1rem;
      padding: 1rem;
      background: rgba(var(--bg-0), 0.5);
      border-top: 1px solid var(--stroke-weak);
      max-width: 900px;
      width: 100%;
      margin-left: auto;
      margin-right: auto;
  }
  .chat-input-form input { flex-grow: 1; }

  /* Admin Forum Tools */
  .admin-mod-tools { display: inline-block; position: relative; }
  .mod-icon { cursor: pointer; opacity: 0.7; margin-left: 8px; }
  .mod-menu {
      display: none; position: absolute; left: 100%; top: 0;
      background: var(--bg-1); border: 1px solid var(--stroke-strong);
      border-radius: 8px; padding: .5rem; z-index: 10; box-shadow: var(--shadow-1); white-space: nowrap;
  }
  .mod-menu a { display: block; padding: .4rem .6rem; color: var(--text); text-decoration: none; border-radius: 6px; }
  .mod-menu a:hover { background: rgba(255,255,255,0.1); }
  .mod-menu a.danger { color: var(--danger); }
  .admin-mod-tools:hover .mod-menu { display: block; }
  
  /* Pinned Announcement */
  #announcementContainer { 
    display: none; 
    position: sticky;
    top: 0;
    z-index: 5;
  }
  .pinned-announcement {
    padding: 1rem;
    margin-bottom: 1rem;
    border-radius: 12px;
    background: linear-gradient(135deg, rgba(var(--accent-rgb), 0.15), rgba(var(--accent-rgb), 0.25));
    border: 1px solid rgba(var(--accent-rgb), 0.3);
  }
  .pinned-announcement .announcement-header { display:flex; justify-content: space-between; align-items: center; font-size: 0.9em; margin-bottom: 0.5rem; }
  .pinned-announcement .announcement-header .author { font-weight: 800; }
  .pinned-announcement .announcement-body { font-size: 1.05em; }

  /* Friends & DMs */
  .sub-nav { display: flex; gap: .5rem; border-bottom: 1px solid var(--stroke-weak); margin-bottom: 1rem; padding: 0 1rem; }
  .sub-nav-item { padding: .5rem 1rem; border-bottom: 2px solid transparent; cursor: pointer; font-weight: 700; color: var(--muted); }
  .sub-nav-item.active { border-bottom-color: var(--accent); color: var(--accent-soft); }

  .friends-list, .requests-list { display: flex; flex-direction: column; gap: 1rem; padding: 0 1rem; }
  .friend-card, .request-card {
    display: flex; align-items: center; gap: 1rem;
    background: linear-gradient(180deg, rgba(255,255,255,.045), rgba(255,255,255,.02));
    border: 1px solid var(--stroke-weak); padding: 1rem; border-radius: var(--card-radius);
  }
  .friend-card .avatar, .request-card .avatar { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; }
  .friend-card .info .name, .request-card .info .name { font-weight: 700; font-size: 1.1rem; }
  .friend-card .actions, .request-card .actions { margin-left: auto; display: flex; gap: .5rem; }

  /* DM Layout */
  #dmTab { flex-direction: row; padding: 0; }
  #dmUserList { width: 280px; flex-shrink: 0; background: rgba(var(--bg-0), 0.4); border-right: 1px solid var(--stroke-weak); overflow-y: auto; padding: .8rem; }
  #dmContent { flex-grow: 1; display: flex; flex-direction: column; }
  .dm-user-item { display: flex; align-items: center; gap: 10px; padding: .8rem; border-radius: 10px; cursor: pointer; transition: background .2s; }
  .dm-user-item:hover { background: rgba(var(--accent-rgb), 0.08); }
  .dm-user-item.active { background: rgba(var(--accent-rgb), 0.2); }
  .dm-user-item img { width: 40px; height: 40px; border-radius: 50%; object-fit: cover; }
  #dmChatWindow { flex-grow: 1; /* uses .chat-container styles */ }
  #dmInputForm { /* uses .chat-input-form styles */ }
  .dm-placeholder { display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; color: var(--muted); text-align: center; }

  /* Profile Modal Redesign */
  #userProfileModal .modal-content { max-width: 480px; }
  .profile-banner { height: 100px; background: linear-gradient(135deg, var(--accent), var(--bg-2)); border-radius: 14px 14px 0 0; margin: -1.5rem -1.5rem 0 -1.5rem; }
  .profile-header { display: flex; flex-direction: column; align-items: center; margin-bottom: 1.5rem; text-align: center; }
  .profile-avatar { width: 90px; height: 90px; border-radius: 50%; object-fit: cover; border: 4px solid var(--bg-1); margin-top: -45px; margin-bottom: 0.5rem; }
  .profile-name { font-size: 1.8rem; font-weight: 800; }
  .profile-role-badge {
    font-size: 0.8rem; font-weight: 900; text-transform: uppercase; letter-spacing: 0.5px;
    padding: .2rem .6rem; border-radius: 6px; display: inline-block; margin-top: .25rem;
  }
  .profile-role-badge.admin { background: var(--accent); color: var(--bg-0); }
  .profile-role-badge.elite { background: #c278ff; color: var(--bg-0); }
  .profile-role-badge.pro { background: #00bcd4; color: var(--bg-0); }
  .profile-role-badge.starter { background: var(--muted); color: var(--bg-0); }
  .profile-details { display: grid; grid-template-columns: 1fr; gap: 1rem; }
  .profile-detail-card { text-align: center; padding: .8rem; background: rgba(255,255,255,0.03); border-radius: 8px; }
  .profile-detail-card .label { font-size: .8em; color: var(--muted); margin-bottom: .25rem; }
  .profile-detail-card .value { font-weight: 700; }
  .profile-actions { margin-top: 1.5rem; display: flex; justify-content: center; }


  /* ADMIN PANEL */
  .admin-user-table { width: 100%; border-collapse: collapse; margin-top: 1rem; table-layout: fixed; }
  .admin-user-table th, .admin-user-table td { padding: 12px 10px; text-align: left; border-bottom: 1px solid var(--stroke-weak); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .admin-user-table th { color: var(--accent-soft); font-weight: 800; }
  .admin-user-table tr.banned-user { background: rgba(var(--danger-rgb, 255, 107, 107), 0.1); }
  .admin-user-table tr.banned-user td { opacity: 0.6; }
  .admin-user-table tr:not(.banned-user):hover { background: rgba(255,255,255,0.03); }
  .admin-user-pfp { width: 40px; height: 40px; border-radius: 50%; object-fit: cover; border: 1px solid var(--stroke-strong); vertical-align: middle; margin-right: 10px; }
  .admin-user-info { display: inline-block; vertical-align: middle; }
  .admin-user-info span { display: block; }
  .admin-user-info .email { color: var(--muted); font-size: 0.9em; }
  .admin-activity-log { background: rgba(0,0,0,0.2); border-radius: 8px; padding: 1rem; max-height: 200px; overflow-y: auto; border: 1px solid var(--stroke-weak);}
  .admin-activity-log div { padding: 4px 0; border-bottom: 1px solid var(--stroke-weak); font-size: 0.95em; }
  .admin-activity-log div:last-child { border: none; }
  .btn-ban { background: var(--danger); color: white; padding: .4rem .8rem; font-size: .85em; }
  .btn-unban { background: var(--success); color: white; padding: .4rem .8rem; font-size: .85em; }

  /* Settings Toggle Switch */
  .toggle-switch { display: flex; align-items: center; gap: 10px; }
  .switch { position: relative; display: inline-block; width: 50px; height: 28px; }
  .switch input { opacity: 0; width: 0; height: 0; }
  .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: rgba(255,255,255,0.1); transition: .4s; border-radius: 28px; }
  .slider:before { position: absolute; content: ""; height: 20px; width: 20px; left: 4px; bottom: 4px; background-color: white; transition: .4s; border-radius: 50%; }
  input:checked + .slider { background-color: var(--accent); }
  input:checked + .slider:before { transform: translateX(22px); }

  /* NEW: Settings Page Redesign */
  #settingsTab .settings-container { display: flex; gap: 1.5rem; }
  #settingsTab .settings-nav { display: flex; flex-direction: column; gap: 0.5rem; width: 200px; flex-shrink: 0; }
  #settingsTab .settings-nav-item {
    padding: .8rem 1rem; border-radius: 10px; font-weight: 700; cursor: pointer;
    border: 1px solid transparent;
  }
  #settingsTab .settings-nav-item:hover { background: rgba(255,255,255,0.05); }
  #settingsTab .settings-nav-item.active {
    background: rgba(var(--accent-rgb), 0.1);
    border-color: rgba(var(--accent-rgb), 0.2);
    color: var(--accent-soft);
  }
  #settingsTab .settings-content { flex-grow: 1; }
  #settingsTab .settings-pane { display: none; }
  #settingsTab .settings-pane.active { display: block; }
  #settingsTab .settings-card {
    background: linear-gradient(180deg, rgba(255,255,255,.04), rgba(255,255,255,.02));
    border: 1px solid var(--stroke-weak);
    padding: 1.5rem;
    border-radius: 14px;
    margin-bottom: 1.5rem;
  }
  #settingsTab .settings-card h3 { color: var(--accent-soft); margin-top: 0; margin-bottom: 1rem; }
  #settingsTab .settings-field { margin-bottom: 1.25rem; }
  #settingsTab .settings-field label { display: block; color: var(--muted); margin-bottom: 0.3rem; font-size: 0.9em; }
  #settingsTab .settings-field .field-group { display: flex; gap: 0.5rem; align-items: center; }


  @media (max-width:1080px){
    header{ grid-template-columns: 1fr auto auto; grid-auto-flow: row dense; }
    .nav-left{ grid-column: 1 / -1; justify-content:center; order:2 }
    .nav-right{ grid-column: 1 / -1; justify-content:center; order:3 }
    .logo{ order:1; text-align:center }
    .auth-buttons{ order:4; grid-column: 1 / -1; justify-content:center }
    .home-hero { grid-template-columns: 1fr; text-align: center; }
    .hero-cta { justify-content: center; }
  }
  @media (max-width:920px){
    #dashboardPage.active .container{ flex-direction: column; height: auto; }
    #sidebar{ position:relative; top:0; height: auto; width: 100%; }
    .weekday-header, .trades-grid{ grid-template-columns:repeat(7, minmax(64px,1fr)); }
    #homePage .feature-showcase, #homePage .feature-showcase.reversed { grid-template-columns: 1fr; }
    #homePage .feature-showcase .text-content, #homePage .feature-showcase.reversed .text-content { grid-row: 1; text-align: center; }
    #homePage .feature-showcase .mockup-content, #homePage .feature-showcase.reversed .mockup-content { grid-row: 2; }
    #dmTab { flex-direction: column; }
    #dmUserList { width: 100%; max-height: 150px; border-right: none; border-bottom: 1px solid var(--stroke-weak); }
  }
  @media (max-width:640px){
    .weekday-header, .trades-grid{ grid-template-columns:repeat(7, minmax(44px,1fr)); }
    .day-number{font-size:.95rem}
    .hero-text h1 { font-size: 3rem; }
    #settingsTab .settings-container { flex-direction: column; }
    #settingsTab .settings-nav { flex-direction: row; width: 100%; overflow-x: auto; }
  }
</style>

<!-- Dynamic color override style (injected by JS) -->
<style id="dynamicColorStyle"></style>
</head>
<body class="page-home">

<!-- Decorative background layers -->
<div class="fx-stripes" aria-hidden="true"></div>
<canvas id="bgCanvas"></canvas>

<header>
  <!-- Left nav group -->
  <nav class="nav-left">
    <a id="homeNav">Home</a>
    <a id="featuresNav">Features</a>
    <a id="aboutNav">About Us</a>
    <a id="subsNav">Subscriptions</a>
  </nav>

  <!-- Centered logo -->
  <div class="logo" id="logo">INTRADEL</div>

  <!-- Right nav group -->
  <nav class="nav-right">
    <a id="giveawaysNav">Giveaways</a>
    <a id="contactNav">Contact Us</a>
    <a id="supportNav">Support</a>
  </nav>

  <!-- Auth buttons + user chip -->
  <div class="auth-buttons">
    <div class="user-chip">
      <img id="userAvatar" alt="Avatar"/>
      <span id="userDisplay" style="display:none;color:var(--accent-soft)"></span>
    </div>
    <button class="btn-secondary" id="loginBtn">Login</button>
    <button class="btn-primary" id="registerBtn">Register</button>
    <button class="btn-primary" id="logoutBtn" style="display:none">Logout</button>
  </div>
</header>

<main>
    <!-- Home Page -->
    <div id="homePage" class="page active">
      <div class="container">
          <section class="home-hero">
              <div class="hero-text">
                  <h1>Your AI-Powered Trading Co-Pilot</h1>
                  <p>Transform your trading with a journal that does more than just record. Get deep insights, personalized feedback, and the discipline to reach your peak performance.</p>
                  <div class="hero-cta">
                      <button class="btn-primary" id="ctaBtn">Start for Free</button>
                      <button class="btn-secondary" id="homeToDashboard">View Dashboard</button>
                  </div>
                  <div id="hero-3d-card-wrapper">
                      <div id="hero-3d-card" class="mockup-card">
                          <div id="hero-3d-card-inner">
                              <div class="mock-header">
                                <div class="mock-dot" style="background:#ff5f57;"></div>
                                <div class="mock-dot" style="background:#febc2e;"></div>
                                <div class="mock-dot" style="background:#28c840;"></div>
                              </div>
                              <div class="mock-body">
                                <div class="mock-sidebar">
                                    <div class="mock-sidebar-item active">Dashboard</div>
                                    <div class="mock-sidebar-item">Journal</div>
                                    <div class="mock-sidebar-item">Statistics</div>
                                    <div class="mock-sidebar-item">ZEN</div>
                                    <div class="mock-sidebar-item">AI Mentor</div>
                                    <div class="mock-sidebar-item">Settings</div>
                                </div>
                                <div class="mock-main">
                                    <h2>Dashboard</h2>
                                    <div class="mock-stat-grid">
                                        <div class="mock-stat-card">
                                            <div class="mock-stat-label">Total Trades</div>
                                            <div class="mock-stat-value">42</div>
                                        </div>
                                        <div class="mock-stat-card">
                                            <div class="mock-stat-label">Total P&L</div>
                                            <div class="mock-stat-value profit">$1,284.50</div>
                                        </div>
                                    </div>
                                    <div class="mock-trades-grid">
                                        <!-- FIX: Displaying PnL instead of day numbers -->
                                        <div class="mock-trade-day profit">+$124</div>
                                        <div class="mock-trade-day loss">-$87</div>
                                        <div class="mock-trade-day profit">+$210</div>
                                        <div class="mock-trade-day profit">+$155</div>
                                        <div class="mock-trade-day loss">-$45</div>
                                        <div class="mock-trade-day"></div><div class="mock-trade-day"></div>
                                        <div class="mock-trade-day profit">+$302</div>
                                        <div class="mock-trade-day"></div>
                                        <div class="mock-trade-day loss">-$110</div>
                                        <div class="mock-trade-day"></div><div class="mock-trade-day"></div>
                                        <div class="mock-trade-day"></div><div class="mock-trade-day"></div>
                                    </div>
                                </div>
                              </div>
                          </div>
                      </div>
                  </div>
              </div>
          </section>

          <section class="home-section">
            <div class="feature-showcase">
                <div class="text-content">
                    <h3>Precision Journaling</h3>
                    <p>Log every detail that matters. Capture your setup, notes, and chart screenshots to build a rich, searchable history of your trades. Turn hindsight into foresight.</p>
                </div>
                <div class="mockup-content">
                    <div id="mockup-journal-card" class="mockup-card">
                        <!-- High-fidelity HTML Journal Card Mockup -->
                        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:.25rem">
                            <div style="display:flex;gap:.35rem;flex-wrap:wrap">
                                <span class="mock-tag">#SPY</span>
                                <span class="mock-tag">LONG</span>
                                <span class="mock-tag mock-pnl-tag">$345.20</span>
                            </div>
                            <span class="mock-tag" style="background: rgba(from var(--mock-success-color) r g b / 0.1); color: var(--mock-success-color);">WIN</span>
                        </div>
                        <div style="color:var(--muted); font-size: 0.9em;">2025-10-17</div>
                        <p style="color:var(--muted);margin-top:.6rem;white-space:pre-wrap">Break and retest of key level after morning consolidation. Entry was clean, could have held for longer.</p>
                        <div style="display:flex; gap:.6rem; justify-content:flex-end; margin-top:.8rem">
                            <button class="btn-secondary" style="font-size: 0.8em; padding: .4rem .8rem; pointer-events: none;">Edit</button>
                            <button class="btn-secondary" style="font-size: 0.8em; padding: .4rem .8rem; pointer-events: none;">Delete</button>
                        </div>
                    </div>
                </div>
            </div>
          </section>

          <section class="home-section" style="display: none;">
            <div class="feature-showcase reversed">
                <div class="text-content">
                    <h3>Advanced Statistics</h3>
                    <p>Go beyond P&L. Visualize your equity curve, win/loss ratios, and profit factor to truly understand your performance edge and identify areas for improvement.</p>
                </div>
                <div class="mockup-content">
                    <div id="mockup-stats-card" class="mockup-card">
                        <div class="charts-grid">
                            <canvas id="mockupEquityCanvas"></canvas>
                            <canvas id="mockupWinLossDonutCanvas"></canvas>
                        </div>
                    </div>
                </div>
            </div>
          </section>

          <section class="home-section">
            <div class="feature-showcase">
                <div class="text-content">
                    <h3>AI-Powered Mentor</h3>
                    <p>Get unbiased, actionable feedback from IntradelAI. It analyzes your journal to find hidden patterns in your behavior, helping you conquer FOMO, revenge trading, and hesitation.</p>
                </div>
                <div class="mockup-content">
                    <div id="mockup-ai-card" class="mockup-card">
                         <div class="bento-ai-chat">
                            <div class="line user">> How can I improve my entries?</div>
                            <div class="line ai" id="ai-typewriter"></div>
                        </div>
                    </div>
                </div>
            </div>
          </section>

          <section class="home-section">
            <div class="feature-showcase reversed">
                <div class="text-content">
                    <h3>Find Your Focus with ZEN</h3>
                    <p>A clear mind is a trader's greatest asset. Use built-in guided meditation sessions to reduce emotional trading, detach from market noise, and prepare for your next opportunity.</p>
                </div>
                <div class="mockup-content">
                    <div class="mockup-card">
                        <div class="zen-pulse-mockup">
                            <div class="zen-pulse"></div>
                        </div>
                    </div>
                </div>
            </div>
          </section>

          <section class="home-section">
            <div class="feature-showcase">
                <div class="text-content">
                    <h3>Make It Yours</h3>
                    <p>Your workspace should inspire you. Customize the platform's accent and P&L colors to match your personal style and create a trading environment where you feel in control.</p>
                </div>
                <div class="mockup-content">
                    <div id="mockup-color-card" class="mockup-card">
                        <div class="color-picker-mockup">
                            <div class="color-swatch">
                                <div class="swatch-box" id="swatch-accent-box" data-picker="mock-color-accent"></div>
                                <span>Accent</span>
                                <input type="color" id="mock-color-accent" value="#a070ff" data-target="accent">
                            </div>
                            <div class="color-swatch">
                                <div class="swatch-box" id="swatch-win-box" data-picker="mock-color-win"></div>
                                <span>Win</span>
                                <input type="color" id="mock-color-win" value="#2ddc90" data-target="success">
                            </div>
                            <div class="color-swatch">
                                <div class="swatch-box" id="swatch-loss-box" data-picker="mock-color-loss"></div>
                                <span>Loss</span>
                                <input type="color" id="mock-color-loss" value="#ff5555" data-target="danger">
                            </div>
                        </div>
                    </div>
                </div>
            </div>
          </section>

          <section class="home-section final-cta">
              <h2>Ready to Unlock Your Potential?</h2>
              <p>Join thousands of traders who are leveling up with INTRADEL.</p>
              <button class="btn-primary" id="finalCtaRegisterBtn" style="padding: 1rem 2rem; font-size: 1.1rem;">Claim Your Free Account</button>
          </section>
      </div>
    </div>

    <!-- Features Page -->
    <div id="featuresPage" class="page">
        <div class="container">
            <div class="hero">
                <h1>An Arsenal for the Modern Trader</h1>
                <p>INTRADEL is more than a journal. It's a complete ecosystem designed to help you analyze, adapt, and accelerate your trading performance.</p>
            </div>
            <div class="features-grid">
                <div class="feature-card">
                    <div class="icon">📝</div>
                    <h3>Precision Journaling</h3>
                    <p>Log every detail with fields for P&L, side, symbol, notes, and screenshot uploads. Build a rich, searchable history of your trades.</p>
                </div>
                <div class="feature-card">
                    <div class="icon">📈</div>
                    <h3>Advanced Statistics</h3>
                    <p>Visualize your performance with an equity curve, profit factor, win/loss ratios, and more. Identify your edge and eliminate weaknesses.</p>
                </div>
                <div class="feature-card">
                    <div class="icon">🤖</div>
                    <h3>AI-Powered Mentor</h3>
                    <p>Get unbiased, actionable feedback from IntradelAI. It analyzes your data to find hidden patterns and suggest improvements.</p>
                </div>
                <div class="feature-card">
                    <div class="icon">🧘</div>
                    <h3>ZEN Focus Mode</h3>
                    <p>A clear mind is a trader's greatest asset. Use built-in guided meditation sessions, breathing exercises, and focus sounds to improve focus.</p>
                </div>
                <div class="feature-card">
                    <div class="icon">🎨</div>
                    <h3>Deep Customization</h3>
                    <p>Make the platform yours. Choose from multiple themes and override accent and P&L colors to create your perfect workspace.</p>
                </div>
                <div class="feature-card">
                    <div class="icon">🔒</div>
                    <h3>Local-First & Secure</h3>
                    <p>Your data is yours. Everything is stored securely in your browser's local storage, ensuring complete privacy and control.</p>
                </div>
            </div>
        </div>
    </div>
    
    <div id="aboutPage" class="page">
        <div class="container">
            <div class="hero">
                <h1>From Data to Discipline</h1>
                <p>We're a team of traders and developers who believe that consistent profitability comes from a repeatable process. We built INTRADEL to be the tool we always wished we had.</p>
            </div>
        </div>
    </div>
    
    <!-- Subscriptions Page -->
    <div id="subscriptionsPage" class="page">
      <div class="container">
        <div class="hero">
          <h1>Choose Your Edge</h1>
          <p>Find the plan that fits your journey. Upgrade or cancel anytime.</p>
        </div>
        <div class="pricing-grid">
            <div class="pricing-card recommended">
                <h3>Pro</h3>
                <p class="price">$19 <span style="font-size: 1rem; font-weight: 500;">/ month</span></p>
                <p class="price-note">Billed monthly. Unlock the full power of AI analysis.</p>
                <ul>
                    <li>Unlimited Journal Entries</li>
                    <li>Advanced Statistics & Charts</li>
                    <li>AI-Powered Mentor</li>
                    <li>ZEN Meditation Sessions</li>
                    <li>Full Color Customization</li>
                    <li>Data Import/Export</li>
                </ul>
                <button class="btn-primary" id="planProBtn">Start 7-Day Free Trial</button>
            </div>
            <div class="pricing-card">
                <h3>Elite</h3>
                <p class="price">$39 <span style="font-size: 1rem; font-weight: 500;">/ month</span></p>
                <p class="price-note">For dedicated traders who want it all.</p>
                <ul>
                    <li>Everything in Pro</li>
                    <li>Priority Support</li>
                    <li>Early Access to New Features</li>
                    <li>Exclusive Community Access</li>
                    <li>Advanced AI Models</li>
                    <li>Access to elite only giveaways</li>
                </ul>
                <button class="btn-secondary" id="planEliteBtn">Go Elite</button>
            </div>
        </div>
      </div>
    </div>

    <div id="giveawaysPage" class="page"> <div class="container"> <div class="hero"> <h1>Giveaways</h1><p class="muted">Periodic raffles for premium months, merch, and more.</p></div></div></div>
    <div id="contactPage" class="page"> <div class="container"> <div class="hero"> <h1>Contact Us</h1><p class="muted">Questions, feedback, partnership ideas—drop us a line at <a href="mailto:INTRADELAI@GMAIL.COM">INTRADELAI@GMAIL.COM</a>.</p></div></div></div>
    <div id="supportPage" class="page"> <div class="container"> <div class="hero"> <h1>Support</h1><p class="muted">Find quick answers or reach our team.</p></div></div></div>


    <!-- Dashboard Page -->
    <div id="dashboardPage" class="page">
      <div class="container">
        <div id="sidebar">
          <div class="nav-item active" data-tab="dashboard">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
            Dashboard
          </div>
          <div class="nav-item" data-tab="journal">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path></svg>
            Journal
          </div>
          <div class="nav-item" data-tab="stats">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20V10"></path><path d="M18 20V4"></path><path d="M6 20V16"></path></svg>
            Statistics
          </div>
          <div class="nav-item" data-tab="zen">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M12 12c-2 0-4.5 1-4.5 3.5V20h9v-4.5c0-2.5-2.5-3.5-4.5-3.5z"></path><path d="M20.2 13.8c.6-3.4-1.9-6.3-5.2-6.3-2.4 0-4.5 1.4-5.2 3.5"></path><path d="M7.7 12.8c-.6-3.4 1.9-6.3 5.2-6.3 2.4 0 4.5 1.4 5.2 3.5"></path><path d="M12 2a2.4 2.4 0 0 0-1 4.3 2.4 2.4 0 0 0 2 0A2.4 2.4 0 0 0 12 2z"></path></svg>
            ZEN
          </div>
          <div class="nav-item" data-tab="ai">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M12 8V4H8"></path><path d="M16 4h-4"></path><rect x="4" y="12" width="16" height="8" rx="2"></rect><path d="M9 16.5v-1"></path><path d="M15 16.5v-1"></path></svg>
            AI Mentor
          </div>
          <div class="nav-item" data-tab="forum" id="forumNav" style="display:none;">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg>
            Forum
          </div>
          <div class="nav-item" data-tab="friends" id="friendsNav" style="display:none;">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="8.5" cy="7" r="4"></circle><path d="M20 8v6"></path><path d="M23 11h-6"></path></svg>
            Friends
          </div>
          <div class="nav-item" data-tab="dm" id="dmNav" style="display:none;">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>
            Direct Messages
          </div>
          <div class="nav-item" data-tab="settings">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"></circle><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path></svg>
            Settings
          </div>
          <div class="nav-item" data-tab="admin" id="adminNav" style="display:none; border-color: var(--accent-strong);">
            <svg class="nav-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path></svg>
            Admin
          </div>
        </div>

        <div id="dashboardTab" class="tab">
          <h2 style="margin-bottom:1rem">Dashboard</h2>
          <div class="stat-grid">
            <div class="stat-card"><div class="stat-label">Total Trades</div><div class="stat-value" id="totalTrades">0</div></div>
            <div class="stat-card"><div class="stat-label">Total P&L</div><div class="stat-value" id="totalPnL">$0.00</div></div>
            <div class="stat-card"><div class="stat-label">Win Rate</div><div class="stat-value" id="winRate">0.0%</div></div>
            <div class="stat-card"><div class="stat-label">Winning Days</div><div class="stat-value" id="winningDays">0</div></div>
          </div>

          <div class="period-selector">
            <select id="periodSelect">
              <option value="currentMonth">Current Month</option>
              <option value="last14days">Last 14 Days</option>
              <option value="lastMonth">Last Month</option>
              <option value="allTime">All Time</option>
            </select>
          </div>

          <h3 id="periodTitle" style="margin:0 0 .5rem 0">Current Month</h3>

          <div id="weekdayHeader" class="weekday-header">
            <div>Mon</div><div>Tue</div><div>Wed</div><div>Thu</div><div>Fri</div><div>Sat</div><div>Sun</div>
          </div>

          <div class="trades-grid" id="tradesGrid"></div>
        </div>

        <div id="journalTab" class="tab" style="display:none">
          <h2>Journal</h2>
          <div id="journalTools" class="journal-tools" style="display:flex;gap:.6rem;align-items:center;flex-wrap:wrap;margin-top:.6rem">
            <input id="journalSearch" type="text" placeholder="Search notes or symbol..." style="max-width:260px">
            <select id="journalSort" style="width:180px">
              <option value="dateDesc">Newest first</option>
              <option value="dateAsc">Oldest first</option>
              <option value="winFirst">Wins first</option>
              <option value="lossFirst">Losses first</option>
            </select>
            <select id="journalFilterOutcome" style="width:140px">
              <option value="all">All outcomes</option>
              <option value="win">Wins</option>
              <option value="loss">Losses</option>
            </select>
            <select id="journalFilterSide" style="width:140px">
              <option value="all">All sides</option>
              <option value="long">Long</option>
              <option value="short">Short</option>
            </select>
            <select id="journalRange" style="width:160px">
              <option value="last7days">Last 7 Days</option>
              <option value="last14days" selected>Last 14 Days</option>
              <option value="currentMonth">Current Month</option>
              <option value="all">All Time</option>
            </select>
            <span id="journalMeta" class="muted"></span>
          </div>

          <!-- Journal Composer -->
          <div style="background:linear-gradient(180deg, rgba(255,255,255,.055), rgba(255,255,255,.03));border:1px solid var(--stroke-weak);padding:1rem;border-radius:14px;margin-top:.8rem">
            <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:.6rem">
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Symbol</label><input id="jSymbol" type="text" placeholder="e.g. AAPL"></div>
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Side</label><select id="jSide"><option value="long">Long</option><option value="short">Short</option></select></div>
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Outcome</label><select id="tradeOutcome"><option value="win">Win</option><option value="loss">Loss</option></select></div>
              <div class="date-input-wrapper"><label class="muted" style="display:block;margin-bottom:.25rem">Date</label><input id="tradeDate" type="date"></div>
            </div>

            <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:.6rem;margin-top:.6rem">
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Entry</label><input id="jEntry" type="number" step="0.0001" placeholder="Entry price"></div>
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Exit</label><input id="jExit" type="number" step="0.0001" placeholder="Exit price"></div>
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Size</label><input id="jSize" type="number" step="0.01" placeholder="Shares/contracts"></div>
              <div><label class="muted" style="display:block;margin-bottom:.25rem">Screenshot (optional)</label><input id="jScreenshot" type="file" accept="image/*"></div>
            </div>

            <div style="margin-top:.6rem"><label class="muted" style="display:block;margin-bottom:.25rem">Notes</label><textarea id="tradeNotes" placeholder="What was your setup? What went well? What to improve next time..." style="min-height:120px"></textarea></div>

            <div style="display:flex;align-items:center;gap:.5rem;margin-top:.6rem">
              <button class="btn-primary" id="logTradeBtn">Add to Journal</button>
              <button class="btn-secondary" id="clearJournalBtn">Clear</button>
            </div>
          </div>

          <h3 style="margin-top:1rem">Journal Entries</h3>
          <div class="trades-log" id="tradesLog"></div>
        </div>

        <div id="statsTab" class="tab" style="display:none">
            <h2>Statistics</h2>
            <div class="stat-grid" style="margin-top:.75rem; grid-template-columns: repeat(2, 1fr); gap: 1rem;">
                <div class="stat-card"><div class="stat-label">Total P&L</div><div class="stat-value" id="statTotalPnL">$0.00</div></div>
                <div class="stat-card"><div class="stat-label">Profit Factor</div><div class="stat-value" id="statProfitFactor">0.00</div></div>
                <div class="stat-card"><div class="stat-label">Avg. Winning Day</div><div class="stat-value profit" id="statAvgWin">$0.00</div></div>
                <div class="stat-card"><div class="stat-label">Avg. Losing Day</div><div class="stat-value loss" id="statAvgLoss">$0.00</div></div>
                <div class="stat-card"><div class="stat-label">Best Day</div><div class="stat-value profit" id="statBestTrade">$0.00</div></div>
                <div class="stat-card"><div class="stat-label">Worst Day</div><div class="stat-value loss" id="statWorstTrade">$0.00</div></div>
            </div>

            <div id="statsCharts" class="charts-grid">
                <div class="chart-card">
                    <div class="muted" style="margin-bottom:.4rem">Equity Curve & Daily P&L</div>
                    <canvas id="equityCanvas" height="180"></canvas>
                </div>
                <div class="chart-card">
                    <div class="muted" style="margin-bottom:.4rem">Daily Trades</div>
                    <canvas id="tradesBarCanvas" height="180"></canvas>
                </div>
                <div class="chart-card">
                    <div class="muted" style="margin-bottom:.4rem">Win vs Loss Days</div>
                    <canvas id="winLossDonutCanvas" height="180"></canvas>
                </div>
            </div>
        </div>

        <div id="zenTab" class="tab" style="display:none">
            <div class="zen-hero">
                <h1>The Trader's Sanctuary</h1>
                <p>A clear mind is your greatest asset. Use these tools to detach from market noise, reduce emotional trading, and find your focus.</p>
            </div>

            <div class="zen-grid">
                <div class="zen-feature-card">
                    <h3>Guided Meditation</h3>
                    <p class="muted" style="margin-bottom:1rem">Quick sessions to reset your emotional state before or after trading.</p>
                    <div class="meditation-options">
                        <div class="meditation-option-card" id="startMeditation5">
                            <h4>5 min</h4>
                            <p>Quick Reset</p>
                        </div>
                        <div class="meditation-option-card" id="startMeditation10">
                            <h4>10 min</h4>
                            <p>Deeper Focus</p>
                        </div>
                    </div>
                </div>

                <div class="zen-feature-card">
                    <h3>Breathing Exercise</h3>
                    <p class="muted" style="margin-bottom:1rem">Use this visual pacer to regulate your nervous system through box breathing.</p>
                    <div class="breathing-visualizer" id="breathingVisualizer">
                        <div id="breathPacer"><span id="breathInstruction">Start</span></div>
                        <button class="btn-secondary" id="toggleBreathworkBtn">Start Breathing</button>
                    </div>
                </div>
            </div>

            <div class="zen-grid" style="margin-top:1.5rem">
                 <div class="zen-feature-card">
                    <h3>Focus Soundscapes</h3>
                    <p class="muted" style="margin-bottom:1rem">Tune out distractions with calming ambient sounds.</p>
                    <div class="soundscape-controls">
                        <button class="sound-btn" data-sound="rain">🌧️</button>
                        <button class="sound-btn" data-sound="forest">🌳</button>
                        <button class="sound-btn" data-sound="waves">🌊</button>
                        <button class="sound-btn" data-sound="none">🚫</button>
                    </div>
                    <p id="nowPlayingSound">Now Playing: None</p>
                </div>
                <div class="zen-feature-card">
                    <h3>Mindfulness Journal</h3>
                    <p class="muted" style="margin-bottom:1rem">A private space to log your mental state. How do you feel right now?</p>
                    <textarea id="mindfulnessJournal" placeholder="e.g., Feeling anxious about the open..."></textarea>
                    <button class="btn-secondary" id="saveMindfulnessBtn">Save Note</button>
                </div>
            </div>
        </div>


        <div id="aiTab" class="tab" style="display:none">
            <h2>AI Mentor - IntradelAI</h2>
            <div class="ai-suggested-prompts">
                <button class="btn-secondary ai-prompt-btn">Analyze my biggest weakness</button>
                <button class="btn-secondary ai-prompt-btn">What are the patterns in my winning trades?</button>
                <button class="btn-secondary ai-prompt-btn">How can I improve my risk management?</button>
            </div>
            <div id="aiChatWindow">
                <div class="ai-chat-bubble ai">
                    Hello! I'm IntradelAI. I'm ready to analyze your journal. Ask me a question or use one of the prompts above to get started.
                </div>
            </div>
            <div class="ai-input-form">
                <input id="aiQuestion" type="text" placeholder="Ask IntradelAI...">
                <button class="btn-primary" id="askAIBtn">Send</button>
            </div>
        </div>

        <div id="forumTab" class="tab" style="display:none">
            <h2>Community Forum</h2>
            <div id="adminAnnouncementControls" style="display:none; background: rgba(255,255,255,0.05); padding: 1rem; border-radius: 12px; margin-bottom: 1rem;">
                <h4 style="color: var(--accent-soft);">Admin Announcement</h4>
                <textarea id="announcementInput" placeholder="Type your announcement..." style="min-height: 60px; margin: 0.5rem 0;"></textarea>
                <div style="display:flex; align-items: center; gap: 1rem;">
                    <select id="announcementDuration">
                        <option value="3600">Pin for 1 Hour</option>
                        <option value="86400">Pin for 1 Day</option>
                        <option value="604800">Pin for 1 Week</option>
                    </select>
                    <button id="postAnnouncementBtn" class="btn-primary">Post & Pin</button>
                </div>
            </div>
            <div id="announcementContainer"></div>
            <div id="forumChatWindow" class="chat-container"></div>
            <div id="forumInputForm" class="chat-input-form">
                <input id="forumMessageInput" type="text" placeholder="Type a message...">
                <input type="file" id="forumImageInput" accept="image/*" style="display: none;">
                <button id="forumImageUploadBtn" class="btn-secondary" title="Upload Image">🖼️</button>
                <button id="forumSendMessageBtn" class="btn-primary">Send</button>
            </div>
        </div>

        <div id="friendsTab" class="tab" style="display:none">
            <div class="sub-nav">
                <div class="sub-nav-item active" data-subtab="friendsList">Friends</div>
                <div class="sub-nav-item" data-subtab="friendsRequests">Requests</div>
            </div>
            <div id="friendsListContainer">
                <h2>My Friends</h2>
                <div id="friendsList" class="friends-list"></div>
            </div>
            <div id="friendsRequestsContainer" style="display:none;">
                <h2>Incoming Friend Requests</h2>
                <div id="requestsList" class="requests-list"></div>
            </div>
        </div>

        <div id="dmTab" class="tab" style="display:none">
            <div id="dmUserList"></div>
            <div id="dmContent">
                <div id="dmChatWindow" class="chat-container">
                    <div class="dm-placeholder">
                        <h3>Direct Messages</h3>
                        <p>Select a friend to start a conversation.</p>
                    </div>
                </div>
                <div id="dmInputForm" class="chat-input-form">
                    <input id="dmMessageInput" type="text" placeholder="Type a message...">
                    <button id="dmSendMessageBtn" class="btn-primary">Send</button>
                </div>
            </div>
        </div>

        <div id="settingsTab" class="tab" style="display:none">
            <h2>Settings</h2>
            <div class="settings-container">
                <div class="settings-nav">
                    <div class="settings-nav-item active" data-pane="profile">Profile</div>
                    <div class="settings-nav-item" data-pane="account">Account</div>
                    <div class="settings-nav-item" data-pane="appearance">Appearance</div>
                </div>
                <div class="settings-content">
                    <!-- Profile Pane -->
                    <div id="profilePane" class="settings-pane active">
                        <div class="settings-card">
                            <h3>Public Profile</h3>
                            <div class="settings-field">
                                <label for="newUsername">Username</label>
                                <div class="field-group">
                                    <input id="newUsername" type="text" placeholder="Enter new username">
                                    <button id="saveUsernameBtn" class="btn-secondary">Save</button>
                                </div>
                                <small id="usernameCooldown" class="muted"></small>
                            </div>
                            <div class="settings-field">
                                <label for="profilePicInput">Profile Avatar</label>
                                <div class="field-group">
                                    <input id="profilePicInput" type="file" accept="image/*,image/gif,image/jfif">
                                    <button id="saveProfileBtn" class="btn-secondary">Upload</button>
                                </div>
                                <img id="profilePicPreview" alt="Profile preview" style="width:96px;height:96px;border-radius:50%;object-fit:cover;border:1px solid var(--stroke-weak);display:none; margin-top: 1rem;"/>
                            </div>
                        </div>
                    </div>
                    <!-- Account Pane -->
                    <div id="accountPane" class="settings-pane">
                        <div class="settings-card">
                            <h3>Account Details</h3>
                            <div class="settings-field">
                                <label>Email Address</label>
                                <div id="settingsEmail"></div>
                            </div>
                             <div class="settings-field">
                                <label>Member Since</label>
                                <div id="settingsCreatedDate"></div>
                            </div>
                            <div class="settings-field">
                                <label for="newEmail">Change Email</label>
                                <input id="newEmail" type="email" placeholder="Enter new email">
                            </div>
                            <div class="settings-field">
                                <label for="newPassword">Change Password</label>
                                <input id="newPassword" type="password" placeholder="Enter new password">
                            </div>
                            <button class="btn-primary" id="saveAccountSettings">Save Account Changes</button>
                            <div id="settingsMsg" style="display:none;margin-top:.5rem;color:#9fe6a7"></div>
                        </div>
                        <div class="settings-card">
                            <h3>Privacy</h3>
                            <div class="settings-field">
                                <label>Profile Visibility</label>
                                <div class="toggle-switch">
                                    <label class="muted">Private</label>
                                    <label class="switch">
                                        <input type="checkbox" id="profilePrivacyToggle">
                                        <span class="slider"></span>
                                    </label>
                                    <label class="muted">Public</label>
                                </div>
                                <p class="muted" style="font-size:0.9em; max-width: 500px; margin-top: 0.5rem;">When public, other users can see your subscription plan and join date.</p>
                            </div>
                        </div>
                    </div>
                    <!-- Appearance Pane -->
                    <div id="appearancePane" class="settings-pane">
                        <div class="settings-card">
                            <h3>Theme Presets</h3>
                            <p class="muted" style="margin-bottom:.6rem">Choose a base theme for the application.</p>
                            <select id="themeSelect">
                                <option value="theme-light">☀️ Light Mode</option>
                                <option value="theme-redblack">🔥 Red & Black (Default)</option>
                                <option value="theme-blueblack">💧 Blue & Black</option>
                                <option value="theme-greenblack">🍃 Green & Black</option>
                                <option value="theme-purpleblack">💜 Purple & Black</option>
                            </select>
                        </div>
                        <div class="settings-card">
                            <h3>Color Customization</h3>
                            <p class="muted" style="margin-bottom:1rem">Override the current theme's colors.</p>
                            <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:1rem">
                                <div>
                                    <label class="muted" style="display:block;margin-bottom:.25rem">Accent</label>
                                    <input type="color" id="colorAccent" value="#ff6b6b"/>
                                </div>
                                <div>
                                    <label class="muted" style="display:block;margin-bottom:.25rem">Winning Day (Green)</label>
                                    <input type="color" id="colorSuccess" value="#2ddc90"/>
                                </div>
                                <div>
                                    <label class="muted" style="display:block;margin-bottom:.25rem">Losing Day (Red)</label>
                                    <input type="color" id="colorDanger" value="#ff6b6b"/>
                                </div>
                            </div>
                            <div style="display:flex;gap:.6rem;margin-top:1rem">
                                <button class="btn-primary" id="applyColorsBtn">Apply & Save</button>
                                <button class="btn-secondary" id="resetColorsBtn">Reset to Default</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div id="adminTab" class="tab" style="display:none">
            <h2>Admin Dashboard</h2>
            <div class="stat-grid" style="grid-template-columns: repeat(3, 1fr); margin-top: 1rem;">
                <div class="stat-card">
                    <div class="stat-label">Total Registered Users</div>
                    <div class="stat-value" id="adminTotalUsers">0</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Total Journal Entries</div>
                    <div class="stat-value" id="adminTotalEntries">0</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Total Platform P&L</div>
                    <div class="stat-value" id="adminTotalPnl">$0</div>
                </div>
            </div>

            <div style="display:grid; grid-template-columns: 1.7fr 1fr; gap: 1rem;">
                <div>
                    <h3 style="color: var(--accent-soft); margin-bottom: 0.5rem;">User Management</h3>
                     <select id="adminUserFilter" style="max-width: 200px; margin-bottom: 1rem;">
                        <option value="24h">Active (24h)</option>
                        <option value="7d">Active (7d)</option>
                        <option value="30d">Active (30d)</option>
                        <option value="all" selected>All Users</option>
                    </select>
                    <div style="overflow-x: auto;">
                        <table class="admin-user-table">
                            <thead>
                                <tr>
                                    <th>User</th>
                                    <th>Subscription</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="adminUserTableBody">
                                <!-- User rows will be injected here -->
                            </tbody>
                        </table>
                    </div>
                </div>
                <div>
                    <h3 style="color: var(--accent-soft); margin-bottom: 0.5rem;">Recent Activity</h3>
                    <div id="adminActivityLog" class="admin-activity-log">
                        <!-- Activity will be injected here -->
                    </div>
                </div>
            </div>
        </div>

      </div>
    </div>
</main>


<!-- Modals -->
<div id="loginModal" class="modal" aria-hidden="true">
  <div class="modal-content">
    <span class="close" data-close="loginModal">&times;</span>
    <h2>Login</h2>
    <div id="loginForm">
      <div style="margin-bottom:.6rem"><label class="muted" style="display:block;margin-bottom:.3rem">Email</label><input id="loginEmail" type="email" placeholder="you@email.com"></div>
      <div style="margin-bottom:.6rem;position:relative">
        <label class="muted" style="display:block;margin-bottom:.3rem">Password</label>
        <input id="loginPassword" type="password" placeholder="Password">
        <label style="position:absolute;right:10px;top:36px;cursor:pointer;color:var(--muted)"><input id="loginShowPassword" type="checkbox" style="margin-right:.35rem">Show</label>
      </div>
      <div style="margin-bottom:.6rem;display:flex;align-items:center;gap:.5rem"><input id="rememberMe" type="checkbox"><label for="rememberMe" class="muted">Remember me</label></div>
      <div><button class="btn-primary" id="performLoginBtn">Login</button></div>
      <p class="muted" style="margin-top:.5rem">Don't have an account? <a id="openRegisterLink" style="color:var(--accent-soft);cursor:pointer">Register here</a></p>
      <div id="loginError" style="display:none;color:#ff8080;margin-top:.4rem"></div>
    </div>
  </div>
</div>

<div id="registerModal" class="modal" aria-hidden="true">
  <div class="modal-content">
    <span class="close" data-close="registerModal">&times;</span>
    <h2>Register</h2>
    <div id="registerForm">
      <div style="margin-bottom:.45rem"><label class="muted" style="display:block;margin-bottom:.3rem">Username</label><input id="registerName" type="text"></div>
      <div style="margin-bottom:.45rem"><label class="muted" style="display:block;margin-bottom:.3rem">Email</label><input id="registerEmail" type="email"></div>
      <div style="display:flex;gap:.6rem">
        <div style="flex:1;position:relative"><label class="muted" style="display:block;margin-bottom:.3rem">Password</label><input id="registerPassword" type="password"></div>
        <div style="flex:1;position:relative"><label class="muted" style="display:block;margin-bottom:.3rem">Confirm</label><input id="registerConfirmPassword" type="password"></div>
      </div>
      <div style="margin-top:.4rem"><label style="cursor:pointer;color:var(--muted)"><input id="registerShowPassword" type="checkbox" style="margin-right:.35rem">Show passwords</label></div>
      <div style="margin-top:.6rem"><button class="btn-primary" id="performRegisterBtn">Register</button></div>
      <p class="muted" style="margin-top:.5rem">Already have an account? <a id="openLoginLink" style="color:var(--accent-soft);cursor:pointer">Login here</a></p>
      <div id="registerError" style="display:none;color:#ff8080;margin-top:.4rem"></div>
    </div>
  </div>
</div>

<div id="editTradeModal" class="modal" aria-hidden="true">
  <div class="modal-content">
    <span class="close" data-close="editTradeModal">&times;</span>
    <h2>Edit Journal Entry</h2>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:.6rem">
      <div><label class="muted" style="display:block;margin-bottom:.25rem">Symbol</label><input id="editTradeSymbol" type="text"></div>
      <div><label class="muted" style="display:block;margin-bottom:.25rem">Side</label>
        <select id="editTradeSide"><option value="long">Long</option><option value="short">Short</option></select>
      </div>
      <div><label class="muted" style="display:block;margin-bottom:.25rem">Outcome</label>
        <select id="editTradeOutcome"><option value="win">Win</option><option value="loss">Loss</option></select>
      </div>
    </div>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:.6rem;margin-top:.6rem">
      <div><label class="muted" style="display:block;margin-bottom:.25rem">Entry</label><input id="editTradeEntry" type="number" step="0.0001"></div>
      <div><label class="muted" style="display:block;margin-bottom:.25rem">Exit</label><input id="editTradeExit" type="number" step="0.0001"></div>
      <div><label class="muted" style="display:block;margin-bottom:.25rem">Size</label><input id="editTradeSize" type="number" step="0.01"></div>
    </div>
    <div style="margin-top:.6rem"><label class="muted" style="display:block;margin-bottom:.25rem">Notes</label><textarea id="editTradeNotes" style="min-height:90px"></textarea></div>
    <div style="margin-top:.6rem">
      <label class="muted" style="display:block;margin-bottom:.25rem">Screenshot</label>
      <input id="editTradeScreenshot" type="file" accept="image/*">
      <div id="editScreenshotPreview" style="margin-top:.5rem;display:none">
        <img id="editScreenshotImg" src="" alt="Screenshot" style="max-width:100%;border-radius:.5rem;border:1px solid var(--stroke-weak)"/>
        <div style="margin-top:.4rem"><button class="btn-secondary" id="removeEditScreenshotBtn">Remove Image</button></div>
      </div>
    </div>
    <div style="margin-top:.8rem"><button class="btn-primary" id="saveEditTradeBtn">Save Changes</button></div>
  </div>
</div>

<div id="dayModal" class="modal" aria-hidden="true">
  <div class="modal-content">
    <span class="close" data-close="dayModal">&times;</span>
    <h2>Edit Day <span id="modalDay"></span></h2>

    <div class="muted" id="daySummary" style="margin:.25rem 0 .6rem 0"></div>

    <div style="margin-bottom:.6rem">
      <label class="muted" style="display:block;margin-bottom:.3rem">Number of Trades</label>
      <input id="dayTrades" type="number" min="0">
    </div>

    <div style="display:flex;gap:.6rem;margin-bottom:.6rem">
      <div style="flex:1">
        <label class="muted" style="display:block;margin-bottom:.3rem">Result</label>
        <select id="dayResult" style="width:100%">
          <option value="win">Win</option>
          <option value="loss">Loss</option>
        </select>
      </div>
      <div style="flex:1">
        <label class="muted" style="display:block;margin-bottom:.3rem">Amount ($)</label>
        <input id="dayPnLAmount" type="number" step="0.01" min="0" placeholder="e.g. 500.00">
      </div>
    </div>

    <div style="margin-bottom:.6rem">
      <label class="muted" style="display:block;margin-bottom:.3rem">Attach Trade Image (optional)</label>
      <input id="dayImage" type="file" accept="image/*">
      <div id="dayImagePreview" style="margin-top:.5rem; display:none">
        <img id="dayImagePreviewImg" src="" alt="Trade image" style="max-width:100%;border-radius:.5rem;border:1px solid var(--stroke-weak)"/>
        <div style="margin-top:.4rem"><button class="btn-secondary" id="removeDayImageBtn">Remove Image</button></div>
      </div>
    </div>

    <div><button class="btn-primary" id="saveDayBtn">Save</button></div>
  </div>
</div>

<div id="deleteConfirmModal" class="modal" aria-hidden="true">
  <div class="modal-content" style="max-width:420px;text-align:center">
    <h3 id="deleteConfirmTitle">Confirm Delete</h3>
    <p id="deleteConfirmText" style="color:var(--muted);margin-top:.4rem">Are you sure you want to delete this entry?</p>
    <div style="display:flex;gap:.6rem;justify-content:center;margin-top:1rem">
      <button class="btn-secondary" id="cancelDeleteBtn">Cancel</button>
      <button class="btn-primary" id="confirmDeleteBtn">Delete</button>
    </div>
  </div>
</div>

<div id="userProfileModal" class="modal" aria-hidden="true">
    <div class="modal-content">
        <span class="close" data-close="userProfileModal">&times;</span>
        <div id="userProfileModalContent">
            <!-- Profile content injected by JS -->
        </div>
    </div>
</div>

<div id="muteUserModal" class="modal" aria-hidden="true">
    <div class="modal-content" style="max-width: 480px;">
        <span class="close" data-close="muteUserModal">&times;</span>
        <h2 id="muteUserTitle">Mute User</h2>
        <p class="muted" style="margin-bottom: 1rem;">Select a duration to mute this user from the forum.</p>
        <select id="muteDurationSelect" style="margin-bottom: 1rem;">
            <option value="900">15 Minutes</option>
            <option value="3600">1 Hour</option>
            <option value="86400">1 Day</option>
            <option value="-1">Permanent</option>
        </select>
        <button id="confirmMuteBtn" class="btn-primary">Apply Mute</button>
    </div>
</div>


<footer id="footer">
  <div class="inner">
    <span>&copy; 2025 INTRADEL - AI Trading Journal & Mentor Platform. All rights reserved.</span>
  </div>
</footer>

<script>
/* ========== Helpers ========== */
function $(id){ return document.getElementById(id); }
function on(id, evt, handler, options){ const el = $(id); if(el) el.addEventListener(evt, handler, options||false); }
function val(id, dflt){ const el = $(id); return el && typeof el.value !== 'undefined' ? el.value : (dflt||''); }

/* State */
let currentUser = null;
let trades = []; // journal entries (type: 'journal')
let dailyPnL = {}; // DEPRECATED: This will now be computed on the fly.
let selectedDateKey = null;
let currentPeriod = 'currentMonth';
let meditationTimer = null;
let instructionInterval = null;
let meditationTimeLeft = 0;
let editingTradeId = null;
let dayImageRemove = false;
const ADMIN_EMAIL = 'intradelai@gmail.com';
let breathworkInterval = null;
let pendingDeleteCallback = null;
let forumPollingInterval = null;
let currentOpenDm = null;
let userToMute = null;

/* Social/Profile state */
let userProfile = { avatarData: null, mindfulnessJournal: '', isPublic: true, friends: [], friendRequests: [] };
let userColors = { accent: null, success: null, danger: null };


/* Charts */
let equityChart = null;
let tradesBarChart = null;
let winLossDonutChart = null;
let mockupEquityChart = null;
let mockupWinLossDonutChart = null;
let ai_pipeline = null;


/* Storage + Accounts */
function initializeUserAccounts(){
  let users = JSON.parse(localStorage.getItem('intradelUsers') || '{}');
  if(!users[ADMIN_EMAIL]) {
      users[ADMIN_EMAIL] = { name: 'INTRADEL', email: ADMIN_EMAIL, password: 'INTRADEL', createdAt: new Date().toISOString(), lastLogin: new Date().toISOString(), isBanned: false, subscription: 'Elite', usernameLastChanged: null };
      localStorage.setItem('intradelUsers', JSON.stringify(users));
  }
  if(!localStorage.getItem('intradelUserData')) localStorage.setItem('intradelUserData', JSON.stringify({}));
  if(!localStorage.getItem('intradelActivityLog')) localStorage.setItem('intradelActivityLog', JSON.stringify([]));
  if(!localStorage.getItem('intradelForumMessages')) localStorage.setItem('intradelForumMessages', JSON.stringify([]));
  if(!localStorage.getItem('intradelAnnouncements')) localStorage.setItem('intradelAnnouncements', JSON.stringify([]));
  if(!localStorage.getItem('intradelMutedUsers')) localStorage.setItem('intradelMutedUsers', JSON.stringify({}));
  if(!localStorage.getItem('intradelDms')) localStorage.setItem('intradelDms', JSON.stringify({}));
}
function getAllUsers(){ return JSON.parse(localStorage.getItem('intradelUsers') || '{}'); }
function getAllUserData(){ return JSON.parse(localStorage.getItem('intradelUserData') || '{}'); }
function getActivityLog(){ return JSON.parse(localStorage.getItem('intradelActivityLog') || '[]'); }
function getMutedUsers() { return JSON.parse(localStorage.getItem('intradelMutedUsers') || '{}'); }
function saveMutedUsers(muted) { localStorage.setItem('intradelMutedUsers', JSON.stringify(muted)); }
function getAllDms() { return JSON.parse(localStorage.getItem('intradelDms') || '{}'); }
function saveAllDms(dms) { localStorage.setItem('intradelDms', JSON.stringify(dms)); }
function getAnnouncements() { return JSON.parse(localStorage.getItem('intradelAnnouncements') || '[]'); }
function saveAnnouncements(ann) { localStorage.setItem('intradelAnnouncements', JSON.stringify(ann)); }

function addActivityLog(message){
    let log = getActivityLog();
    log.unshift({ message, timestamp: new Date().toISOString() });
    if (log.length > 50) log.pop(); // Keep log from getting too large
    localStorage.setItem('intradelActivityLog', JSON.stringify(log));
}

function saveUserAccount(email, userData){
  const users = getAllUsers();
  if(users[email]) return false;
  users[email] = userData;
  localStorage.setItem('intradelUsers', JSON.stringify(users));
  return true;
}
function updateUserAccount(email, userData){
  const users = getAllUsers();
  users[email] = Object.assign({}, users[email] || {}, userData);
  localStorage.setItem('intradelUsers', JSON.stringify(users));
}
function getUserByEmail(email){ const users = getAllUsers(); return users[email] || null; }
function saveUserData(email, data){
  const all = getAllUserData();
  all[email] = data;
  localStorage.setItem('intradelUserData', JSON.stringify(all));
}
function buildUserData(){
  const defaultProfile = { avatarData: null, mindfulnessJournal: '', isPublic: true, friends: [], friendRequests: [] };
  // dailyPnL is no longer saved, it's computed from trades
  return { trades, profile: userProfile || defaultProfile, colors: userColors || { accent:null, success:null, danger:null } };
}
function loadUserData(email){
  const all = getAllUserData();
  const base = all[email] || { trades: [] };
  const defaultProfile = { avatarData: null, mindfulnessJournal: '', isPublic: true, friends: [], friendRequests: [] };
  base.profile = Object.assign(defaultProfile, base.profile || {});
  base.colors = base.colors || { accent: null, success: null, danger: null };
  return base;
}
function saveRemembered(creds){ localStorage.setItem('intradelRemember', JSON.stringify(creds)); }
function loadRemembered(){ try { return JSON.parse(localStorage.getItem('intradelRemember') || 'null'); } catch(e){ return null; } }
function clearRemembered(){ localStorage.removeItem('intradelRemember'); }

function showNotification(msg){
  const prev = $('notification');
  if(prev) prev.remove();
  const n = document.createElement('div');
  n.id = 'notification';
  n.textContent = msg;
  document.body.appendChild(n);
  setTimeout(()=>n.remove(), 3000);
}
function showError(id, msg){
  const el = $(id);
  if(!el) return;
  el.textContent = msg;
  el.style.display = 'block';
  setTimeout(()=>el.style.display='none', 4000);
}

/* Auth */
function performRegister(){
  const name = val('registerName').trim();
  const email = val('registerEmail').trim().toLowerCase();
  const password = val('registerPassword');
  const confirm = val('registerConfirmPassword');
  if(!name || !email || !password || !confirm){ showError('registerError','Please fill all fields'); return; }
  if (name.length < 3 || name.length > 20) { showError('registerError','Username must be 3-20 characters.'); return; }
  if(password !== confirm){ showError('registerError','Passwords do not match'); return; }
  if(password.length < 6){ showError('registerError','Password must be at least 6 characters'); return; }
  if(getUserByEmail(email)){ showError('registerError','Account already exists. Please login.'); return; }
  const now = new Date().toISOString();
  const userObj = { name, email, password, createdAt: now, lastLogin: now, isBanned: false, subscription: 'Starter', usernameLastChanged: null };
  const ok = saveUserAccount(email, userObj);
  if(!ok){ showError('registerError','Unable to create account'); return; }
  
  const defaultProfile = { avatarData: null, mindfulnessJournal: '', isPublic: true, friends: [], friendRequests: [] };
  saveUserData(email, { trades: [], profile: defaultProfile, colors: { accent:null, success:null, danger:null } });
  
  addActivityLog(`New user registered: ${name} (${email})`);

  currentUser = { email, name, subscription: 'Starter' };
  trades = [];
  userProfile = defaultProfile;
  userColors = { accent:null, success:null, danger:null };
  updateUI();
  closeModal('registerModal');
  showPage('dashboard');
  renderRecentTrades();
  renderDashboard();
  applyUserColors(userColors);
  startForumPolling();
  showNotification('Welcome to INTRADEL, ' + name + '!');
}
function performLogin(){
  const email = val('loginEmail').trim().toLowerCase();
  const password = val('loginPassword');
  if(!email || !password){ showError('loginError','Please provide email and password'); return; }
  const user = getUserByEmail(email);
  if(!user || user.password !== password){ showError('loginError','Invalid email or password'); return; }
  if(user.isBanned) { showError('loginError', 'This account has been suspended.'); return; }
  
  user.lastLogin = new Date().toISOString();
  updateUserAccount(email, user);

  addActivityLog(`User logged in: ${user.name} (${email})`);

  currentUser = { email: user.email, name: user.name, subscription: user.subscription || 'Starter' };
  const ud = loadUserData(email);
  trades = ud.trades || [];
  userProfile = ud.profile;
  userColors = ud.colors || { accent:null, success:null, danger:null };
  loadTheme(); // Load theme after user data is loaded
  updateUI();
  closeModal('loginModal');
  showPage('dashboard');
  renderRecentTrades();
  renderDashboard();
  applyUserColors(userColors);
  startForumPolling();
  showNotification('Welcome back, ' + (user.name || '') + '!');
  const rememberBox = $('rememberMe');
  const remember = rememberBox && rememberBox.checked;
  if(remember) saveRemembered({ email, password });
  else clearRemembered();
}
function tryAutoLogin(){
  const creds = loadRemembered();
  if(!creds) return;
  const user = getUserByEmail(creds.email);
  if(user && user.password === creds.password){
    if($('loginEmail')) $('loginEmail').value = creds.email;
    if($('loginPassword')) $('loginPassword').value = creds.password;
    const rm = $('rememberMe'); if(rm) rm.checked = true;
    performLogin();
  } else {
    clearRemembered();
  }
}
function updateAvatarUI(){
  const img = $('userAvatar');
  if(img){
    if(currentUser && userProfile && userProfile.avatarData){
      img.src = userProfile.avatarData;
      img.style.display = 'inline-block';
    } else {
      img.removeAttribute('src');
      img.style.display = 'none';
    }
  }
  const prev = $('profilePicPreview');
  if(prev){
    if(userProfile && userProfile.avatarData){
      prev.src = userProfile.avatarData;
      prev.style.display = 'block';
    } else {
      prev.removeAttribute('src');
      prev.style.display = 'none';
    }
  }
}
function updateUI(){
  const loginBtn = $('loginBtn');
  const registerBtn = $('registerBtn');
  const logoutBtn = $('logoutBtn');
  const userDisplay = $('userDisplay');
  const ctaBtn = $('ctaBtn');
  const adminNav = $('adminNav');
  const forumNav = $('forumNav');
  const friendsNav = $('friendsNav');
  const dmNav = $('dmNav');
  const finalCtaBtn = $('finalCtaRegisterBtn');

  if(currentUser){
    document.body.classList.remove('page-home');
    if(loginBtn) loginBtn.style.display = 'none';
    if(registerBtn) registerBtn.style.display = 'none';
    if(logoutBtn) logoutBtn.style.display = 'inline-block';
    if(userDisplay){ userDisplay.style.display = 'inline-block'; userDisplay.textContent = currentUser.name || currentUser.email; }
    if(ctaBtn) ctaBtn.style.display = 'none';
    if(finalCtaBtn) finalCtaBtn.style.display = 'none';
    
    const isAdmin = currentUser.email === ADMIN_EMAIL;
    if(adminNav) adminNav.style.display = isAdmin ? 'flex' : 'none';
    if($('adminAnnouncementControls')) $('adminAnnouncementControls').style.display = isAdmin ? 'block' : 'none';

    
    const isSubscribed = currentUser.subscription === 'Pro' || currentUser.subscription === 'Elite' || isAdmin;
    if(forumNav) forumNav.style.display = isSubscribed ? 'flex' : 'none';
    if(friendsNav) friendsNav.style.display = isSubscribed ? 'flex' : 'none';
    if(dmNav) dmNav.style.display = isSubscribed ? 'flex' : 'none';

    // Friend request notification
    const reqCount = (userProfile.friendRequests || []).length;
    const badgeEl = friendsNav ? friendsNav.querySelector('.notification-badge') : null;
    if (reqCount > 0 && friendsNav) {
        if (badgeEl) {
            badgeEl.textContent = reqCount;
        } else {
            const newBadge = document.createElement('span');
            newBadge.className = 'notification-badge';
            newBadge.textContent = reqCount;
            friendsNav.appendChild(newBadge);
        }
    } else if (badgeEl) {
        badgeEl.remove();
    }

  } else {
    document.body.classList.add('page-home');
    if(loginBtn) loginBtn.style.display = 'inline-block';
    if(registerBtn) registerBtn.style.display = 'inline-block';
    if(logoutBtn) logoutBtn.style.display = 'none';
    if(userDisplay) userDisplay.style.display = 'none';
    if(ctaBtn) ctaBtn.style.display = 'inline-block';
    if(finalCtaBtn) finalCtaBtn.style.display = 'block';
    if(adminNav) adminNav.style.display = 'none';
    if(forumNav) forumNav.style.display = 'none';
    if(friendsNav) friendsNav.style.display = 'none';
    if(dmNav) dmNav.style.display = 'none';
  }
  updateAvatarUI();
}
function logout(){
  if(currentUser) saveUserData(currentUser.email, buildUserData());
  if (forumPollingInterval) clearInterval(forumPollingInterval);
  currentUser = null;
  trades = [];
  userProfile = { avatarData:null, mindfulnessJournal: '', isPublic: true, friends: [], friendRequests: [] };
  userColors = { accent:null, success:null, danger:null };
  updateUI();
  showPage('home');
  renderRecentTrades();
  renderDashboard();
  showNotification('Logged out successfully');
  clearRemembered();
}
function showPage(pageId){
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  const el = $(pageId + 'Page');
  if(el) el.classList.add('active');
  if(pageId === 'home') document.body.classList.add('page-home');
  else document.body.classList.remove('page-home');
}
function switchToTab(tab){
  document.querySelectorAll('#sidebar .nav-item').forEach(n => n.classList.remove('active'));
  document.querySelectorAll('#sidebar .nav-item').forEach(n => { if(n.dataset.tab === tab) n.classList.add('active'); });
  document.querySelectorAll('.tab').forEach(t => t.style.display = 'none');
  const tabEl = $(tab + 'Tab');
  if(tabEl) tabEl.style.display = 'block';
  
  // Tab-specific render functions
  if(tab === 'settings') renderProfileSettings();
  if(tab === 'admin') renderAdminDashboard();
  if(tab === 'forum') renderForum();
  if(tab === 'friends') renderFriendsList();
  if(tab === 'dm') renderDmView();
  if(tab === 'zen') {
      const journalText = (userProfile && userProfile.mindfulnessJournal) ? userProfile.mindfulnessJournal : '';
      if($('mindfulnessJournal')) $('mindfulnessJournal').value = journalText;
  }
}

function renderAdminDashboard() {
    if (!currentUser || currentUser.email !== ADMIN_EMAIL) return;

    const allUsers = getAllUsers();
    const allUserData = getAllUserData();
    const activityLog = getActivityLog();

    // Platform-wide stats
    const totalUsers = Object.keys(allUsers).length;
    let totalEntries = 0;
    let totalPnl = 0;
    for (const email in allUserData) {
        const data = allUserData[email];
        if (data.trades) totalEntries += data.trades.length;
        // Recompute PNL from trades for accuracy
        if (data.trades) {
             totalPnl += data.trades.reduce((sum, trade) => sum + (trade.pnl || 0), 0);
        }
    }
    $('adminTotalUsers').textContent = totalUsers;
    $('adminTotalEntries').textContent = totalEntries;
    const pnlEl = $('adminTotalPnl');
    pnlEl.textContent = `$${totalPnl.toFixed(2)}`;
    pnlEl.className = `stat-value ${totalPnl >= 0 ? 'profit' : 'loss'}`;

    // Render Recent Activity
    const activityLogEl = $('adminActivityLog');
    activityLogEl.innerHTML = activityLog.length > 0
        ? activityLog.slice(0, 10).map(item => `<div>${new Date(item.timestamp).toLocaleString()}: ${item.message}</div>`).join('')
        : '<div>No recent activity.</div>';

    // Filter and render user table
    const userTableBody = $('adminUserTableBody');
    userTableBody.innerHTML = '';
    const filter = val('adminUserFilter');
    const now = new Date().getTime();
    let cutoff;
    switch(filter) {
        case '24h': cutoff = now - (24 * 60 * 60 * 1000); break;
        case '7d': cutoff = now - (7 * 24 * 60 * 60 * 1000); break;
        case '30d': cutoff = now - (30 * 24 * 60 * 60 * 1000); break;
        case 'all': default: cutoff = 0;
    }

    const userList = Object.values(allUsers).filter(user => {
        if (filter === 'all') return true;
        const lastLogin = user.lastLogin ? new Date(user.lastLogin).getTime() : 0;
        return lastLogin >= cutoff;
    }).sort((a, b) => new Date(b.lastLogin || 0) - new Date(a.lastLogin || 0));

    if (userList.length === 0) {
        userTableBody.innerHTML = '<tr><td colspan="4" style="text-align:center; color: var(--muted);">No users match this filter.</td></tr>';
        return;
    }
    
    userList.forEach(user => {
        if (user.email === ADMIN_EMAIL) return; // Don't show admin in the list
        const userData = (getAllUserData()[user.email] || {});
        const pfpData = (userData.profile || {}).avatarData;
        const pfp = pfpData ? `<img src="${pfpData}" class="admin-user-pfp">` : `<img src="data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=" class="admin-user-pfp" style="background: var(--bg-2);">`;

        const row = document.createElement('tr');
        if (user.isBanned) row.classList.add('banned-user');
        
        const banButton = `<button class="btn-${user.isBanned ? 'unban' : 'ban'}" onclick="toggleBanStatus('${user.email}')">${user.isBanned ? 'Unban' : 'Ban'}</button>`;
        
        row.innerHTML = `
            <td>
                ${pfp}
                <div class="admin-user-info">
                    <span>${user.name}</span>
                    <span class="email">${user.email}</span>
                </div>
            </td>
            <td>
                <select onchange="updateUserSubscription('${user.email}', this.value)">
                    <option value="Starter" ${user.subscription === 'Starter' ? 'selected' : ''}>Starter</option>
                    <option value="Pro" ${user.subscription === 'Pro' ? 'selected' : ''}>Pro</option>
                    <option value="Elite" ${user.subscription === 'Elite' ? 'selected' : ''}>Elite</option>
                </select>
            </td>
            <td>${user.isBanned ? 'Banned' : 'Active'}</td>
            <td>${banButton}</td>
        `;
        userTableBody.appendChild(row);
    });
}
function toggleBanStatus(email) {
    let user = getUserByEmail(email);
    if (!user) return;
    user.isBanned = !user.isBanned;
    updateUserAccount(email, user);
    addActivityLog(`User ${user.name} was ${user.isBanned ? 'banned' : 'unbanned'}.`);
    renderAdminDashboard();
    renderForum(); // Re-render forum to reflect changes if any
}
function updateUserSubscription(email, newPlan) {
    let user = getUserByEmail(email);
    if (!user) return;
    user.subscription = newPlan;
    updateUserAccount(email, user);
    addActivityLog(`User ${user.name}'s subscription updated to ${newPlan}.`);
    renderAdminDashboard();
}

function renderProfileSettings() {
    if (!currentUser) return;
    const user = getUserByEmail(currentUser.email);
    if (!user) return;
    
    // Profile Pane
    $('newUsername').value = user.name || '';
    const usernameInput = $('newUsername');
    const usernameBtn = $('saveUsernameBtn');
    const cooldownEl = $('usernameCooldown');
    const lastChanged = user.usernameLastChanged ? new Date(user.usernameLastChanged).getTime() : 0;
    const now = new Date().getTime();
    const sevenDays = 7 * 24 * 60 * 60 * 1000;
    if (now - lastChanged < sevenDays) {
        usernameInput.disabled = true;
        usernameBtn.disabled = true;
        const nextChangeDate = new Date(lastChanged + sevenDays);
        cooldownEl.textContent = `You can change your username again on ${nextChangeDate.toLocaleDateString()}.`;
    } else {
        usernameInput.disabled = false;
        usernameBtn.disabled = false;
        cooldownEl.textContent = 'You can change your username once every 7 days.';
    }

    const prev = $('profilePicPreview');
    if(userProfile && userProfile.avatarData){
      prev.src = userProfile.avatarData;
      prev.style.display = 'block';
    } else {
      prev.style.display = 'none';
    }
    
    // Account Pane
    $('settingsEmail').textContent = user.email || 'N/A';
    $('settingsCreatedDate').textContent = user.createdAt ? new Date(user.createdAt).toLocaleDateString() : 'N/A';
    const privacyToggle = $('profilePrivacyToggle');
    if (privacyToggle) {
        privacyToggle.checked = userProfile.isPublic;
    }
    
    // Appearance Pane
    const theme = localStorage.getItem('intradelTheme') || 'theme-redblack';
    $('themeSelect').value = theme;
    $('colorAccent').value = (userColors && userColors.accent) || getComputedStyle(document.body).getPropertyValue('--accent').trim();
    $('colorSuccess').value = (userColors && userColors.success) || getComputedStyle(document.body).getPropertyValue('--success').trim();
    $('colorDanger').value = (userColors && userColors.danger) || getComputedStyle(document.body).getPropertyValue('--danger').trim();
}


/* Persist autosave */
setInterval(()=>{ if(currentUser) saveUserData(currentUser.email, buildUserData()); }, 30000);
window.addEventListener('beforeunload', ()=>{ if(currentUser) saveUserData(currentUser.email, buildUserData()); });

/* Theme */
function applyTheme(name){
  document.body.classList.remove('theme-redblack','theme-blueblack','theme-greenblack','theme-purpleblack', 'theme-light');
  document.body.classList.add(name);
  localStorage.setItem('intradelTheme', name || 'theme-redblack');
  const sel = $('themeSelect'); if(sel) sel.value = name || 'theme-redblack';
  
  // Re-apply custom colors on top of the new theme
  applyUserColors(userColors);

  // Redraw charts with new theme colors
  renderStatsCharts(getPeriodData(currentPeriod));
}
function loadTheme(){
  const t = localStorage.getItem('intradelTheme') || 'theme-redblack';
  const sel = $('themeSelect'); if(sel) sel.value = t;
  applyTheme(t);
}

/* Journal */
function clearJournalForm(){
  const ids = ['jSymbol','jEntry','jExit','jSize','tradeNotes','tradeDate'];
  ids.forEach(id => { const el = $(id); if(el) el.value = ''; });
  const side = $('jSide'); if(side) side.value='long';
  const out = $('tradeOutcome'); if(out) out.value='win';
  const file = $('jScreenshot'); if(file) file.value='';
  // Set date to today by default
  $('tradeDate').value = new Date().toISOString().split('T')[0];
}
function logTrade(){
  if(!currentUser){ showNotification('Please login to use Journal'); return; }
  const symbol = val('jSymbol').trim().toUpperCase();
  const side = val('jSide','long') || 'long';
  const outcome = val('tradeOutcome','win') || 'win';
  const dateVal = val('tradeDate');
  const entry = parseFloat(val('jEntry'));
  const exit = parseFloat(val('jExit'));
  const size = parseFloat(val('jSize'));
  const notes = val('tradeNotes').trim();
  const fileInput = $('jScreenshot');
  const file = fileInput && fileInput.files && fileInput.files[0] ? fileInput.files[0] : null;

  if(!dateVal){ showNotification('Please select a date'); return; }
  if(!notes && !symbol){ showNotification('Add at least a symbol or notes'); return; }

  const ts = new Date(dateVal + 'T12:00:00.000Z').toISOString();
  let pnl = 0;
  if(!Number.isNaN(entry) && !Number.isNaN(exit) && !Number.isNaN(size)){
    pnl = (exit - entry) * (side === 'short' ? -1 : 1) * size;
    if(outcome === 'loss' && pnl > 0) pnl = -Math.abs(pnl);
    if(outcome === 'win' && pnl < 0) pnl = Math.abs(pnl);
  }

  const base = { id: Date.now(), type: 'journal', symbol, side, outcome, entry, exit, size, notes, timestamp: ts, pnl: pnl, screenshotData: null };

  const finalize = (obj) => {
    trades.push(obj);
    autoSave();
    renderRecentTrades();
    renderDashboard();
    clearJournalForm();
    showNotification('Journal entry added');
  };

  if(file){
    const reader = new FileReader();
    reader.onload = (e)=> finalize(Object.assign({}, base, { screenshotData: e.target.result }));
    reader.onerror = ()=> finalize(base);
    reader.readAsDataURL(file);
  } else {
    finalize(base);
  }
}

function getJournalFilters(){
  const qEl = $('journalSearch'); const q = qEl ? (qEl.value || '').toLowerCase() : '';
  const sEl = $('journalSort'); const sort = sEl ? sEl.value : 'dateDesc';
  const oEl = $('journalFilterOutcome'); const fOutcome = oEl ? oEl.value : 'all';
  const sideEl = $('journalFilterSide'); const fSide = sideEl ? sideEl.value : 'all';
  const rEl = $('journalRange'); const range = rEl ? rEl.value : 'last14days';
  return { q, sort, fOutcome, fSide, range };
}
function getRangeDates(range){
  const now = new Date();
  let start, end;
  switch(range){
    case 'last7days': start = new Date(now); start.setDate(now.getDate()-6); end = now; break;
    case 'last14days': start = new Date(now); start.setDate(now.getDate()-13); end = now; break;
    case 'currentMonth': start = new Date(now.getFullYear(), now.getMonth(), 1); end = new Date(now.getFullYear(), now.getMonth()+1, 0); break;
    case 'all': default: start = new Date(0); end = now;
  }
  start = new Date(start.getFullYear(), start.getMonth(), start.getDate());
  end = new Date(end.getFullYear(), end.getMonth(), end.getDate());
  return { start, end };
}
function inRange(ts, start, end){
  if(!ts) return false;
  const d = new Date(ts.split('T')[0] + 'T00:00:00Z');
  return d >= start && d <= end;
}

function renderRecentTrades(){
  const container = $('tradesLog');
  if(!container) return;
  container.innerHTML = '';

  const { q, sort, fOutcome, fSide, range } = getJournalFilters();
  let items = trades.filter(t => t.type === 'journal');

  const { start, end } = getRangeDates(range);
  items = items.filter(t => inRange(t.timestamp, start, end));

  items = items.filter(t => {
    if(fOutcome !== 'all' && t.outcome !== fOutcome) return false;
    if(fSide !== 'all' && t.side !== fSide) return false;
    if(q){
      const hay = [t.symbol, (t.notes||'')].join(' ').toLowerCase();
      if(!hay.includes(q)) return false;
    }
    return true;
  });

  items.sort((a,b)=>{
    const ta = a.timestamp || '';
    const tb = b.timestamp || '';
    switch(sort){
      case 'dateAsc': return ta.localeCompare(tb);
      case 'winFirst':
        if(a.outcome !== b.outcome) return a.outcome === 'win' ? -1 : 1;
        return tb.localeCompare(ta);
      case 'lossFirst':
        if(a.outcome !== b.outcome) return a.outcome === 'loss' ? -1 : 1;
        return tb.localeCompare(ta);
      case 'dateDesc':
      default: return tb.localeCompare(ta);
    }
  });

  const meta = $('journalMeta');
  if(meta) meta.textContent = items.length + ' entr' + (items.length===1?'y':'ies');

  items.forEach(t => {
    const card = document.createElement('div');
    card.className = 'trade-card';
    const pillClass = t.outcome === 'win' ? 'pill-win' : 'pill-loss';
    const day = (t.timestamp||'').split('T')[0];
    const symStr = t.symbol ? `<span class="tag" title="Symbol" style="background:rgba(255,255,255,.06);border:1px solid var(--stroke-weak);padding:.18rem .5rem;border-radius:999px;font-size:.78rem">#${t.symbol}</span>` : '';
    const sideStr = t.side ? `<span class="tag" title="Side" style="background:rgba(255,255,255,.06);border:1px solid var(--stroke-weak);padding:.18rem .5rem;border-radius:999px;font-size:.78rem">${t.side.toUpperCase()}</span>` : '';
    const pnlStr = (typeof t.pnl === 'number' && !Number.isNaN(t.pnl)) ? `<span class="tag" title="P&L" style="background:rgba(255,255,255,.06);border:1px solid ${t.pnl>=0?'rgba(45,220,144,.35)':'rgba(255,107,107,.35)'};color:${t.pnl>=0?'#aef5d5':'#ffb3c1'};padding:.18rem .5rem;border-radius:999px;font-size:.78rem">$${t.pnl.toFixed(2)}</span>` : '';

    const thumb = t.screenshotData ? `<img src="${t.screenshotData}" alt="screenshot" style="width:100%;max-height:180px;object-fit:cover;margin-top:.6rem"/>` : '';

    card.innerHTML = `
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:.25rem">
        <div style="display:flex;gap:.35rem;flex-wrap:wrap">${symStr}${sideStr}${pnlStr}</div>
        <span class="pill ${pillClass}" style="padding:.18rem .5rem;border-radius:999px;border:1px solid var(--stroke-weak);background:rgba(255,255,255,.06);font-weight:1000">${t.outcome.toUpperCase()}</span>
      </div>
      <div style="display:flex;justify-content:space-between;gap:.6rem">
        <div style="color:var(--muted)">${day}</div>
      </div>
      <p style="color:var(--muted);margin-top:.6rem;white-space:pre-wrap">${t.notes || ''}</p>
      ${thumb}
      <div class="trade-actions">
        <button title="Edit" class="btn-secondary edit-trade" data-id="${t.id}">Edit</button>
        <button title="Delete" class="btn-secondary delete-trade" data-id="${t.id}">Delete</button>
      </div>`;
    container.appendChild(card);
  });

  document.querySelectorAll('.edit-trade').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const id = e.currentTarget.getAttribute('data-id');
      openEditTradeModal(id);
    });
  });
  document.querySelectorAll('.delete-trade').forEach(btn => {
    btn.addEventListener('click', (e) => {
      const id = e.currentTarget.getAttribute('data-id');
      openConfirmDelete('Are you sure you want to delete this journal entry?', () => {
        trades = trades.filter(tt => String(tt.id) !== String(id));
        autoSave();
        renderRecentTrades();
        renderDashboard();
        showNotification('Entry deleted');
      });
    });
  });
}

function openEditTradeModal(id){
  if(!currentUser){ showNotification('Please login to edit'); return; }
  const t = trades.find(x => String(x.id) === String(id));
  if(!t) return;
  editingTradeId = id;
  $('editTradeSymbol').value = t.symbol || '';
  $('editTradeSide').value = t.side || 'long';
  $('editTradeOutcome').value = t.outcome || 'win';
  $('editTradeEntry').value = (t.entry==null?'':t.entry);
  $('editTradeExit').value = (t.exit==null?'':t.exit);
  $('editTradeSize').value = (t.size==null?'':t.size);
  $('editTradeNotes').value = t.notes || '';

  const prevWrap = $('editScreenshotPreview');
  const prevImg = $('editScreenshotImg');
  if(t.screenshotData){
    if(prevImg) prevImg.src = t.screenshotData;
    if(prevWrap) prevWrap.style.display = 'block';
  } else {
    if(prevImg) prevImg.src = '';
    if(prevWrap) prevWrap.style.display = 'none';
  }
  const rem = $('removeEditScreenshotBtn');
  if(rem) rem.onclick = () => {
    const img = $('editScreenshotImg');
    if(img) img.src = '';
    if(prevWrap) prevWrap.style.display = 'none';
    const idx = trades.findIndex(x=>String(x.id)===String(editingTradeId));
    if(idx>=0){ trades[idx].screenshotData = null; }
  };

  openModal('editTradeModal');
}
function saveEditedTrade(){
  if(editingTradeId == null) return;
  const idx = trades.findIndex(t => String(t.id) === String(editingTradeId));
  if(idx < 0) return;
  const fileInput = $('editTradeScreenshot');
  const file = fileInput && fileInput.files && fileInput.files[0] ? fileInput.files[0] : null;
  const patch = {
    symbol: val('editTradeSymbol').trim().toUpperCase(),
    side: val('editTradeSide','long') || 'long',
    outcome: val('editTradeOutcome','win') || 'win',
    entry: parseFloat(val('editTradeEntry')),
    exit: parseFloat(val('editTradeExit')),
    size: parseFloat(val('editTradeSize')),
    notes: val('editTradeNotes').trim()
  };
  let pnl = trades[idx].pnl || 0;
  if(!Number.isNaN(patch.entry) && !Number.isNaN(patch.exit) && !Number.isNaN(patch.size)){
    pnl = (patch.exit - patch.entry) * (patch.side === 'short' ? -1 : 1) * patch.size;
    if(patch.outcome === 'loss' && pnl > 0) pnl = -Math.abs(pnl);
    if(patch.outcome === 'win' && pnl < 0) pnl = Math.abs(pnl);
  }

  const finalize = (imgData) => {
    if(typeof imgData !== 'undefined') patch.screenshotData = imgData;
    trades[idx] = Object.assign({}, trades[idx], patch, { pnl });
    autoSave();
    renderRecentTrades();
    renderDashboard();
    closeModal('editTradeModal');
    editingTradeId = null;
    showNotification('Journal entry updated');
  };

  if(file){
    const reader = new FileReader();
    reader.onload = (e)=> finalize(e.target.result);
    reader.onerror = ()=> finalize(undefined);
    reader.readAsDataURL(file);
  } else {
    finalize(undefined);
  }
}

function openConfirmDelete(text, callback) {
    $('deleteConfirmText').textContent = text;
    pendingDeleteCallback = callback;
    openModal('deleteConfirmModal');
}
function confirmDelete() {
    if (pendingDeleteCallback) {
        pendingDeleteCallback();
    }
    cancelDelete(); // Resets state
}
function cancelDelete() {
    pendingDeleteCallback = null;
    closeModal('deleteConfirmModal');
}

/* Period + Statistics */
// NEW: Function to compute daily P&L from the trades array
function computeDailyPnL() {
    const daily = {};
    trades.forEach(trade => {
        if (!trade.timestamp) return;
        const dateKey = trade.timestamp.split('T')[0];
        if (!daily[dateKey]) {
            daily[dateKey] = { trades: 0, pnl: 0, wins: 0, losses: 0 };
        }
        daily[dateKey].trades++;
        daily[dateKey].pnl += trade.pnl || 0;
        if ((trade.pnl || 0) > 0) daily[dateKey].wins++;
        if ((trade.pnl || 0) < 0) daily[dateKey].losses++;
    });
    return daily;
}

function getPeriodData(period){
  const now = new Date();
  let startDate, endDate;
  switch(period){
    case 'last14days': startDate = new Date(now); startDate.setDate(now.getDate() - 13); endDate = new Date(now); break;
    case 'currentMonth': startDate = new Date(now.getFullYear(), now.getMonth(), 1); endDate = new Date(now.getFullYear(), now.getMonth() + 1, 0); break;
    case 'lastMonth': startDate = new Date(now.getFullYear(), now.getMonth() - 1, 1); endDate = new Date(now.getFullYear(), now.getMonth(), 0); break;
    case 'allTime': 
        startDate = trades.length > 0 ? new Date(trades.map(t => t.timestamp).sort()[0]) : new Date(now);
        endDate = new Date(now);
        break;
    default: startDate = new Date(now.getFullYear(), now.getMonth(), 1); endDate = new Date(now.getFullYear(), now.getMonth() + 1, 0);
  }
  startDate.setHours(0,0,0,0);
  endDate.setHours(23,59,59,999);
  
  const dailyPnL = computeDailyPnL();
  const periodDaily = {};
  
  const cur = new Date(startDate);
  while(cur <= endDate){
    const key = cur.toISOString().split('T')[0];
    periodDaily[key] = dailyPnL[key] || { trades: 0, pnl: 0, wins: 0, losses: 0 };
    cur.setDate(cur.getDate() + 1);
  }

  const periodTrades = trades.filter(t => {
      const tradeDate = new Date(t.timestamp);
      return tradeDate >= startDate && tradeDate <= endDate;
  });

  return { dailyPnL: periodDaily, trades: periodTrades, startDate, endDate };
}

function updateStatistics(){
  const pd = getPeriodData(currentPeriod);
  const periodTrades = pd.trades;

  let totalTrades = periodTrades.length;
  let totalPnL = periodTrades.reduce((sum, t) => sum + (t.pnl || 0), 0);
  let wins = periodTrades.filter(t => t.outcome === 'win').length;
  let winRate = totalTrades > 0 ? (wins / totalTrades * 100) : 0;
  
  let winningDays = 0;
  Object.values(pd.dailyPnL).forEach(day => {
      if (day.pnl > 0) winningDays++;
  });

  $('totalTrades').textContent = totalTrades;
  $('totalPnL').textContent = '$' + totalPnL.toFixed(2);
  $('totalPnL').className = `stat-value ${totalPnL >= 0 ? 'profit' : 'loss'}`;
  $('winRate').textContent = winRate.toFixed(1) + '%';
  $('winningDays').textContent = winningDays;

  // Stats Tab
  let grossProfit = periodTrades.filter(t => t.pnl > 0).reduce((sum, t) => sum + t.pnl, 0);
  let grossLoss = Math.abs(periodTrades.filter(t => t.pnl < 0).reduce((sum, t) => sum + t.pnl, 0));
  let profitFactor = grossLoss > 0 ? (grossProfit / grossLoss) : (grossProfit > 0 ? Infinity : 0);
  
  let winningTrades = periodTrades.filter(t => t.pnl > 0);
  let losingTrades = periodTrades.filter(t => t.pnl < 0);
  let avgWin = winningTrades.length > 0 ? grossProfit / winningTrades.length : 0;
  let avgLoss = losingTrades.length > 0 ? grossLoss / losingTrades.length : 0;

  let bestTrade = periodTrades.reduce((max, t) => t.pnl > max ? t.pnl : max, -Infinity);
  let worstTrade = periodTrades.reduce((min, t) => t.pnl < min ? t.pnl : min, Infinity);

  $('statTotalPnL').textContent = '$' + totalPnL.toFixed(2);
  $('statTotalPnL').className = `stat-value ${totalPnL >= 0 ? 'profit' : 'loss'}`;
  $('statProfitFactor').textContent = profitFactor === Infinity ? '∞' : profitFactor.toFixed(2);
  $('statAvgWin').textContent = '$' + avgWin.toFixed(2);
  $('statAvgLoss').textContent = '$' + avgLoss.toFixed(2);
  $('statBestTrade').textContent = '$' + (isFinite(bestTrade) ? bestTrade.toFixed(2) : '0.00');
  $('statWorstTrade').textContent = '$' + (isFinite(worstTrade) ? worstTrade.toFixed(2) : '0.00');

  renderStatsCharts(pd);
}

/* Charts (Statistics) */
function renderStatsCharts(pd){
  if(typeof Chart === 'undefined') return;

  const labels = Object.keys(pd.dailyPnL).map(d => d.slice(5));
  const pnlData = Object.values(pd.dailyPnL).map(d => d.pnl);

  let cum = 0;
  const equity = pnlData.map(p => { cum += p; return cum; });
  const tradesData = Object.values(pd.dailyPnL).map(d => d.trades);

  const winDays = Object.values(pd.dailyPnL).filter(d => d.pnl > 0).length;
  const lossDays = Object.values(pd.dailyPnL).filter(d => d.pnl < 0).length;

  const css = getComputedStyle(document.body);
  const accent = (userColors && userColors.accent) ? userColors.accent : (css.getPropertyValue('--accent').trim() || '#ff6b6b');
  const accentSoft = css.getPropertyValue('--accent-soft').trim() || '#ffbaba';
  const success = (userColors && userColors.success) ? userColors.success : (css.getPropertyValue('--success').trim() || '#2ddc90');
  const danger = (userColors && userColors.danger) ? userColors.danger : (css.getPropertyValue('--danger').trim() || '#ff6b6b');
  const muted = css.getPropertyValue('--muted').trim() || '#9fb0c8';
  const text = css.getPropertyValue('--text').trim() || '#e8eef7';
  const gridColor = 'rgba(150,150,150,0.15)';

  const eqCanvas = $('equityCanvas');
  if(eqCanvas){
    if (equityChart) equityChart.destroy();
    const eqCtx = eqCanvas.getContext('2d');
    const eqConfig = {
      type: 'bar',
      data: {
        labels,
        datasets: [
          {
            type: 'line',
            label: 'Equity',
            data: equity,
            borderColor: accent,
            backgroundColor: accentSoft + '22',
            tension: .35,
            pointRadius: 0,
            yAxisID: 'y'
          },
          {
            type: 'bar',
            label: 'Daily P&L',
            data: pnlData,
            backgroundColor: pnlData.map(v => v>=0 ? success+'99' : danger+'99'),
            borderColor: pnlData.map(v => v>=0 ? success : danger),
            borderWidth: 1,
            yAxisID: 'y1'
          }
        ]
      },
      options: {
        plugins: { legend: { display:false }, tooltip: { enabled:true } },
        scales: {
          x: { grid:{ color:gridColor }, ticks:{ color: muted, maxTicksLimit: 10 } },
          y: { position:'left', grid:{ color:gridColor }, ticks:{ color: muted } },
          y1: { position:'right', grid:{ display:false }, ticks:{ color: muted } }
        }
      }
    };
    equityChart = new Chart(eqCtx, eqConfig);
  }

  const tCanvas = $('tradesBarCanvas');
  if(tCanvas){
    if (tradesBarChart) tradesBarChart.destroy();
    const tCtx = tCanvas.getContext('2d');
    const tConfig = {
      type: 'bar',
      data: {
        labels,
        datasets: [{
          label: 'Trades',
          data: tradesData,
          backgroundColor: accent+'88',
          borderColor: accent,
          borderWidth: 1
        }]
      },
      options: {
        plugins: { legend: { display:false } },
        scales: {
          x: { grid:{ display:false }, ticks:{ color:muted, maxTicksLimit: 10 } },
          y: { grid:{ color:gridColor }, ticks:{ color:muted, precision:0 } }
        }
      }
    };
    tradesBarChart = new Chart(tCtx, tConfig);
  }

  const wlCanvas = $('winLossDonutCanvas');
  if(wlCanvas){
    if (winLossDonutChart) winLossDonutChart.destroy();
    const wlCtx = wlCanvas.getContext('2d');
    const wlConfig = {
      type: 'doughnut',
      data: { labels: ['Winning Days','Losing Days'], datasets: [{ data: [winDays, lossDays], backgroundColor: [success, danger], borderWidth: 0 }] },
      options: { plugins: { legend: { position:'bottom', labels:{ color:text } } }, cutout: '62%' }
    };
    winLossDonutChart = new Chart(wlCtx, wlConfig);
  }
}

/* Calendar */
function mondayIndex(dateObj){
  const jsDay = dateObj.getDay(); // 0=Sun..6=Sat
  return (jsDay + 6) % 7;
}
function renderDashboard(){
  const pd = getPeriodData(currentPeriod);
  const dailyPnL = computeDailyPnL(); // Use computed PnL
  const grid = $('tradesGrid');
  if(!grid) return;
  grid.innerHTML = '';

  const leading = mondayIndex(pd.startDate);
  for(let i=0;i<leading;i++){
    const ph = document.createElement('div');
    ph.className = 'trade-day placeholder';
    ph.innerHTML = '&nbsp;';
    grid.appendChild(ph);
  }

  const cur = new Date(pd.startDate);
  const end = new Date(pd.endDate);
  while(cur <= end){
    const key = cur.toISOString().split('T')[0];
    const d = dailyPnL[key] || { trades: 0, pnl: 0 };
    const el = document.createElement('div');
    
    const dayOfWeek = cur.getDay(); // 0=Sun, 6=Sat
    let cls = '';
    if(d.pnl > 0) cls = 'profit';
    else if(d.pnl < 0) cls = 'loss';
    
    if (dayOfWeek === 6 || dayOfWeek === 0) { // Saturday or Sunday
        el.className = 'trade-day weekend';
    } else {
        el.className = 'trade-day ' + cls;
        // The day modal is for manual override, which we've removed in favor of auto-calculation
        // el.addEventListener('click', () => openDayModal(key));
    }

    el.innerHTML = `
      <div class="day-number">${cur.getDate()}</div>
      <div class="pnl">$${(d.pnl||0).toFixed(2)}</div>
      <div style="font-size:.85rem;color:var(--muted)">${d.trades||0} trades</div>`;
    grid.appendChild(el);
    cur.setDate(cur.getDate() + 1);
  }

  const totalDays = Math.floor((end - pd.startDate)/(1000*60*60*24)) + 1 + leading;
  const trailing = (7 - (totalDays % 7)) % 7;
  for(let i=0;i<trailing;i++){
    const ph = document.createElement('div');
    ph.className = 'trade-day placeholder';
    ph.innerHTML = '&nbsp;';
    grid.appendChild(ph);
  }

  updateStatistics();
  const titles = { last14days: 'Last 14 Days', currentMonth: 'Current Month', lastMonth: 'Last Month', allTime: 'All Time' };
  const tEl = $('periodTitle'); if(tEl) tEl.textContent = titles[currentPeriod] || 'Current Month';
}

/* Day modal (Kept for manual overrides if ever needed, but click listener is removed for now) */
function openDayModal(dateKey){
  if(!currentUser){ showNotification('Please login to edit days'); return; }
  selectedDateKey = dateKey;
  dayImageRemove = false;
  const d = new Date(dateKey + 'T12:00:00');
  $('modalDay').textContent = d.toDateString();
  const computedPnL = computeDailyPnL();
  const dd = computedPnL[dateKey] || { trades: 0, pnl: 0 };
  const result = dd.pnl < 0 ? 'Loss' : (dd.pnl > 0 ? 'Win' : 'Neutral');
  const summary = $('daySummary');
  if(summary) summary.textContent = 'Trades: ' + (dd.trades || 0) + ' • Result: ' + result + ' • Amount: $' + Math.abs(dd.pnl || 0).toFixed(2);

  $('dayTrades').value = dd.trades || 0;
  $('dayResult').value = dd.pnl < 0 ? 'loss' : 'win';
  $('dayPnLAmount').value = Math.abs(dd.pnl || 0);

  // Image handling remains the same as it's a manual attachment
  openModal('dayModal');
}
function saveDayPnL(){
    // This function is now for manual overrides only and is not called from the dashboard grid.
    // Its logic would need to be re-evaluated if manual overrides are re-introduced.
    showNotification("Manual day editing is disabled in favor of automatic calculation from journal entries.");
    closeModal('dayModal');
}

/* AI */
function addAiChatMessage(sender, text) {
    const chatWindow = $('aiChatWindow');
    const bubble = document.createElement('div');
    bubble.className = `ai-chat-bubble ${sender}`;
    bubble.innerHTML = text; // Use innerHTML to allow for lists etc.
    chatWindow.appendChild(bubble);
    chatWindow.scrollTop = chatWindow.scrollHeight;
    return bubble;
}
async function askAI(prompt) {
  if(!currentUser){ showNotification('Please login to use AI Mentor'); return; }
  const q = prompt || val('aiQuestion').trim();
  if (!q) return;

  addAiChatMessage('user', q);
  if (!prompt) $('aiQuestion').value = '';

  const thinkingBubble = addAiChatMessage('ai', '');
  thinkingBubble.innerHTML = `<div class="ai-thinking-indicator"><span></span><span></span><span></span></div>`;

  try {
    const { pipeline } = await import('https://cdn.jsdelivr.net/npm/@xenova/transformers@2.17.1');

    if (!ai_pipeline) {
      thinkingBubble.innerHTML = 'Initializing AI model... (This may take a moment on first use)';
      ai_pipeline = await pipeline('text-generation', 'Xenova/distilgpt2');
      thinkingBubble.innerHTML = `<div class="ai-thinking-indicator"><span></span><span></span><span></span></div>`;
    }

    const userTrades = trades.filter(t => t.type === 'journal');
    let context = `You are IntradelAI, an expert trading psychologist and performance coach. You are direct, insightful, and provide actionable advice. Use markdown for formatting. `;

    if (userTrades.length < 5) {
      context += `The user has very few trades logged (${userTrades.length}). You cannot perform a deep analysis. Your primary goal is to encourage them to log at least 5-10 more trades to get meaningful insights. Briefly answer their question but pivot to the importance of building a larger dataset.`;
    } else {
      const wins = userTrades.filter(t => t.outcome === 'win');
      const losses = userTrades.filter(t => t.outcome === 'loss');
      const winRate = userTrades.length > 0 ? (wins.length / userTrades.length) * 100 : 0;
      const grossProfit = wins.reduce((sum, t) => sum + (t.pnl || 0), 0);
      const grossLoss = losses.reduce((sum, t) => sum + Math.abs(t.pnl || 0), 0);
      const profitFactor = grossLoss > 0 ? (grossProfit / grossLoss) : (grossProfit > 0 ? Infinity : 0);
      const avgWin = wins.length > 0 ? grossProfit / wins.length : 0;
      const avgLoss = losses.length > 0 ? grossLoss / losses.length : 0;
      const notesString = userTrades.map(t => (t.notes || '').toLowerCase()).join(' ');
      const fomoCount = (notesString.match(/fomo|hesitated|chased/g) || []).length;
      const revengeCount = (notesString.match(/revenge|angry|frustrated/g) || []).length;
      const disciplineCount = (notesString.match(/discipline|stuck to plan|followed rules/g) || []).length;

      context += `
        Here is a summary of the user's trading performance. Use this data to inform your answer.
        - Total Trades: ${userTrades.length}
        - Win Rate: ${winRate.toFixed(1)}%
        - Profit Factor: ${profitFactor === Infinity ? 'Excellent (no losses)' : profitFactor.toFixed(2)}
        - Average Win: $${avgWin.toFixed(2)}
        - Average Loss: $${avgLoss.toFixed(2)}
        - Psychological keywords found in notes: ${fomoCount} mentions of FOMO/chasing, ${revengeCount} mentions of revenge/anger, ${disciplineCount} mentions of discipline.
      `;
    }

    const finalPrompt = `
      ${context}
      
      Based on the context, provide an expert analysis and answer for the user's question.
      User's Question: "${q}"
      
      Your analysis:
    `;

    const result = await ai_pipeline(finalPrompt, {
      max_new_tokens: 250,
      temperature: 0.7,
      repetition_penalty: 1.2,
      no_repeat_ngram_size: 3,
      num_beams: 2,
    });

    // Clean up the generated text
    let generatedText = result[0].generated_text.replace(finalPrompt, '').trim();
    // Remove incomplete sentences at the end
    const lastPeriod = generatedText.lastIndexOf('.');
    if (lastPeriod > 0 && lastPeriod < generatedText.length - 1) {
        generatedText = generatedText.substring(0, lastPeriod + 1);
    }

    thinkingBubble.innerHTML = generatedText.replace(/\n/g, '<br>');

  } catch (error) {
    console.error('AI Error:', error);
    thinkingBubble.textContent = 'Error: Could not run AI analysis. The model may have failed to load. Please try refreshing the page.';
  } finally {
      $('aiChatWindow').scrollTop = $('aiChatWindow').scrollHeight;
  }
}

/* Forum & Social */
function startForumPolling() {
    if (forumPollingInterval) clearInterval(forumPollingInterval);
    let lastForumState = localStorage.getItem('intradelForumMessages');
    let lastDmState = localStorage.getItem('intradelDms');
    let lastUsersState = localStorage.getItem('intradelUsers');
    let lastAnnouncementsState = localStorage.getItem('intradelAnnouncements');
    let lastMutedState = localStorage.getItem('intradelMutedUsers');
    
    forumPollingInterval = setInterval(() => {
        const currentForumState = localStorage.getItem('intradelForumMessages');
        const currentDmState = localStorage.getItem('intradelDms');
        const currentUsersState = localStorage.getItem('intradelUsers');
        const currentAnnouncementsState = localStorage.getItem('intradelAnnouncements');
        const currentMutedState = localStorage.getItem('intradelMutedUsers');

        if (currentForumState !== lastForumState || currentAnnouncementsState !== lastAnnouncementsState || currentMutedState !== lastMutedState) {
            lastForumState = currentForumState;
            lastAnnouncementsState = currentAnnouncementsState;
            lastMutedState = currentMutedState;
            if (document.getElementById('forumTab').style.display === 'block') {
                renderForum();
            }
        }
        if (currentDmState !== lastDmState) {
            lastDmState = currentDmState;
            if (document.getElementById('dmTab').style.display === 'block') {
                renderDmView(currentOpenDm, false);
            }
        }
        if (currentUsersState !== lastUsersState) {
            lastUsersState = currentUsersState;
            const myData = loadUserData(currentUser.email);
            if (JSON.stringify(myData.profile.friendRequests) !== JSON.stringify(userProfile.friendRequests)) {
                userProfile.friendRequests = myData.profile.friendRequests;
                updateUI();
                if (document.getElementById('friendsTab').style.display === 'block') {
                    renderFriendsList();
                }
            }
        }
    }, 2500);
}
function loadForumMessages() {
    return JSON.parse(localStorage.getItem('intradelForumMessages') || '[]');
}
function saveForumMessages(messages) {
    localStorage.setItem('intradelForumMessages', JSON.stringify(messages));
}

function renderForum(messagesToRender) {
    const chatWindow = $('forumChatWindow');
    const announcementContainer = $('announcementContainer');
    if (!chatWindow || !announcementContainer) return;

    const announcements = getAnnouncements();
    const now = new Date().getTime();
    const activeAnnouncement = announcements.find(a => new Date(a.pinnedUntil).getTime() > now);

    if (activeAnnouncement) {
        const author = getUserByEmail(activeAnnouncement.authorEmail);
        announcementContainer.style.display = 'block';
        announcementContainer.innerHTML = `
            <div class="pinned-announcement">
                <div class="announcement-header">
                    <span>📌 ANNOUNCEMENT by <span class="author">${author.name}</span></span>
                    <span class="muted">Pinned until ${new Date(activeAnnouncement.pinnedUntil).toLocaleTimeString()}</span>
                </div>
                <div class="announcement-body">${activeAnnouncement.text}</div>
            </div>
        `;
    } else {
        announcementContainer.style.display = 'none';
    }
    
    const currentMessages = messagesToRender || loadForumMessages();
    const mutedUsers = getMutedUsers();
    chatWindow.innerHTML = ''; 
    
    const visibleMessages = currentMessages.filter(msg => {
        const muteInfo = mutedUsers[msg.email];
        if (!muteInfo) return true;
        if (muteInfo === 'permanent') return false;
        return now > new Date(muteInfo).getTime();
    });

    if (visibleMessages.length === 0) {
        chatWindow.innerHTML = `<div style="text-align:center; color: var(--muted); margin: auto;">Welcome to the forum! Be the first to say something.</div>`;
        return;
    }

    visibleMessages.forEach(msg => {
        const author = getUserByEmail(msg.email) || { name: 'Unknown User' };
        const authorData = (getAllUserData()[msg.email] || {});
        const avatarSrc = (authorData.profile || {}).avatarData || 'data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=';

        const isOwnMessage = currentUser && msg.email === currentUser.email;
        const isAdmin = currentUser && currentUser.email === ADMIN_EMAIL;

        const msgGroup = document.createElement('div');
        msgGroup.className = `message-group ${isOwnMessage ? 'own' : ''}`;
        msgGroup.dataset.messageId = msg.id;

        const imageContent = msg.imageData ? `<img src="${msg.imageData}" alt="User upload">` : '';
        const textContent = msg.text.replace(/</g, "&lt;").replace(/>/g, "&gt;");

        let modTools = '';
        if (isAdmin && !isOwnMessage) {
            const muteInfo = mutedUsers[msg.email];
            const isMuted = muteInfo && (muteInfo === 'permanent' || new Date(muteInfo).getTime() > now);
            modTools = `
                <div class="admin-mod-tools">
                    <span class="mod-icon">🛡️</span>
                    <div class="mod-menu">
                        <a href="#" onclick="openUserProfileModal('${msg.email}')">View Profile</a>
                        <a href="#" onclick="deleteForumMessage('${msg.id}')" class="danger">Delete Message</a>
                        <a href="#" onclick="toggleMuteUser('${msg.email}')">${isMuted ? 'Unmute' : 'Mute'} User</a>
                        <a href="#" onclick="toggleBanStatus('${msg.email}')" class="danger">Ban User</a>
                    </div>
                </div>
            `;
        }
        
        msgGroup.innerHTML = `
            <img src="${avatarSrc}" class="chat-avatar" onclick="openUserProfileModal('${msg.email}')" style="${!(authorData.profile || {}).avatarData ? 'background: var(--bg-2);' : ''}">
            <div class="message-content">
                <div class="message-header">
                    <span class="username" onclick="openUserProfileModal('${msg.email}')">${author.name}</span>
                    <span class="timestamp">${new Date(msg.timestamp).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</span>
                    ${modTools}
                </div>
                <div class="message-bubble">
                    ${textContent}
                    ${imageContent}
                </div>
            </div>
        `;
        chatWindow.appendChild(msgGroup);
    });

    chatWindow.scrollTop = chatWindow.scrollHeight;
}

function postAnnouncement() {
    const text = val('announcementInput').trim();
    if (!text) return;
    const durationSeconds = parseInt(val('announcementDuration'));
    const pinnedUntil = new Date(new Date().getTime() + durationSeconds * 1000).toISOString();

    const announcement = {
        id: Date.now(),
        text,
        authorEmail: currentUser.email,
        timestamp: new Date().toISOString(),
        pinnedUntil,
    };
    
    saveAnnouncements([announcement]);
    $('announcementInput').value = '';
    renderForum();
    showNotification('Announcement posted and pinned!');
}

function sendForumMessage() {
    if (!currentUser) { showNotification('Please log in to post.'); return; }
    const input = $('forumMessageInput');
    const text = input.value.trim();
    if (!text) return;

    let currentMessages = loadForumMessages();
    const message = { id: Date.now(), email: currentUser.email, text: text, timestamp: new Date().toISOString(), imageData: null };
    currentMessages.push(message);
    saveForumMessages(currentMessages);
    renderForum(currentMessages);
    input.value = '';
}
function sendForumImage(imageData) {
    if (!currentUser) return;
    let currentMessages = loadForumMessages();
    const message = { id: Date.now(), email: currentUser.email, text: '', timestamp: new Date().toISOString(), imageData: imageData };
    currentMessages.push(message);
    saveForumMessages(currentMessages);
    renderForum(currentMessages);
}
function deleteForumMessage(messageId) {
    openConfirmDelete('Are you sure you want to delete this message?', () => {
        let currentMessages = loadForumMessages();
        currentMessages = currentMessages.filter(msg => String(msg.id) !== String(messageId));
        saveForumMessages(currentMessages);
        renderForum(currentMessages);
        addActivityLog(`Admin deleted a forum message.`);
        showNotification('Message deleted.');
    });
}
function toggleMuteUser(email) {
    let muted = getMutedUsers();
    const user = getUserByEmail(email);
    const name = user ? user.name : 'Unknown';
    const now = new Date().getTime();
    const muteInfo = muted[email];
    const isMuted = muteInfo && (muteInfo === 'permanent' || new Date(muteInfo).getTime() > now);

    if (isMuted) {
        delete muted[email];
        addActivityLog(`Admin unmuted ${name}.`);
        showNotification(`${name} has been unmuted.`);
        saveMutedUsers(muted);
        renderForum();
    } else {
        userToMute = email;
        $('muteUserTitle').textContent = `Mute ${name}`;
        openModal('muteUserModal');
    }
}
function confirmMute() {
    if (!userToMute) return;

    let muted = getMutedUsers();
    const durationSeconds = parseInt(val('muteDurationSelect'));
    const user = getUserByEmail(userToMute);
    const name = user ? user.name : 'Unknown';
    let muteUntil;
    let durationText;

    if (durationSeconds === -1) {
        muteUntil = 'permanent';
        durationText = 'permanently';
    } else {
        muteUntil = new Date(new Date().getTime() + durationSeconds * 1000).toISOString();
        durationText = `for ${val('muteDurationSelect', 'selectedOptions')[0].text}`;
    }

    muted[userToMute] = muteUntil;
    saveMutedUsers(muted);
    addActivityLog(`Admin muted ${name} ${durationText}.`);
    showNotification(`${name} has been muted ${durationText}.`);
    
    closeModal('muteUserModal');
    userToMute = null;
    renderForum();
}

function openUserProfileModal(email) {
    const user = getUserByEmail(email);
    if (!user) return;
    
    const allUserData = getAllUserData();
    const userData = allUserData[email] || {};
    const userProf = Object.assign({avatarData: null, isPublic: false}, userData.profile || {});

    const modalContent = $('userProfileModalContent');
    const avatarSrc = userProf.avatarData || 'data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=';

    let roleBadge = '';
    const sub = user.subscription || 'Starter';
    if (user.email === ADMIN_EMAIL) {
        roleBadge = `<span class="profile-role-badge admin">Admin</span>`;
    } else {
        roleBadge = `<span class="profile-role-badge ${sub.toLowerCase()}">${sub} Member</span>`;
    }

    let profileStatsHtml = `<p class="muted" style="text-align:center;">This user's profile is private.</p>`;
    if (userProf.isPublic || currentUser.email === email || currentUser.email === ADMIN_EMAIL) {
        profileStatsHtml = `
            <div class="profile-details">
                <div class="profile-detail-card">
                    <div class="label">Member Since</div>
                    <div class="value">${new Date(user.createdAt).toLocaleDateString()}</div>
                </div>
            </div>
        `;
    }

    let friendButtonHtml = '';
    if (currentUser && currentUser.email !== email) {
        const myFriends = userProfile.friends || [];
        const theirRequests = userProf.friendRequests || [];
        if (myFriends.includes(email)) {
            friendButtonHtml = `<button class="btn-secondary" onclick="removeFriend('${email}')">Remove Friend</button>`;
        } else if (theirRequests.includes(currentUser.email)) {
            friendButtonHtml = `<button class="btn-secondary" disabled>Request Sent</button>`;
        } else {
            friendButtonHtml = `<button class="btn-primary" onclick="sendFriendRequest('${email}')">Add Friend</button>`;
        }
    }

    modalContent.innerHTML = `
        <div class="profile-banner"></div>
        <div class="profile-header">
            <img src="${avatarSrc}" class="profile-avatar" style="${!userProf.avatarData ? 'background: var(--bg-2);' : ''}">
            <h2 class="profile-name">${user.name}</h2>
            ${roleBadge}
        </div>
        ${profileStatsHtml}
        <div class="profile-actions">
            ${friendButtonHtml}
        </div>
    `;
    openModal('userProfileModal');
}
function sendFriendRequest(targetEmail) {
    const allUserData = getAllUserData();
    const targetUserData = allUserData[targetEmail];
    if (!targetUserData || !targetUserData.profile) return;
    
    targetUserData.profile.friendRequests = targetUserData.profile.friendRequests || [];
    if (!targetUserData.profile.friendRequests.includes(currentUser.email)) {
        targetUserData.profile.friendRequests.push(currentUser.email);
        saveUserData(targetEmail, targetUserData);
        showNotification('Friend request sent!');
        openUserProfileModal(targetEmail); // Re-render to update button
    }
}
function handleFriendRequest(requesterEmail, accept) {
    userProfile.friendRequests = (userProfile.friendRequests || []).filter(email => email !== requesterEmail);

    if (accept) {
        userProfile.friends = userProfile.friends || [];
        if (!userProfile.friends.includes(requesterEmail)) {
            userProfile.friends.push(requesterEmail);
        }
        
        const allUserData = getAllUserData();
        const requesterData = allUserData[requesterEmail];
        if (requesterData && requesterData.profile) {
            requesterData.profile.friends = requesterData.profile.friends || [];
            if (!requesterData.profile.friends.includes(currentUser.email)) {
                requesterData.profile.friends.push(currentUser.email);
                saveUserData(requesterEmail, requesterData);
            }
        }
        showNotification('Friend request accepted!');
    } else {
        showNotification('Friend request declined.');
    }
    
    saveUserData(currentUser.email, buildUserData());
    
    renderFriendsList();
    updateUI();
}
function removeFriend(friendEmail) {
    userProfile.friends = (userProfile.friends || []).filter(f => f !== friendEmail);
    
    const allUserData = getAllUserData();
    const friendData = allUserData[friendEmail];
    if (friendData && friendData.profile) {
        friendData.profile.friends = (friendData.profile.friends || []).filter(f => f !== currentUser.email);
        saveUserData(friendEmail, friendData);
    }

    saveUserData(currentUser.email, buildUserData());

    showNotification('Friend removed.');
    renderFriendsList();
}

function renderFriendsList(subtab = 'friendsList') {
    const friendsContainer = $('friendsListContainer');
    const requestsContainer = $('friendsRequestsContainer');
    const friendsSubtab = document.querySelector('[data-subtab="friendsList"]');
    const requestsSubtab = document.querySelector('[data-subtab="friendsRequests"]');

    if (subtab === 'friendsRequests') {
        friendsContainer.style.display = 'none';
        requestsContainer.style.display = 'block';
        friendsSubtab.classList.remove('active');
        requestsSubtab.classList.add('active');
        renderFriendRequests();
    } else {
        friendsContainer.style.display = 'block';
        requestsContainer.style.display = 'none';
        friendsSubtab.classList.add('active');
        requestsSubtab.classList.remove('active');
        renderMyFriends();
    }
}
function renderMyFriends() {
    const container = $('friendsList');
    if (!container || !userProfile) return;

    const friends = userProfile.friends || [];
    if (friends.length === 0) {
        container.innerHTML = `<p class="muted" style="text-align:center; margin-top: 2rem;">Your friends list is empty.</p>`;
        return;
    }
    container.innerHTML = '';
    friends.forEach(email => {
        const user = getUserByEmail(email);
        if (!user) return;
        const pfpData = (getAllUserData()[email]?.profile || {}).avatarData;
        const avatarSrc = pfpData || 'data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=';

        const card = document.createElement('div');
        card.className = 'friend-card';
        card.innerHTML = `
            <img src="${avatarSrc}" class="avatar" style="${!pfpData ? 'background: var(--bg-2);' : ''}">
            <div class="info">
                <div class="name">${user.name}</div>
            </div>
            <div class="actions">
                <button class="btn-primary" onclick="openDm('${email}')">Message</button>
                <button class="btn-secondary" onclick="openUserProfileModal('${email}')">Profile</button>
            </div>
        `;
        container.appendChild(card);
    });
}
function renderFriendRequests() {
    const container = $('requestsList');
    const requests = userProfile.friendRequests || [];

    if (requests.length === 0) {
        container.innerHTML = `<p class="muted" style="text-align:center; margin-top: 2rem;">No pending friend requests.</p>`;
        return;
    }
    container.innerHTML = '';
    requests.forEach(email => {
        const user = getUserByEmail(email);
        if (!user) return;
        const pfpData = (getAllUserData()[email]?.profile || {}).avatarData;
        const avatarSrc = pfpData || 'data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=';

        const card = document.createElement('div');
        card.className = 'request-card';
        card.innerHTML = `
            <img src="${avatarSrc}" class="avatar" style="${!pfpData ? 'background: var(--bg-2);' : ''}">
            <div class="info">
                <div class="name">${user.name}</div>
            </div>
            <div class="actions">
                <button class="btn-primary" onclick="handleFriendRequest('${email}', true)">Accept</button>
                <button class="btn-secondary" onclick="handleFriendRequest('${email}', false)">Decline</button>
            </div>
        `;
        container.appendChild(card);
    });
}

/* Direct Messages (DMs) */
function getDmKey(email1, email2) {
    return [email1, email2].sort().join(':');
}
function openDm(userEmail) {
    switchToTab('dm');
    renderDmView(userEmail);
}
function renderDmView(activeUserEmail, switchUser = true) {
    if (switchUser) currentOpenDm = activeUserEmail;
    
    const userListEl = $('dmUserList');
    const chatWindowEl = $('dmChatWindow');
    userListEl.innerHTML = '';
    chatWindowEl.innerHTML = '';

    const friends = userProfile.friends || [];
    if (friends.length === 0) {
        userListEl.innerHTML = `<p class="muted" style="padding: 1rem; text-align: center;">Add friends to start a conversation.</p>`;
        chatWindowEl.innerHTML = `<div class="dm-placeholder"><h3>Direct Messages</h3><p>Select a friend to start a conversation.</p></div>`;
        return;
    }

    friends.forEach(email => {
        const user = getUserByEmail(email);
        if (!user) return;
        const pfpData = (getAllUserData()[email]?.profile || {}).avatarData;
        const avatarSrc = pfpData || 'data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=';
        const item = document.createElement('div');
        item.className = 'dm-user-item';
        if (email === currentOpenDm) item.classList.add('active');
        item.onclick = () => renderDmView(email);
        item.innerHTML = `<img src="${avatarSrc}" style="${!pfpData ? 'background: var(--bg-2);' : ''}"><span>${user.name}</span>`;
        userListEl.appendChild(item);
    });

    if (currentOpenDm) {
        const allDms = getAllDms();
        const dmKey = getDmKey(currentUser.email, currentOpenDm);
        const messages = allDms[dmKey] || [];

        messages.forEach(msg => {
            const isOwn = msg.sender === currentUser.email;
            const bubble = document.createElement('div');
            bubble.className = `message-group ${isOwn ? 'own' : ''}`;
            // For DMs, we don't need the avatar inside the chat window
            bubble.innerHTML = `
                <div class="message-content">
                    <div class="message-bubble">${msg.text.replace(/</g, "&lt;").replace(/>/g, "&gt;")}</div>
                </div>
            `;
            chatWindowEl.appendChild(bubble);
        });
        chatWindowEl.scrollTop = chatWindowEl.scrollHeight;
    } else {
        chatWindowEl.innerHTML = `<div class="dm-placeholder"><h3>Direct Messages</h3><p>Select a friend to start a conversation.</p></div>`;
    }
    
    $('dmInputForm').style.display = currentOpenDm ? 'flex' : 'none';
}
function sendDm() {
    if (!currentOpenDm) return;
    const input = $('dmMessageInput');
    const text = input.value.trim();
    if (!text) return;

    const allDms = getAllDms();
    const dmKey = getDmKey(currentUser.email, currentOpenDm);
    if (!allDms[dmKey]) allDms[dmKey] = [];
    
    allDms[dmKey].push({ sender: currentUser.email, text, timestamp: new Date().toISOString() });
    saveAllDms(allDms);
    input.value = '';
    renderDmView(currentOpenDm, false);
}


/* Settings */
function saveAccountSettings(){
  if(!currentUser){ showNotification('Please login to change account settings'); return; }
  const newEmail = val('newEmail').trim().toLowerCase();
  const newPassword = val('newPassword');
  if(!newEmail && !newPassword){ showNotification('No changes to save'); return; }
  const users = getAllUsers();
  const me = users[currentUser.email];
  if(!me){ showNotification('Current account not found'); return; }
  if(newEmail && newEmail !== currentUser.email && users[newEmail]){ showNotification('Email already in use'); return; }
  if(newPassword) me.password = newPassword;
  if(newEmail && newEmail !== currentUser.email){
    me.email = newEmail;
    const allData = getAllUserData();
    allData[newEmail] = allData[currentUser.email] || buildUserData();
    delete allData[currentUser.email];
    localStorage.setItem('intradelUserData', JSON.stringify(allData));
    const allUsers = getAllUsers();
    allUsers[newEmail] = Object.assign({}, me);
    delete allUsers[currentUser.email];
    localStorage.setItem('intradelUsers', JSON.stringify(allUsers));
    currentUser.email = newEmail;
  } else {
    updateUserAccount(currentUser.email, me);
  }
  updateUserAccount(currentUser.email, me);
  saveUserData(currentUser.email, buildUserData());
  if($('newEmail')) $('newEmail').value = '';
  if($('newPassword')) $('newPassword').value = '';
  const msg = $('settingsMsg');
  if(msg){ msg.textContent = 'Account updated'; msg.style.display = 'block'; setTimeout(()=>msg.style.display='none',3000); }
  renderProfileSettings();
}

function saveUsername() {
    if (!currentUser) return;
    const newUsername = val('newUsername').trim();
    if (!newUsername) { showNotification('Username cannot be empty.'); return; }
    if (newUsername.length < 3 || newUsername.length > 20) { showNotification('Username must be 3-20 characters.'); return; }

    const user = getUserByEmail(currentUser.email);
    const now = new Date().toISOString();
    user.name = newUsername;
    user.usernameLastChanged = now;
    
    updateUserAccount(currentUser.email, user);
    currentUser.name = newUsername;
    
    autoSave();
    updateUI();
    renderProfileSettings();
    showNotification('Username updated!');
}

function autoSave(){ if(currentUser) saveUserData(currentUser.email, buildUserData()); }

/* Color overrides */
function hexToRgb(hex){
  let h = hex.replace('#','').trim();
  if(h.length===3){ h = h.split('').map(c=>c+c).join(''); }
  const bigint = parseInt(h,16);
  return { r:(bigint>>16)&255, g:(bigint>>8)&255, b:bigint&255 };
}
function applyUserColors(colors){
  if(colors && colors.accent){ document.documentElement.style.setProperty('--accent', colors.accent); } else { document.documentElement.style.removeProperty('--accent'); }
  if(colors && colors.success){ document.documentElement.style.setProperty('--success', colors.success); } else { document.documentElement.style.removeProperty('--success'); }
  if(colors && colors.danger){ document.documentElement.style.setProperty('--danger', colors.danger); } else { document.documentElement.style.removeProperty('--danger'); }
  
  const styleEl = $('dynamicColorStyle');
  if(styleEl){
    const succ = (colors && colors.success) ? colors.success : getComputedStyle(document.body).getPropertyValue('--success').trim();
    const dang = (colors && colors.danger) ? colors.danger : getComputedStyle(document.body).getPropertyValue('--danger').trim();
    let css = '';
    if(succ){
      const s = hexToRgb(succ);
      css += `
.trade-day.profit{background: linear-gradient(180deg, rgba(${s.r},${s.g},${s.b},0.45), rgba(${s.r},${s.g},${s.b},0.25)) !important; border-color: rgba(${s.r},${s.g},${s.b},0.6) !important; box-shadow: 0 12px 28px rgba(${s.r},${s.g},${s.b},0.12);}
.home-cal .home-day.profit{background: linear-gradient(180deg, rgba(${s.r},${s.g},${s.b},0.40), rgba(${s.r},${s.g},${s.b},0.22)) !important; border-color: rgba(${s.r},${s.g},${s.b},0.45);}
      `;
    }
    if(dang){
      const d = hexToRgb(dang);
      css += `
.trade-day.loss{background: linear-gradient(180deg, rgba(${d.r},${d.g},${d.b},0.45), rgba(${d.r},${d.g},${d.b},0.25)) !important; border-color: rgba(${d.r},${d.g},${d.b},0.6) !important; box-shadow: 0 12px 28px rgba(${d.r},${d.g},${d.b},0.12);}
.home-cal .home-day.loss{background: linear-gradient(180deg, rgba(${d.r},${d.g},${d.b},0.40), rgba(${d.r},${d.g},${d.b},0.22)) !important; border-color: rgba(${d.r},${d.g},${d.b},0.45);}
      `;
    }
    styleEl.textContent = css;
  }
  renderDashboard();
  renderStatsCharts(getPeriodData(currentPeriod));
}

/* DOM bindings */
document.addEventListener('DOMContentLoaded', () => {
  initializeUserAccounts();
  initializeSoundscapes();
  loadTheme(); // Load theme first
  tryAutoLogin(); // Then autologin which might load user colors
  updateUI(); // Initial UI state
  clearJournalForm(); // Set default date

  // Header nav
  on('logo','click', () => showPage('home'));
  on('homeNav','click', () => showPage('home'));
  on('featuresNav','click', () => showPage('features'));
  on('aboutNav','click', () => showPage('about'));
  on('subsNav','click', () => showPage('subscriptions'));
  on('giveawaysNav','click', () => showPage('giveaways'));
  on('contactNav','click', () => showPage('contact'));
  on('supportNav','click', () => showPage('support'));

  on('ctaBtn','click', () => { if(currentUser) showPage('dashboard'); else openModal('registerModal'); });
  on('homeToDashboard', 'click', () => { if(currentUser) showPage('dashboard'); else openModal('loginModal'); });

  // Auth
  on('loginBtn','click', () => openModal('loginModal'));
  on('registerBtn','click', () => openModal('registerModal'));
  on('logoutBtn','click', logout);

  document.querySelectorAll('.close').forEach(btn => {
    const target = btn.getAttribute('data-close');
    if(target) btn.addEventListener('click', () => closeModal(target));
    else btn.addEventListener('click', () => { const m = btn.closest('.modal'); if(m) m.style.display = 'none'; });
  });
  on('openRegisterLink','click', (e)=> { e.preventDefault(); closeModal('loginModal'); openModal('registerModal'); });
  on('openLoginLink','click', (e)=> { e.preventDefault(); closeModal('registerModal'); openModal('loginModal'); });
  on('performRegisterBtn','click', performRegister);
  on('performLoginBtn','click', performLogin);
  on('loginShowPassword','change', (e)=> { const lp = $('loginPassword'); if(lp) lp.type = e.target.checked ? 'text' : 'password'; });
  on('registerShowPassword','change', (e)=> {
    const t1 = $('registerPassword');
    const t2 = $('registerConfirmPassword');
    if(t1) t1.type = e.target.checked ? 'text' : 'password';
    if(t2) t2.type = e.target.checked ? 'text' : 'password';
  });


  // Dashboard tab switching
  const sidebar = $('sidebar');
  if(sidebar){
    sidebar.addEventListener('click', (e) => {
      const item = e.target.closest('.nav-item');
      if(!item) return;
      const tab = item.getAttribute('data-tab');
      if(tab) switchToTab(tab);
    });
  }

  on('periodSelect','change', (e) => {
    currentPeriod = e.target.value;
    renderDashboard();
  });

  // Journal actions
  on('logTradeBtn','click', logTrade);
  on('clearJournalBtn','click', clearJournalForm);
  on('journalSearch','input', renderRecentTrades);
  on('journalSort','change', renderRecentTrades);
  on('journalFilterOutcome','change', renderRecentTrades);
  on('journalFilterSide','change', renderRecentTrades);
  on('journalRange','change', renderRecentTrades);

  // Day modal (Manual override - currently disabled from grid)
  on('saveDayBtn','click', saveDayPnL);

  // Zen tab actions
  on('startMeditation5','click', () => startMeditation(300));
  on('startMeditation10','click', () => startMeditation(600));
  on('toggleBreathworkBtn', 'click', toggleBreathwork);
  document.querySelector('.soundscape-controls').addEventListener('click', (e) => {
    const btn = e.target.closest('.sound-btn');
    if (btn) {
        const sound = btn.dataset.sound;
        playZenSound(sound);
    }
  });
  on('saveMindfulnessBtn', 'click', () => {
      if(currentUser && userProfile) {
          userProfile.mindfulnessJournal = val('mindfulnessJournal');
          autoSave();
          showNotification('Mindfulness note saved.');
      } else {
          showNotification('Please log in to save notes.');
      }
  });

  // AI
  on('askAIBtn','click', () => askAI());
  document.querySelector('.ai-suggested-prompts').addEventListener('click', (e) => {
      if(e.target.classList.contains('ai-prompt-btn')) {
          askAI(e.target.textContent);
      }
  });
  on('aiQuestion', 'keydown', (e) => { if(e.key === 'Enter') askAI(); });
  
  // Forum, Friends, DMs
  on('forumSendMessageBtn', 'click', sendForumMessage);
  on('forumMessageInput', 'keydown', (e) => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendForumMessage(); } });
  on('forumImageUploadBtn', 'click', () => $('forumImageInput').click());
  on('forumImageInput', 'change', (e) => {
      const file = e.target.files[0];
      if (file) {
          const reader = new FileReader();
          reader.onload = (event) => sendForumImage(event.target.result);
          reader.readAsDataURL(file);
      }
  });
  on('postAnnouncementBtn', 'click', postAnnouncement);

  document.querySelector('#friendsTab .sub-nav').addEventListener('click', (e) => {
    if (e.target.classList.contains('sub-nav-item')) {
        renderFriendsList(e.target.dataset.subtab);
    }
  });
  on('dmSendMessageBtn', 'click', sendDm);
  on('dmMessageInput', 'keydown', (e) => { if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendDm(); } });


  // Edit journal modal
  on('saveEditTradeBtn','click', saveEditedTrade);

  // Settings Page Redesign
  document.querySelector('.settings-nav').addEventListener('click', (e) => {
      if (e.target.classList.contains('settings-nav-item')) {
          const pane = e.target.dataset.pane;
          document.querySelectorAll('.settings-nav-item').forEach(i => i.classList.remove('active'));
          e.target.classList.add('active');
          document.querySelectorAll('.settings-pane').forEach(p => p.classList.remove('active'));
          $(pane + 'Pane').classList.add('active');
      }
  });

  on('themeSelect','change', (e)=> {
      applyTheme(e.target.value);
      showNotification('Theme applied. Colors have been reset to theme defaults.');
      if(!currentUser){ return; }
      userColors = { accent:null, success:null, danger:null };
      saveUserData(currentUser.email, buildUserData());
      renderProfileSettings(); // update color inputs
  });

  on('applyColorsBtn','click', () => {
    if(!currentUser){ showNotification('Login to save colors'); return; }
    const accent = val('colorAccent') || null;
    const success = val('colorSuccess') || null;
    const danger = val('colorDanger') || null;
    userColors = { accent, success, danger };
    applyUserColors(userColors);
    saveUserData(currentUser.email, buildUserData());
    showNotification('Colors saved');
  });
  on('resetColorsBtn','click', () => {
    if(!currentUser){ showNotification('Login to reset colors'); return; }
    userColors = { accent:null, success:null, danger:null };
    applyUserColors(userColors); // This will remove inline styles and fall back to stylesheet
    saveUserData(currentUser.email, buildUserData());
    renderProfileSettings(); // Re-render to show default color values in inputs
    showNotification('Colors reset to theme defaults');
  });

  // Profile picture handlers
  on('saveProfileBtn','click', () => {
    if(!currentUser){ showNotification('Login to save profile'); return; }
    const fileInput = $('profilePicInput');
    const file = fileInput.files && fileInput.files[0];
    if (!file) {
        showNotification('No new avatar selected.');
        return;
    }

    const isEliteOrAdmin = currentUser.subscription === 'Elite' || currentUser.email === ADMIN_EMAIL;
    const isAnimated = file.type === 'image/gif' || file.type === 'image/jfif';

    if (isAnimated && !isEliteOrAdmin) {
      showNotification('Animated avatars are for Elite members only.');
      fileInput.value = '';
      return;
    }

    const reader = new FileReader();
    reader.onload = (e) => {
      userProfile.avatarData = e.target.result;
      autoSave();
      updateAvatarUI();
      renderProfileSettings();
      showNotification('Profile picture saved!');
    };
    reader.readAsDataURL(file);
  });

  // Account
  on('saveAccountSettings','click', saveAccountSettings);
  on('saveUsernameBtn', 'click', saveUsername);
  on('profilePrivacyToggle', 'change', (e) => {
      if (userProfile) {
          userProfile.isPublic = e.target.checked;
          autoSave();
          showNotification(`Profile set to ${userProfile.isPublic ? 'Public' : 'Private'}`);
      }
  });
  
  // Admin
  on('adminUserFilter', 'change', renderAdminDashboard);
  on('confirmMuteBtn', 'click', confirmMute);

  // Misc page actions
  on('planProBtn','click', ()=> showNotification('Pro plan trial started!'));
  on('planEliteBtn','click', ()=> showNotification('Elite plan activated!'));
  on('finalCtaRegisterBtn', 'click', () => openModal('registerModal'));


  // Login enter submit
  const lp = $('loginPassword');
  if(lp) lp.addEventListener('keydown', (e) => { if(e.key === 'Enter') performLogin(); });

  // Delete modal
  on('cancelDeleteBtn','click', cancelDelete);
  on('confirmDeleteBtn','click', confirmDelete);

  if (!currentUser) {
    renderDashboard();
  }
  initBackgroundCanvas();
  initHomepageAnimations();
});

/* ZEN Page Features */
const soundscapes = {};

function initializeSoundscapes() {
    // Short, loopable base64 audio files.
    const sounds = {
        rain: 'data:audio/mp3;base64,SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU4LjQ1LjEwMAAAAAAAAAAAAAAA//tAwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB1cHJvY2Vzc2VkIGJ5IG1wM2RpYWcgdjEuMTIgKGh0dHA6Ly9tcDNkaWFnLm1hZ25vLml0KQCrgAAAAAAB9AAAKgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA//tAwBMAAAAAAABQoAAAAAAAAAABEwVVIuAAgEAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICA//tAwBLAAAACAAADSAAAAAPAERAIAgICAoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tá',
        forest: 'data:audio/mp3;base64,SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU4LjQ1LjEwMAAAAAAAAAAAAAAA//tAwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB1cHJvY2Vzc2VkIGJ5IG1wM2RpYWcgdjEuMTIgKGh0dHA6Ly9tcDNkaWFnLm1hZ25vLml0KQCrgAAAAAAB9AAAKgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA//tAwBMAAAAAAABQoAAAAAAAAAABEwVVIuAAgEAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICA//tAwBLAAAACAAADSAAAAAPAERAIAgICAoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tá',
        waves: 'data:audio/mp3;base64,SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU4LjQ1LjEwMAAAAAAAAAAAAAAA//tAwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB1cHJvY2Vzc2VkIGJ5IG1wM2RpYWcgdjEuMTIgKGh0dHA6Ly9tcDNkaWFnLm1hZ25vLml0KQCrgAAAAAAB9AAAKgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA//tAwBMAAAAAAABQoAAAAAAAAAABEwVVIuAAgEAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICA//tAwBLAAAACAAADSAAAAAPAERAIAgICAoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tAwBIAAABCAAADSAAAAAPcBAQCAgKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoK//tá'
    };
    Object.keys(sounds).forEach(key => {
        soundscapes[key] = new Audio(sounds[key]);
        soundscapes[key].loop = true;
    });
}

function playZenSound(soundName) {
    Object.keys(soundscapes).forEach(key => {
        const sound = soundscapes[key];
        if (!sound.paused) {
            sound.pause();
            sound.currentTime = 0;
        }
    });

    if (soundName !== 'none' && soundscapes[soundName]) {
        soundscapes[soundName].play().catch(e => console.error("Audio play failed:", e));
    }

    document.querySelectorAll('.sound-btn').forEach(b => b.classList.remove('active'));
    if (soundName !== 'none') {
      const activeBtn = document.querySelector(`.sound-btn[data-sound="${soundName}"]`);
      if (activeBtn) activeBtn.classList.add('active');
    }

    const displayName = soundName.charAt(0).toUpperCase() + soundName.slice(1);
    $('nowPlayingSound').textContent = `Now Playing: ${displayName}`;
    if (soundName !== 'none') showNotification(`Playing ${displayName} soundscape`);
}

function startMeditation(duration){
  if(meditationTimer) return;
  meditationTimeLeft = duration;
  document.body.classList.add('in-meditation');
  
  const overlay = document.createElement('div');
  overlay.className = 'meditation-overlay';
  overlay.id = 'meditationOverlay';
  
  const bg = document.createElement('div');
  bg.className = 'meditation-bg';
  overlay.appendChild(bg);
  
  const screen = document.createElement('div');
  screen.className = 'meditation-screen';
  screen.innerHTML = `<div class="meditation-panel">
    <div style="font-size:2.2rem;margin-bottom:.5rem">🧘 Begin Meditation</div>
    <div class="meditation-instruction" id="meditationInstruction">Breathe in... Breathe out...</div>
    <div class="meditation-timer" id="meditationTimer">${formatTime(meditationTimeLeft)}</div>
    <div style="margin-top:1rem"><button class="btn-secondary" id="endMeditationBtn">End Session</button></div>
  </div>`;
  overlay.appendChild(screen);

  document.body.appendChild(overlay);
  
  on('endMeditationBtn','click', stopMeditation, {once: true}); // Use once to avoid multiple listeners

  let idx = 0;
  instructionInterval = setInterval(() => {
    idx = (idx + 1) % 5;
    const el = $('meditationInstruction');
    if(el){
      const arr = ["Breathe in... Breathe out...","Calm your thoughts...","Feel the breath in your belly...","Release tension from your body...","Stay present with each breath..."];
      el.textContent = arr[idx];
    }
  }, 8000);

  meditationTimer = setInterval(() => {
    meditationTimeLeft--;
    const t = $('meditationTimer');
    if(t) t.textContent = formatTime(meditationTimeLeft);
    if(meditationTimeLeft <= 0) {
      stopMeditation();
      showNotification('Meditation session completed! 🧘');
    }
  }, 1000);
}
function stopMeditation(){
  if(instructionInterval){ clearInterval(instructionInterval); instructionInterval = null; }
  if(meditationTimer){ clearInterval(meditationTimer); meditationTimer = null; }
  const overlay = $('meditationOverlay'); if(overlay) overlay.remove();
  document.body.classList.remove('in-meditation');
}
function formatTime(s){ const m = Math.floor(s/60); const sec = s % 60; return (String(m).padStart(2,'0') + ':' + String(sec).padStart(2,'0')); }

function toggleBreathwork() {
    const viz = $('breathingVisualizer');
    const btn = $('toggleBreathworkBtn');
    const instruction = $('breathInstruction');
    
    if (viz.classList.contains('active')) {
        viz.classList.remove('active');
        btn.textContent = 'Start Breathing';
        instruction.textContent = 'Stopped';
        clearInterval(breathworkInterval);
        breathworkInterval = null;
    } else {
        viz.classList.add('active');
        btn.textContent = 'Stop Breathing';
        let step = 0;
        const instructions = ['Inhale', 'Hold', 'Exhale', 'Hold'];
        
        const updateInstruction = () => {
            instruction.textContent = instructions[step % 4];
            step++;
        };
        
        updateInstruction(); // Initial instruction
        breathworkInterval = setInterval(updateInstruction, 4000); // Update every 4 seconds
    }
}


/* Background canvas */
function initBackgroundCanvas(){
  const canvas = $('bgCanvas');
  if(!canvas) return;
  const ctx = canvas.getContext('2d');
  let w = canvas.width = innerWidth, h = canvas.height = innerHeight;
  const particles = [];
  const count = Math.max(42, Math.floor((w*h) / 12000));
  function rand(a,b){ return Math.random() * (b-a) + a; }
  for(let i=0;i<count;i++){
    particles.push({ x: rand(0,w), y: rand(0,h), r: rand(.6,2.2), vx: rand(-.10,.10), vy: rand(-.05,.05), hue: rand(185,205),
