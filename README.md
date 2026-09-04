<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Speshelty — Tea & Coffee Bar</title>
<meta name="description" content="Speshelty — українська мережа to-go барів. Власне виробництво, американський стиль, українське ком'юніті.">

<!-- Open Graph -->
<meta property="og:title" content="Speshelty — Tea & Coffee Bar">
<meta property="og:description" content="Власне виробництво. Американський стиль. Українське ком'юніті.">
<meta property="og:type" content="website">

<!-- Fonts -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700;800;900&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">

<!-- Leaflet -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/supabase-js/2.45.4/supabase.min.js"></script>

<style>
/* ============================================================
   SPESHELTY — Tea & Coffee Bar
   Design tokens
   ============================================================ */
:root{
  --black: #0A0A0A;
  --dark: #1A1A1A;
  --dark-2: #141414;
  --yellow: #FFD700;
  --green: #7CB342;
  --pink: #FF4081;
  --orange: #FF6D00;
  --blue: #2979FF;
  --text: #FFFFFF;
  --text-2: #B0B0B0;
  --gradient: linear-gradient(135deg, #FFD700 0%, #FF6D00 50%, #FF4081 100%);
  --gradient-cool: linear-gradient(135deg, #7CB342 0%, #2979FF 100%);
  --glass-bg: rgba(255,255,255,0.05);
  --glass-border: rgba(255,255,255,0.10);
  --radius: 18px;
  --ease: cubic-bezier(.16,.84,.44,1);
}

*{ box-sizing:border-box; margin:0; padding:0; }
html{ scroll-behavior:smooth; }
html,body{ background:var(--black); overflow-x:hidden; }

body{
  font-family:'Inter', -apple-system, sans-serif;
  color:var(--text);
  line-height:1.6;
  font-weight:400;
  min-height:100vh;
}

@media (prefers-reduced-motion: reduce){
  *{ animation-duration:0.01ms !important; animation-iteration-count:1 !important; transition-duration:0.01ms !important; scroll-behavior:auto !important; }
}

h1,h2,h3,h4{
  font-family:'Montserrat', sans-serif;
  font-weight:900;
  text-transform:uppercase;
  letter-spacing:-0.02em;
  line-height:1.05;
}
h2{ font-size:clamp(2rem, 5vw, 3.5rem); }
h3{ font-size:clamp(1.3rem, 3vw, 1.9rem); font-weight:800; }

.eyebrow{
  font-family:'Montserrat', sans-serif;
  font-weight:700;
  text-transform:uppercase;
  letter-spacing:0.25em;
  font-size:0.75rem;
  color:var(--yellow);
  display:inline-block;
  margin-bottom:14px;
}

a{ color:inherit; text-decoration:none; }
img{ max-width:100%; display:block; }
ul{ list-style:none; }
button{ font-family:inherit; cursor:pointer; border:none; background:none; color:inherit; }

.container{
  width:100%;
  max-width:1240px;
  margin:0 auto;
  padding:0 24px;
}

section{ position:relative; padding:110px 0; }
@media (max-width:768px){ section{ padding:70px 0; } }

.section-head{ text-align:center; max-width:640px; margin:0 auto 56px; }
.section-head h2{ margin-bottom:10px; }
.section-head p{ color:var(--text-2); font-size:1.05rem; }

/* ===================== GLASS + BUTTONS ===================== */
.glass{
  background:var(--glass-bg);
  border:1px solid var(--glass-border);
  backdrop-filter:blur(16px);
  -webkit-backdrop-filter:blur(16px);
  border-radius:var(--radius);
}

.btn{
  font-family:'Montserrat', sans-serif;
  font-weight:800;
  text-transform:uppercase;
  letter-spacing:0.04em;
  font-size:0.85rem;
  padding:16px 34px;
  border-radius:100px;
  display:inline-flex;
  align-items:center;
  gap:10px;
  transition:transform .35s var(--ease), box-shadow .35s var(--ease), filter .35s var(--ease);
  white-space:nowrap;
}
.btn-primary{
  background:var(--gradient);
  color:#0A0A0A;
  box-shadow:0 0 0px rgba(255,215,0,0);
}
.btn-primary:hover{
  transform:translateY(-3px) scale(1.03);
  box-shadow:0 8px 30px rgba(255,109,0,0.45), 0 0 40px rgba(255,215,0,0.3);
}
.btn-outline{
  border:1.5px solid rgba(255,255,255,0.5);
  color:#fff;
  background:rgba(255,255,255,0.02);
}
.btn-outline:hover{
  transform:translateY(-3px) scale(1.03);
  border-color:var(--yellow);
  box-shadow:0 0 24px rgba(255,215,0,0.25);
  background:rgba(255,215,0,0.06);
}
.btn-glovo{
  background:#FFC244;
  color:#0A0A0A;
}
.btn-glovo:hover{ transform:translateY(-3px) scale(1.03); box-shadow:0 8px 26px rgba(255,194,68,0.4); }

/* ===================== HEADER / NAV ===================== */
.site-header{
  position:fixed; top:0; left:0; right:0; z-index:900;
  padding:18px 0;
  transition:background .4s var(--ease), padding .4s var(--ease), border-color .4s var(--ease);
  border-bottom:1px solid transparent;
}
.site-header.scrolled{
  background:rgba(10,10,10,0.85);
  backdrop-filter:blur(14px);
  padding:12px 0;
  border-bottom:1px solid var(--glass-border);
}
.site-header .container{ display:flex; align-items:center; justify-content:space-between; gap:20px; }

.logo{
  font-family:'Montserrat', sans-serif;
  font-weight:900;
  font-size:1.4rem;
  letter-spacing:-0.02em;
  text-transform:uppercase;
  display:flex;
  align-items:center;
  gap:10px;
}
.logo .dot{
  width:9px; height:9px; border-radius:50%;
  background:var(--gradient);
  box-shadow:0 0 10px var(--yellow);
}

.nav-links{
  display:flex; align-items:center; gap:34px;
  font-family:'Montserrat', sans-serif;
  font-weight:700;
  font-size:0.82rem;
  text-transform:uppercase;
  letter-spacing:0.05em;
}
.nav-links a{
  position:relative; padding:6px 0; color:var(--text-2);
  transition:color .25s var(--ease);
}
.nav-links a::after{
  content:''; position:absolute; left:0; bottom:0; width:0; height:2px;
  background:var(--gradient); transition:width .3s var(--ease);
}
.nav-links a:hover, .nav-links a.active{ color:#fff; }
.nav-links a:hover::after, .nav-links a.active::after{ width:100%; }

.header-actions{ display:flex; align-items:center; gap:16px; }

.lang-switch{
  display:flex; align-items:center; gap:2px;
  font-family:'Montserrat', sans-serif;
  font-weight:800; font-size:0.75rem;
  border:1px solid var(--glass-border);
  border-radius:100px; padding:4px; background:var(--glass-bg);
}
.lang-switch button{
  padding:6px 12px; border-radius:100px; color:var(--text-2);
  transition:all .25s var(--ease);
}
.lang-switch button.active{ background:var(--gradient); color:#0A0A0A; }

.burger{
  display:none; flex-direction:column; gap:5px; width:28px; z-index:1100;
}
.burger span{ display:block; height:2px; width:100%; background:#fff; transition:all .35s var(--ease); }
.burger.open span:nth-child(1){ transform:translateY(7px) rotate(45deg); }
.burger.open span:nth-child(2){ opacity:0; }
.burger.open span:nth-child(3){ transform:translateY(-7px) rotate(-45deg); }

@media (max-width:960px){
  .nav-links{
    position:fixed; inset:0 0 0 auto; width:78%; max-width:360px;
    background:rgba(10,10,10,0.98); backdrop-filter:blur(20px);
    flex-direction:column; justify-content:center; align-items:flex-start;
    gap:26px; padding:40px; font-size:1.15rem;
    transform:translateX(100%); transition:transform .45s var(--ease);
    border-left:1px solid var(--glass-border);
  }
  .nav-links.open{ transform:translateX(0); }
  .burger{ display:flex; }
}

/* ===================== HERO ===================== */
.hero{
  min-height:100svh;
  display:flex; align-items:center;
  position:relative;
  padding-top:120px;
  overflow:hidden;
  background:
    radial-gradient(ellipse 60% 50% at 15% 15%, rgba(255,215,0,0.16), transparent 60%),
    radial-gradient(ellipse 55% 55% at 90% 25%, rgba(255,64,129,0.16), transparent 60%),
    radial-gradient(ellipse 60% 60% at 50% 100%, rgba(255,109,0,0.14), transparent 60%),
    var(--black);
}
.hero::before{
  content:''; position:absolute; inset:0;
  background-image:
    linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
  background-size:46px 46px;
  mask-image:radial-gradient(ellipse 80% 60% at 50% 30%, black, transparent 75%);
  pointer-events:none;
}
.hero-visual{
  position:absolute; right:-2%; bottom:-8%; width:min(42vw, 620px); height:min(58vw, 760px); z-index:1; pointer-events:none;
  opacity:0; animation:fadeUp .9s var(--ease) .2s forwards;
}
.hero-visual::before{
  content:''; position:absolute; left:10%; right:7%; bottom:4%; height:34%; border-radius:50%;
  background:radial-gradient(ellipse at center, rgba(255,180,70,0.26), rgba(255,109,0,0.1) 30%, rgba(0,0,0,0) 70%);
  filter:blur(24px);
}
.hero-bunny{
  position:absolute; inset:0; transform-style:preserve-3d; transition:transform .17s linear;
  filter:drop-shadow(0 34px 44px rgba(0,0,0,0.7));
}
.hero-bunny .hero-bunny-image{
  position:absolute; inset:0; width:100%; height:100%; object-fit:contain; object-position:center bottom;
  filter:drop-shadow(0 28px 38px rgba(0,0,0,0.7));
}
.hero-bunny::before{
  content:''; position:absolute; left:17%; right:17%; top:8%; bottom:12%; border-radius:45% 45% 40% 40%;
  background:radial-gradient(ellipse at center, rgba(255,134,0,0.15), transparent 62%);
  filter:blur(32px);
  z-index:-2;
}
.hero-bunny .bunny-body{
  position:absolute; left:16%; right:16%; bottom:0; height:47%; border-radius:52% 52% 40% 40% / 58% 58% 26% 26%;
  background:
    radial-gradient(circle at 30% 18%, rgba(255,255,255,0.15), transparent 18%),
    radial-gradient(circle at 66% 28%, rgba(255,255,255,0.08), transparent 22%),
    radial-gradient(circle at 52% 60%, rgba(0,0,0,0.46), transparent 34%),
    linear-gradient(135deg, #222 0%, #0b0b0b 28%, #1a1a1a 100%);
  box-shadow:inset -28px -18px 30px rgba(0,0,0,0.82), inset 18px 18px 26px rgba(255,255,255,0.03), 0 14px 28px rgba(0,0,0,0.42);
  overflow:hidden;
}
.hero-bunny .bunny-body::before,
.hero-bunny .bunny-body::after{
  content:''; position:absolute; inset:auto auto 12% 12%; width:23%; height:17%; border-radius:50%;
  background:radial-gradient(circle, rgba(255,255,255,0.07), transparent 70%); filter:blur(10px);
}
.hero-bunny .bunny-body::after{ left:auto; right:14%; }
.hero-bunny .bunny-body::before{
  background-image:repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 2px, rgba(0,0,0,0.03) 2px 4px);
  opacity:0.22;
}
.hero-bunny .bunny-head{
  position:absolute; left:17%; right:17%; top:12%; height:46%; border-radius:56% 56% 48% 48% / 68% 68% 32% 32%;
  background:
    radial-gradient(circle at 28% 18%, rgba(255,255,255,0.18), transparent 16%),
    radial-gradient(circle at 60% 68%, rgba(0,0,0,0.34), transparent 26%),
    repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 2px, rgba(0,0,0,0.04) 2px 4px),
    linear-gradient(135deg, #1d1d1d 0%, #090909 28%, #1c1c1c 100%);
  box-shadow:inset -18px -12px 26px rgba(0,0,0,0.78), inset 18px 20px 22px rgba(255,255,255,0.05), 0 16px 34px rgba(0,0,0,0.38);
  transform:translateZ(26px);
  overflow:hidden;
}
.hero-bunny .bunny-head::before{
  content:''; position:absolute; inset:9% 11% 8%; border-radius:50%;
  background:radial-gradient(circle at 50% 25%, rgba(255,255,255,0.12), transparent 36%), repeating-radial-gradient(circle at 50% 35%, rgba(255,255,255,0.025) 0 2px, rgba(0,0,0,0.05) 2px 4px);
  opacity:1;
}
.hero-bunny .bunny-ear{
  position:absolute; top:0; width:29%; height:43%; border-radius:48% 48% 32% 32%;
  background:
    radial-gradient(circle at 40% 28%, rgba(255,255,255,0.14), transparent 12%),
    linear-gradient(135deg, #1a1a1a 0%, #070707 50%, #1b1b1b 100%);
  box-shadow:inset -10px -8px 18px rgba(0,0,0,0.72), inset 8px 10px 16px rgba(255,255,255,0.04);
  overflow:hidden;
}
.hero-bunny .bunny-ear.left{ left:11%; transform:translateZ(18px) rotate(-18deg); }
.hero-bunny .bunny-ear.right{ right:11%; transform:translateZ(18px) rotate(18deg); }
.hero-bunny .bunny-ear::before{
  content:''; position:absolute; inset:14% 27% 12%; border-radius:50%;
  background:linear-gradient(180deg, rgba(255,255,255,0.08), rgba(0,0,0,0.2));
}
.hero-bunny .bunny-face{
  position:absolute; inset:14% 8% 12%; border-radius:46% 46% 42% 42%;
}
.hero-bunny .bunny-eye{
  position:absolute; top:40%; width:21%; height:26%; border-radius:48% 52% 46% 54%;
  background:radial-gradient(circle at 50% 50%, #fff 0 13%, #fcd9cc 13% 20%, #2d0707 21%, #140606 58%, #000 100%);
  box-shadow:inset 0 0 0 3px rgba(255,255,255,0.04), 0 0 0 4px rgba(255,120,0,0.1), 0 0 24px rgba(255,109,0,0.64);
  overflow:hidden;
}
.hero-bunny .bunny-eye.left{ left:17%; }
.hero-bunny .bunny-eye.right{ right:17%; }
.hero-bunny .bunny-eye::before{
  content:''; position:absolute; left:8%; right:8%; top:10%; bottom:10%; border-radius:50%;
  border:2px solid rgba(255,255,255,0.08);
  opacity:0.8;
}
.hero-bunny .bunny-eye .pupil{
  position:absolute; left:50%; top:50%; width:54%; height:54%; border-radius:50%;
  background:radial-gradient(circle at 35% 35%, #ffce97 0%, #ff6c00 20%, #692000 42%, #150303 100%);
  transform:translate(-50%, -50%) translate(var(--look-x, 0px), var(--look-y, 0px));
  box-shadow:0 0 22px rgba(255,109,0,0.7), inset 0 0 12px rgba(0,0,0,0.75);
}
.hero-bunny .bunny-eye .pupil::after{
  content:''; position:absolute; width:25%; height:25%; border-radius:50%; background:rgba(255,255,255,0.8); top:18%; left:18%;
}
.hero-bunny .bunny-nose{
  position:absolute; left:50%; top:56%; width:13%; height:11%; border-radius:58% 42% 58% 42%; background:linear-gradient(180deg, #f5f5f5, #d1d1d1); transform:translateX(-50%) rotate(-12deg); box-shadow:inset -5px -3px 0 rgba(0,0,0,0.14);
}
.hero-bunny .bunny-mouth{
  position:absolute; left:50%; top:66%; width:20%; height:12%; border-bottom:4px solid rgba(255,255,255,0.58); border-radius:0 0 30px 30px; transform:translateX(-50%);
}
.hero-bunny .bunny-paw{
  position:absolute; bottom:6%; width:27%; height:15%; border-radius:42% 42% 48% 48%; background:linear-gradient(135deg, #111 0%, #1b1b1b 100%);
  box-shadow:inset -12px -8px 16px rgba(0,0,0,0.72);
}
.hero-bunny .bunny-paw.left{ left:23%; }
.hero-bunny .bunny-paw.right{ right:23%; }
.hero-bunny .bunny-shadow{
  position:absolute; inset:auto 15% 0; height:16%; z-index:-1; border-radius:50%; background:radial-gradient(ellipse at center, rgba(0,0,0,0.75), transparent 68%);
  transform:translateZ(-40px) scale(1.15);
}
.hero-smoke{
  position:absolute; inset:0; pointer-events:none; z-index:0;
}
.hero-smoke span{
  position:absolute; display:block; border-radius:50%; background:radial-gradient(circle, rgba(255,255,255,0.4), rgba(255,255,255,0.14) 26%, transparent 72%);
  filter:blur(18px); opacity:0.75;
  animation:smokeFloat 12s ease-in-out infinite alternate;
}
.hero-smoke .smoke-1{ width:120px; height:120px; right:10%; top:9%; }
.hero-smoke .smoke-2{ width:150px; height:150px; right:20%; top:18%; animation-delay:1.5s; }
.hero-smoke .smoke-3{ width:180px; height:180px; right:26%; top:20%; animation-delay:3s; }
.hero-smoke .smoke-4{ width:210px; height:210px; right:18%; bottom:10%; opacity:0.55; animation-delay:4s; }
@keyframes smokeFloat{ 0%{ transform:translate3d(0,0,0) scale(0.9); opacity:0.35; } 50%{ transform:translate3d(-8px, -12px, 0) scale(1.08); opacity:0.7; } 100%{ transform:translate3d(18px, -26px, 0) scale(1.18); opacity:0.3; } }
@media (max-width:960px){
  .hero-visual{ right:-8%; width:min(68vw, 420px); height:min(86vw, 540px); }
}
@media (max-width:600px){
  .hero-visual{ right:-18%; bottom:-10%; width:min(88vw, 330px); height:min(110vw, 420px); }
  .hero-bunny .bunny-eye{ width:18%; height:20%; }
}
.hero-grid{
  position:relative; z-index:2; width:100%;
  display:grid; place-items:center; text-align:center;
}
.hero-eyebrow{
  font-family:'Montserrat'; font-weight:700; letter-spacing:0.35em;
  text-transform:uppercase; color:var(--text-2); font-size:0.85rem; margin-bottom:22px;
  opacity:0; animation:fadeUp .8s var(--ease) .1s forwards;
}
.hero-wordmark{
  display:block; width:min(90vw, 1180px); max-width:100%; margin:0 auto;
  filter:drop-shadow(0 0 46px rgba(255,215,0,0.18));
  opacity:0; animation:fadeUp .9s var(--ease) .25s forwards;
}
.hero-sub{
  font-family:'Montserrat'; font-weight:300; letter-spacing:0.35em;
  text-transform:uppercase; font-size:clamp(0.9rem,2vw,1.3rem);
  margin-top:14px; color:var(--yellow);
  opacity:0; animation:fadeUp .9s var(--ease) .4s forwards;
}
.hero-tagline{
  max-width:560px; margin:26px auto 0; color:var(--text-2); font-size:1.05rem;
  opacity:0; animation:fadeUp .9s var(--ease) .55s forwards;
}
.hero-cta{
  display:flex; gap:18px; justify-content:center; margin-top:42px; flex-wrap:wrap;
  opacity:0; animation:fadeUp .9s var(--ease) .7s forwards;
}
.hero-scroll{
  position:absolute; bottom:34px; left:50%; transform:translateX(-50%);
  display:flex; flex-direction:column; align-items:center; gap:8px;
  color:var(--text-2); font-size:0.7rem; letter-spacing:0.2em; text-transform:uppercase;
  opacity:0; animation:fadeUp .9s var(--ease) 1s forwards;
}
.hero-scroll .line{ width:1px; height:34px; background:linear-gradient(var(--yellow), transparent); animation:scrollLine 1.8s ease-in-out infinite; }

@keyframes fadeUp{ from{ opacity:0; transform:translateY(28px);} to{opacity:1; transform:translateY(0);} }
@keyframes scrollLine{ 0%{ transform:scaleY(0); transform-origin:top;} 50%{transform:scaleY(1); transform-origin:top;} 51%{transform-origin:bottom;} 100%{transform:scaleY(0); transform-origin:bottom;} }

/* Mascot */
.mascot{
  position:fixed; right:22px; bottom:22px; z-index:600;
  width:78px; height:78px; cursor:pointer;
  animation:float 4.5s ease-in-out infinite;
  filter:drop-shadow(0 6px 18px rgba(0,0,0,0.5));
  transition:transform .3s var(--ease);
}
.mascot .mascot-image{
  display:block; width:100%; height:100%; object-fit:contain; user-select:none; pointer-events:none;
}
.mascot:hover{ transform:scale(1.08); }
.mascot.blink .mascot-image{ animation:blink .35s ease; }
@keyframes float{ 0%,100%{ transform:translateY(0) rotate(-2deg);} 50%{ transform:translateY(-16px) rotate(2deg);} }
@keyframes blink{ 0%,100%{ transform:scaleY(1);} 50%{ transform:scaleY(0.1);} }

.mascot-bubble{
  position:absolute; bottom:100%; right:0; margin-bottom:14px;
  background:#fff; color:#0A0A0A; padding:10px 16px; border-radius:14px;
  font-family:'Montserrat'; font-weight:700; font-size:0.8rem; max-width:200px;
  opacity:0; transform:translateY(6px) scale(0.9); pointer-events:none;
  transition:all .3s var(--ease);
}
.mascot-bubble::after{
  content:''; position:absolute; top:100%; right:20px;
  border:7px solid transparent; border-top-color:#fff;
}
.mascot-bubble.show{ opacity:1; transform:translateY(0) scale(1); }

@media (max-width:768px){ .mascot{ width:58px; height:58px; right:14px; bottom:14px; } }

/* ===================== REVEAL ===================== */
.reveal{ opacity:0; transform:translateY(36px); transition:opacity .8s var(--ease), transform .8s var(--ease); }
.reveal.in{ opacity:1; transform:translateY(0); }
.reveal-stagger > *{ transition-delay:calc(var(--i, 0) * 90ms); }

/* ===================== FEATURES STRIP ===================== */
.features{ padding-top:70px; padding-bottom:70px; }
.features-grid{
  display:grid; grid-template-columns:repeat(4,1fr); gap:20px;
}
@media (max-width:960px){ .features-grid{ grid-template-columns:1fr 1fr; } }
.feature-card{
  padding:30px 24px; text-align:left;
  transition:transform .4s var(--ease), box-shadow .4s var(--ease), border-color .4s var(--ease);
}
.feature-card .icon{ font-size:1.8rem; margin-bottom:16px; display:block; }
.feature-card h4{ font-size:1rem; margin-bottom:8px; letter-spacing:0; }
.feature-card p{ color:var(--text-2); font-size:0.9rem; }
.feature-card:hover{
  transform:translateY(-8px);
  border-color:rgba(255,215,0,0.4);
  box-shadow:0 16px 40px rgba(0,0,0,0.4), 0 0 24px rgba(255,215,0,0.12);
}

/* ===================== DRINK / MENU CARDS ===================== */
.drinks-grid{ display:grid; grid-template-columns:repeat(3,1fr); gap:24px; }
@media (max-width:960px){ .drinks-grid{ grid-template-columns:repeat(2,1fr); } }
@media (max-width:600px){ .drinks-grid{ grid-template-columns:1fr; } }

.drink-card{
  position:relative; overflow:hidden;
  padding:2px;
  border-radius:var(--radius);
  transition:transform .45s var(--ease);
}
.drink-card::before{
  content:''; position:absolute; inset:0; border-radius:inherit;
  background:var(--gradient); opacity:0; transition:opacity .4s var(--ease);
  z-index:0;
}
.drink-card:hover{ transform:translateY(-8px); }
.drink-card:hover::before{ opacity:1; }
.drink-card-inner{
  position:relative; z-index:1; background:var(--dark); border-radius:calc(var(--radius) - 2px);
  overflow:hidden; height:100%;
}
.drink-photo{
  height:170px; display:flex; align-items:center; justify-content:center;
  font-size:3.4rem; position:relative;
}
.drink-photo .badge{
  position:absolute; top:12px; left:12px;
  font-family:'Montserrat'; font-weight:800; font-size:0.65rem; text-transform:uppercase;
  padding:5px 11px; border-radius:100px; color:#0A0A0A; letter-spacing:0.03em;
}
.badge-hit{ background:var(--yellow); }
.badge-new{ background:var(--pink); color:#fff; }
.badge-wellness{ background:var(--green); }
.badge-own{ background:var(--orange); }

.drink-body{ padding:20px; }
.drink-body h4{ font-size:1.05rem; letter-spacing:0; margin-bottom:6px; }
.drink-body .desc{ color:var(--text-2); font-size:0.85rem; margin-bottom:14px; min-height:40px;}
.drink-foot{ display:flex; justify-content:space-between; align-items:center; }
.price{ font-family:'Montserrat'; font-weight:700; color:var(--yellow); font-size:0.95rem; }
.wellness-note{ font-size:0.72rem; color:var(--green); margin-top:8px; font-weight:600; }
.addons{ display:flex; gap:6px; margin-top:10px; font-size:1rem; }

.menu-cta{ text-align:center; margin-top:50px; }

/* ===================== MENU PAGE ===================== */
.menu-tabs{
  position:sticky; top:70px; z-index:400;
  display:flex; gap:10px; overflow-x:auto; padding:14px 0 22px;
  background:linear-gradient(var(--black) 60%, transparent);
  scrollbar-width:none;
}
.menu-tabs::-webkit-scrollbar{ display:none; }
.menu-tab{
  flex:0 0 auto; padding:10px 20px; border-radius:100px;
  font-family:'Montserrat'; font-weight:700; font-size:0.8rem; text-transform:uppercase;
  border:1px solid var(--glass-border); color:var(--text-2); background:var(--glass-bg);
  transition:all .3s var(--ease);
}
.menu-tab:hover{ color:#fff; border-color:rgba(255,255,255,0.3); }
.menu-tab.active{ background:var(--gradient); color:#0A0A0A; border-color:transparent; }

.menu-list{ display:grid; grid-template-columns:1fr 1fr; gap:20px; margin-top:10px; }
@media (max-width:768px){ .menu-list{ grid-template-columns:1fr; } }

.menu-item{
  display:flex; gap:16px; padding:18px; align-items:center;
  transition:transform .3s var(--ease), border-color .3s var(--ease);
}
.menu-item:hover{ transform:translateX(4px); border-color:rgba(255,215,0,0.35); }
.menu-item .thumb{
  width:64px; height:64px; border-radius:14px; flex:none;
  display:flex; align-items:center; justify-content:center; font-size:1.7rem;
}
.menu-item-main{ flex:1; min-width:0; }
.menu-item-top{ display:flex; justify-content:space-between; gap:10px; align-items:baseline; }
.menu-item-top h4{ font-size:0.98rem; letter-spacing:0; }
.menu-item .desc{ color:var(--text-2); font-size:0.82rem; margin-top:4px; }
.menu-item .badges{ display:flex; gap:6px; margin-top:8px; flex-wrap:wrap; }
.mini-badge{
  font-size:0.6rem; font-weight:800; text-transform:uppercase; padding:3px 9px;
  border-radius:100px; letter-spacing:0.03em; color:#0A0A0A;
}

.category-heading{ margin:56px 0 22px; display:flex; align-items:center; gap:16px; }
.category-heading h3{ white-space:nowrap; }
.category-heading .line{ height:1px; flex:1; background:linear-gradient(90deg, var(--glass-border), transparent); }

/* ===================== ABOUT ===================== */
.about-hero{
  position:relative; border-radius:24px; overflow:hidden; min-height:340px;
  display:flex; align-items:flex-end; padding:40px;
  background:
    linear-gradient(0deg, rgba(10,10,10,0.95), rgba(10,10,10,0.3)),
    radial-gradient(ellipse at 30% 20%, rgba(255,215,0,0.25), transparent 55%),
    radial-gradient(ellipse at 80% 70%, rgba(255,64,129,0.22), transparent 55%),
    var(--dark);
}
.about-hero h1{ font-size:clamp(2.4rem, 7vw, 4.5rem); }

.story-block{ display:grid; grid-template-columns:1fr 1fr; gap:60px; align-items:center; margin-top:90px; }
@media (max-width:860px){ .story-block{ grid-template-columns:1fr; gap:36px; } }
.story-text p{ color:var(--text-2); font-size:1.05rem; margin-top:16px; }

.timeline{ display:flex; flex-direction:column; gap:0; }
.timeline-item{ display:flex; gap:20px; position:relative; padding-bottom:34px; }
.timeline-item::before{
  content:''; position:absolute; left:9px; top:26px; bottom:0; width:1px;
  background:var(--glass-border);
}
.timeline-item:last-child::before{ display:none; }
.timeline-dot{
  width:20px; height:20px; border-radius:50%; background:var(--gradient); flex:none;
  box-shadow:0 0 14px rgba(255,215,0,0.5);
}
.timeline-year{ font-family:'Montserrat'; font-weight:900; color:var(--yellow); font-size:1.1rem; }
.timeline-desc{ color:var(--text-2); font-size:0.92rem; margin-top:4px; }

.philosophy-grid{ display:grid; grid-template-columns:repeat(3,1fr); gap:24px; margin-top:90px; }
@media (max-width:860px){ .philosophy-grid{ grid-template-columns:1fr; } }
.philosophy-card{ padding:34px 28px; text-align:center; }
.philosophy-card .icon{ font-size:2.2rem; margin-bottom:18px; display:block; }
.philosophy-card p{ color:var(--text-2); font-size:0.9rem; margin-top:10px; }

.mascot-story{
  margin-top:100px; text-align:center; padding:70px 30px; border-radius:24px;
  background:radial-gradient(ellipse at 50% 0%, rgba(255,215,0,0.1), transparent 60%), var(--dark);
}
.mascot-story .big-mascot{ width:min(260px, 62vw); margin:0 auto 30px; display:block; filter:drop-shadow(0 20px 32px rgba(0,0,0,0.42)); animation:float 4.5s ease-in-out infinite; }
.mascot-story p{ max-width:600px; margin:16px auto 0; color:var(--text-2); }

/* ===================== LOCATIONS ===================== */
.locations-layout{ display:grid; grid-template-columns:0.9fr 1.1fr; gap:32px; align-items:start; }
@media (max-width:900px){ .locations-layout{ grid-template-columns:1fr; } }
.locations-list{ display:flex; flex-direction:column; gap:14px; max-height:640px; overflow-y:auto; padding-right:6px; }
.locations-list::-webkit-scrollbar{ width:5px; }
.locations-list::-webkit-scrollbar-thumb{ background:var(--glass-border); border-radius:10px; }

.location-card{
  padding:18px 20px; display:flex; justify-content:space-between; gap:14px; align-items:center;
  transition:border-color .3s var(--ease), transform .3s var(--ease);
  cursor:pointer;
}
.location-card:hover, .location-card.active{ border-color:rgba(255,215,0,0.4); transform:translateX(4px); }
.location-card h4{ font-size:0.95rem; letter-spacing:0; margin-bottom:4px; }
.location-card .addr{ color:var(--text-2); font-size:0.8rem; }
.location-card .status{ font-size:0.65rem; font-weight:800; text-transform:uppercase; padding:4px 9px; border-radius:100px; white-space:nowrap; }
.status-open{ background:rgba(124,179,66,0.2); color:var(--green); border:1px solid rgba(124,179,66,0.4); }
.location-card .route{ font-size:0.7rem; color:var(--yellow); margin-top:6px; display:inline-block; font-weight:700; }

#map{ width:100%; height:640px; border-radius:var(--radius); overflow:hidden; border:1px solid var(--glass-border); }
.leaflet-popup-content-wrapper{ background:#161616; color:#fff; border-radius:12px; }
.leaflet-popup-tip{ background:#161616; }
.leaflet-popup-content a{ color:var(--yellow); }

/* ===================== COMMUNITY ===================== */
.community-collage{
  display:grid; grid-template-columns:repeat(4, 1fr); grid-auto-rows:110px; gap:12px; margin-bottom:70px;
}
.community-collage div{
  border-radius:14px; display:flex; align-items:center; justify-content:center; font-size:1.8rem;
}
.community-collage div:nth-child(1){ grid-column:span 2; grid-row:span 2; font-size:2.6rem; }
.community-collage div:nth-child(6){ grid-column:span 2; }
@media (max-width:700px){
  .community-collage{ grid-template-columns:repeat(3,1fr); grid-auto-rows:80px; }
  .community-collage div:nth-child(1){ grid-column:span 3; grid-row:span 1; }
}

.glovo-block{
  display:flex; justify-content:space-between; align-items:center; gap:30px;
  padding:44px; flex-wrap:wrap;
  background:linear-gradient(120deg, rgba(255,194,68,0.12), rgba(255,109,0,0.08));
}
.glovo-block h3{ margin-bottom:8px; }
.glovo-block p{ color:var(--text-2); max-width:420px; }

/* ===================== REVIEWS ===================== */
.reviews-grid{ display:grid; grid-template-columns:repeat(3,1fr); gap:22px; }
@media (max-width:900px){ .reviews-grid{ grid-template-columns:1fr 1fr; } }
@media (max-width:600px){ .reviews-grid{ grid-template-columns:1fr; } }
.review-card{ padding:26px; }
.review-top{ display:flex; align-items:center; gap:12px; margin-bottom:14px; }
.avatar{
  width:44px; height:44px; border-radius:50%; display:flex; align-items:center; justify-content:center;
  font-family:'Montserrat'; font-weight:800; color:#0A0A0A; flex:none;
}
.review-name{ font-family:'Montserrat'; font-weight:700; font-size:0.9rem; }
.stars{ color:var(--yellow); font-size:0.8rem; letter-spacing:2px; }
.review-text{ color:var(--text-2); font-size:0.9rem; }
.review-drink{ margin-top:14px; font-size:0.75rem; color:var(--text-2); display:flex; align-items:center; gap:6px; }

.review-form-trigger{ text-align:center; margin-top:50px; }

/* ===================== CONTACTS ===================== */
.contacts-grid{ display:grid; grid-template-columns:1fr 1.1fr; gap:50px; }
@media (max-width:860px){ .contacts-grid{ grid-template-columns:1fr; } }
.contact-info-item{ display:flex; gap:16px; align-items:flex-start; margin-bottom:26px; }
.contact-info-item .ic{ font-size:1.4rem; }
.contact-info-item h4{ font-size:0.9rem; letter-spacing:0.03em; text-transform:uppercase; margin-bottom:4px; color:var(--yellow); font-family:'Montserrat'; }
.contact-info-item p, .contact-info-item a{ color:var(--text-2); font-size:0.92rem; }
.contact-info-item a:hover{ color:#fff; }

.contact-form{ padding:34px; }
.form-row{ margin-bottom:18px; }
.form-row label{ display:block; font-size:0.75rem; text-transform:uppercase; letter-spacing:0.05em; color:var(--text-2); margin-bottom:8px; font-weight:700; font-family:'Montserrat'; }
.form-row input, .form-row textarea{
  width:100%; background:rgba(255,255,255,0.04); border:1px solid var(--glass-border); border-radius:10px;
  padding:13px 16px; color:#fff; font-family:'Inter'; font-size:0.92rem; transition:border-color .3s var(--ease);
}
.form-row input:focus, .form-row textarea:focus{ outline:none; border-color:var(--yellow); }
.form-row textarea{ resize:vertical; min-height:110px; }

/* ===================== FOOTER ===================== */
footer{
  border-top:1px solid var(--glass-border); padding:70px 0 30px;
  background:linear-gradient(180deg, transparent, rgba(255,109,0,0.04));
}
.footer-top{ display:flex; justify-content:space-between; gap:50px; flex-wrap:wrap; margin-bottom:50px; }
.footer-brand .logo{ margin-bottom:14px; }
.footer-brand p{ color:var(--text-2); font-size:0.88rem; max-width:280px; }
.footer-cols{ display:flex; gap:70px; flex-wrap:wrap; }
.footer-col h5{ font-family:'Montserrat'; font-weight:800; font-size:0.78rem; text-transform:uppercase; letter-spacing:0.06em; margin-bottom:16px; color:var(--yellow); }
.footer-col a, .footer-col p{ display:block; color:var(--text-2); font-size:0.88rem; margin-bottom:10px; transition:color .25s var(--ease); }
.footer-col a:hover{ color:#fff; }

.social-row{ display:flex; gap:12px; }
.social-icon{
  width:42px; height:42px; border-radius:50%; display:flex; align-items:center; justify-content:center;
  border:1px solid var(--glass-border); font-size:1.1rem; transition:all .3s var(--ease);
}
.social-icon:hover{ border-color:var(--yellow); box-shadow:0 0 18px rgba(255,215,0,0.35); transform:translateY(-3px); }

.footer-bottom{
  padding-top:26px; border-top:1px solid var(--glass-border);
  display:flex; justify-content:space-between; flex-wrap:wrap; gap:10px;
  font-size:0.78rem; color:var(--text-2);
}

/* ===================== MODAL ===================== */
.modal-overlay{
  position:fixed; inset:0; background:rgba(0,0,0,0.7); backdrop-filter:blur(6px);
  z-index:1200; display:flex; align-items:center; justify-content:center; padding:20px;
  opacity:0; pointer-events:none; transition:opacity .35s var(--ease);
}
.modal-overlay.show{ opacity:1; pointer-events:auto; }
.modal-box{
  max-width:460px; width:100%; padding:36px; position:relative;
  transform:translateY(20px) scale(0.97); transition:transform .35s var(--ease);
  background:#141414;
}
.modal-overlay.show .modal-box{ transform:translateY(0) scale(1); }
.modal-close{ position:absolute; top:18px; right:18px; font-size:1.3rem; color:var(--text-2); }
.modal-close:hover{ color:#fff; }
.star-picker{ display:flex; gap:8px; font-size:1.6rem; cursor:pointer; margin-bottom:6px; }
.star-picker span{ opacity:0.3; transition:opacity .2s; }
.star-picker span.active{ opacity:1; }

/* ===================== TOAST ===================== */
.toast{
  position:fixed; bottom:26px; left:50%; transform:translateX(-50%) translateY(20px);
  background:var(--gradient); color:#0A0A0A; font-family:'Montserrat'; font-weight:800;
  padding:14px 26px; border-radius:100px; z-index:1300; font-size:0.85rem;
  opacity:0; transition:all .4s var(--ease); pointer-events:none;
}
.toast.show{ opacity:1; transform:translateX(-50%) translateY(0); }

/* ===================== PAGE UTILS ===================== */
.page-hero-simple{ padding-top:150px; padding-bottom:40px; text-align:center; }
.hidden{ display:none !important; }

::selection{ background:var(--yellow); color:#0A0A0A; }

.gradient-text{
  background:var(--gradient);
  -webkit-background-clip:text; background-clip:text; color:transparent;
}
</style>

</head>
<body>

<!-- ============ MASCOT ============ -->
<div class="mascot" id="mascot" title="Клікни на зайчика">
  <div class="mascot-bubble" id="mascotBubble"></div>
  <img class="mascot-image" src="./чорнийзаяць.png" alt="Чорний заєць" />
</div>

<!-- ============ HEADER ============ -->
<header class="site-header" id="siteHeader">
  <div class="container">
    <a href="#home" class="logo"><span class="dot"></span>SPESHELTY</a>
    <nav class="nav-links" id="navLinks">
      <a href="#home">Головна</a>
      <a href="#menu">Меню</a>
      <a href="#about">Про нас</a>
      <a href="#locations">Локації</a>
      <a href="#community">Ком'юніті</a>
      <a href="#reviews">Відгуки</a>
      <a href="#contacts">Контакти</a>
    </nav>
    <div class="header-actions">
      <div class="lang-switch">
        <button data-lang="ua" class="active">UA</button>
        <button data-lang="en">EN</button>
      </div>
      <div class="burger" id="burger"><span></span><span></span><span></span></div>
    </div>
  </div>
</header>

<!-- ============ HOME / HERO ============ -->
<section class="hero" id="home">
  <div class="hero-visual" aria-hidden="true">
    <div class="hero-smoke">
      <span class="smoke-1"></span>
      <span class="smoke-2"></span>
      <span class="smoke-3"></span>
      <span class="smoke-4"></span>
    </div>
    <div class="hero-bunny" id="heroBunny">
      <img class="hero-bunny-image" src="./чорнийзаяць.png" alt="Чорний заєць" />
    </div>
  </div>
  <div class="container hero-grid">
    <div>
      <div class="hero-eyebrow">Own Production · Kyiv, Ukraine</div>
      <img class="hero-wordmark" src="./СПЕШЕЛТІ-НОУ-ФОН.png" alt="Speshelty" />
      <div class="hero-sub">TEA &amp; COFFEE BAR</div>
      <p class="hero-tagline">Власне виробництво. Американський стиль. Українське ком'юніті.</p>
      <div class="hero-cta">
        <a href="#menu" class="btn btn-primary">Переглянути меню</a>
        <a href="#locations" class="btn btn-outline">Знайти точку</a>
      </div>
    </div>
  </div>
  <div class="hero-scroll"><span>Гортай далі</span><span class="line"></span></div>
</section>

<!-- ============ FEATURES STRIP ============ -->
<section class="features">
  <div class="container">
    <div class="features-grid reveal reveal-stagger" id="featuresGrid">
      <div class="feature-card glass" style="--i:0">
        <span class="icon">⚡</span>
        <h4>Швидкість</h4>
        <p>To-go формат — твій напій готовий за 2 хвилини.</p>
      </div>
      <div class="feature-card glass" style="--i:1">
        <span class="icon">🎨</span>
        <h4>Фірмові напої</h4>
        <p>90% меню — власна розробка, яку не знайдеш більше ніде.</p>
      </div>
      <div class="feature-card glass" style="--i:2">
        <span class="icon">💪</span>
        <h4>Wellness</h4>
        <p>Колаген, протеїн та амінокислоти у наших смузі.</p>
      </div>
      <div class="feature-card glass" style="--i:3">
        <span class="icon">🐰</span>
        <h4>Spesh Vibe</h4>
        <p>Унікальний стиль, характер і зайчик з чашкою в лапах.</p>
      </div>
    </div>
  </div>
</section>

<!-- ============ BESTSELLERS ============ -->
<section id="bestsellers">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Must Try</div>
      <h2>Хіти продажів</h2>
      <p>Те, що замовляють найчастіше — і не дарма.</p>
    </div>
    <div class="drinks-grid reveal reveal-stagger" id="bestGrid"></div>
    <div class="menu-cta">
      <a href="#menu" class="btn btn-primary">Все меню</a>
    </div>
  </div>
</section>

<!-- ============ INSTAGRAM ============ -->
<section id="instagram" style="padding-bottom:100px;">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">@speshelty</div>
      <h2>Слідкуй за нами</h2>
    </div>
    <div class="community-collage reveal reveal-stagger" id="instaGrid"></div>
    <div style="display:flex; justify-content:center; align-items:center; gap:18px;">
      <a href="https://www.instagram.com/speshelty" target="_blank" class="social-icon" aria-label="Instagram">📷</a>
      <a href="https://www.tiktok.com/@speshelty.ua" target="_blank" class="social-icon" aria-label="TikTok">🎵</a>
      <a href="https://www.instagram.com/speshelty" target="_blank" class="btn btn-outline">Більше в Instagram</a>
    </div>
  </div>
</section>

<!-- ============ MENU PAGE ============ -->
<section id="menu" style="padding-top:150px;">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Full Menu</div>
      <h2>Меню</h2>
      <p>Кожен напій — фірмовий рецепт Speshelty.</p>
    </div>

    <div class="menu-tabs" id="menuTabs"></div>
    <div id="menuContent"></div>
  </div>
</section>

<!-- ============ ABOUT ============ -->
<section id="about">
  <div class="container">
    <div class="about-hero reveal">
      <div>
        <div class="eyebrow">Since 2022</div>
        <h1>Ми — Speshelty</h1>
      </div>
    </div>

    <div class="story-block">
      <div class="story-text reveal">
        <div class="eyebrow">Наша історія</div>
        <h2 style="font-size:clamp(1.6rem,3.5vw,2.2rem)">Від ідеї до ком'юніті</h2>
        <p>Speshelty народився з ідеї поєднати американську культуру to-go напоїв з українською любов'ю до якості. Ми створили простір, де кожен напій — це фірмовий рецепт, кожна банка — частинка нашого стилю, а кожен клієнт — частина ком'юніті.</p>
      </div>
      <div class="timeline reveal" id="timeline"></div>
    </div>

    <div class="section-head reveal" style="margin-top:110px;">
      <div class="eyebrow">Наш підхід</div>
      <h2>Філософія</h2>
    </div>
    <div class="philosophy-grid reveal reveal-stagger" id="philosophyGrid"></div>

    <div class="mascot-story reveal">
      <img class="big-mascot" src="./чорнийзаяць.png" alt="Чорний заєць" />
      <h3>Чорний Заєць</h3>
      <p>Чорний заєць — душа Speshelty. Він трохи бунтар, трохи мрійник і завжди з чашкою улюбленого напою. Як і ми.</p>
    </div>
  </div>
</section>

<!-- ============ LOCATIONS ============ -->
<section id="locations">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">8 точок у Києві</div>
      <h2>Знайди свою точку</h2>
    </div>
    <div class="locations-layout reveal">
      <div class="locations-list glass" id="locationsList" style="padding:14px;"></div>
      <div id="map"></div>
    </div>
  </div>
</section>

<!-- ============ COMMUNITY ============ -->
<section id="community">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Spesh Family</div>
      <h2>Ком'юніті</h2>
      <p>Ми створюємо культуру, а не просто продаємо напої.</p>
    </div>
    <div class="community-collage reveal reveal-stagger" id="communityCollage"></div>

    <div class="glovo-block glass reveal">
      <div>
        <h3>Не можеш завітати?</h3>
        <p>Замовляй улюблені напої через Glovo. Доставка доступна з обраних точок — шукай «Speshelty» у додатку.</p>
      </div>
      <a href="https://glovo.go.link/open?link_type=store&store_id=574504&adjust_t=s321jkn"
   class="btn btn-glovo"
   target="_blank"
   rel="noopener noreferrer">
  Замовити на Glovo
</a>
    </div>
  </div>
</section>

<!-- ============ REVIEWS ============ -->
<section id="reviews">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">4.9 / 5 · 1200+ відгуків</div>
      <h2>Що кажуть наші клієнти</h2>
    </div>
    <div class="reviews-grid reveal reveal-stagger" id="reviewsGrid"></div>
    <div class="review-form-trigger">
      <button class="btn btn-outline" id="openReviewModal">Залишити відгук</button>
    </div>
  </div>
</section>

<!-- ============ CONTACTS ============ -->
<section id="contacts">
  <div class="container">
    <div class="section-head reveal">
      <div class="eyebrow">Contacts</div>
      <h2>Зв'яжись з нами</h2>
    </div>
    <div class="contacts-grid reveal">
      <div>
        <div class="contact-info-item">
          <span class="ic">📷</span>
          <div><h4>Instagram</h4><a href="https://www.instagram.com/speshelty" target="_blank">@speshelty</a></div>
        </div>
        <div class="contact-info-item">
          <span class="ic">🎵</span>
          <div><h4>TikTok</h4><a href="https://www.tiktok.com/@speshelty.ua" target="_blank">@speshelty.ua</a></div>
        </div>
        <div class="contact-info-item">
          <span class="ic">🤝</span>
          <div><h4>Для співпраці</h4><a href="mailto:partnership@speshelty.com">partnership@speshelty.com</a></div>
        </div>
        <div class="contact-info-item">
          <span class="ic">💼</span>
          <div><h4>Careers</h4><a href="https://www.instagram.com/speshelty" target="_blank">Хочеш у команду? Пиши нам в Instagram</a></div>
        </div>
      </div>
      <form class="contact-form glass"
      id="contactForm"
      action="https://formsubmit.co/garfilov333@gmail.com"
      method="POST">
  <input type="hidden" name="_subject" value="Нове повідомлення з сайту Speshelty">
  <input type="hidden" name="_captcha" value="false">
  <input type="hidden" name="_template" value="table">

  <div class="form-row">
    <label>Ім'я</label>
    <input type="text" name="name" required>
  </div>

  <div class="form-row">
    <label>Email</label>
    <input type="email" name="email" required>
  </div>

  <div class="form-row">
    <label>Тема</label>
    <input type="text" name="subject" required>
  </div>

  <div class="form-row">
    <label>Повідомлення</label>
    <textarea name="message" required></textarea>
  </div>

  <button type="submit" class="btn btn-primary" style="width:100%; justify-content:center;">
    Надіслати
  </button>
</form>
    </div>
  </div>
</section>

<!-- ============ FOOTER ============ -->
<footer>
  <div class="container">
    <div class="footer-top">
      <div class="footer-brand">
        <div class="logo"><span class="dot"></span>SPESHELTY</div>
        <p>Власне виробництво. Американський стиль. Українське ком'юніті.</p>
        <div class="social-row" style="margin-top:20px;">
          <a href="https://www.instagram.com/speshelty" target="_blank" class="social-icon">📷</a>
          <a href="https://www.tiktok.com/@speshelty.ua" target="_blank" class="social-icon">🎵</a>
        </div>
      </div>
      <div class="footer-cols">
        <div class="footer-col">
          <h5>Навігація</h5>
          <a href="#menu">Меню</a>
          <a href="#locations">Локації</a>
          <a href="#about">Про нас</a>
          <a href="#community">Ком'юніті</a>
        </div>
        <div class="footer-col">
          <h5>Мова</h5>
          <a href="#" data-lang-footer="ua">Українська</a>
          <a href="#" data-lang-footer="en">English</a>
        </div>
      </div>
    </div>
    <div class="footer-bottom">
      <span>© 2025 Speshelty. All rights reserved.</span>
      <span>Працюємо в ТРЦ та Епіцентрах по всій Україні</span>
    </div>
  </div>
</footer>

<!-- ============ REVIEW MODAL ============ -->
<div class="modal-overlay" id="reviewModal">
  <div class="modal-box glass">
    <button class="modal-close" id="closeReviewModal">✕</button>
    <h3 style="margin-bottom:18px;">Залишити відгук</h3>
    <form id="reviewForm">
      <div class="form-row">
        <label>Ім'я</label>
        <input type="text" required>
      </div>
      <div class="form-row">
        <label>Оцінка</label>
        <div class="star-picker" id="starPicker">
          <span data-v="1">★</span><span data-v="2">★</span><span data-v="3">★</span><span data-v="4">★</span><span data-v="5">★</span>
        </div>
      </div>
      <div class="form-row">
        <label>Твій відгук</label>
        <textarea required></textarea>
      </div>
      <button type="submit" class="btn btn-primary" style="width:100%; justify-content:center;">Надіслати відгук</button>
    </form>
  </div>
</div>

<div class="toast" id="toast"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>

<script>
/* ============================================================
   SPESHELTY — Content data
   ============================================================ */

// gradient backgrounds per category (used for photo placeholders)
const CAT_STYLE = {
  kava:     { bg: 'linear-gradient(135deg,#3E2723,#795548)', emoji: '☕' },
  bumble:   { bg: 'linear-gradient(135deg,#FFD700,#7CB342)', emoji: '🍯' },
  matcha:   { bg: 'linear-gradient(135deg,#7CB342,#2E7D32)', emoji: '🍵' },
  bubble:   { bg: 'linear-gradient(135deg,#7C4DFF,#FF4081)', emoji: '🧋' },
  lemon:    { bg: 'linear-gradient(135deg,#2979FF,#00BCD4)', emoji: '🍋' },
  smoothie: { bg: 'linear-gradient(135deg,#7CB342,#FFD700)', emoji: '🥤' },
  shake:    { bg: 'linear-gradient(135deg,#8D6E63,#D7CCC8)', emoji: '🍨' },
  fresh:    { bg: 'linear-gradient(135deg,#FF6D00,#FFD700)', emoji: '🍊' },
  dessert:  { bg: 'linear-gradient(135deg,#EF6C00,#D84315)', emoji: '🍰' },
};

const CATEGORIES = [
  { id:'all',      ua:'Все',        en:'All' },
  { id:'kava',     ua:'Кава',       en:'Coffee' },
  { id:'bumble',   ua:'Бамбл',      en:'Bumble' },
  { id:'matcha',   ua:'Матча',      en:'Matcha' },
  { id:'bubble',   ua:'Бабл ті',    en:'Bubble Tea' },
  { id:'lemon',    ua:'Лимонади',   en:'Lemonades' },
  { id:'smoothie', ua:'Смузі',      en:'Smoothies' },
  { id:'shake',    ua:'Мілкшейки',  en:'Milkshakes' },
  { id:'fresh',    ua:'Фреш',       en:'Fresh' },
  { id:'dessert',  ua:'Десерти',    en:'Desserts' },
];

// badges: hit | new | wellness | own
let MENU = [
  // KAVA
  { cat:'kava', ua:'Еспресо', en:'Espresso', descUa:'Класичний подвійний еспресо', descEn:'Classic double espresso', price:'45', badges:[] },
  { cat:'kava', ua:'Американо', en:'Americano', descUa:'Еспресо з гарячою водою, 300мл', descEn:'Espresso with hot water, 300ml', price:'55', badges:[] },
  { cat:'kava', ua:'Капучино', en:'Cappuccino', descUa:'Еспресо, молочна піна, 300мл', descEn:'Espresso, milk foam, 300ml', price:'65', badges:[] },
  { cat:'kava', ua:'Лате', en:'Latte', descUa:'Еспресо, молоко, ніжна піна', descEn:'Espresso, milk, soft foam', price:'70', badges:['hit'] },
  { cat:'kava', ua:'Флет Уайт', en:'Flat White', descUa:'Подвійний еспресо, мікропіна', descEn:'Double espresso, microfoam', price:'75', badges:[] },
  { cat:'kava', ua:'Раф Кава', en:'Raf Coffee', descUa:'Еспресо, вершки, ванільний цукор', descEn:'Espresso, cream, vanilla sugar', price:'80', badges:['own'] },
  { cat:'kava', ua:'Айс Лате', en:'Iced Latte', descUa:'Холодна класика, 400мл', descEn:'Cold classic, 400ml', price:'75', badges:['hit'] },
  { cat:'kava', ua:'Кокосовий Лате', en:'Coconut Latte', descUa:'Еспресо на кокосовому молоці', descEn:'Espresso with coconut milk', price:'85', badges:['own'] },

  // BUMBLE
  { cat:'bumble', ua:'Bumble Matcha', en:'Bumble Matcha', descUa:'Матча, молочна піна, мед', descEn:'Matcha, milk foam, honey', price:'95', badges:['hit','own'] },
  { cat:'bumble', ua:'Bumble Cherry', en:'Bumble Cherry', descUa:'Вишневий чай, піна, м\'ята', descEn:'Cherry tea, foam, mint', price:'90', badges:['own'] },
  { cat:'bumble', ua:'Bumble Mango-Passion', en:'Bumble Mango-Passion', descUa:'Манго, маракуйя, легка піна', descEn:'Mango, passion fruit, light foam', price:'95', badges:['new','own'] },
  { cat:'bumble', ua:'Bumble Citrus', en:'Bumble Citrus', descUa:'Цитрусовий мікс з піною', descEn:'Citrus mix with foam', price:'90', badges:['own'] },

  // MATCHA
  { cat:'matcha', ua:'Matcha Latte', en:'Matcha Latte', descUa:'Церемоніальна матча, молоко', descEn:'Ceremonial matcha, milk', price:'85', badges:[] },
  { cat:'matcha', ua:'Matcha Mood', en:'Matcha Mood', descUa:'Матча + ягідний мікс, шаровий', descEn:'Matcha + berry mix, layered', price:'95', badges:['hit','new'] },
  { cat:'matcha', ua:'Айс Матча', en:'Iced Matcha', descUa:'Холодна матча на льоду', descEn:'Cold matcha over ice', price:'90', badges:[] },
  { cat:'matcha', ua:'Matcha Coconut', en:'Matcha Coconut', descUa:'Матча на кокосовому молоці', descEn:'Matcha with coconut milk', price:'95', badges:['own'] },

  // BUBBLE TEA
  { cat:'bubble', ua:'Bubble Tea Taro', en:'Bubble Tea Taro', descUa:'Таро, тапіока, вершки, M/L', descEn:'Taro, tapioca, cream, M/L', price:'110', badges:['hit'], addons:['🧋','🍮'] },
  { cat:'bubble', ua:'Bubble Tea Strawberry', en:'Bubble Tea Strawberry', descUa:'Полуниця, тапіока, джус болы', descEn:'Strawberry, tapioca, juice balls', price:'110', badges:[], addons:['🧋','🍓'] },
  { cat:'bubble', ua:'Bubble Tea Brown Sugar', en:'Bubble Tea Brown Sugar', descUa:'Коричневий цукор, тапіока, молоко', descEn:'Brown sugar, tapioca, milk', price:'115', badges:['new'], addons:['🧋'] },
  { cat:'bubble', ua:'Bubble Tea Mango', en:'Bubble Tea Mango', descUa:'Манго, желе, тапіока', descEn:'Mango, jelly, tapioca', price:'110', badges:[], addons:['🧋','🥭'] },

  // LEMONADES
  { cat:'lemon', ua:'Полуниця-Базилік', en:'Strawberry-Basil', descUa:'Домашній лимонад, 400мл', descEn:'Homemade lemonade, 400ml', price:'75', badges:['own'] },
  { cat:'lemon', ua:'Маракуйя-М\'ята', en:'Passion Fruit-Mint', descUa:'Освіжаючий мікс', descEn:'Refreshing mix', price:'75', badges:['own'] },
  { cat:'lemon', ua:'Класичний Лимонад', en:'Classic Lemonade', descUa:'Лимон, м\'ята, содова', descEn:'Lemon, mint, soda', price:'65', badges:[] },
  { cat:'lemon', ua:'Кавун-Лайм', en:'Watermelon-Lime', descUa:'Літній фаворит', descEn:'Summer favourite', price:'80', badges:['hit'] },

  // SMOOTHIES
  { cat:'smoothie', ua:'Ананас-Авокадо', en:'Pineapple-Avocado', descUa:'Кремовий смузі', descEn:'Creamy smoothie', price:'110', badges:['wellness'], wellness:true },
  { cat:'smoothie', ua:'Полуниця-Банан', en:'Strawberry-Banana', descUa:'Класичний фруктовий смузі', descEn:'Classic fruit smoothie', price:'100', badges:['hit'] },
  { cat:'smoothie', ua:'Манго-Маракуйя', en:'Mango-Passion Fruit', descUa:'Тропічний мікс', descEn:'Tropical mix', price:'105', badges:[] },
  { cat:'smoothie', ua:'Зелений Detox', en:'Green Detox', descUa:'Шпинат, яблуко, огірок, імбир', descEn:'Spinach, apple, cucumber, ginger', price:'110', badges:['wellness'], wellness:true },

  // MILKSHAKES
  { cat:'shake', ua:'Ванільний Мілкшейк', en:'Vanilla Milkshake', descUa:'Класика з ванільним морозивом', descEn:'Classic with vanilla ice cream', price:'95', badges:[] },
  { cat:'shake', ua:'Шоколадний Мілкшейк', en:'Chocolate Milkshake', descUa:'Насичений шоколадний смак', descEn:'Rich chocolate flavour', price:'95', badges:[] },
  { cat:'shake', ua:'Карамельний Мілкшейк', en:'Caramel Milkshake', descUa:'Солена карамель', descEn:'Salted caramel', price:'100', badges:['hit'] },
  { cat:'shake', ua:'Oreo Мілкшейк', en:'Oreo Milkshake', descUa:'Крихта Oreo, вершки', descEn:'Oreo crumbs, cream', price:'105', badges:['new'] },

  // FRESH
  { cat:'fresh', ua:'Апельсиновий Фреш', en:'Orange Fresh', descUa:'100% свіжовичавлений', descEn:'100% freshly squeezed', price:'85', badges:[] },
  { cat:'fresh', ua:'Яблучний Фреш', en:'Apple Fresh', descUa:'Свіжий яблучний сік', descEn:'Fresh apple juice', price:'80', badges:[] },
  { cat:'fresh', ua:'Морквяний Фреш', en:'Carrot Fresh', descUa:'Вітамінний заряд', descEn:'Vitamin boost', price:'75', badges:['wellness'] },
  { cat:'fresh', ua:'Грейпфрутовий Фреш', en:'Grapefruit Fresh', descUa:'Бадьорість з першого ковтка', descEn:'Energy from the first sip', price:'90', badges:[] },

  // DESSERTS
  { cat:'dessert', ua:'Чізкейк Нью-Йорк', en:'New York Cheesecake', descUa:'Класичний рецепт', descEn:'Classic recipe', price:'95', badges:['hit'] },
  { cat:'dessert', ua:'Брауні', en:'Brownie', descUa:'Шоколадний, вологий', descEn:'Chocolate, fudgy', price:'75', badges:[] },
  { cat:'dessert', ua:'Мафін Шоколадний', en:'Chocolate Muffin', descUa:'З шоколадними краплями', descEn:'With chocolate chips', price:'65', badges:[] },
  { cat:'dessert', ua:'Круасан Мигдальний', en:'Almond Croissant', descUa:'Хрумкий, з мигдальним кремом', descEn:'Crispy, almond cream filling', price:'85', badges:['own'] },
];

const BESTSELLERS = ['Bumble Matcha','Раф Кава','Matcha Mood','Bubble Tea Taro','Ананас-Авокадо','Айс Лате']
  .map(name => MENU.find(m => m.ua === name));

let LOCATIONS = [
  { name:'ТРЦ Республіка', addr:'просп. Перемоги, 100, Київ', lat:50.4720, lng:30.4108, open:true },
  { name:'ТРЦ Проспект', addr:'просп. Ватутіна, 2, Київ', lat:50.4870, lng:30.5900, open:true },
  { name:'ТРЦ Піраміда', addr:'вул. Здолбунівська, 7Д, Київ', lat:50.4050, lng:30.6100, open:false },
  { name:'ТРЦ Sky Mall', addr:'просп. Генерала Ватутіна, 2Б, Київ', lat:50.4880, lng:30.5920, open:true },
  { name:'ТРЦ DreamYellow', addr:'вул. Луначарського, 4, Київ', lat:50.4400, lng:30.6100, open:true },
  { name:'Епіцентр Чабани', addr:'с. Чабани, Обухівська траса, Київська обл.', lat:50.3300, lng:30.4600, open:false },
  { name:'Епіцентр Полярна', addr:'вул. Полярна, 20, Київ', lat:50.5300, lng:30.4700, open:true },
  { name:'ТРЦ City Center', addr:'Таїрова, Одеса', lat:46.4200, lng:30.7000, open:false },
];

let REVIEWS = [
  { name:'Марія К.', textUa:'Найкращий бамбл матча в Києві! Обожнюю їхні алюмінієві банки.', textEn:'The best bumble matcha in Kyiv! I love their aluminium cans.', drinkUa:'Bumble Matcha', drinkEn:'Bumble Matcha', color:'#FFD700' },
  { name:'Олег П.', textUa:'Wellness-смузі з колагеном — мій must-have після тренування.', textEn:'The wellness smoothie with collagen is my post-workout must-have.', drinkUa:'Ананас-Авокадо', drinkEn:'Pineapple-Avocado', color:'#7CB342' },
  { name:'Настя В.', textUa:"Чорний заєць — це любов з першого погляду 🖤", textEn:'The black bunny is love at first sight 🖤', drinkUa:'Matcha Mood', drinkEn:'Matcha Mood', color:'#FF4081' },
  { name:'Дмитро С.', textUa:'Швидко, смачно, стильно. Ідеально для ранку перед роботою.', textEn:'Fast, tasty, stylish. Perfect before work.', drinkUa:'Раф Кава', drinkEn:'Raf Coffee', color:'#FF6D00' },
  { name:'Юлія М.', textUa:'Бабл-ті з таро — топ! Тапіока завжди свіжа.', textEn:'The taro bubble tea is top tier! Tapioca is always fresh.', drinkUa:'Bubble Tea Taro', drinkEn:'Bubble Tea Taro', color:'#2979FF' },
  { name:'Артем Р.', textUa:'Найдобріший персонал і улюблений маскот на стіні.', textEn:'The kindest staff and my favourite mascot on the wall.', drinkUa:'Айс Лате', drinkEn:'Iced Latte', color:'#FFD700' },
  { name:'Софія Т.', textUa:'Лимонад полуниця-базилік — це щось неймовірне влітку.', textEn:'The strawberry-basil lemonade is incredible in summer.', drinkUa:'Полуниця-Базилік', drinkEn:'Strawberry-Basil', color:'#FF4081' },
  { name:'Ігор Л.', textUa:'Заходжу щодня по дорозі в офіс. Вже звичка :)', textEn:'I stop by every day on my way to the office. It\'s a habit now :)', drinkUa:'Американо', drinkEn:'Americano', color:'#7CB342' },
];

const TIMELINE = [
  { year:'2022', ua:'Перша точка в ТРЦ', en:'First spot opens in a mall' },
  { year:'2023', ua:'5 точок, запуск wellness-лінійки', en:'5 locations, wellness line launches' },
  { year:'2024', ua:'8 точок, маскот, ком\'юніті', en:'8 locations, mascot, community' },
  { year:'2025', ua:'Розширення мережи', en:'Network expansion' },
];

const PHILOSOPHY = [
  { icon:'🏭', ua:'Власне виробництво', en:'Own production', descUa:'90% напоїв готуємо за власними рецептами.', descEn:'90% of drinks are made from our own recipes.' },
  { icon:'🚀', ua:'Швидкість без компромісів', en:'Speed without compromise', descUa:'To-go формат, але преміум якість.', descEn:'To-go format, but premium quality.' },
  { icon:"🤝", ua:"Ком'юніті", en:'Community', descUa:'Ми створюємо культуру, а не просто продаємо напої.', descEn:'We build a culture, not just sell drinks.' },
];

const MASCOT_PHRASES = [
  'Сьогодні відмінний день для матчі! 🍵',
  'Пий бабл ті — будь щасливим! 🧋',
  'Заєць рекомендує: спробуй Bumble Matcha!',
  'Твоя кава чекає на тебе ☕',
  'Wellness-смузі — це турбота про себе 💚',
];

let currentLang = 'ua';

const I18N = {
  ua: {
    'route': 'Маршрут',
    'status.open': 'Відкрито',
    'status.closed': 'Закрито',
    'toast.contact': '✓ Повідомлення надіслано!',
    'toast.review': '✓ Дякуємо за відгук!',
  },
  en: {
    'route': 'Route',
    'status.open': 'Open',
    'status.closed': 'Closed',
    'toast.contact': '✓ Message sent!',
    'toast.review': '✓ Thanks for your review!',
  }
};
const SUPABASE_URL = 'https://ijumdwnwfbikqrdlfacc.supabase.co';
const SUPABASE_KEY = 'sb_publishable_RiFZUqklO0m4wWWsx7QOOw_epeJqESE';

async function loadSupabaseContent(){
  if (!window.supabase) return;
  const client = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);
  const [drinksResult, addressResult, contactsResult] = await Promise.all([
    client.from('drinks').select('*').eq('is_available', true).order('category').order('sort_order'),
    client.from('adres').select('*').limit(1).maybeSingle(),
    client.from('kontakts').select('*').limit(1).maybeSingle()
  ]);

  if (!drinksResult.error && drinksResult.data?.length) {
    const categoryIds = new Set(CATEGORIES.map(category => category.id));
    MENU = drinksResult.data.map(item => ({
      cat: categoryIds.has(item.category) ? item.category : 'kava',
      ua: item.name || '',
      en: item.name || '',
      descUa: item.description || '',
      descEn: item.description || '',
      price: item.price ?? 0,
      badges: [],
      image: item.image_url || ''
    }));
    BESTSELLERS = MENU.slice(0, 6);
  }

  if (!addressResult.error && addressResult.data?.address) {
    LOCATIONS = [{
      name: 'Speshelty',
      addr: addressResult.data.address,
      lat: 50.4501,
      lng: 30.5234,
      open: true
    }];
  }

  if (!contactsResult.error && contactsResult.data) {
    const contacts = contactsResult.data;
    const infoItems = document.querySelectorAll('#contacts .contact-info-item');
    const instagram = infoItems[0]?.querySelector('a');
    const tiktok = infoItems[1]?.querySelector('a');
    const cooperation = infoItems[2]?.querySelector('a');
    if (instagram && contacts.instagram) {
      instagram.href = contacts.instagram;
      instagram.textContent = contacts.instagram.replace(/^https?:\/\/(www\.)?/, '').replace(/\/$/, '');
    }
    if (tiktok && contacts.tiktok) {
      tiktok.href = contacts.tiktok;
      tiktok.textContent = contacts.tiktok.replace(/^https?:\/\/(www\.)?/, '').replace(/\/$/, '');
    }
    if (cooperation && contacts.email) {
      cooperation.href = `mailto:${contacts.email}`;
      cooperation.textContent = contacts.email;
    }
  }
}

function applyTranslations(lang) {
  currentLang = lang;
  document.documentElement.lang = lang === 'ua' ? 'uk' : 'en';

  document.querySelectorAll('.lang-switch button').forEach(button => {
    button.classList.toggle('active', button.dataset.lang === lang);
  });

  if (window.renderDynamicContent) window.renderDynamicContent();
  if (window.translateStaticContent) window.translateStaticContent();

  document.title = lang === 'ua'
    ? 'Speshelty — Tea & Coffee Bar'
    : 'Speshelty — Tea & Coffee Bar';

  document
    .querySelector('meta[name="description"]')
    .setAttribute(
      'content',
      lang === 'ua'
        ? "Speshelty — українська мережа to-go барів. Власне виробництво, американський стиль, українське ком'юніті."
        : 'Speshelty — Ukrainian to-go bar chain. Own production, American style, Ukrainian community.'
    );
}

/* ============================================================
   SPESHELTY — main script
   ============================================================ */
(function(){

  /* ---------- helpers ---------- */
  const $ = (sel, ctx=document) => ctx.querySelector(sel);
  const $$ = (sel, ctx=document) => Array.from(ctx.querySelectorAll(sel));

  function badgeLabel(b){
    const map = { hit:{ua:'HIT',en:'HIT'}, new:{ua:'NEW',en:'NEW'}, wellness:{ua:'WELLNESS',en:'WELLNESS'}, own:{ua:'ВЛАСНЕ',en:'OWN'} };
    return map[b][currentLang];
  }

  function drinkCardHTML(item){
    const style = CAT_STYLE[item.cat];
    const name = currentLang==='ua' ? item.ua : item.en;
    const desc = currentLang==='ua' ? item.descUa : item.descEn;
    return `
      <div class="drink-card">
        <div class="drink-card-inner">
          <div class="drink-photo" style="background:${style.bg}">
            ${item.badges[0] ? `<span class="badge badge-${item.badges[0]}">${badgeLabel(item.badges[0])}</span>` : ''}
            <span>${style.emoji}</span>
          </div>
          <div class="drink-body">
            <h4>${name}</h4>
            <p class="desc">${desc}</p>
            <div class="drink-foot">
              <span class="price">${item.price} ₴</span>
              ${item.addons ? `<span class="addons">${item.addons.join(' ')}</span>` : ''}
            </div>
            ${item.wellness ? `<div class="wellness-note">+ ${currentLang==='ua'?'колаген / протеїн / амінокислоти':'collagen / protein / amino acids'}</div>` : ''}
          </div>
        </div>
      </div>`;
  }

  function menuItemHTML(item){
    const style = CAT_STYLE[item.cat];
    const name = currentLang==='ua' ? item.ua : item.en;
    const desc = currentLang==='ua' ? item.descUa : item.descEn;
    return `
      <div class="menu-item glass" data-cat="${item.cat}">
        <div class="thumb" style="background:${style.bg}">${style.emoji}</div>
        <div class="menu-item-main">
          <div class="menu-item-top">
            <h4>${name}</h4>
            <span class="price">${item.price} ₴</span>
          </div>
          <p class="desc">${desc}${item.wellness ? (currentLang==='ua' ? ' · + колаген/протеїн' : ' · + collagen/protein') : ''}</p>
          ${item.badges.length ? `<div class="badges">${item.badges.map(b=>`<span class="mini-badge badge-${b}">${badgeLabel(b)}</span>`).join('')}</div>` : ''}
        </div>
      </div>`;
  }

  /* ---------- render dynamic content (called on load + lang switch) ---------- */
  let activeCat = 'all';

  function renderBestsellers(){
    $('#bestGrid').innerHTML = BESTSELLERS.map(drinkCardHTML).join('');
  }

  function renderInstaGrid(){
    const emojis = ['📸','☕','🧋','🍵','🐰','✨','🥤','🍰'];
    $('#instaGrid').innerHTML = emojis.map((e,i)=>{
      const hues = ['#FFD70022','#FF408122','#7CB34222','#FF6D0022','#2979FF22'];
      return `<div style="background:${hues[i%hues.length]}; border:1px solid var(--glass-border);">${e}</div>`;
    }).join('');
  }

  function renderMenuTabs(){
    $('#menuTabs').innerHTML = CATEGORIES.map(c=>`
      <button class="menu-tab ${c.id===activeCat?'active':''}" data-cat="${c.id}">${currentLang==='ua'?c.ua:c.en}</button>
    `).join('');
    $$('.menu-tab').forEach(btn=>{
      btn.addEventListener('click', ()=>{
        activeCat = btn.dataset.cat;
        renderMenuTabs();
        renderMenuList();
      });
    });
  }

  function renderMenuList(){
    const wrap = $('#menuContent');
    if(activeCat === 'all'){
      wrap.innerHTML = CATEGORIES.filter(c=>c.id!=='all').map(cat=>{
        const items = MENU.filter(m=>m.cat===cat.id);
        return `
          <div class="category-heading"><h3>${currentLang==='ua'?cat.ua:cat.en}</h3><div class="line"></div></div>
          <div class="menu-list">${items.map(menuItemHTML).join('')}</div>
        `;
      }).join('');
    } else {
      const cat = CATEGORIES.find(c=>c.id===activeCat);
      const items = MENU.filter(m=>m.cat===activeCat);
      wrap.innerHTML = `
        <div class="category-heading"><h3>${currentLang==='ua'?cat.ua:cat.en}</h3><div class="line"></div></div>
        <div class="menu-list">${items.map(menuItemHTML).join('')}</div>
      `;
    }
  }

  function renderTimeline(){
    $('#timeline').innerHTML = TIMELINE.map(t=>`
      <div class="timeline-item">
        <div class="timeline-dot"></div>
        <div><div class="timeline-year">${t.year}</div><div class="timeline-desc">${currentLang==='ua'?t.ua:t.en}</div></div>
      </div>
    `).join('');
  }

  function renderPhilosophy(){
    $('#philosophyGrid').innerHTML = PHILOSOPHY.map(p=>`
      <div class="philosophy-card glass">
        <span class="icon">${p.icon}</span>
        <h4>${currentLang==='ua'?p.ua:p.en}</h4>
        <p>${currentLang==='ua'?p.descUa:p.descEn}</p>
      </div>
    `).join('');
  }

  let selectedLocationIndex = null;

  function renderLocations(){
    $('#locationsList').innerHTML = LOCATIONS.map((loc,i)=>`
      <div class="location-card glass" data-idx="${i}">
        <div>
          <h4>${loc.name}</h4>
          <div class="addr">${loc.addr}</div>
          <a class="route" href="https://www.google.com/maps/search/?api=1&query=${loc.lat},${loc.lng}" target="_blank">${I18N[currentLang]['route']}</a>
        </div>
        <span class="status ${loc.open?'status-open':''}">${loc.open ? I18N[currentLang]['status.open'] : I18N[currentLang]['status.closed']}</span>
      </div>
    `).join('');
    $$('.location-card').forEach(card=>{
      card.addEventListener('click', (e)=>{
        if(e.target.tagName === 'A') return;
        const idx = parseInt(card.dataset.idx);
        focusLocation(idx);
      });
    });
  }

  function renderCommunityCollage(){
    const emojis = ['👥','☕','🐰','🎉','🧋','🥤','✨','💚'];
    const hues = ['#FFD70020','#FF408120','#7CB34220','#FF6D0020','#2979FF20'];
    $('#communityCollage').innerHTML = emojis.map((e,i)=>`<div style="background:${hues[i%hues.length]}; border:1px solid var(--glass-border);">${e}</div>`).join('');
  }

  function renderReviews(){
    $('#reviewsGrid').innerHTML = REVIEWS.map(r=>`
      <div class="review-card glass">
        <div class="review-top">
          <div class="avatar" style="background:${r.color}">${r.name.charAt(0)}</div>
          <div>
            <div class="review-name">${r.name}</div>
            <div class="stars">★★★★★</div>
          </div>
        </div>
        <p class="review-text">${currentLang==='ua'?r.textUa:r.textEn}</p>
        <div class="review-drink">🥤 ${currentLang==='ua'?r.drinkUa:r.drinkEn}</div>
      </div>
    `).join('');
  }

  window.renderDynamicContent = function(){
    renderBestsellers();
    renderMenuTabs();
    renderMenuList();
    renderTimeline();
    renderPhilosophy();
    renderLocations();
    renderReviews();
  };

  /* ---------- header scroll state + active nav link ---------- */
  const header = $('#siteHeader');
  function onScroll(){
    header.classList.toggle('scrolled', window.scrollY > 30);
    const scrollPos = window.scrollY + 140;
    $$('.nav-links a').forEach(a=>{
      const sec = document.querySelector(a.getAttribute('href'));
      if(!sec) return;
      const in_ = scrollPos >= sec.offsetTop && scrollPos < sec.offsetTop + sec.offsetHeight;
      a.classList.toggle('active', in_);
    });
  }
  window.addEventListener('scroll', onScroll, { passive:true });

  /* ---------- burger menu ---------- */
  const burger = $('#burger');
  const navLinks = $('#navLinks');
  burger.addEventListener('click', ()=>{
    burger.classList.toggle('open');
    navLinks.classList.toggle('open');
  });
  $$('.nav-links a').forEach(a=>a.addEventListener('click', ()=>{
    burger.classList.remove('open');
    navLinks.classList.remove('open');
  }));

  /* ---------- language switch ---------- */
  $$('.lang-switch button').forEach(btn=>{
    btn.addEventListener('click', ()=> applyTranslations(btn.dataset.lang));
  });
  $$('[data-lang-footer]').forEach(a=>{
    a.addEventListener('click', (e)=>{ e.preventDefault(); applyTranslations(a.dataset.langFooter); });
  });

  /* ---------- reveal on scroll ---------- */
  const io = new IntersectionObserver((entries)=>{
    entries.forEach(entry=>{
      if(entry.isIntersecting){
        entry.target.classList.add('in');
        io.unobserve(entry.target);
      }
    });
  }, { threshold:0.12 });
  function observeReveals(){ $$('.reveal').forEach(el=>io.observe(el)); }

  /* ---------- mascot easter egg ---------- */
  const mascot = $('#mascot');
  const bubble = $('#mascotBubble');
  let bubbleTimer;
  mascot.addEventListener('click', ()=>{
    mascot.classList.remove('blink'); void mascot.offsetWidth; mascot.classList.add('blink');
    const phrase = MASCOT_PHRASES[Math.floor(Math.random()*MASCOT_PHRASES.length)];
    bubble.textContent = phrase;
    bubble.classList.add('show');
    clearTimeout(bubbleTimer);
    bubbleTimer = setTimeout(()=>bubble.classList.remove('show'), 3200);
  });

  /* ---------- hero bunny follow cursor ---------- */
  const heroBunny = $('#heroBunny');
  const heroEyes = $$('.hero-bunny .bunny-eye .pupil');
  if (heroBunny && heroEyes.length) {
    const heroSection = $('#home');
    const updateHeroBunny = (event) => {
      const rect = heroSection.getBoundingClientRect();
      const px = (event.clientX - rect.left) / rect.width;
      const py = (event.clientY - rect.top) / rect.height;
      const tiltX = (px - 0.5) * 22;
      const tiltY = (0.5 - py) * 20;
      heroBunny.style.transform = `rotateX(${tiltY}deg) rotateY(${tiltX}deg) translate3d(${(px - 0.5) * 18}px, ${(py - 0.5) * 18}px, 0)`;

      const lookX = (px - 0.5) * 10;
      const lookY = (py - 0.5) * 8;
      heroEyes.forEach((eye) => {
        eye.style.setProperty('--look-x', `${lookX}px`);
        eye.style.setProperty('--look-y', `${lookY}px`);
      });
    };

    document.addEventListener('pointermove', updateHeroBunny, { passive: true });
    document.addEventListener('pointerleave', () => {
      heroBunny.style.transform = 'rotateX(0deg) rotateY(0deg) translate3d(0,0,0)';
      heroEyes.forEach((eye) => {
        eye.style.setProperty('--look-x', '0px');
        eye.style.setProperty('--look-y', '0px');
      });
    });
  }

  /* ---------- toast ---------- */
  const toast = $('#toast');
  let toastTimer;
  function showToast(msg){
    toast.textContent = msg;
    toast.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer = setTimeout(()=>toast.classList.remove('show'), 2600);
  }

  /* ---------- contact form ---------- */


  /* ---------- review modal ---------- */
  const reviewModal = $('#reviewModal');
  $('#openReviewModal').addEventListener('click', ()=> reviewModal.classList.add('show'));
  $('#closeReviewModal').addEventListener('click', ()=> reviewModal.classList.remove('show'));
  reviewModal.addEventListener('click', (e)=>{ if(e.target === reviewModal) reviewModal.classList.remove('show'); });

  let selectedStars = 0;
  $$('#starPicker span').forEach(star=>{
    star.addEventListener('click', ()=>{
      selectedStars = parseInt(star.dataset.v);
      $$('#starPicker span').forEach(s=> s.classList.toggle('active', parseInt(s.dataset.v) <= selectedStars));
    });
  });
  $('#reviewForm').addEventListener('submit', (e)=>{
    e.preventDefault();
    reviewModal.classList.remove('show');
    showToast(I18N[currentLang]['toast.review']);
    e.target.reset();
    selectedStars = 0;
    $$('#starPicker span').forEach(s=> s.classList.remove('active'));
  });

  /* ---------- Leaflet map ---------- */
  let map, markers = [];
  function initMap(){
    map = L.map('map', { scrollWheelZoom:false }).setView([50.45, 30.52], 10);
    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
      attribution: '&copy; OpenStreetMap &copy; CARTO',
      maxZoom: 19
    }).addTo(map);

    const bunnyIcon = L.divIcon({
      className:'',
      html:`<div style="width:34px;height:34px;border-radius:50%;background:linear-gradient(135deg,#FFD700,#FF6D00,#FF4081);display:flex;align-items:center;justify-content:center;font-size:16px;box-shadow:0 0 14px rgba(255,215,0,0.6);border:2px solid #0A0A0A;">🐰</div>`,
      iconSize:[34,34], iconAnchor:[17,17]
    });

    LOCATIONS.forEach((loc,i)=>{
      const m = L.marker([loc.lat, loc.lng], { icon:bunnyIcon }).addTo(map);
      m.bindPopup(`<strong>${loc.name}</strong><br>${loc.addr}<br><a href="https://www.google.com/maps/search/?api=1&query=${loc.lat},${loc.lng}" target="_blank">${I18N[currentLang]['route']}</a>`);
      m.on('click', ()=> focusLocation(i, false));
      markers.push(m);
    });
  }

  function translateStaticContent() {
  const translations = [
    ['.nav-links a', ['Головна','Меню','Про нас','Локації',"Ком'юніті",'Відгуки','Контакти'], ['Home','Menu','About','Locations','Community','Reviews','Contacts']],
    ['.hero-eyebrow', 'Own Production · Kyiv, Ukraine', 'Own Production · Kyiv, Ukraine'],
    ['.hero-sub', 'TEA & COFFEE BAR', 'TEA & COFFEE BAR'],
    ['.hero-tagline', "Власне виробництво. Американський стиль. Українське ком'юніті.", 'Own production. American style. Ukrainian community.'],
    ['.hero-cta a', ['Переглянути меню','Знайти точку'], ['View menu','Find a location']],
    ['.hero-scroll span:first-child', 'Гортай далі', 'Scroll down'],

    ['.feature-card h4', ['Швидкість','Фірмові напої','Wellness','Spesh Vibe'], ['Speed','Signature drinks','Wellness','Spesh Vibe']],
    ['.feature-card p', ['To-go формат — твій напій готовий за 2 хвилини.','90% меню — власна розробка, яку не знайдеш більше ніде.','Колаген, протеїн та амінокислоти у наших смузі.','Унікальний стиль, характер і зайчик з чашкою в лапах.'], ['To-go format — your drink is ready in 2 minutes.','90% of the menu is our own creation.','Collagen, protein and amino acids in our smoothies.','Unique style, character and a bunny with a cup.']],

    ['#bestsellers .eyebrow', 'Must Try', 'Must Try'],
    ['#bestsellers h2', 'Хіти продажів', 'Best sellers'],
    ['#bestsellers .section-head p', 'Те, що замовляють найчастіше — і не дарма.', 'The drinks our customers order most often — for good reason.'],
    ['#bestsellers .menu-cta a', 'Все меню', 'Full menu'],

    ['#instagram h2', 'Слідкуй за нами', 'Follow us'],
    ['#instagram .btn', 'Більше в Instagram', 'More on Instagram'],

    ['#menu h2', 'Меню', 'Menu'],
    ['#menu .section-head p', 'Кожен напій — фірмовий рецепт Speshelty.', 'Every drink is a signature Speshelty recipe.'],

    ['.about-hero .eyebrow', 'Since 2022', 'Since 2022'],
    ['.about-hero h1', 'Ми — Speshelty', 'We are Speshelty'],
    ['.story-text .eyebrow', 'Наша історія', 'Our story'],
    ['.story-text h2', "Від ідеї до ком'юніті", 'From an idea to a community'],
    ['.story-text p', "Speshelty народився з ідеї поєднати американську культуру to-go напоїв з українською любов'ю до якості. Ми створили простір, де кожен напій — це фірмовий рецепт, кожна банка — частинка нашого стилю, а кожен клієнт — частина ком'юніті.", 'Speshelty was born from the idea of combining American to-go drink culture with Ukrainian love for quality. We created a space where every drink is a signature recipe, every can is part of our style, and every customer is part of the community.'],
    ['#about .section-head h2', 'Філософія', 'Philosophy'],
    ['.mascot-story h3', 'Чорний Заєць', 'The Black Bunny'],
    ['.mascot-story p', 'Чорний заєць — душа Speshelty. Він трохи бунтар, трохи мрійник і завжди з чашкою улюбленого напою. Як і ми.', 'The Black Bunny is the soul of Speshelty. A little rebellious, a little dreamy, and always with a cup of his favourite drink. Just like us.'],

    ['#locations h2', 'Знайди свою точку', 'Find your location'],

    ['#community h2', "Ком'юніті", 'Community'],
    ['#community .section-head p', 'Ми створюємо культуру, а не просто продаємо напої.', 'We create culture, not just sell drinks.'],
    ['.glovo-block h3', 'Не можеш завітати?', "Can't visit us?"],
    ['.glovo-block p', 'Замовляй улюблені напої через Glovo. Доставка доступна з обраних точок — шукай «Speshelty» у додатку.', 'Order your favourite drinks through Glovo. Delivery is available from selected locations — search for “Speshelty” in the app.'],
    ['.glovo-block .btn', 'Замовити на Glovo', 'Order on Glovo'],

    ['#reviews h2', 'Що кажуть наші клієнти', 'What our customers say'],
    ['#openReviewModal', 'Залишити відгук', 'Leave a review'],

    ['#contacts h2', "Зв'яжись з нами", 'Contact us'],
    ['#contactForm label', ['Ім’я','Email','Тема','Повідомлення'], ['Name','Email','Subject','Message']],
    ['#contactForm button', 'Надіслати', 'Send'],

    ['.footer-col h5', ['Навігація','Мова'], ['Navigation','Language']],
    ['.footer-bottom span:last-child', 'Працюємо в ТРЦ та Епіцентрах по всій Україні', 'Available in malls and Epicentr stores across Ukraine'],

    ['#reviewModal h3', 'Залишити відгук', 'Leave a review'],
    ['#reviewForm label', ['Ім’я','Оцінка','Твій відгук'], ['Name','Rating','Your review']],
    ['#reviewForm button', 'Надіслати відгук', 'Submit review']
  ];

  translations.forEach(([selector, ua, en]) => {
    const elements = document.querySelectorAll(selector);
    elements.forEach((element, index) => {
      const value = Array.isArray(ua)
        ? (currentLang === 'ua' ? ua[index] : en[index])
        : (currentLang === 'ua' ? ua : en);

      if (value !== undefined) element.textContent = value;
    });
  });

  document.querySelector('#mascot').title =
    currentLang === 'ua' ? 'Клікни на зайчика' : 'Click the bunny';

  document.querySelectorAll('#contactForm input, #contactForm textarea').forEach((input, index) => {
    input.placeholder = currentLang === 'ua'
      ? ['Ваше ім’я', 'Ваш email', 'Тема повідомлення', 'Ваше повідомлення'][index]
      : ['Your name', 'Your email', 'Message subject', 'Your message'][index];
  });
}

window.translateStaticContent = translateStaticContent;

  /* ---------- init ---------- */
  document.addEventListener('DOMContentLoaded', async ()=>{
    await loadSupabaseContent();
    renderCommunityCollage();
    renderDynamicContent();
    observeReveals();
    onScroll();
    initMap();
    applyTranslations('ua');
  });

})();
</script>
</body>
</html>
