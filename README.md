<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EditorClubs — The AI Studio for Editors</title>
<meta name="description" content="EditorClubs — prompt libraries, an AI prompt generator, and every editing tool and app in one luxurious workspace.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@300;400;500;600;700;800&family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://unpkg.com/lucide@latest/dist/umd/lucide.min.js"></script>
<style>
/* ============ TOKENS ============ */
:root{
  --navy: #0a0d16;
  --navy-2: #0e1220;
  --charcoal: #15161c;
  --charcoal-2: #1b1c24;
  --steel: #7e9bb8;
  --steel-soft: #6b8cae;
  --pink: #c99da3;
  --pink-soft: #d9b3b8;
  --purple: #9b8bc4;
  --purple-soft: #b3a5d4;
  --text-hi: #f2eef2;
  --text-mid: #b9b8c4;
  --text-low: #7c7b8a;
  --line: rgba(242,238,242,0.08);
  --glass: rgba(255,255,255,0.045);
  --glass-hi: rgba(255,255,255,0.075);
  --radius: 20px;
  --ease: cubic-bezier(.16,.8,.24,1);
}
*{box-sizing:border-box; margin:0; padding:0;}
html{scroll-behavior:smooth;}
body{
  background: var(--navy);
  color: var(--text-hi);
  font-family:'Inter',sans-serif;
  overflow-x:hidden;
  position:relative;
}
::selection{ background: var(--purple); color:#0a0d16; }
::-webkit-scrollbar{width:10px;}
::-webkit-scrollbar-track{background:var(--navy);}
::-webkit-scrollbar-thumb{background:linear-gradient(var(--steel-soft),var(--purple)); border-radius:10px;}
a{color:inherit; text-decoration:none;}
button{font-family:inherit; cursor:pointer;}
h1,h2,h3,h4{font-family:'Sora',sans-serif; letter-spacing:-0.02em;}
.mono{font-family:'JetBrains Mono',monospace;}
img,svg{display:block;}

/* ============ BACKDROP ============ */
.bg-field{
  position:fixed; inset:0; z-index:0; pointer-events:none;
  background:
    radial-gradient(60% 45% at 18% 8%, rgba(155,139,196,0.16), transparent 60%),
    radial-gradient(50% 40% at 85% 15%, rgba(107,140,174,0.14), transparent 60%),
    radial-gradient(55% 45% at 50% 100%, rgba(201,157,163,0.10), transparent 60%),
    linear-gradient(180deg, var(--navy) 0%, var(--navy-2) 40%, var(--charcoal) 100%);
}
.grain{
  position:fixed; inset:0; z-index:1; pointer-events:none; opacity:.035; mix-blend-mode:overlay;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='120' height='120'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
.container{ max-width:1280px; margin:0 auto; padding:0 32px; position:relative; z-index:2;}

/* ============ NAV ============ */
header{
  position:fixed; top:0; left:0; right:0; z-index:100;
  padding:18px 0;
  transition: background .4s var(--ease), border-color .4s var(--ease), padding .4s var(--ease);
  border-bottom:1px solid transparent;
}
header.scrolled{
  background:rgba(10,13,22,0.72);
  backdrop-filter:blur(18px) saturate(140%);
  -webkit-backdrop-filter:blur(18px) saturate(140%);
  border-bottom:1px solid var(--line);
  padding:12px 0;
}
.nav-inner{ display:flex; align-items:center; justify-content:space-between; gap:24px;}
.logo-mark{ display:flex; align-items:center; gap:12px; }
.logo-word{ font-family:'Sora',sans-serif; font-weight:700; font-size:19px; letter-spacing:-0.01em; }
.logo-word span{ color:var(--pink-soft); font-weight:400;}
nav.links{ display:flex; align-items:center; gap:34px;}
nav.links a{ font-size:14px; color:var(--text-mid); transition:color .25s var(--ease); position:relative;}
nav.links a:hover{ color:var(--text-hi);}
nav.links a::after{
  content:''; position:absolute; left:0; bottom:-6px; height:1px; width:0%;
  background:linear-gradient(90deg,var(--pink),var(--purple)); transition:width .3s var(--ease);
}
nav.links a:hover::after{ width:100%; }
.nav-actions{ display:flex; align-items:center; gap:14px;}
.btn{
  display:inline-flex; align-items:center; gap:8px; justify-content:center;
  padding:11px 20px; border-radius:100px; font-size:13.5px; font-weight:500;
  border:1px solid var(--line); transition:all .3s var(--ease); white-space:nowrap;
}
.btn-ghost{ color:var(--text-hi); background:var(--glass);}
.btn-ghost:hover{ background:var(--glass-hi); border-color:rgba(242,238,242,0.18); transform:translateY(-1px);}
.btn-primary{
  background:linear-gradient(135deg, var(--pink) 0%, var(--purple) 100%);
  color:#0a0d16; font-weight:600; border:none; box-shadow:0 8px 24px -8px rgba(155,139,196,0.55);
}
.btn-primary:hover{ transform:translateY(-2px); box-shadow:0 14px 30px -8px rgba(155,139,196,0.7);}
.avatar-btn{
  width:38px; height:38px; border-radius:50%; display:flex; align-items:center; justify-content:center;
  background:linear-gradient(135deg,var(--steel-soft),var(--purple)); font-weight:700; font-size:13px; color:#0a0d16;
  border:1px solid rgba(255,255,255,0.2);
}
.menu-toggle{ display:none; }

/* ============ HERO ============ */
.hero{ position:relative; padding:180px 0 120px; overflow:visible;}
.hero-grid{ display:grid; grid-template-columns:1.05fr .95fr; gap:60px; align-items:center;}
.eyebrow{
  display:inline-flex; align-items:center; gap:8px; padding:7px 14px; border-radius:100px;
  border:1px solid var(--line); background:var(--glass); font-size:12.5px; color:var(--text-mid);
  font-family:'JetBrains Mono',monospace; margin-bottom:26px;
}
.eyebrow .dot{ width:6px; height:6px; border-radius:50%; background:var(--pink); box-shadow:0 0 10px var(--pink); animation:pulse 2s infinite;}
@keyframes pulse{ 0%,100%{opacity:1;} 50%{opacity:.3;} }
.hero h1{
  font-size:clamp(40px,5.4vw,68px); line-height:1.03; font-weight:700; color:var(--text-hi);
}
.hero h1 em{
  font-style:normal; font-weight:600;
  background:linear-gradient(100deg,var(--pink) 10%, var(--purple) 55%, var(--steel) 100%);
  -webkit-background-clip:text; background-clip:text; color:transparent;
}
.hero p.lead{ font-size:17px; line-height:1.7; color:var(--text-mid); max-width:520px; margin:24px 0 36px;}
.hero-actions{ display:flex; gap:14px; margin-bottom:44px; flex-wrap:wrap;}
.hero-stats{ display:flex; gap:38px; flex-wrap:wrap;}
.hero-stats .stat b{ display:block; font-family:'Sora',sans-serif; font-size:24px; font-weight:700; color:var(--text-hi);}
.hero-stats .stat span{ font-size:12.5px; color:var(--text-low); }

/* hero search */
.hero-search{
  margin-top:38px; display:flex; align-items:center; gap:10px; padding:8px 8px 8px 18px;
  border-radius:100px; background:var(--glass); border:1px solid var(--line); max-width:480px;
  backdrop-filter:blur(14px);
}
.hero-search input{
  flex:1; background:none; border:none; outline:none; color:var(--text-hi); font-size:14px; font-family:'Inter';
}
.hero-search input::placeholder{ color:var(--text-low);}
.hero-search button{
  padding:11px 20px; border-radius:100px; border:none; background:linear-gradient(135deg,var(--steel-soft),var(--purple));
  color:#0a0d16; font-weight:600; font-size:13px;
}

/* floating scene */
.scene{ position:relative; height:520px; perspective:1200px;}
.orb{
  position:absolute; top:50%; left:50%; width:280px; height:280px; border-radius:50%;
  background: radial-gradient(circle at 32% 28%, rgba(217,179,184,0.9), rgba(155,139,196,0.55) 45%, rgba(107,140,174,0.25) 75%, transparent 80%);
  transform:translate(-50%,-50%); filter:blur(2px);
  box-shadow: 0 0 120px 20px rgba(155,139,196,0.25);
  animation: float-orb 8s ease-in-out infinite;
}
@keyframes float-orb{ 0%,100%{ transform:translate(-50%,-50%) scale(1);} 50%{ transform:translate(-50%,-54%) scale(1.04);} }
.fcard{
  position:absolute; width:210px; padding:16px; border-radius:16px;
  background:var(--glass-hi); border:1px solid rgba(255,255,255,0.12);
  backdrop-filter:blur(20px) saturate(150%); -webkit-backdrop-filter:blur(20px) saturate(150%);
  box-shadow:0 20px 40px -16px rgba(0,0,0,0.6);
  animation: float-card 6s ease-in-out infinite;
}
.fcard h5{ font-size:12px; color:var(--text-mid); font-weight:500; margin-bottom:8px; display:flex; align-items:center; gap:6px;}
.fcard p{ font-size:12.5px; color:var(--text-hi); line-height:1.5; font-family:'JetBrains Mono',monospace;}
.fcard.c1{ top:6%; left:0%; animation-delay:.2s;}
.fcard.c2{ bottom:10%; left:6%; animation-delay:1.4s; width:180px;}
.fcard.c3{ top:38%; right:-4%; animation-delay:.8s;}
.fcard.c4{ bottom:0%; right:10%; animation-delay:2s; width:170px;}
@keyframes float-card{ 0%,100%{ transform:translateY(0px);} 50%{ transform:translateY(-16px);} }

/* ============ SECTION GENERIC ============ */
section{ position:relative; padding:110px 0; z-index:2;}
.section-head{ max-width:640px; margin-bottom:56px;}
.section-head .eyebrow{ margin-bottom:18px;}
.section-head h2{ font-size:clamp(30px,3.6vw,44px); font-weight:700; margin-bottom:16px; }
.section-head p{ color:var(--text-mid); font-size:15.5px; line-height:1.7;}
.reveal{ opacity:0; transform:translateY(28px); transition:opacity .8s var(--ease), transform .8s var(--ease);}
.reveal.in{ opacity:1; transform:translateY(0);}

/* ============ GLASS CARD BASE ============ */
.glass{
  background:var(--glass); border:1px solid var(--line); border-radius:var(--radius);
  backdrop-filter:blur(16px) saturate(140%); -webkit-backdrop-filter:blur(16px) saturate(140%);
  transition: transform .4s var(--ease), border-color .4s var(--ease), background .4s var(--ease), box-shadow .4s var(--ease);
}
.glass:hover{ border-color:rgba(255,255,255,0.16); background:var(--glass-hi); transform:translateY(-4px); box-shadow:0 24px 50px -20px rgba(0,0,0,0.5);}

/* ============ LIBRARY ============ */
.lib-toolbar{ display:flex; flex-wrap:wrap; gap:12px; align-items:center; justify-content:space-between; margin-bottom:32px;}
.search-box{
  display:flex; align-items:center; gap:10px; padding:11px 16px; border-radius:12px;
  background:var(--glass); border:1px solid var(--line); min-width:260px; flex:1; max-width:380px;
}
.search-box input{ background:none; border:none; outline:none; color:var(--text-hi); font-size:13.5px; width:100%;}
.search-box input::placeholder{color:var(--text-low);}
.chips{ display:flex; gap:8px; flex-wrap:wrap;}
.chip{
  padding:8px 15px; border-radius:100px; font-size:12.5px; border:1px solid var(--line); background:var(--glass);
  color:var(--text-mid); transition:all .25s var(--ease); white-space:nowrap;
}
.chip:hover{ color:var(--text-hi); border-color:rgba(255,255,255,0.2);}
.chip.active{
  background:linear-gradient(135deg,var(--pink),var(--purple)); color:#0a0d16; border-color:transparent; font-weight:600;
}
.prompt-grid{ display:grid; grid-template-columns:repeat(3,1fr); gap:20px;}
.pcard{ padding:22px; display:flex; flex-direction:column; gap:14px; position:relative;}
.pcard-top{ display:flex; justify-content:space-between; align-items:flex-start;}
.pcard-cat{
  font-size:10.5px; text-transform:uppercase; letter-spacing:.08em; color:var(--steel);
  font-family:'JetBrains Mono',monospace; display:flex; align-items:center; gap:6px;
}
.pcard-icons{ display:flex; gap:6px;}
.icon-btn{
  width:30px; height:30px; border-radius:50%; display:flex; align-items:center; justify-content:center;
  background:rgba(255,255,255,0.05); border:1px solid var(--line); color:var(--text-mid); transition:all .25s var(--ease);
}
.icon-btn:hover{ color:var(--text-hi); background:rgba(255,255,255,0.1); transform:scale(1.08);}
.icon-btn.active{ color:var(--pink); border-color:var(--pink);}
.icon-btn svg{width:14px; height:14px;}
.pcard h4{ font-size:15px; font-weight:600; color:var(--text-hi);}
.pcard .ptext{ font-size:13px; line-height:1.65; color:var(--text-mid); font-family:'JetBrains Mono',monospace; max-height:82px; overflow:hidden;}
.pcard-foot{ display:flex; align-items:center; justify-content:space-between; margin-top:auto; padding-top:12px; border-top:1px solid var(--line);}
.trend-badge{ display:flex; align-items:center; gap:5px; font-size:11px; color:var(--pink-soft); }
.copy-btn{
  display:flex; align-items:center; gap:6px; font-size:12px; font-weight:500; color:var(--text-hi);
  padding:7px 14px; border-radius:100px; background:rgba(255,255,255,0.06); border:1px solid var(--line);
  transition:all .25s var(--ease);
}
.copy-btn:hover{ background:linear-gradient(135deg,var(--pink),var(--purple)); color:#0a0d16; border-color:transparent;}
.copy-btn.copied{ background:linear-gradient(135deg,var(--steel-soft),var(--purple)); color:#0a0d16; border-color:transparent;}
.load-more-wrap{ display:flex; justify-content:center; margin-top:40px;}

/* ============ GENERATOR ============ */
.gen-panel{ display:grid; grid-template-columns:.95fr 1.05fr; gap:0; border-radius:28px; overflow:hidden; border:1px solid var(--line); background:var(--glass); backdrop-filter:blur(20px);}
.gen-left{ padding:48px; border-right:1px solid var(--line);}
.gen-left label{ font-size:12.5px; color:var(--text-mid); font-family:'JetBrains Mono',monospace; text-transform:uppercase; letter-spacing:.06em; display:block; margin-bottom:10px;}
.gen-left textarea{
  width:100%; min-height:130px; background:rgba(255,255,255,0.03); border:1px solid var(--line); border-radius:14px;
  padding:16px; color:var(--text-hi); font-size:14px; font-family:'Inter'; resize:vertical; outline:none; margin-bottom:24px;
  transition:border-color .3s var(--ease);
}
.gen-left textarea:focus{ border-color:var(--purple);}
.select-row{ display:flex; gap:10px; flex-wrap:wrap; margin-bottom:28px;}
.pill-select{
  padding:9px 15px; border-radius:100px; font-size:12.5px; border:1px solid var(--line); background:rgba(255,255,255,0.03);
  color:var(--text-mid);
}
.pill-select.active{ background:linear-gradient(135deg,var(--steel-soft),var(--purple)); color:#0a0d16; border-color:transparent; font-weight:600;}
.generate-btn{
  width:100%; padding:16px; border-radius:14px; border:none; font-weight:600; font-size:14.5px;
  background:linear-gradient(135deg,var(--pink),var(--purple)); color:#0a0d16; display:flex; align-items:center; justify-content:center; gap:10px;
  box-shadow:0 14px 30px -10px rgba(155,139,196,0.5); transition:transform .3s var(--ease);
}
.generate-btn:hover{ transform:translateY(-2px);}
.generate-btn:disabled{ opacity:.6; transform:none; cursor:wait;}
.gen-right{ padding:48px; display:flex; flex-direction:column; gap:16px; max-height:560px; overflow-y:auto;}
.gen-empty{ margin:auto; text-align:center; color:var(--text-low); font-size:13.5px; max-width:260px;}
.gen-empty svg{ margin:0 auto 16px; opacity:.5;}
.result-card{
  padding:20px; border-radius:16px; background:rgba(255,255,255,0.04); border:1px solid var(--line); position:relative;
  animation: rise .5s var(--ease) both;
}
@keyframes rise{ from{opacity:0; transform:translateY(14px);} to{opacity:1; transform:translateY(0);} }
.result-card h5{ font-size:12.5px; color:var(--purple-soft); margin-bottom:10px; font-family:'JetBrains Mono',monospace;}
.result-card p{ font-size:13.5px; line-height:1.7; color:var(--text-hi); font-family:'JetBrains Mono',monospace; white-space:pre-wrap;}
.spinner{ width:16px; height:16px; border-radius:50%; border:2px solid rgba(10,13,22,0.3); border-top-color:#0a0d16; animation:spin .7s linear infinite;}
@keyframes spin{ to{ transform:rotate(360deg);} }

/* ============ TOOLS ============ */
.tool-grid{ display:grid; grid-template-columns:repeat(5,1fr); gap:18px;}
.tool-card{ padding:26px 20px; text-align:left; display:flex; flex-direction:column; gap:14px;}
.tool-icon{
  width:46px; height:46px; border-radius:13px; display:flex; align-items:center; justify-content:center;
  background:linear-gradient(135deg, rgba(201,157,163,0.18), rgba(155,139,196,0.18)); color:var(--pink-soft); border:1px solid var(--line);
}
.tool-icon svg{ width:22px; height:22px;}
.tool-card h4{ font-size:14.5px; font-weight:600;}
.tool-card p{ font-size:12.5px; color:var(--text-low); line-height:1.5;}
.tool-card .go{ font-size:12px; color:var(--steel); display:flex; align-items:center; gap:5px; margin-top:auto;}

/* ============ APPS ============ */
.app-grid{ display:grid; grid-template-columns:repeat(3,1fr); gap:20px;}
.app-card{ padding:26px; display:flex; flex-direction:column; gap:14px;}
.app-top{ display:flex; align-items:center; gap:14px;}
.app-logo{
  width:50px; height:50px; border-radius:13px; display:flex; align-items:center; justify-content:center;
  font-family:'Sora'; font-weight:700; font-size:16px; color:#0a0d16; flex-shrink:0;
}
.app-name{ font-size:15.5px; font-weight:600;}
.app-rating{ display:flex; align-items:center; gap:5px; font-size:12px; color:var(--pink-soft); margin-top:2px;}
.app-desc{ font-size:13px; color:var(--text-mid); line-height:1.6;}
.app-features{ display:flex; flex-wrap:wrap; gap:6px;}
.feat-tag{ font-size:10.5px; padding:5px 10px; border-radius:100px; background:rgba(255,255,255,0.05); color:var(--text-low); border:1px solid var(--line);}
.app-foot{ display:flex; align-items:center; justify-content:space-between; margin-top:auto; padding-top:14px; border-top:1px solid var(--line);}
.dl-link{ font-size:12.5px; font-weight:600; color:var(--text-hi); display:flex; align-items:center; gap:6px;}
.dl-link svg{ width:14px; height:14px; transition:transform .25s var(--ease);}
.dl-link:hover svg{ transform:translate(2px,-2px);}

/* ============ DASHBOARD PREVIEW ============ */
.dash-wrap{ border-radius:28px; border:1px solid var(--line); background:linear-gradient(160deg, rgba(255,255,255,0.05), rgba(255,255,255,0.015)); padding:4px; overflow:hidden; box-shadow:0 40px 90px -30px rgba(0,0,0,0.7);}
.dash-inner{ border-radius:24px; background:var(--charcoal); display:grid; grid-template-columns:220px 1fr; min-height:460px; overflow:hidden;}
.dash-side{ border-right:1px solid var(--line); padding:26px 18px; display:flex; flex-direction:column; gap:6px;}
.dash-side .d-item{ display:flex; align-items:center; gap:10px; padding:10px 12px; border-radius:10px; font-size:13px; color:var(--text-mid);}
.dash-side .d-item svg{ width:16px; height:16px;}
.dash-side .d-item.active{ background:rgba(255,255,255,0.06); color:var(--text-hi);}
.dash-main{ padding:30px 34px;}
.dash-main h3{ font-size:19px; margin-bottom:4px;}
.dash-main .sub{ font-size:13px; color:var(--text-low); margin-bottom:26px;}
.kpi-row{ display:grid; grid-template-columns:repeat(3,1fr); gap:14px; margin-bottom:26px;}
.kpi{ padding:18px; border-radius:14px; background:rgba(255,255,255,0.03); border:1px solid var(--line);}
.kpi b{ font-family:'Sora'; font-size:22px; display:block;}
.kpi span{ font-size:11.5px; color:var(--text-low);}
.mini-row{ display:grid; grid-template-columns:repeat(4,1fr); gap:14px;}
.mini-card{ padding:16px; border-radius:14px; background:rgba(255,255,255,0.03); border:1px solid var(--line); height:100px; display:flex; flex-direction:column; justify-content:space-between;}
.mini-card .tag{ font-size:10px; color:var(--purple-soft); font-family:'JetBrains Mono';}
.mini-card .txt{ font-size:11.5px; color:var(--text-mid); line-height:1.4;}

/* ============ CTA ============ */
.cta-band{
  border-radius:32px; padding:70px 60px; text-align:center; position:relative; overflow:hidden;
  background:linear-gradient(135deg, rgba(201,157,163,0.14), rgba(107,140,174,0.1), rgba(155,139,196,0.16));
  border:1px solid var(--line);
}
.cta-band h2{ font-size:clamp(28px,3.4vw,42px); margin-bottom:16px;}
.cta-band p{ color:var(--text-mid); margin-bottom:32px; font-size:15px;}
.cta-actions{ display:flex; gap:14px; justify-content:center; flex-wrap:wrap;}

/* ============ FOOTER ============ */
footer{ padding:70px 0 34px; border-top:1px solid var(--line); position:relative; z-index:2;}
.foot-grid{ display:grid; grid-template-columns:1.4fr 1fr 1fr 1fr; gap:40px; margin-bottom:50px;}
.foot-brand p{ font-size:13px; color:var(--text-low); line-height:1.7; margin-top:14px; max-width:280px;}
.foot-col h5{ font-size:12.5px; text-transform:uppercase; letter-spacing:.06em; color:var(--text-low); margin-bottom:16px; font-family:'JetBrains Mono';}
.foot-col a{ display:block; font-size:13.5px; color:var(--text-mid); margin-bottom:12px; transition:color .25s;}
.foot-col a:hover{ color:var(--text-hi);}
.foot-bottom{ display:flex; justify-content:space-between; align-items:center; padding-top:28px; border-top:1px solid var(--line); font-size:12.5px; color:var(--text-low); flex-wrap:wrap; gap:14px;}
.foot-social{ display:flex; gap:12px;}
.foot-social a{ width:34px; height:34px; border-radius:50%; border:1px solid var(--line); display:flex; align-items:center; justify-content:center; color:var(--text-mid);}
.foot-social a:hover{ color:var(--text-hi); border-color:rgba(255,255,255,0.2);}

/* ============ MODAL / TOAST ============ */
.toast{
  position:fixed; bottom:28px; left:50%; transform:translateX(-50%) translateY(20px); z-index:300;
  padding:13px 22px; border-radius:100px; background:var(--charcoal-2); border:1px solid var(--line);
  font-size:13px; color:var(--text-hi); display:flex; align-items:center; gap:10px; opacity:0; pointer-events:none;
  transition:all .4s var(--ease); box-shadow:0 20px 40px -10px rgba(0,0,0,0.5);
}
.toast.show{ opacity:1; transform:translateX(-50%) translateY(0); }
.toast svg{ width:15px; height:15px; color:var(--pink-soft);}

/* ============ RESPONSIVE ============ */
@media(max-width:1080px){
  .hero-grid{ grid-template-columns:1fr;}
  .scene{ height:380px; margin-top:20px;}
  .prompt-grid{ grid-template-columns:repeat(2,1fr);}
  .tool-grid{ grid-template-columns:repeat(3,1fr);}
  .app-grid{ grid-template-columns:repeat(2,1fr);}
  .gen-panel{ grid-template-columns:1fr;}
  .gen-left{ border-right:none; border-bottom:1px solid var(--line);}
  .foot-grid{ grid-template-columns:1fr 1fr;}
}
@media(max-width:720px){
  nav.links{ display:none;}
  .container{ padding:0 20px;}
  .prompt-grid{ grid-template-columns:1fr;}
  .tool-grid{ grid-template-columns:repeat(2,1fr);}
  .app-grid{ grid-template-columns:1fr;}
  .dash-inner{ grid-template-columns:1fr;}
  .dash-side{ display:none;}
  .kpi-row{ grid-template-columns:1fr;}
  .mini-row{ grid-template-columns:1fr 1fr;}
  .lib-toolbar{ flex-direction:column; align-items:stretch;}
  .search-box{ max-width:none;}
  .foot-grid{ grid-template-columns:1fr;}
  .hero{ padding:140px 0 80px;}
  .cta-band{ padding:50px 24px;}
}
@media (prefers-reduced-motion: reduce){
  *{ animation-duration:0.01ms !important; animation-iteration-count:1 !important; transition-duration:0.01ms !important; scroll-behavior:auto !important;}
}
</style>
</head>
<body>

<div class="bg-field"></div>
<div class="grain"></div>

<!-- ============ NAV ============ -->
<header id="siteHeader">
  <div class="container nav-inner">
    <div class="logo-mark">
      <svg width="34" height="34" viewBox="0 0 48 48" fill="none">
        <defs>
          <linearGradient id="logoGrad" x1="4" y1="6" x2="44" y2="44" gradientUnits="userSpaceOnUse">
            <stop offset="0%" stop-color="#d9b3b8"/>
            <stop offset="50%" stop-color="#9b8bc4"/>
            <stop offset="100%" stop-color="#6b8cae"/>
          </linearGradient>
        </defs>
        <circle cx="24" cy="24" r="22" stroke="url(#logoGrad)" stroke-width="1.4" opacity="0.5"/>
        <g>
          <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.95" transform="rotate(0 24 24)"/>
          <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.85" transform="rotate(60 24 24)"/>
          <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.75" transform="rotate(120 24 24)"/>
          <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.95" transform="rotate(180 24 24)"/>
          <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.85" transform="rotate(240 24 24)"/>
          <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.75" transform="rotate(300 24 24)"/>
        </g>
        <circle cx="24" cy="24" r="5.5" fill="#0a0d16" stroke="url(#logoGrad)" stroke-width="1.2"/>
      </svg>
      <div class="logo-word">Editor<span>Clubs</span></div>
    </div>
    <nav class="links">
      <a href="#library">Prompt Library</a>
      <a href="#generator">AI Generator</a>
      <a href="#tools">Tools</a>
      <a href="#apps">Apps</a>
      <a href="#dashboard">Dashboard</a>
    </nav>
    <div class="nav-actions">
      <a href="#" class="btn btn-ghost" id="signInBtn">Sign in</a>
      <a href="#generator" class="btn btn-primary">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z"/></svg>
        Try free
      </a>
      <div class="avatar-btn">EC</div>
    </div>
  </div>
</header>

<!-- ============ HERO ============ -->
<section class="hero">
  <div class="container hero-grid">
    <div>
      <div class="eyebrow"><span class="dot"></span> Built for editors, powered by AI</div>
      <h1>Your entire <em>editing studio</em>,<br> reimagined for 2026.</h1>
      <p class="lead">EditorClubs brings together curated AI prompts, an intelligent prompt generator, and every editing tool and app you rely on — in one cinematic, glass-clear workspace.</p>
      <div class="hero-actions">
        <a href="#generator" class="btn btn-primary">
          <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 3v18M3 5h4M3 19h4M14 3l2.3 6.9H22l-5.7 4.1 2.2 6.9L14 16.9l-4.5 3.1 2.2-6.9L6 9.9h5.7z"/></svg>
          Generate a prompt
        </a>
        <a href="#library" class="btn btn-ghost">Browse the library</a>
      </div>
      <div class="hero-stats">
        <div class="stat"><b id="statPrompts">0</b><span>Curated prompts</span></div>
        <div class="stat"><b id="statTools">0</b><span>Editing tools</span></div>
        <div class="stat"><b id="statApps">0</b><span>Pro apps indexed</span></div>
        <div class="stat"><b id="statEditors">0</b><span>Editors onboard</span></div>
      </div>
      <div class="hero-search">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="#7c7b8a" stroke-width="2"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4-4"/></svg>
        <input id="heroSearch" placeholder="Search “cinematic color grade for portraits”…">
        <button id="heroSearchBtn">Search</button>
      </div>
    </div>
    <div class="scene">
      <div class="orb"></div>
      <div class="fcard c1">
        <h5><svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="#c99da3" stroke-width="2"><path d="M23 7l-7 5 7 5V7z"/><rect x="1" y="5" width="15" height="14" rx="2"/></svg> Cinematic</h5>
        <p>"Moody teal &amp; orange grade, 35mm grain, soft halation…"</p>
      </div>
      <div class="fcard c2">
        <h5><svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="#9b8bc4" stroke-width="2"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 3"/></svg> Reels</h5>
        <p>"9:16 hook in 1.5s, punch-in zoom, kinetic captions…"</p>
      </div>
      <div class="fcard c3">
        <h5><svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="#6b8cae" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="3"/><circle cx="8.5" cy="8.5" r="1.5"/><path d="M21 15l-5-5L5 21"/></svg> AI Image</h5>
        <p>"Studio portrait, Rembrandt light, dusty pink backdrop…"</p>
      </div>
      <div class="fcard c4">
        <h5><svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="#d9b3b8" stroke-width="2"><path d="M17 2l4 4-4 4M3 12h18M7 22l-4-4 4-4"/></svg> Motion</h5>
        <p>"Kinetic type reveal, easing 0.7 spring, glass parallax…"</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ LOGO STRIP ============ -->
<section style="padding:20px 0 70px;">
  <div class="container">
    <p style="text-align:center; font-size:11.5px; letter-spacing:.1em; text-transform:uppercase; color:var(--text-low); font-family:'JetBrains Mono'; margin-bottom:26px;">Works alongside the tools you already use</p>
    <div style="display:flex; justify-content:center; gap:44px; flex-wrap:wrap; opacity:.6; font-family:'Sora'; font-weight:600; font-size:14px; color:var(--text-mid);">
      <span>Premiere Pro</span><span>After Effects</span><span>Photoshop</span><span>DaVinci Resolve</span><span>CapCut</span><span>Final Cut Pro</span><span>Figma</span><span>Blender</span>
    </div>
  </div>
</section>

<!-- ============ PROMPT LIBRARY ============ -->
<section id="library">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Prompt library</div>
      <h2>Every editing prompt, already written.</h2>
      <p>Hand-tuned prompts across video, photo, motion and AI generation — filter by category, save favorites, and copy straight into your workflow.</p>
    </div>

    <div class="lib-toolbar reveal">
      <div class="search-box">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="#7c7b8a" stroke-width="2"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4-4"/></svg>
        <input id="libSearch" placeholder="Search prompts…">
      </div>
      <div class="chips" id="chipRow"></div>
    </div>

    <div class="prompt-grid" id="promptGrid"></div>
    <div class="load-more-wrap">
      <button class="btn btn-ghost" id="loadMoreBtn">Show more prompts</button>
    </div>
  </div>
</section>

<!-- ============ AI PROMPT GENERATOR ============ -->
<section id="generator">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">AI prompt generator</div>
      <h2>Describe your project. Get the exact prompt.</h2>
      <p>Tell EditorClubs what you're making — the AI writes tailored, ready-to-use prompts for your editing tool of choice.</p>
    </div>

    <div class="gen-panel reveal">
      <div class="gen-left">
        <label>What are you creating?</label>
        <textarea id="genInput" placeholder="e.g. A moody travel vlog shot in Kyoto at night, I want a cinematic teal-and-orange grade and smooth handheld stabilization…"></textarea>

        <label>Category</label>
        <div class="select-row" id="genCategoryRow"></div>

        <label>Platform (optional)</label>
        <div class="select-row" id="genPlatformRow"></div>

        <button class="generate-btn" id="genBtn">
          <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z"/></svg>
          <span id="genBtnLabel">Generate prompts</span>
        </button>
      </div>
      <div class="gen-right" id="genOutput">
        <div class="gen-empty">
          <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#7c7b8a" stroke-width="1.5"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z"/></svg>
          Your tailored prompts will appear here — three variations, ready to copy.
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ============ TOOLS ============ -->
<section id="tools">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Editing tools</div>
      <h2>Fast, free, browser-based tools.</h2>
      <p>No installs. Upload, process, and export — everything an editor reaches for daily, in one toolbar.</p>
    </div>
    <div class="tool-grid" id="toolGrid"></div>
  </div>
</section>

<!-- ============ APPS ============ -->
<section id="apps">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Editing apps</div>
      <h2>The professional software behind every edit.</h2>
      <p>Compare features and ratings, then jump straight to the official download.</p>
    </div>
    <div class="app-grid" id="appGrid"></div>
  </div>
</section>

<!-- ============ DASHBOARD ============ -->
<section id="dashboard">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Your dashboard</div>
      <h2>Everything saved, organized, at a glance.</h2>
      <p>Bookmarked prompts, favorite tools, and generation history — all in one clean workspace.</p>
    </div>
    <div class="dash-wrap reveal">
      <div class="dash-inner">
        <div class="dash-side">
          <div class="d-item active"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="7" rx="1.5"/><rect x="14" y="3" width="7" height="7" rx="1.5"/><rect x="3" y="14" width="7" height="7" rx="1.5"/><rect x="14" y="14" width="7" height="7" rx="1.5"/></svg> Overview</div>
          <div class="d-item"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 21l-7-5-7 5V5a2 2 0 012-2h10a2 2 0 012 2z"/></svg> Bookmarks</div>
          <div class="d-item"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z"/></svg> Favorites</div>
          <div class="d-item"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z" opacity="0"/><path d="M4 4h16v16H4z" opacity="0"/><path d="M21 12a9 9 0 11-9-9"/><path d="M21 3v6h-6"/></svg> Generations</div>
          <div class="d-item"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 4-6 8-6s8 2 8 6"/></svg> Profile</div>
          <div class="d-item"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.7 1.7 0 00.3 1.9l.1.1a2 2 0 11-2.8 2.8l-.1-.1a1.7 1.7 0 00-1.9-.3 1.7 1.7 0 00-1 1.5V21a2 2 0 11-4 0v-.1a1.7 1.7 0 00-1-1.6 1.7 1.7 0 00-1.9.3l-.1.1a2 2 0 11-2.8-2.8l.1-.1a1.7 1.7 0 00.3-1.9 1.7 1.7 0 00-1.5-1H3a2 2 0 110-4h.1a1.7 1.7 0 001.5-1 1.7 1.7 0 00-.3-1.9l-.1-.1a2 2 0 112.8-2.8l.1.1a1.7 1.7 0 001.9.3H9a1.7 1.7 0 001-1.5V3a2 2 0 114 0v.1a1.7 1.7 0 001 1.5 1.7 1.7 0 001.9-.3l.1-.1a2 2 0 112.8 2.8l-.1.1a1.7 1.7 0 00-.3 1.9V9a1.7 1.7 0 001.5 1h.1a2 2 0 110 4h-.1a1.7 1.7 0 00-1.5 1z"/></svg> Settings</div>
        </div>
        <div class="dash-main">
          <h3>Welcome back, Maya</h3>
          <p class="sub">Here's what's happening in your studio this week.</p>
          <div class="kpi-row">
            <div class="kpi"><b>128</b><span>Prompts used</span></div>
            <div class="kpi"><b>34</b><span>Bookmarks saved</span></div>
            <div class="kpi"><b>19</b><span>AI generations this week</span></div>
          </div>
          <div class="mini-row">
            <div class="mini-card"><span class="tag">CINEMATIC</span><span class="txt">Teal &amp; orange grade — cinema LUT</span></div>
            <div class="mini-card"><span class="tag">CAPTIONS</span><span class="txt">Bold kinetic subtitle pack</span></div>
            <div class="mini-card"><span class="tag">THUMBNAIL</span><span class="txt">High-contrast YouTube thumbnail</span></div>
            <div class="mini-card"><span class="tag">REELS</span><span class="txt">3-second hook structure</span></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ============ CTA ============ -->
<section>
  <div class="container">
    <div class="cta-band reveal">
      <h2>Start editing smarter today.</h2>
      <p>Join thousands of creators using EditorClubs to plan, prompt, and publish faster.</p>
      <div class="cta-actions">
        <a href="#generator" class="btn btn-primary">Get started free</a>
        <a href="#library" class="btn btn-ghost">Explore the library</a>
      </div>
    </div>
  </div>
</section>

<!-- ============ FOOTER ============ -->
<footer>
  <div class="container">
    <div class="foot-grid">
      <div class="foot-brand">
        <div class="logo-mark">
          <svg width="30" height="30" viewBox="0 0 48 48" fill="none">
            <circle cx="24" cy="24" r="22" stroke="url(#logoGrad)" stroke-width="1.4" opacity="0.5"/>
            <g>
              <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.95"/>
              <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.85" transform="rotate(60 24 24)"/>
              <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.75" transform="rotate(120 24 24)"/>
              <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.95" transform="rotate(180 24 24)"/>
              <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.85" transform="rotate(240 24 24)"/>
              <path d="M24 6 L30 20 L24 24 Z" fill="url(#logoGrad)" opacity="0.75" transform="rotate(300 24 24)"/>
            </g>
            <circle cx="24" cy="24" r="5.5" fill="#0a0d16" stroke="url(#logoGrad)" stroke-width="1.2"/>
          </svg>
          <div class="logo-word">Editor<span>Clubs</span></div>
        </div>
        <p>The AI studio for video and photo editors — prompts, tools, and apps in one luxurious workspace.</p>
      </div>
      <div class="foot-col">
        <h5>Product</h5>
        <a href="#library">Prompt Library</a>
        <a href="#generator">AI Generator</a>
        <a href="#tools">Tools</a>
        <a href="#apps">Apps</a>
      </div>
      <div class="foot-col">
        <h5>Company</h5>
        <a href="#">About</a>
        <a href="#">Careers</a>
        <a href="#">Blog</a>
        <a href="#">Contact</a>
      </div>
      <div class="foot-col">
        <h5>Resources</h5>
        <a href="#">Help center</a>
        <a href="#">Community</a>
        <a href="#">Changelog</a>
        <a href="#">Status</a>
      </div>
    </div>
    <div class="foot-bottom">
      <span>© 2026 EditorClubs. All rights reserved.</span>
      <div class="foot-social">
        <a href="#" aria-label="X"><svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M18.9 2H22l-7.6 8.7L23 22h-6.9l-5.4-6.8L4.4 22H1.3l8.2-9.3L1 2h7.1l4.9 6.2L18.9 2z"/></svg></a>
        <a href="#" aria-label="Instagram"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1"/></svg></a>
        <a href="#" aria-label="YouTube"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="5" width="20" height="14" rx="4"/><path d="M10 9l5 3-5 3z" fill="currentColor" stroke="none"/></svg></a>
      </div>
    </div>
  </div>
</footer>

<div class="toast" id="toast"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg><span id="toastMsg">Copied to clipboard</span></div>

<script>
lucide.createIcons();

/* ============ HEADER SCROLL ============ */
const header = document.getElementById('siteHeader');
window.addEventListener('scroll', () => {
  header.classList.toggle('scrolled', window.scrollY > 20);
});

/* ============ REVEAL ON SCROLL ============ */
const revealEls = document.querySelectorAll('.reveal');
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('in'); });
}, { threshold: 0.15 });
revealEls.forEach(el => io.observe(el));

/* ============ PARALLAX FLOAT CARDS ============ */
document.addEventListener('mousemove', (e) => {
  const x = (e.clientX / window.innerWidth - 0.5);
  const y = (e.clientY / window.innerHeight - 0.5);
  document.querySelectorAll('.fcard').forEach((c, i) => {
    const depth = (i + 1) * 6;
    c.style.transform = `translate(${x * depth}px, ${y * depth}px)`;
  });
  const orb = document.querySelector('.orb');
  if (orb) orb.style.transform = `translate(calc(-50% + ${x * -16}px), calc(-50% + ${y * -16}px))`;
});

/* ============ TOAST ============ */
function showToast(msg){
  const t = document.getElementById('toast');
  document.getElementById('toastMsg').textContent = msg;
  t.classList.add('show');
  clearTimeout(window._toastTimer);
  window._toastTimer = setTimeout(()=> t.classList.remove('show'), 2200);
}

/* ============ COUNT UP STATS ============ */
function countUp(el, target, suffix=''){
  let cur = 0; const step = Math.max(1, Math.round(target/60));
  const t = setInterval(()=>{
    cur += step;
    if(cur >= target){ cur = target; clearInterval(t); }
    el.textContent = cur.toLocaleString() + suffix;
  }, 22);
}
const statSection = document.querySelector('.hero-stats');
const statIO = new IntersectionObserver((entries)=>{
  entries.forEach(e=>{
    if(e.isIntersecting){
      countUp(document.getElementById('statPrompts'), 480, '+');
      countUp(document.getElementById('statTools'), 10);
      countUp(document.getElementById('statApps'), 11);
      countUp(document.getElementById('statEditors'), 52000, '+');
      statIO.disconnect();
    }
  });
}, {threshold:0.4});
statIO.observe(statSection);

/* ==========================================================
   PROMPT LIBRARY DATA
========================================================== */
const CATEGORIES = ["All","Video Editing","Photo Editing","Color Grading","Cinematic Effects","Thumbnails","Captions","Storytelling","Motion Graphics","YouTube","TikTok","Instagram Reels","AI Image Generation","AI Video Generation"];

const PROMPTS = [
  {cat:"Color Grading", title:"Moody Teal & Orange Cinema Grade", text:"Grade this footage with a cinematic teal-and-orange split tone: cool shadows near #0e2a30, warm skin tones lifted toward #d99a6c, filmic S-curve contrast, soft highlight rolloff, and fine 35mm grain at 8% opacity.", trending:true},
  {cat:"Video Editing", title:"Fast-Paced Vlog Cutdown", text:"Cut this raw vlog footage into a punchy 90-second edit: jump cuts every 1.5-3s, remove filler words, add whip-pan transitions on action beats, and sync cuts to the beat of the background track.", trending:false},
  {cat:"Cinematic Effects", title:"Anamorphic Lens Flare Overlay", text:"Add subtle blue anamorphic lens flares that trigger on bright highlights, horizontal streak length 60% of frame width, opacity 25%, blended in Screen mode, only on outdoor daylight shots.", trending:true},
  {cat:"Thumbnails", title:"High-CTR YouTube Thumbnail", text:"Design a YouTube thumbnail: subject on the right third with rim light, bold 3-4 word headline in condensed sans, high saturation background, red-to-purple gradient accent, and a subtle drop shadow for depth.", trending:true},
  {cat:"Captions", title:"Bold Kinetic Subtitle Pack", text:"Generate animated captions: one to three words per line, scale-in pop on emphasis words, soft purple highlight box behind keywords, bottom-third safe position, 0.3s spring easing per word.", trending:false},
  {cat:"Storytelling", title:"Three-Act Micro Documentary Structure", text:"Structure this interview footage into a three-act mini-doc: cold open with the emotional peak (0-8s), context and stakes (act one), rising complication (act two), resolution with a callback line (act three).", trending:false},
  {cat:"Motion Graphics", title:"Kinetic Type Title Reveal", text:"Animate a title card: letters slide up from a 20px offset with staggered 40ms delay, spring easing (mass 1, stiffness 220), soft motion blur on entry, dusty pink to steel blue gradient fill.", trending:true},
  {cat:"YouTube", title:"Retention-Optimized Intro Hook", text:"Write and structure a 10-second YouTube intro hook: state the payoff in the first sentence, show a quick preview cut of the best moment, then cut to title card before the 8-second mark.", trending:false},
  {cat:"TikTok", title:"Pattern-Interrupt Opening Frame", text:"Design a TikTok opening frame that interrupts the scroll: extreme close-up, unexpected motion in the first 0.5s, on-screen text posing a question, native-feeling handheld camera shake.", trending:true},
  {cat:"Instagram Reels", title:"Aesthetic Transition Sequence", text:"Create a seamless Reels transition: match-cut on movement between two clips, whip-blur bridging frame at 6 frames, consistent color temperature across cuts, synced to a snare hit.", trending:false},
  {cat:"AI Image Generation", title:"Editorial Studio Portrait", text:"Studio portrait, Rembrandt lighting with a 45-degree key light, dusty pink seamless backdrop, soft steel-blue rim light, medium format lens look, shallow depth of field at f/2, editorial fashion tone.", trending:true},
  {cat:"AI Video Generation", title:"Cinematic Product Reveal", text:"Generate a cinematic product reveal: slow dolly-in on a dark charcoal set, single soft purple accent light sweeping across the product, subtle atmospheric haze, 24fps motion, shallow depth of field.", trending:true},
  {cat:"Photo Editing", title:"Dusty Pastel Portrait Retouch", text:"Retouch this portrait with a dusty pastel finish: soften skin while preserving texture, lift shadows toward muted mauve, desaturate background by 20%, add gentle vignette, warm highlight glow on skin.", trending:false},
  {cat:"Color Grading", title:"Desaturated Editorial Fashion Look", text:"Apply a desaturated editorial grade: overall saturation -18%, lifted blacks toward charcoal grey, cool highlights, warm midtones on skin only, subtle halation on light sources.", trending:false},
  {cat:"Video Editing", title:"Silent Gaps & Dead Air Removal", text:"Automatically detect and remove silences longer than 0.6 seconds, tighten pauses between sentences to 0.2 seconds, preserve natural breathing room around emphasis words.", trending:false},
  {cat:"Cinematic Effects", title:"Handheld Documentary Camera Shake", text:"Add subtle handheld camera movement: low-frequency drift at 0.4Hz, micro-jitter at 8Hz with 2px amplitude, slight rotational sway, keep the horizon within a 3-degree tolerance.", trending:false},
  {cat:"Thumbnails", title:"Split-Screen Comparison Thumbnail", text:"Design a before/after split-screen thumbnail with a diagonal divider, contrasting color grade on each half, bold arrow graphic pointing right, condensed bold headline across the top third.", trending:false},
  {cat:"Captions", title:"Minimal Karaoke-Style Subtitles", text:"Generate karaoke-style captions: full sentence visible, active word highlighted in soft purple as spoken, clean sans-serif font, centered lower third, no background box, subtle fade in/out.", trending:false},
  {cat:"Storytelling", title:"Emotional Hook + Payoff Framework", text:"Reorder this footage using a hook-and-payoff framework: open on the emotional payoff moment, cut to 'three days earlier' context card, build tension through obstacles, return to the payoff with new context.", trending:true},
  {cat:"Motion Graphics", title:"Glassmorphic Lower Third", text:"Build a glassmorphic lower third: frosted glass panel at 12% opacity with 20px blur, thin gradient border from dusty pink to purple, name slides in from left, title fades in 100ms later.", trending:false},
  {cat:"YouTube", title:"Chapter-Based Edit Structure", text:"Structure this long-form video into clear chapters: title card per chapter with number and label, consistent transition style between chapters, recap card every third chapter.", trending:false},
  {cat:"TikTok", title:"Text-Overlay Storytelling Beats", text:"Add sequential text overlays that reveal a story beat every 2-3 seconds, bold rounded font, high contrast white text with soft shadow, synced entrance to cut points.", trending:false},
  {cat:"Instagram Reels", title:"Cinematic Travel Montage", text:"Cut this travel footage into a cinematic 30-second montage: establishing wide shot first, alternate wide and close-up every 2 clips, consistent warm color grade, synced cuts to musical downbeats.", trending:true},
  {cat:"AI Image Generation", title:"Moody Cinematic Landscape", text:"Wide cinematic landscape, golden hour side light, atmospheric fog in the valley, deep navy sky transitioning to dusty pink horizon, anamorphic aspect ratio, subtle film grain.", trending:false},
  {cat:"AI Video Generation", title:"Abstract Motion Background Loop", text:"Generate a seamless looping abstract background: slow-moving soft purple and steel blue gradient waves, subtle grain texture, gentle bloom on highlights, 10-second loop, no hard cuts.", trending:false},
];

let activeCat = "All";
let visibleCount = 9;
let favorites = new Set();
let bookmarks = new Set();

function renderChips(){
  const row = document.getElementById('chipRow');
  row.innerHTML = CATEGORIES.map(c => `<button class="chip ${c===activeCat?'active':''}" data-cat="${c}">${c}</button>`).join('');
  row.querySelectorAll('.chip').forEach(chip=>{
    chip.addEventListener('click', ()=>{
      activeCat = chip.dataset.cat; visibleCount = 9;
      renderChips(); renderPrompts();
    });
  });
}

function filteredPrompts(){
  const q = document.getElementById('libSearch').value.trim().toLowerCase();
  return PROMPTS.filter(p=>{
    const matchesCat = activeCat === "All" || p.cat === activeCat;
    const matchesQ = !q || p.title.toLowerCase().includes(q) || p.text.toLowerCase().includes(q) || p.cat.toLowerCase().includes(q);
    return matchesCat && matchesQ;
  });
}

function renderPrompts(){
  const grid = document.getElementById('promptGrid');
  const list = filteredPrompts().slice(0, visibleCount);
  if(list.length === 0){
    grid.innerHTML = `<div style="grid-column:1/-1; text-align:center; padding:50px 0; color:var(--text-low); font-size:14px;">No prompts match your search yet.</div>`;
    document.getElementById('loadMoreBtn').style.display = 'none';
    return;
  }
  grid.innerHTML = list.map((p, idx)=>{
    const realIdx = PROMPTS.indexOf(p);
    const isFav = favorites.has(realIdx);
    const isBook = bookmarks.has(realIdx);
    return `
    <div class="pcard glass reveal in">
      <div class="pcard-top">
        <div class="pcard-cat"><svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 4h16v16H4z" opacity="0"/><circle cx="12" cy="12" r="3"/></svg>${p.cat}</div>
        <div class="pcard-icons">
          <button class="icon-btn ${isFav?'active':''}" data-fav="${realIdx}" aria-label="Favorite" title="Favorite">
            <svg viewBox="0 0 24 24" fill="${isFav?'currentColor':'none'}" stroke="currentColor" stroke-width="2"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z"/></svg>
          </button>
          <button class="icon-btn ${isBook?'active':''}" data-bookmark="${realIdx}" aria-label="Bookmark" title="Bookmark">
            <svg viewBox="0 0 24 24" fill="${isBook?'currentColor':'none'}" stroke="currentColor" stroke-width="2"><path d="M19 21l-7-5-7 5V5a2 2 0 012-2h10a2 2 0 012 2z"/></svg>
          </button>
        </div>
      </div>
      <h4>${p.title}</h4>
      <p class="ptext">${p.text}</p>
      <div class="pcard-foot">
        <div class="trend-badge">${p.trending? '<svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke=\'currentColor\' stroke-width="2"><path d="M23 6l-9.5 9.5-5-5L1 18"/><path d="M17 6h6v6"/></svg> Trending' : '&nbsp;'}</div>
        <button class="copy-btn" data-copy="${realIdx}">
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"/></svg>
          Copy
        </button>
      </div>
    </div>`;
  }).join('');

  grid.querySelectorAll('[data-copy]').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      const idx = btn.dataset.copy;
      navigator.clipboard.writeText(PROMPTS[idx].text).then(()=>{
        btn.classList.add('copied');
        btn.innerHTML = `<svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg> Copied`;
        showToast('Prompt copied to clipboard');
        setTimeout(()=>{ renderPrompts(); }, 1600);
      });
    });
  });
  grid.querySelectorAll('[data-fav]').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      const idx = parseInt(btn.dataset.fav);
      favorites.has(idx) ? favorites.delete(idx) : favorites.add(idx);
      renderPrompts();
    });
  });
  grid.querySelectorAll('[data-bookmark]').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      const idx = parseInt(btn.dataset.bookmark);
      bookmarks.has(idx) ? bookmarks.delete(idx) : bookmarks.add(idx);
      showToast(bookmarks.has(idx) ? 'Saved to bookmarks' : 'Removed from bookmarks');
      renderPrompts();
    });
  });

  document.getElementById('loadMoreBtn').style.display = filteredPrompts().length > visibleCount ? 'inline-flex' : 'none';
}

document.getElementById('libSearch').addEventListener('input', ()=>{ visibleCount = 9; renderPrompts(); });
document.getElementById('loadMoreBtn').addEventListener('click', ()=>{ visibleCount += 9; renderPrompts(); });
document.getElementById('heroSearchBtn').addEventListener('click', ()=>{
  const v = document.getElementById('heroSearch').value;
  document.getElementById('libSearch').value = v;
  document.getElementById('library').scrollIntoView({behavior:'smooth'});
  visibleCount = 9; renderPrompts();
});

renderChips();
renderPrompts();

/* ==========================================================
   AI PROMPT GENERATOR
========================================================== */
const GEN_CATS = ["Video Editing","Photo Editing","Color Grading","Cinematic Effects","Thumbnails","Captions","Motion Graphics","AI Image","AI Video"];
const GEN_PLATFORMS = ["None","YouTube","TikTok","Instagram Reels"];
let genCat = "Video Editing";
let genPlatform = "None";

function renderGenSelectors(){
  document.getElementById('genCategoryRow').innerHTML = GEN_CATS.map(c=>`<button class="pill-select ${c===genCat?'active':''}" data-gc="${c}">${c}</button>`).join('');
  document.getElementById('genPlatformRow').innerHTML = GEN_PLATFORMS.map(c=>`<button class="pill-select ${c===genPlatform?'active':''}" data-gp="${c}">${c}</button>`).join('');
  document.querySelectorAll('[data-gc]').forEach(b=>b.addEventListener('click', ()=>{ genCat = b.dataset.gc; renderGenSelectors(); }));
  document.querySelectorAll('[data-gp]').forEach(b=>b.addEventListener('click', ()=>{ genPlatform = b.dataset.gp; renderGenSelectors(); }));
}
renderGenSelectors();

async function generatePrompts(){
  const desc = document.getElementById('genInput').value.trim();
  const output = document.getElementById('genOutput');
  const btn = document.getElementById('genBtn');
  const label = document.getElementById('genBtnLabel');

  if(!desc){
    showToast('Describe your project first');
    return;
  }

  btn.disabled = true;
  label.textContent = 'Generating…';
  btn.insertAdjacentHTML('afterbegin', '');
  output.innerHTML = `<div class="gen-empty"><div class="spinner" style="margin:0 auto 16px; border-top-color:#c99da3; border-color:rgba(201,157,163,0.25);"></div>Writing three tailored prompts…</div>`;

  const platformNote = genPlatform !== "None" ? ` optimized for ${genPlatform}` : "";
  const sys = `You are an expert prompt writer for video and photo editors. Given a project description and an editing category, write exactly 3 distinct, highly specific, ready-to-use editing prompts${platformNote}. Each prompt should be usable directly inside an editing tool or AI generation tool. Respond ONLY with valid JSON, no markdown, no preamble, in this exact shape: {"prompts":[{"label":"short 3-5 word label","text":"the full prompt, 2-4 sentences, concrete and technical"},{"label":"...","text":"..."},{"label":"...","text":"..."}]}`;
  const userMsg = `Category: ${genCat}\nProject description: ${desc}`;

  try{
    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 1000,
        system: sys,
        messages: [{ role: "user", content: userMsg }]
      })
    });
    const data = await response.json();
    const raw = (data.content || []).map(b => b.text || "").join("\n");
    const clean = raw.replace(/```json|```/g, "").trim();
    const parsed = JSON.parse(clean);

    output.innerHTML = parsed.prompts.map((p,i)=>`
      <div class="result-card" style="animation-delay:${i*0.08}s">
        <h5>${p.label.toUpperCase()}</h5>
        <p>${p.text}</p>
        <div style="display:flex; justify-content:flex-end; margin-top:12px;">
          <button class="copy-btn" data-gencopy="${i}">
            <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"/></svg>
            Copy
          </button>
        </div>
      </div>`).join('');

    output.querySelectorAll('[data-gencopy]').forEach(b=>{
      b.addEventListener('click', ()=>{
        navigator.clipboard.writeText(parsed.prompts[b.dataset.gencopy].text);
        showToast('Prompt copied to clipboard');
      });
    });
  }catch(err){
    output.innerHTML = `<div class="gen-empty">Something went wrong generating your prompts. Please try again.</div>`;
  }finally{
    btn.disabled = false;
    label.textContent = 'Generate prompts';
  }
}
document.getElementById('genBtn').addEventListener('click', generatePrompts);

/* ==========================================================
   TOOLS
========================================================== */
const TOOLS = [
  {name:"Image Upscaler", desc:"Upscale photos up to 4x with AI-restored detail.", icon:"maximize-2"},
  {name:"Background Remover", desc:"Instantly cut out subjects with clean edges.", icon:"scissors"},
  {name:"Image Compressor", desc:"Shrink file size while keeping visual quality.", icon:"shrink"},
  {name:"Video Compressor", desc:"Reduce video size for fast, clean uploads.", icon:"film"},
  {name:"AI Subtitle Generator", desc:"Auto-transcribe and caption in seconds.", icon:"captions"},
  {name:"Color Palette Generator", desc:"Extract grading palettes from any image.", icon:"palette"},
  {name:"Aspect Ratio Converter", desc:"Reframe footage for any platform instantly.", icon:"crop"},
  {name:"Video Trimmer", desc:"Cut and trim clips with frame precision.", icon:"scissors-line-dashed"},
  {name:"Audio Remover", desc:"Strip audio tracks cleanly from any video.", icon:"volume-x"},
  {name:"File Converter", desc:"Convert between every major media format.", icon:"repeat"},
];
document.getElementById('toolGrid').innerHTML = TOOLS.map(t=>`
  <div class="tool-card glass reveal in">
    <div class="tool-icon"><i data-lucide="${t.icon}"></i></div>
    <h4>${t.name}</h4>
    <p>${t.desc}</p>
    <span class="go">Open tool <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M7 17L17 7M7 7h10v10"/></svg></span>
  </div>`).join('');

/* ==========================================================
   APPS
========================================================== */
const APPS = [
  {name:"Adobe Premiere Pro", initials:"Pr", grad:["#9b59d0","#5c6bc0"], desc:"Industry-standard timeline editor for professional video production.", features:["Multicam","Color","Auto Reframe"], rating:"4.6", url:"https://www.adobe.com/products/premiere.html"},
  {name:"Adobe After Effects", initials:"Ae", grad:["#8a5cf0","#4b3a9c"], desc:"Motion graphics and visual effects compositing powerhouse.", features:["Compositing","Expressions","3D"], rating:"4.6", url:"https://www.adobe.com/products/aftereffects.html"},
  {name:"Adobe Photoshop", initials:"Ps", grad:["#3fa9f5","#1e5fa8"], desc:"The definitive tool for photo editing and compositing.", features:["Layers","Generative Fill","Retouch"], rating:"4.7", url:"https://www.adobe.com/products/photoshop.html"},
  {name:"Adobe Lightroom", initials:"Lr", grad:["#5eb0e8","#2f6fb0"], desc:"Fast, non-destructive photo organizing and color grading.", features:["Presets","Masking","Cloud Sync"], rating:"4.6", url:"https://www.adobe.com/products/photoshop-lightroom.html"},
  {name:"DaVinci Resolve", initials:"DR", grad:["#c99da3","#6b8cae"], desc:"Pro-grade editing, color, VFX, and audio in one suite.", features:["Color Wheels","Fusion VFX","Fairlight Audio"], rating:"4.7", url:"https://www.blackmagicdesign.com/products/davinciresolve"},
  {name:"CapCut", initials:"Cc", grad:["#3fd2c7","#2a9d8f"], desc:"Mobile-first editor built for fast social content.", features:["Templates","Auto Captions","Effects"], rating:"4.7", url:"https://www.capcut.com/"},
  {name:"Final Cut Pro", initials:"FC", grad:["#e0607a","#a13a56"], desc:"Magnetic timeline editing optimized for Apple silicon.", features:["Magnetic Timeline","Multicam","HDR"], rating:"4.6", url:"https://www.apple.com/final-cut-pro/"},
  {name:"Canva", initials:"Ca", grad:["#6fd3f0","#5b7fe0"], desc:"Drag-and-drop design for thumbnails, posts, and graphics.", features:["Templates","Brand Kit","Animate"], rating:"4.7", url:"https://www.canva.com/"},
  {name:"Figma", initials:"Fg", grad:["#a259ff","#f24e1e"], desc:"Collaborative design tool for interfaces and prototypes.", features:["Auto Layout","Prototyping","Dev Mode"], rating:"4.7", url:"https://www.figma.com/"},
  {name:"Blender", initials:"Bl", grad:["#e07a3f","#c46a2d"], desc:"Free, open-source 3D creation and motion suite.", features:["3D Modeling","Rendering","Animation"], rating:"4.8", url:"https://www.blender.org/"},
  {name:"VN Editor", initials:"VN", grad:["#7e9bb8","#9b8bc4"], desc:"Clean, free mobile and desktop video editor for creators.", features:["Keyframes","LUTs","Multi-track"], rating:"4.8", url:"https://vn.video/"},
];
document.getElementById('appGrid').innerHTML = APPS.map(a=>`
  <div class="app-card glass reveal in">
    <div class="app-top">
      <div class="app-logo" style="background:linear-gradient(135deg, ${a.grad[0]}, ${a.grad[1]});">${a.initials}</div>
      <div>
        <div class="app-name">${a.name}</div>
        <div class="app-rating"><svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor" stroke="none"><path d="M12 2l2.4 7.2H22l-6 4.4 2.3 7.2L12 16.4 5.7 20.8 8 13.6 2 9.2h7.6z"/></svg> ${a.rating} · User rating</div>
      </div>
    </div>
    <p class="app-desc">${a.desc}</p>
    <div class="app-features">${a.features.map(f=>`<span class="feat-tag">${f}</span>`).join('')}</div>
    <div class="app-foot">
      <a class="dl-link" href="${a.url}" target="_blank" rel="noopener">
        Official site <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M7 17L17 7M7 7h10v10"/></svg>
      </a>
      <button class="icon-btn" aria-label="Bookmark app"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 21l-7-5-7 5V5a2 2 0 012-2h10a2 2 0 012 2z"/></svg></button>
    </div>
  </div>`).join('');

lucide.createIcons();

/* sign in placeholder */
document.getElementById('signInBtn').addEventListener('click', (e)=>{
  e.preventDefault();
  showToast('Sign in coming soon');
});
</script>
</body>
</html>
