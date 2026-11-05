<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MPI Berpikir Komputasional - Informatika Kelas 7</title>
<style>
  :root{
    --bg:#0A0E27; --bg2:#111633;
    --cyan:#00F5FF; --magenta:#FF00C8; --purple:#7B2FF7;
    --green:#00FF88; --red:#FF4D6D; --yellow:#FFD93D; --orange:#FF9F1C;
    --text:#E8ECFF; --muted:#8892B0;
    --glass:rgba(255,255,255,0.06);
    --glass-border:rgba(255,255,255,0.12);
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  body{
    font-family:'Segoe UI',system-ui,-apple-system,sans-serif;
    background:var(--bg); color:var(--text);
    min-height:100vh; overflow-x:hidden; position:relative;
  }
  body::before{
    content:''; position:fixed; inset:0; z-index:-2;
    background:
      radial-gradient(circle at 15% 20%, rgba(0,245,255,0.12), transparent 40%),
      radial-gradient(circle at 85% 80%, rgba(255,0,200,0.12), transparent 40%),
      radial-gradient(circle at 50% 50%, rgba(123,47,247,0.08), transparent 60%);
    animation:pulseBg 8s ease-in-out infinite alternate;
  }
  body::after{
    content:''; position:fixed; inset:0; z-index:-1;
    background-image:
      linear-gradient(rgba(0,245,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,245,255,0.03) 1px, transparent 1px);
    background-size:50px 50px;
    mask-image:radial-gradient(circle at center, black 30%, transparent 75%);
  }
  @keyframes pulseBg{0%{opacity:.6;}100%{opacity:1;}}

  .app{display:grid;grid-template-columns:280px 1fr;min-height:100vh;max-width:1400px;margin:0 auto;}

  /* SIDEBAR */
  .sidebar{background:rgba(10,14,39,.85);backdrop-filter:blur(20px);border-right:1px solid var(--glass-border);padding:24px 16px;position:sticky;top:0;height:100vh;overflow-y:auto;}
  .brand{display:flex;align-items:center;gap:10px;padding:8px 12px 20px;border-bottom:1px solid var(--glass-border);margin-bottom:20px;}
  .brand-icon{width:40px;height:40px;border-radius:12px;background:linear-gradient(135deg,var(--cyan),var(--magenta));display:flex;align-items:center;justify-content:center;font-size:22px;box-shadow:0 0 20px rgba(0,245,255,.4);}
  .brand-text h1{font-size:13px;font-weight:800;letter-spacing:.5px;background:linear-gradient(90deg,var(--cyan),var(--magenta));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;}
  .brand-text p{font-size:10px;color:var(--muted);letter-spacing:1px;}

  .nav-list{list-style:none;display:flex;flex-direction:column;gap:6px;}
  .nav-item{display:flex;align-items:center;gap:12px;padding:12px 14px;border-radius:10px;cursor:pointer;transition:all .25s;font-size:13px;color:var(--muted);border:1px solid transparent;position:relative;}
  .nav-item:hover{background:var(--glass);color:var(--text);transform:translateX(4px);}
  .nav-item.active{background:linear-gradient(90deg,rgba(0,245,255,.15),rgba(255,0,200,.1));color:var(--text);border-color:rgba(0,245,255,.3);box-shadow:0 0 20px rgba(0,245,255,.15);}
  .nav-item.active::before{content:'';position:absolute;left:0;top:20%;bottom:20%;width:3px;border-radius:2px;background:linear-gradient(180deg,var(--cyan),var(--magenta));}
  .nav-num{width:26px;height:26px;border-radius:8px;background:var(--glass);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700;flex-shrink:0;}
  .nav-item.active .nav-num{background:linear-gradient(135deg,var(--cyan),var(--magenta));color:#0A0E27;}
  .nav-item.done .nav-num{background:rgba(0,255,136,.2);color:var(--green);}

  .progress-wrap{margin-top:24px;padding:14px;background:var(--glass);border-radius:12px;border:1px solid var(--glass-border);}
  .progress-label{display:flex;justify-content:space-between;font-size:11px;color:var(--muted);margin-bottom:8px;}
  .progress-bar{height:6px;background:rgba(255,255,255,.08);border-radius:3px;overflow:hidden;}
  .progress-fill{height:100%;width:0%;background:linear-gradient(90deg,var(--cyan),var(--magenta));border-radius:3px;transition:width .5s ease;box-shadow:0 0 10px var(--cyan);}

  /* MAIN */
  .main{padding:32px 40px 60px;overflow-y:auto;min-height:100vh;}
  .topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:24px;}
  .topbar-info{font-size:12px;color:var(--muted);letter-spacing:.5px;}
  .topbar-info b{color:var(--cyan);}
  .sound-btn{background:var(--glass);border:1px solid var(--glass-border);color:var(--text);width:40px;height:40px;border-radius:10px;cursor:pointer;font-size:18px;transition:all .2s;}
  .sound-btn:hover{background:rgba(0,245,255,.15);border-color:var(--cyan);}

  /* SECTIONS */
  .section{display:none;animation:fadeIn .4s ease;}
  .section.active{display:block;}
  @keyframes fadeIn{from{opacity:0;transform:translateY(10px);}to{opacity:1;transform:translateY(0);}}
  .section-header{margin-bottom:28px;}
  .section-tag{display:inline-block;padding:6px 14px;background:linear-gradient(90deg,rgba(0,245,255,.15),rgba(255,0,200,.15));border:1px solid rgba(0,245,255,.3);border-radius:20px;font-size:11px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;color:var(--cyan);margin-bottom:14px;}
  .section-title{font-size:32px;font-weight:800;background:linear-gradient(90deg,#fff,var(--cyan));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:8px;}
  .section-desc{color:var(--muted);font-size:14px;max-width:750px;line-height:1.6;}

  .card{background:var(--glass);border:1px solid var(--glass-border);border-radius:16px;padding:24px;backdrop-filter:blur(10px);margin-bottom:16px;transition:all .3s;}
  .card:hover{border-color:rgba(0,245,255,.3);}
  .card h3{font-size:16px;margin-bottom:10px;color:var(--text);}
  .card p{color:var(--muted);font-size:13.5px;line-height:1.7;margin-bottom:10px;}

  .btn{display:inline-flex;align-items:center;gap:8px;padding:12px 24px;border:none;border-radius:10px;font-size:13px;font-weight:700;cursor:pointer;transition:all .25s;letter-spacing:.5px;font-family:inherit;}
  .btn-primary{background:linear-gradient(135deg,var(--cyan),var(--purple));color:#0A0E27;box-shadow:0 4px 20px rgba(0,245,255,.3);}
  .btn-primary:hover{transform:translateY(-2px);box-shadow:0 8px 24px rgba(0,245,255,.5);}
  .btn-magenta{background:linear-gradient(135deg,var(--magenta),var(--purple));color:white;box-shadow:0 4px 20px rgba(255,0,200,.3);}
  .btn-magenta:hover{transform:translateY(-2px);}
  .btn-ghost{background:var(--glass);border:1px solid var(--glass-border);color:var(--text);}
  .btn-ghost:hover{background:rgba(0,245,255,.1);border-color:var(--cyan);}

  /* ===== APERSEPSI ===== */
  .mission-box{text-align:center;padding:40px 24px;background:linear-gradient(135deg,rgba(255,0,200,.08),rgba(0,245,255,.08));border:1px solid rgba(0,245,255,.25);border-radius:20px;margin-bottom:24px;position:relative;overflow:hidden;}
  .mission-box::before{content:'';position:absolute;inset:0;background:radial-gradient(circle at center,rgba(255,0,200,.15),transparent 70%);animation:pulseBg 3s ease-in-out infinite alternate;pointer-events:none;}
  .robot{font-size:64px;display:inline-block;animation:bounce 2s ease-in-out infinite;margin-bottom:12px;}
  @keyframes bounce{0%,100%{transform:translateY(0);}50%{transform:translateY(-10px);}}
  .mission-box h2{font-size:22px;margin-bottom:10px;color:var(--cyan);}
  .mission-box p{color:var(--text);font-size:14.5px;line-height:1.7;max-width:620px;margin:0 auto 16px;}

  .boxes-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:12px;margin:24px 0;}
  .mystery-box{aspect-ratio:1;background:linear-gradient(135deg,#1a1f4a,#2a2f6a);border:2px solid var(--glass-border);border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:32px;cursor:pointer;transition:all .3s;position:relative;overflow:hidden;}
  .mystery-box:hover:not(.opened):not(.locked){border-color:var(--cyan);transform:translateY(-4px) scale(1.05);box-shadow:0 0 30px rgba(0,245,255,.4);}
  .mystery-box.opened{background:linear-gradient(135deg,rgba(0,245,255,.2),rgba(255,0,200,.2));border-color:var(--cyan);}
  .mystery-box.locked{opacity:.4;cursor:not-allowed;}
  .mystery-box.winner{background:linear-gradient(135deg,rgba(0,255,136,.3),rgba(0,245,255,.3));border-color:var(--green);animation:glow 1s ease-in-out infinite alternate;}
  @keyframes glow{from{box-shadow:0 0 20px rgba(0,255,136,.5);}to{box-shadow:0 0 40px rgba(0,255,136,.8);}}
  .timer-display{font-family:'Courier New',monospace;font-size:32px;font-weight:800;color:var(--magenta);text-shadow:0 0 20px rgba(255,0,200,.5);margin:16px 0;}

  /* ===== SOAL ===== */
  .question-card{background:var(--glass);border:1px solid var(--glass-border);border-radius:16px;padding:28px;margin-bottom:16px;}
  .q-num{display:inline-block;padding:4px 12px;background:linear-gradient(90deg,var(--cyan),var(--magenta));color:#0A0E27;font-weight:800;font-size:11px;border-radius:20px;margin-bottom:12px;letter-spacing:1px;}
  .q-text{font-size:15px;font-weight:600;margin-bottom:18px;line-height:1.5;}
  .options{display:flex;flex-direction:column;gap:10px;}
  .option{padding:14px 18px;background:rgba(255,255,255,.03);border:1.5px solid var(--glass-border);border-radius:10px;cursor:pointer;transition:all .2s;font-size:13.5px;display:flex;align-items:center;gap:12px;}
  .option:hover{border-color:var(--cyan);background:rgba(0,245,255,.05);}
  .option.selected{border-color:var(--cyan);background:rgba(0,245,255,.12);}
  .option.correct{border-color:var(--green);background:rgba(0,255,136,.12);color:var(--green);}
  .option.wrong{border-color:var(--red);background:rgba(255,77,109,.12);color:var(--red);}
  .option-letter{width:28px;height:28px;border-radius:8px;background:var(--glass);display:flex;align-items:center;justify-content:center;font-weight:800;font-size:12px;flex-shrink:0;}
  .option.correct .option-letter{background:var(--green);color:#0A0E27;}
  .option.wrong .option-letter{background:var(--red);color:white;}

  .result-box{text-align:center;padding:32px 24px;background:linear-gradient(135deg,rgba(0,245,255,.1),rgba(255,0,200,.1));border:2px solid var(--cyan);border-radius:20px;margin-bottom:20px;}
  .result-score{font-size:64px;font-weight:900;background:linear-gradient(90deg,var(--cyan),var(--magenta));-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;margin:12px 0;}
  .result-msg{font-size:15px;color:var(--text);margin-bottom:16px;line-height:1.6;}

  /* ===== MATERI ===== */
  .materi-nav{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:24px;}
  .materi-tab{padding:10px 18px;background:var(--glass);border:1.5px solid var(--glass-border);border-radius:10px;cursor:pointer;font-size:13px;font-weight:600;transition:all .2s;color:var(--muted);}
  .materi-tab:hover{border-color:var(--cyan);color:var(--text);}
  .materi-tab.active{background:linear-gradient(135deg,rgba(0,245,255,.2),rgba(255,0,200,.15));border-color:var(--cyan);color:var(--cyan);box-shadow:0 0 20px rgba(0,245,255,.2);}
  .materi-content{display:none;animation:fadeIn .4s ease;}
  .materi-content.active{display:block;}
  .materi-hero{display:flex;align-items:center;gap:16px;margin-bottom:20px;padding:22px;background:linear-gradient(135deg,rgba(0,245,255,.1),rgba(255,0,200,.05));border-radius:16px;border:1px solid rgba(0,245,255,.25);}
  .materi-hero-icon{font-size:52px;}
  .materi-hero-text h3{font-size:22px;color:var(--cyan);margin-bottom:4px;}
  .materi-hero-text p{font-size:13px;color:var(--muted);}
  .materi-section{background:var(--glass);border:1px solid var(--glass-border);border-radius:14px;padding:20px;margin-bottom:14px;}
  .materi-section h4{font-size:13px;color:var(--cyan);margin-bottom:10px;letter-spacing:.8px;text-transform:uppercase;display:flex;align-items:center;gap:6px;}
  .materi-section p{color:var(--text);font-size:13.5px;line-height:1.75;margin-bottom:8px;}
  .materi-section ul{margin-left:20px;color:var(--text);font-size:13.5px;line-height:1.8;}
  .materi-section li{margin-bottom:4px;}

  .analogy-box{background:rgba(255,217,61,.1);border-left:4px solid var(--yellow);padding:14px 18px;border-radius:10px;margin:10px 0;font-size:13.5px;line-height:1.6;}
  .example-box{background:rgba(0,255,136,.08);border-left:4px solid var(--green);padding:14px 18px;border-radius:10px;margin:10px 0;font-size:13.5px;line-height:1.6;}

  /* VISUAL DIAGRAM */
  .diagram-box{background:rgba(0,245,255,.05);border:1px dashed rgba(0,245,255,.35);border-radius:12px;padding:20px;margin:12px 0;font-family:'Courier New',monospace;font-size:12px;color:var(--cyan);white-space:pre;overflow-x:auto;line-height:1.7;}
  .diagram-caption{text-align:center;font-size:11px;color:var(--muted);margin-top:8px;font-style:italic;}

  /* BAR CHART */
  .bar-chart{display:flex;flex-direction:column;gap:12px;margin:16px 0;padding:16px;background:rgba(0,0,0,.2);border-radius:12px;}
  .bar-row{display:flex;align-items:center;gap:12px;font-size:12px;}
  .bar-label{width:120px;color:var(--text);font-weight:600;}
  .bar-track{flex:1;height:22px;background:rgba(255,255,255,.05);border-radius:6px;overflow:hidden;}
  .bar-fill{height:100%;border-radius:6px;display:flex;align-items:center;justify-content:flex-end;padding-right:8px;font-size:11px;font-weight:700;color:#0A0E27;transition:width 1s ease;}
  .bar-fill.c1{background:linear-gradient(90deg,var(--cyan),#00B8D4);}
  .bar-fill.c2{background:linear-gradient(90deg,var(--magenta),#B0008C);}
  .bar-fill.c3{background:linear-gradient(90deg,var(--green),#00B85F);}
  .bar-fill.c4{background:linear-gradient(90deg,var(--yellow),#B89800);}

  /* FLOWCHART visual */
  .flowchart{display:flex;flex-direction:column;align-items:center;gap:8px;padding:16px;background:rgba(0,0,0,.2);border-radius:12px;margin:12px 0;}
  .flow-node{padding:10px 22px;background:linear-gradient(135deg,rgba(0,245,255,.2),rgba(123,47,247,.15));border:1.5px solid var(--cyan);border-radius:10px;font-size:12.5px;font-weight:600;color:var(--text);text-align:center;min-width:140px;}
  .flow-node.start{background:linear-gradient(135deg,rgba(0,255,136,.2),rgba(0,245,255,.15));border-color:var(--green);}
  .flow-node.end{background:linear-gradient(135deg,rgba(255,0,200,.2),rgba(255,77,109,.15));border-color:var(--magenta);}
  .flow-arrow{color:var(--cyan);font-size:18px;line-height:1;}

  /* PIZZA VISUAL */
  .visual-row{display:flex;gap:16px;align-items:center;flex-wrap:wrap;justify-content:center;padding:16px;background:rgba(0,0,0,.2);border-radius:12px;margin:12px 0;}
  .visual-item{text-align:center;font-size:12px;color:var(--muted);}
  .visual-item .big{font-size:48px;display:block;margin-bottom:6px;}
  .visual-plus{font-size:24px;color:var(--cyan);}

  /* SIMULASI */
  .sim-container{background:linear-gradient(135deg,rgba(123,47,247,.1),rgba(0,245,255,.05));border:1px solid rgba(0,245,255,.25);border-radius:16px;padding:24px;margin-bottom:16px;}
  .sim-title{display:flex;align-items:center;gap:10px;font-size:16px;font-weight:700;margin-bottom:16px;color:var(--cyan);}
  .steps-list{display:flex;flex-direction:column;gap:8px;margin:16px 0;}
  .step-item{padding:12px 16px;background:rgba(255,255,255,.03);border:1.5px solid var(--glass-border);border-radius:10px;cursor:grab;display:flex;align-items:center;gap:12px;font-size:13px;transition:all .2s;user-select:none;}
  .step-item:active{cursor:grabbing;}
  .step-item.dragging{opacity:.4;}
  .step-item.correct{border-color:var(--green);background:rgba(0,255,136,.1);}
  .step-item.wrong{border-color:var(--red);background:rgba(255,77,109,.1);}
  .step-item .drag-handle{color:var(--muted);font-size:14px;cursor:grab;}

  .pattern-display{display:flex;gap:12px;flex-wrap:wrap;margin:16px 0;align-items:center;}
  .pattern-item{padding:14px 22px;background:linear-gradient(135deg,rgba(0,245,255,.15),rgba(255,0,200,.1));border:2px solid rgba(0,245,255,.4);border-radius:12px;font-size:20px;font-weight:800;color:var(--cyan);min-width:60px;text-align:center;}
  .pattern-item.unknown{background:rgba(255,0,200,.15);border-color:var(--magenta);color:var(--magenta);animation:pulse 1.5s ease-in-out infinite;}
  @keyframes pulse{0%,100%{transform:scale(1);}50%{transform:scale(1.05);}}

  .robot-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:4px;max-width:300px;margin:16px auto;background:rgba(0,0,0,.3);padding:8px;border-radius:12px;}
  .cell{aspect-ratio:1;background:rgba(255,255,255,.05);border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:18px;}
  .cell.robot{background:rgba(0,245,255,.3);border:1px solid var(--cyan);}
  .cell.target{background:rgba(0,255,136,.3);border:1px solid var(--green);}
  .cell.obstacle{background:rgba(255,77,109,.3);}
  .controls{display:flex;gap:8px;justify-content:center;flex-wrap:wrap;margin-top:12px;}
  .ctrl-btn{padding:12px 18px;background:var(--glass);border:1px solid var(--glass-border);border-radius:10px;color:var(--text);cursor:pointer;font-size:18px;transition:all .2s;}
  .ctrl-btn:hover{border-color:var(--cyan);background:rgba(0,245,255,.1);}

  /* REFLEKSI */
  .emoji-picker{display:flex;gap:12px;flex-wrap:wrap;margin:12px 0;}
  .emoji-opt{width:56px;height:56px;display:flex;align-items:center;justify-content:center;font-size:28px;background:var(--glass);border:2px solid var(--glass-border);border-radius:14px;cursor:pointer;transition:all .2s;}
  .emoji-opt:hover{border-color:var(--cyan);transform:scale(1.1);}
  .emoji-opt.selected{border-color:var(--cyan);background:rgba(0,245,255,.15);box-shadow:0 0 20px rgba(0,245,255,.4);transform:scale(1.15);}

  textarea{width:100%;padding:14px;background:rgba(255,255,255,.03);border:1.5px solid var(--glass-border);border-radius:10px;color:var(--text);font-family:inherit;font-size:13.5px;resize:vertical;min-height:80px;transition:border-color .2s;}
  textarea:focus{outline:none;border-color:var(--cyan);}
  input[type="range"]{width:100%;accent-color:var(--cyan);height:6px;margin:10px 0;}
  .slider-value{font-size:24px;font-weight:800;color:var(--cyan);text-align:center;margin-top:6px;}
  select{width:100%;padding:12px;background:rgba(255,255,255,.05);border:1.5px solid var(--glass-border);border-radius:10px;color:var(--text);font-family:inherit;font-size:13.5px;margin-top:8px;}

  .reflection-card{background:linear-gradient(135deg,rgba(0,245,255,.1),rgba(255,0,200,.1));border:2px solid var(--cyan);border-radius:16px;padding:24px;margin-top:16px;}
  .reflection-card h4{color:var(--cyan);margin-bottom:6px;font-size:12px;letter-spacing:.5px;text-transform:uppercase;}
  .reflection-card p{margin-bottom:14px;font-size:14px;}

  /* TOAST */
  .toast{position:fixed;bottom:24px;right:24px;padding:14px 22px;background:linear-gradient(135deg,var(--cyan),var(--purple));color:#0A0E27;font-weight:700;font-size:13px;border-radius:12px;box-shadow:0 8px 30px rgba(0,245,255,.4);transform:translateY(100px);opacity:0;transition:all .3s;z-index:2000;}
  .toast.show{transform:translateY(0);opacity:1;}

  @media (max-width:768px){
    .app{grid-template-columns:1fr;}
    .sidebar{position:relative;height:auto;padding:16px;border-right:none;border-bottom:1px solid var(--glass-border);}
    .nav-list{flex-direction:row;overflow-x:auto;padding-bottom:8px;}
    .nav-item{flex-shrink:0;padding:8px 12px;font-size:11px;}
    .nav-label{display:none;}
    .main{padding:20px 16px 40px;}
    .section-title{font-size:22px;}
    .boxes-grid{grid-template-columns:repeat(5,1fr);gap:6px;}
    .mystery-box{font-size:20px;}
    .progress-wrap{display:none;}
    .bar-label{width:80px;font-size:11px;}
  }
  .highlight{color:var(--cyan);font-weight:700;}
  .highlight-m{color:var(--magenta);font-weight:700;}
  .highlight-g{color:var(--green);font-weight:700;}
</style>
</head>
<body>

<div class="app">
  <aside class="sidebar">
    <div class="brand">
      <div class="brand-icon">⚡</div>
      <div class="brand-text">
        <h1>BERPIKIR KOMPUTASIONAL</h1>
        <p>INFORMATIKA KELAS 7</p>
      </div>
    </div>

    <ul class="nav-list" id="navList">
      <li class="nav-item active" data-section="apersepsi"><span class="nav-num">1</span><span class="nav-label">🎬 Apersepsi</span></li>
      <li class="nav-item" data-section="asesmen-awal"><span class="nav-num">2</span><span class="nav-label">📊 Asesmen Awal</span></li>
      <li class="nav-item" data-section="materi"><span class="nav-num">3</span><span class="nav-label">📚 Materi Pokok</span></li>
      <li class="nav-item" data-section="simulasi"><span class="nav-num">4</span><span class="nav-label">🧪 Simulasi</span></li>
      <li class="nav-item" data-section="kuis"><span class="nav-num">5</span><span class="nav-label">🎯 Kuis</span></li>
      <li class="nav-item" data-section="asesmen-akhir"><span class="nav-num">6</span><span class="nav-label">🏆 Asesmen Akhir</span></li>
      <li class="nav-item" data-section="refleksi"><span class="nav-num">7</span><span class="nav-label">🪞 Refleksi</span></li>
    </ul>

    <div class="progress-wrap">
      <div class="progress-label"><span>Progres Belajar</span><span id="progressPct">0%</span></div>
      <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
    </div>
  </aside>

  <main class="main">
    <div class="topbar">
      <div class="topbar-info">🎓 Selamat datang, <b>Agen Muda</b>! | Misi hari ini: kuasai 4 pilar berpikir komputasional.</div>
      <button class="sound-btn" id="soundBtn" title="Suara: ON">🔊</button>
    </div>

    <!-- ============================================================ -->
    <!-- [1] APERSEPSI — memancing rasa penasaran, BUKAN menjelaskan -->
    <!-- ============================================================ -->
    <section class="section active" id="apersepsi">
      <div class="section-header">
        <span class="section-tag">Misi 01 · Apersepsi</span>
        <h2 class="section-title">🎬 Misi Rahasia</h2>
        <p class="section-desc">Baca misi dari robot, lalu pilih kotakmu. Tidak ada jawaban salah — yang penting kamu punya alasan.</p>
      </div>

      <div class="mission-box">
        <div class="robot">🤖</div>
        <h2>Halo, Agen!</h2>
        <p>
          Ada <b class="highlight">5 kotak misterius</b> di depanku. Salah satunya berisi
          <b class="highlight-m">kunci keluar dari ruangan ini</b>. Kamu hanya boleh membuka
          <b class="highlight-g">3 kotak</b>. Kotak mana yang kamu pilih? Kenapa?
        </p>
        <div class="timer-display" id="aperTimer">00:30</div>
        <p style="font-size:12px;color:var(--muted);">Sambil berpikir, coba pikirkan: apakah kamu menebak, mencari pola, atau punya strategi?</p>
      </div>

      <div class="boxes-grid" id="boxesGrid">
        <div class="mystery-box" data-idx="0">❓</div>
        <div class="mystery-box" data-idx="1">❓</div>
        <div class="mystery-box" data-idx="2">❓</div>
        <div class="mystery-box" data-idx="3">❓</div>
        <div class="mystery-box" data-idx="4">❓</div>
      </div>

      <div class="card" id="aperFeedback" style="display:none;">
        <h3>🔍 Refleksi Singkat</h3>
        <p id="aperFeedbackText"></p>
        <p style="margin-top:10px;color:var(--cyan);font-weight:600;">Ternyata ada cara berpikir yang dipakai para ilmuwan komputer untuk memecahkan masalah seperti ini. Mau tahu rahasianya? 👇</p>
      </div>

      <button class="btn btn-primary" onclick="goTo('asesmen-awal')">🚀 Mulai Petualangan</button>
    </section>

    <!-- ============================================================ -->
    <!-- [2] ASESMEN AWAL — mengukur pemahaman awal -->
    <!-- ============================================================ -->
    <section class="section" id="asesmen-awal">
      <div class="section-header">
        <span class="section-tag">Misi 02 · Asesmen Awal</span>
        <h2 class="section-title">📊 Cek Radar Pemahaman</h2>
        <p class="section-desc">Jawab dengan jujur. Ini bukan ujian — hanya untuk tahu sejauh mana kamu sudah paham.</p>
      </div>
      <div id="asesmenAwalContainer"></div>
      <button class="btn btn-primary" id="btnAsesmenAwal" style="margin-top:8px;">Kirim Jawaban & Lihat Radar</button>
      <div id="asesmenAwalResult" style="margin-top:16px;"></div>
    </section>

    <!-- ============================================================ -->
    <!-- [3] MATERI POKOK — LENGKAP dengan gambar, diagram, grafik   -->
    <!-- ============================================================ -->
    <section class="section" id="materi">
      <div class="section-header">
        <span class="section-tag">Misi 03 · Materi Pokok</span>
        <h2 class="section-title">📚 4 Pilar Berpikir Komputasional</h2>
        <p class="section-desc">Pelajari materi lengkap: definisi, analogi, contoh, diagram visual, dan grafik. Klik setiap pilar untuk membuka materinya.</p>
      </div>

      <div class="materi-nav">
        <div class="materi-tab active" data-tab="pengantar">🌟 Pengantar</div>
        <div class="materi-tab" data-tab="dekomposisi">🧩 Dekomposisi</div>
        <div class="materi-tab" data-tab="pola">🔁 Pengenalan Pola</div>
        <div class="materi-tab" data-tab="abstraksi">🎯 Abstraksi</div>
        <div class="materi-tab" data-tab="algoritma">📋 Algoritma</div>
        <div class="materi-tab" data-tab="rangkuman">📝 Rangkuman</div>
      </div>

      <!-- PENGANTAR -->
      <div class="materi-content active" id="tab-pengantar">
        <div class="materi-hero">
          <div class="materi-hero-icon">🌟</div>
          <div class="materi-hero-text">
            <h3>Apa itu Berpikir Komputasional?</h3>
            <p>Mengenal cara berpikir yang dipakai para ilmuwan komputer</p>
          </div>
        </div>
        <div class="materi-section">
          <h4>📖 Definisi</h4>
          <p><b>Berpikir komputasional (Computational Thinking)</b> adalah proses berpikir sistematis untuk memecahkan masalah — dengan cara yang mirip bagaimana komputer memproses informasi. Ini bukan berarti kita berpikir <i>seperti</i> komputer, melainkan <b>menggunakan cara berpikir yang terstruktur</b> agar solusi kita bisa dijalankan oleh manusia maupun komputer.</p>
        </div>
        <div class="materi-section">
          <h4>🌍 Mengapa Penting?</h4>
          <ul>
            <li><b>Memecahkan masalah sehari-hari</b> — dari menyusun jadwal sampai mengatur uang saku.</li>
            <li><b>Belajar lebih efektif</b> — memecah materi besar menjadi bagian kecil.</li>
            <li><b>Masa depan</b> — hampir semua bidang pekerjaan modern membutuhkan kemampuan ini.</li>
          </ul>
        </div>
        <div class="materi-section">
          <h4>🧩 4 Pilar yang Akan Kita Pelajari</h4>
          <div class="visual-row">
            <div class="visual-item"><span class="big">🧩</span>Dekomposisi</div>
            <div class="visual-plus">+</div>
            <div class="visual-item"><span class="big">🔁</span>Pengenalan Pola</div>
            <div class="visual-plus">+</div>
            <div class="visual-item"><span class="big">🎯</span>Abstraksi</div>
            <div class="visual-plus">+</div>
            <div class="visual-item"><span class="big">📋</span>Algoritma</div>
          </div>
        </div>
        <div class="materi-section">
          <h4>📊 Diagram Alur Berpikir Komputasional</h4>
          <div class="flowchart">
            <div class="flow-node start">MASALAH</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">🧩 Dekomposisi (pecah masalah)</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">🔁 Pengenalan Pola (cari kesamaan)</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">🎯 Abstraksi (saring hal penting)</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">📋 Algoritma (susun langkah)</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node end">SOLUSI</div>
          </div>
          <div class="diagram-caption">Gambar: Alur berpikir komputasional dari masalah ke solusi</div>
        </div>
      </div>

      <!-- DEKOMPOSISI -->
      <div class="materi-content" id="tab-dekomposisi">
        <div class="materi-hero">
          <div class="materi-hero-icon">🧩</div>
          <div class="materi-hero-text">
            <h3>1. DEKOMPOSISI</h3>
            <p>Memecah masalah besar menjadi bagian kecil</p>
          </div>
        </div>
        <div class="materi-section">
          <h4>📖 Definisi Lengkap</h4>
          <p><b>Dekomposisi</b> adalah kemampuan untuk <b>memecah masalah besar menjadi bagian-bagian kecil yang lebih mudah dikelola</b>. Setiap bagian kecil bisa diselesaikan secara terpisah, lalu digabungkan kembali menjadi solusi utuh.</p>
          <p>Bayangkan kamu ingin membangun sebuah rumah. Tentu tidak mungkin langsung membangunnya sekaligus! Kamu perlu memecahnya menjadi: pondasi, dinding, atap, listrik, dan interior. Setiap bagian dikerjakan satu per satu.</p>
        </div>
        <div class="materi-section">
          <h4>💡 Analogi</h4>
          <div class="analogy-box">🍕 <b>Seperti memotong pizza.</b> Pizza utuh yang besar sulit dimakan. Tapi jika dipotong menjadi 8 bagian kecil, tiap potong mudah dimakan. Masalah besar juga begitu — pecah dulu, selesaikan satu per satu.</div>
        </div>
        <div class="materi-section">
          <h4>🌍 Contoh Nyata</h4>
          <div class="example-box">
            <b>Membuat game sederhana</b> — dipecah menjadi:<br>
            🎨 Desain karakter → 🎮 Aturan main → 🏆 Sistem skor → 🔊 Efek suara → 🗺️ Level permainan
          </div>
          <div class="example-box">
            <b>Menyiapkan pesta ulang tahun</b> — dipecah menjadi:<br>
            🍰 Makanan → 🎈 Dekorasi → 🎵 Musik → 📨 Undangan → 🎁 Hadiah
          </div>
        </div>
        <div class="materi-section">
          <h4>📊 Diagram Pohon Dekomposisi</h4>
          <div class="diagram-box">
MASALAH BESAR
│
├── Sub-masalah 1 ──→ Solusi 1
│
├── Sub-masalah 2 ──→ Solusi 2
│
├── Sub-masalah 3 ──→ Solusi 3
│
└── Sub-masalah 4 ──→ Solusi 4
                    ↓
              SOLUSI UTUH
          </div>
          <div class="diagram-caption">Gambar: Diagram pohon memecah masalah besar menjadi sub-masalah</div>
        </div>
        <div class="materi-section">
          <h4>📈 Grafik: Manfaat Dekomposisi</h4>
          <div class="bar-chart">
            <div class="bar-row"><span class="bar-label">Tugas besar</span><div class="bar-track"><div class="bar-fill c2" style="width:90%;">Sulit</div></div></div>
            <div class="bar-row"><span class="bar-label">Setelah dipecah</span><div class="bar-track"><div class="bar-fill c3" style="width:25%;">Mudah</div></div></div>
            <div class="bar-row"><span class="bar-label">Waktu selesai</span><div class="bar-track"><div class="bar-fill c1" style="width:40%;">Cepat</div></div></div>
          </div>
          <div class="diagram-caption">Grafik: Dekomposisi membuat tugas besar terasa lebih mudah & cepat selesai</div>
        </div>
      </div>

      <!-- POLA -->
      <div class="materi-content" id="tab-pola">
        <div class="materi-hero">
          <div class="materi-hero-icon">🔁</div>
          <div class="materi-hero-text">
            <h3>2. PENGENALAN POLA</h3>
            <p>Menemukan kesamaan yang berulang</p>
          </div>
        </div>
        <div class="materi-section">
          <h4>📖 Definisi Lengkap</h4>
          <p><b>Pengenalan pola (pattern recognition)</b> adalah kemampuan untuk <b>menemukan kesamaan, keteraturan, atau pola berulang</b> dalam suatu masalah. Dengan mengenali pola, kita bisa menyelesaikan masalah serupa dengan cara yang sama — tanpa harus memulai dari nol setiap kali.</p>
        </div>
        <div class="materi-section">
          <h4>💡 Analogi</h4>
          <div class="analogy-box">🍊 <b>Seperti melihat pola di kulit jeruk.</b> Kamu tidak perlu memeriksa setiap jeruk satu per satu untuk tahu bahwa semua jeruk memiliki pola yang mirip. Pola membantu kita mengenali & memprediksi.</div>
        </div>
        <div class="materi-section">
          <h4>🌍 Contoh Nyata</h4>
          <div class="example-box"><b>Deret angka:</b> 2, 4, 6, 8, ... → pola +2, angka berikutnya 10</div>
          <div class="example-box"><b>Jadwal bus:</b> bus datang setiap 15 menit → kamu bisa memprediksi kedatangan berikutnya</div>
          <div class="example-box"><b>Musim:</b> hujan–kemarau–hujan–kemarau → pola tahunan yang bisa diprediksi</div>
        </div>
        <div class="materi-section">
          <h4>📊 Visual Pola Angka & Geometri</h4>
          <div class="diagram-box">
DERET ANGKA:
2 → 4 → 6 → 8 → 10 → 12
  +2  +2  +2  +2   +2

POLA GEOMETRI:
● ●● ●●● ●●●● ●●●●●
1  2   3    4     5
          </div>
          <div class="diagram-caption">Gambar: Pola penambahan angka & pola geometri</div>
        </div>
        <div class="materi-section">
          <h4>📈 Grafik: Pola dalam Kehidupan</h4>
          <div class="bar-chart">
            <div class="bar-row"><span class="bar-label">Jadwal sekolah</span><div class="bar-track"><div class="bar-fill c1" style="width:85%;">Berulang</div></div></div>
            <div class="bar-row"><span class="bar-label">Trafik pagi</span><div class="bar-track"><div class="bar-fill c2" style="width:75%;">Padat</div></div></div>
            <div class="bar-row"><span class="bar-label">Ujian semester</span><div class="bar-track"><div class="bar-fill c3" style="width:60%;">Terjadwal</div></div></div>
          </div>
          <div class="diagram-caption">Grafik: Contoh pola dalam kehidupan sehari-hari</div>
        </div>
      </div>

      <!-- ABSTRAKSI -->
      <div class="materi-content" id="tab-abstraksi">
        <div class="materi-hero">
          <div class="materi-hero-icon">🎯</div>
          <div class="materi-hero-text">
            <h3>3. ABSTRAKSI</h3>
            <p>Fokus pada hal penting, abaikan yang tidak perlu</p>
          </div>
        </div>
        <div class="materi-section">
          <h4>📖 Definisi Lengkap</h4>
          <p><b>Abstraksi</b> adalah kemampuan untuk <b>menyaring informasi penting dan mengabaikan detail yang tidak perlu</b>. Dengan abstraksi, kita fokus pada inti masalah tanpa terganggu hal-hal kecil yang tidak relevan.</p>
        </div>
        <div class="materi-section">
          <h4>💡 Analogi</h4>
          <div class="analogy-box">🗺️ <b>Seperti peta jalan.</b> Peta tidak menunjukkan setiap pohon, rumah, atau orang — hanya jalan dan landmark penting. Itulah abstraksi: menyederhanakan dunia nyata agar mudah dipahami.</div>
        </div>
        <div class="materi-section">
          <h4>🌍 Contoh Nyata</h4>
          <div class="example-box"><b>Aplikasi ojek online:</b> fokus pada fitur inti (pesan, bayar, lacak) — tidak perlu menampilkan warna tombol, jenis font, atau animasi kecil</div>
          <div class="example-box"><b>Resep masakan:</b> hanya menulis bahan & langkah penting — tidak perlu menulis merek panci atau warna celemek</div>
          <div class="example-box"><b>Peta MRT:</b> hanya menunjukkan stasiun & jalur — tidak perlu detail bangunan di sekitar</div>
        </div>
        <div class="materi-section">
          <h4>📊 Visual: Zoom In & Zoom Out</h4>
          <div class="diagram-box">
DUNIA NYATA (banyak detail):
🏠🌳🚗🌳🏢🌳🚦🏠🌳
        ↓ [ABSTRAKSI]
MODEL SEDERHANA (fokus inti):
🏠 — 🏢 — 🚦
        </div>
          <div class="diagram-caption">Gambar: Abstraksi menyaring detail yang tidak perlu</div>
        </div>
        <div class="materi-section">
          <h4>📈 Grafik: Tingkat Detail vs Kegunaan</h4>
          <div class="bar-chart">
            <div class="bar-row"><span class="bar-label">Detail berlebihan</span><div class="bar-track"><div class="bar-fill c2" style="width:95%;">Membingungkan</div></div></div>
            <div class="bar-row"><span class="bar-label">Abstraksi tepat</span><div class="bar-track"><div class="bar-fill c3" style="width:30%;">Jelas</div></div></div>
            <div class="bar-row"><span class="bar-label">Tanpa detail</span><div class="bar-track"><div class="bar-fill c4" style="width:55%;">Kurang jelas</div></div></div>
          </div>
          <div class="diagram-caption">Grafik: Abstraksi yang tepat membuat informasi lebih mudah dipahami</div>
        </div>
      </div>

      <!-- ALGORITMA -->
      <div class="materi-content" id="tab-algoritma">
        <div class="materi-hero">
          <div class="materi-hero-icon">📋</div>
          <div class="materi-hero-text">
            <h3>4. ALGORITMA</h3>
            <p>Langkah logis & berurutan menuju solusi</p>
          </div>
        </div>
        <div class="materi-section">
          <h4>📖 Definisi Lengkap</h4>
          <p><b>Algoritma</b> adalah <b>urutan langkah-langkah logis dan sistematis</b> untuk menyelesaikan suatu masalah. Setiap langkah harus jelas, berurutan, dan bisa dijalankan — baik oleh manusia maupun komputer.</p>
        </div>
        <div class="materi-section">
          <h4>💡 Analogi</h4>
          <div class="analogy-box">🍳 <b>Seperti resep masakan.</b> Resep punya urutan langkah: (1) siapkan bahan, (2) panaskan minyak, (3) tumis bumbu, (4) masukkan nasi, (5) sajikan. Jika urutannya salah, hasilnya bisa berbeda!</div>
        </div>
        <div class="materi-section">
          <h4>🌍 Contoh Nyata</h4>
          <div class="example-box">
            <b>Algoritma membuat teh:</b><br>
            1. Didihkan air<br>
            2. Masukkan teh ke cangkir<br>
            3. Tuang air panas ke cangkir<br>
            4. Tunggu 3 menit<br>
            5. Sajikan
          </div>
          <div class="example-box">
            <b>Algoritma menyiapkan tas sekolah:</b><br>
            1. Cek jadwal pelajaran hari ini<br>
            2. Ambil buku sesuai jadwal<br>
            3. Masukkan alat tulis<br>
            4. Masukkan botol minum<br>
            5. Zip tas
          </div>
        </div>
        <div class="materi-section">
          <h4>📊 Flowchart Algoritma</h4>
          <div class="flowchart">
            <div class="flow-node start">MULAI</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">Siapkan bahan</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">Panaskan minyak</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">Tumis bumbu</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">Masukkan nasi</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node">Aduk rata</div>
            <div class="flow-arrow">↓</div>
            <div class="flow-node end">SAJIKAN</div>
          </div>
          <div class="diagram-caption">Gambar: Flowchart algoritma memasak nasi goreng</div>
        </div>
        <div class="materi-section">
          <h4>📈 Grafik: Ciri Algoritma yang Baik</h4>
          <div class="bar-chart">
            <div class="bar-row"><span class="bar-label">Jelas</span><div class="bar-track"><div class="bar-fill c1" style="width:90%;">Tinggi</div></div></div>
            <div class="bar-row"><span class="bar-label">Berurutan</span><div class="bar-track"><div class="bar-fill c3" style="width:85%;">Tinggi</div></div></div>
            <div class="bar-row"><span class="bar-label">Bisa dijalankan</span><div class="bar-track"><div class="bar-fill c4" style="width:88%;">Tinggi</div></div></div>
            <div class="bar-row"><span class="bar-label">Berhenti</span><div class="bar-track"><div class="bar-fill c2" style="width:80%;">Pasti</div></div></div>
          </div>
          <div class="diagram-caption">Grafik: Ciri algoritma yang baik</div>
        </div>
      </div>

      <!-- RANGKUMAN -->
      <div class="materi-content" id="tab-rangkuman">
        <div class="materi-hero">
          <div class="materi-hero-icon">📝</div>
          <div class="materi-hero-text">
            <h3>Rangkuman 4 Pilar</h3>
            <p>Ringkasan materi yang telah dipelajari</p>
          </div>
        </div>
        <div class="materi-section">
          <h4>📋 Tabel Rangkuman</h4>
          <div style="overflow-x:auto;">
            <table style="width:100%;border-collapse:collapse;font-size:12.5px;margin-top:8px;">
              <thead>
                <tr style="background:rgba(0,245,255,.15);">
                  <th style="padding:12px;text-align:left;border-bottom:1px solid var(--glass-border);">Pilar</th>
                  <th style="padding:12px;text-align:left;border-bottom:1px solid var(--glass-border);">Inti</th>
                  <th style="padding:12px;text-align:left;border-bottom:1px solid var(--glass-border);">Analogi</th>
                </tr>
              </thead>
              <tbody>
                <tr><td style="padding:10px;border-bottom:1px solid var(--glass-border);">🧩 Dekomposisi</td><td style="padding:10px;border-bottom:1px solid var(--glass-border);">Pecah masalah besar → kecil</td><td style="padding:10px;border-bottom:1px solid var(--glass-border);">Memotong pizza</td></tr>
                <tr><td style="padding:10px;border-bottom:1px solid var(--glass-border);">🔁 Pengenalan Pola</td><td style="padding:10px;border-bottom:1px solid var(--glass-border);">Temukan kesamaan berulang</td><td style="padding:10px;border-bottom:1px solid var(--glass-border);">Pola kulit jeruk</td></tr>
                <tr><td style="padding:10px;border-bottom:1px solid var(--glass-border);">🎯 Abstraksi</td><td style="padding:10px;border-bottom:1px solid var(--glass-border);">Saring info penting</td><td style="padding:10px;border-bottom:1px solid var(--glass-border);">Peta jalan</td></tr>
                <tr><td style="padding:10px;">📋 Algoritma</td><td style="padding:10px;">Langkah logis berurutan</td><td style="padding:10px;">Resep masakan</td></tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="materi-section">
          <h4>🎯 Ingat!</h4>
          <p>Keempat pilar ini saling melengkapi dan dipakai bersama-sama. Ketika kamu menghadapi masalah baru, coba urutkan: <b>pecah</b> (dekomposisi) → <b>cari pola</b> → <b>saring</b> (abstraksi) → <b>susun langkah</b> (algoritma).</p>
        </div>
      </div>

      <button class="btn btn-primary" style="margin-top:16px;" onclick="goTo('simulasi')">🧪 Lanjut ke Simulasi</button>
    </section>

    <!-- ============================================================ -->
    <!-- [4] SIMULASI -->
    <!-- ============================================================ -->
    <section class="section" id="simulasi">
      <div class="section-header">
        <span class="section-tag">Misi 04 · Simulasi</span>
        <h2 class="section-title">🧪 Lab Interaktif</h2>
        <p class="section-desc">Praktikkan langsung. Tiga mini-game untuk menguji 4 pilar berpikir komputasional.</p>
      </div>

      <div class="sim-container">
        <div class="sim-title">🥤 Simulasi 1: Susun Algoritma Membuat Teh</div>
        <p style="color:var(--muted);font-size:13px;margin-bottom:8px;">Drag kartu ke urutan yang benar (atas → bawah).</p>
        <div class="steps-list" id="stepsList"></div>
        <div style="display:flex;gap:8px;flex-wrap:wrap;">
          <button class="btn btn-ghost" onclick="resetSteps()">🔄 Acak Ulang</button>
          <button class="btn btn-primary" onclick="checkSteps()">✅ Cek Urutan</button>
        </div>
        <div id="stepsResult" style="margin-top:12px;"></div>
      </div>

      <div class="sim-container">
        <div class="sim-title">🔢 Simulasi 2: Temukan Pola</div>
        <p style="color:var(--muted);font-size:13px;margin-bottom:8px;">Deret angka: <b>2, 4, 6, 8, ...</b> Berapa angka berikutnya?</p>
        <div class="pattern-display">
          <div class="pattern-item">2</div><div class="pattern-item">4</div>
          <div class="pattern-item">6</div><div class="pattern-item">8</div>
          <div class="pattern-item unknown">?</div>
        </div>
        <div style="display:flex;gap:8px;flex-wrap:wrap;margin-top:12px;">
          <button class="btn btn-ghost" onclick="checkPattern(10)">10</button>
          <button class="btn btn-ghost" onclick="checkPattern(12)">12</button>
          <button class="btn btn-ghost" onclick="checkPattern(14)">14</button>
          <button class="btn btn-ghost" onclick="checkPattern(9)">9</button>
        </div>
        <div id="patternResult" style="margin-top:12px;"></div>
      </div>

      <div class="sim-container">
        <div class="sim-title">🤖 Simulasi 3: Navigasi Robot</div>
        <p style="color:var(--muted);font-size:13px;margin-bottom:8px;">Bantu robot (🤖) mencapai target (🎯). Beri perintah arah!</p>
        <div class="robot-grid" id="robotGrid"></div>
        <div class="controls">
          <button class="ctrl-btn" onclick="moveRobot('up')">⬆</button>
          <button class="ctrl-btn" onclick="moveRobot('left')">⬅</button>
          <button class="ctrl-btn" onclick="moveRobot('down')">⬇</button>
          <button class="ctrl-btn" onclick="moveRobot('right')">➡</button>
        </div>
        <div style="text-align:center;margin-top:12px;">
          <button class="btn btn-ghost" onclick="resetRobot()">🔄 Reset</button>
        </div>
        <div id="robotResult" style="margin-top:12px;text-align:center;"></div>
      </div>
    </section>

    <!-- ============================================================ -->
    <!-- [5] KUIS -->
    <!-- ============================================================ -->
    <section class="section" id="kuis">
      <div class="section-header">
        <span class="section-tag">Misi 05 · Kuis</span>
        <h2 class="section-title">🎯 Tantangan Kilat</h2>
        <p class="section-desc">5 soal untuk menguatkan pemahamanmu. Pilih jawaban yang paling tepat.</p>
      </div>
      <div id="kuisContainer"></div>
      <button class="btn btn-primary" id="btnKuis" style="margin-top:8px;">Kirim Jawaban</button>
      <div id="kuisResult" style="margin-top:16px;"></div>
    </section>

    <!-- ============================================================ -->
    <!-- [6] ASESMEN AKHIR -->
    <!-- ============================================================ -->
    <section class="section" id="asesmen-akhir">
      <div class="section-header">
        <span class="section-tag">Misi 06 · Asesmen Akhir</span>
        <h2 class="section-title">🏆 Ujian Akhir Petualangan</h2>
        <p class="section-desc">5 soal pilihan ganda + 1 studi kasus. Tunjukkan bahwa kamu sudah menguasai 4 pilar!</p>
      </div>
      <div id="akhirContainer"></div>
      <button class="btn btn-primary" id="btnAkhir" style="margin-top:8px;">Kirim & Lihat Hasil</button>
      <div id="akhirResult" style="margin-top:16px;"></div>
    </section>

    <!-- ============================================================ -->
    <!-- [7] REFLEKSI -->
    <!-- ============================================================ -->
    <section class="section" id="refleksi">
      <div class="section-header">
        <span class="section-tag">Misi 07 · Refleksi</span>
        <h2 class="section-title">🪞 Cermin Petualang</h2>
        <p class="section-desc">Luangkan waktu sejenak untuk merenungkan pembelajaranmu hari ini.</p>
      </div>

      <div class="card">
        <h3>1. 🌟 Pemahaman</h3>
        <p>Dari 4 pilar, mana yang paling kamu pahami?</p>
        <select id="refleksiPilar">
          <option value="dekomposisi">Dekomposisi</option>
          <option value="pola">Pengenalan Pola</option>
          <option value="abstraksi">Abstraksi</option>
          <option value="algoritma">Algoritma</option>
        </select>
      </div>

      <div class="card">
        <h3>2. 😊 Perasaanmu</h3>
        <p>Bagaimana perasaanmu setelah belajar hari ini?</p>
        <div class="emoji-picker" id="emojiPicker">
          <div class="emoji-opt" data-emoji="😄">😄</div>
          <div class="emoji-opt" data-emoji="🙂">🙂</div>
          <div class="emoji-opt" data-emoji="😐">😐</div>
          <div class="emoji-opt" data-emoji="😕">😕</div>
          <div class="emoji-opt" data-emoji="😢">😢</div>
        </div>
      </div>

      <div class="card">
        <h3>3. 🎯 Kesan</h3>
        <p>Apa hal paling menarik yang kamu pelajari?</p>
        <textarea id="refleksiKesan" placeholder="Tulis di sini..."></textarea>
      </div>

      <div class="card">
        <h3>4. 🚀 Motivasi</h3>
        <p>Apa yang ingin kamu pelajari lebih lanjut?</p>
        <textarea id="refleksiMotivasi" placeholder="Tulis di sini..."></textarea>
      </div>

      <div class="card">
        <h3>5. 📝 Janji</h3>
        <p>Tulis satu hal yang akan kamu lakukan berbeda setelah belajar ini.</p>
        <textarea id="refleksiJanji" placeholder="Tulis di sini..."></textarea>
      </div>

      <button class="btn btn-magenta" id="btnRefleksi">✨ Simpan Kartu Refleksi</button>
      <div id="refleksiResult" style="margin-top:16px;"></div>
    </section>
  </main>
</div>

<div class="toast" id="toast"></div>

<script>
/* ============ STATE ============ */
const state = {
  visited: new Set(['apersepsi']),
  soundOn: true,
  aperBoxesOpened: [],
  aperTimer: 30,
  aperTimerInterval: null,
  aperKeyIdx: Math.floor(Math.random()*5),
  asesmenAwalState: {},
  kuisAnswers: {},
  kuisSubmitted: false,
  akhirAnswers: {},
  akhirSubmitted: false,
  refleksiEmoji: null
};

/* ============ SOUND ============ */
let audioCtx = null;
function beep(freq=440, dur=0.08, type='sine'){
  if(!state.soundOn) return;
  try{
    if(!audioCtx) audioCtx = new (window.AudioContext||window.webkitAudioContext)();
    const o = audioCtx.createOscillator();
    const g = audioCtx.createGain();
    o.type = type; o.frequency.value = freq;
    o.connect(g); g.connect(audioCtx.destination);
    g.gain.setValueAtTime(0.08, audioCtx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + dur);
    o.start(); o.stop(audioCtx.currentTime + dur);
  }catch(e){}
}
function playCorrect(){ beep(880,0.1); setTimeout(()=>beep(1320,0.12),80); }
function playWrong(){ beep(200,0.15,'square'); }
function playClick(){ beep(600,0.04); }

document.getElementById('soundBtn').onclick = (e)=>{
  state.soundOn = !state.soundOn;
  e.target.textContent = state.soundOn ? '🔊' : '🔇';
  if(state.soundOn) playClick();
};

/* ============ NAVIGATION ============ */
const navItems = document.querySelectorAll('.nav-item');
navItems.forEach(item=>{
  item.onclick = ()=>{ playClick(); goTo(item.dataset.section); };
});
function goTo(sectionId){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.getElementById(sectionId).classList.add('active');
  navItems.forEach(n=>{
    n.classList.toggle('active', n.dataset.section === sectionId);
    if(state.visited.has(n.dataset.section) && n.dataset.section !== sectionId) n.classList.add('done');
  });
  state.visited.add(sectionId);
  updateProgress();
  window.scrollTo({top:0,behavior:'smooth'});
}
function updateProgress(){
  const pct = Math.round((state.visited.size/7)*100);
  document.getElementById('progressPct').textContent = pct + '%';
  document.getElementById('progressFill').style.width = pct + '%';
}

/* ============ APERSEPSI ============ */
const aperTimerEl = document.getElementById('aperTimer');
const boxesGrid = document.getElementById('boxesGrid');
const aperFeedback = document.getElementById('aperFeedback');
const aperFeedbackText = document.getElementById('aperFeedbackText');

function startAperTimer(){
  clearInterval(state.aperTimerInterval);
  state.aperTimer = 30;
  aperTimerEl.textContent = '00:30';
  state.aperTimerInterval = setInterval(()=>{
    state.aperTimer--;
    if(state.aperTimer <= 0){
      clearInterval(state.aperTimerInterval);
      aperTimerEl.textContent = '00:00';
      document.querySelectorAll('.mystery-box').forEach(b=>{
        if(!b.classList.contains('opened')) b.classList.add('locked');
      });
      aperFeedback.style.display = 'block';
      aperFeedbackText.innerHTML = `⏰ Waktu habis! Kunci ada di kotak ke-${state.aperKeyIdx+1}.<br><br>
        <b>Pertanyaan:</b> Bagaimana caramu memutuskan tadi? <b>Menebak</b>, <b>mencari pola</b>, atau <b>punya strategi</b>? Tulis alasanmu:`;
      aperFeedbackText.innerHTML += `<textarea id="aperAlasan" placeholder="Alasan saya..."></textarea>`;
      return;
    }
    aperTimerEl.textContent = '00:' + String(state.aperTimer).padStart(2,'0');
    if(state.aperTimer <= 5) beep(400,0.05);
  }, 1000);
}
boxesGrid.querySelectorAll('.mystery-box').forEach(box=>{
  box.onclick = ()=>{
    if(box.classList.contains('opened') || box.classList.contains('locked')) return;
    if(state.aperBoxesOpened.length >= 3){ showToast('Kamu hanya boleh membuka 3 kotak!'); return; }
    openBox(box);
  };
});
function openBox(box){
  playClick();
  box.classList.add('opened');
  state.aperBoxesOpened.push(parseInt(box.dataset.idx));
  if(parseInt(box.dataset.idx) === state.aperKeyIdx){
    box.classList.add('winner');
    box.textContent = '🔑';
    aperFeedback.style.display = 'block';
    aperFeedbackText.innerHTML = `🎉 <b class="highlight-g">Kamu menemukan kunci!</b> Kotak ke-${state.aperKeyIdx+1} berisi kunci.<br><br>
      <b>Pertanyaan:</b> Apakah kamu memilihnya karena <b>menebak</b>, <b>mencari pola</b>, atau <b>punya strategi</b>? Tulis alasanmu:`;
    aperFeedbackText.innerHTML += `<textarea id="aperAlasan" placeholder="Alasan saya..."></textarea>`;
    playCorrect();
    clearInterval(state.aperTimerInterval);
    document.querySelectorAll('.mystery-box').forEach(b=>{ if(b !== box) b.classList.add('locked'); });
    return;
  } else {
    box.textContent = '❌';
    playWrong();
    if(state.aperBoxesOpened.length >= 3){
      clearInterval(state.aperTimerInterval);
      aperFeedback.style.display = 'block';
      aperFeedbackText.innerHTML = `Kotak yang kamu buka: ${state.aperBoxesOpened.map(i=>i+1).join(', ')}. Kunci ada di kotak ke-${state.aperKeyIdx+1}.<br><br>
        <b>Pertanyaan:</b> Bagaimana caramu memutuskan tadi? <b>Menebak</b>, <b>mencari pola</b>, atau <b>punya strategi</b>? Tulis alasanmu:`;
      aperFeedbackText.innerHTML += `<textarea id="aperAlasan" placeholder="Alasan saya..."></textarea>`;
      document.querySelectorAll('.mystery-box').forEach(b=>{
        if(!b.classList.contains('opened')) { b.classList.add('locked'); b.textContent = '🔒'; }
      });
    }
  }
}
startAperTimer();

/* ============ ASESMEN AWAL ============ */
const asesmenAwalData = [
  { q:"Pernahkah kamu memecahkan masalah dengan membaginya menjadi bagian kecil?", type:"single", opts:["Belum pernah","Kadang-kadang","Sering"], map:[0,1,2] },
  { q:"Apakah kamu tahu apa itu 'pola' dalam menyelesaikan masalah?", type:"single", opts:["Tidak tahu","Sedikit tahu","Paham"], map:[0,1,2] },
  { q:"Menurutmu, komputer 'berpikir' seperti manusia? Mengapa?", type:"text" },
  { q:"Jika kamu harus membuat jadwal harian, langkah pertama yang kamu lakukan adalah...", type:"single", opts:["Langsung menulis semua kegiatan","Mengelompokkan kegiatan penting & tidak","Menebak-nebak saja"], map:[0,2,1] },
  { q:"Seberapa yakin kamu bisa menjelaskan 'berpikir komputasional' kepada teman? (1 = belum yakin, 10 = sangat yakin)", type:"slider" }
];
function renderAsesmenAwal(){
  const c = document.getElementById('asesmenAwalContainer');
  c.innerHTML = asesmenAwalData.map((d,i)=>{
    if(d.type==='single'){
      return `<div class="question-card">
        <span class="q-num">Q${i+1}</span>
        <div class="q-text">${d.q}</div>
        <div class="options" data-q="${i}">
          ${d.opts.map((o,oi)=>`<div class="option" data-oi="${oi}"><span class="option-letter">${String.fromCharCode(65+oi)}</span>${o}</div>`).join('')}
        </div>
      </div>`;
    } else if(d.type==='text'){
      return `<div class="question-card">
        <span class="q-num">Q${i+1}</span>
        <div class="q-text">${d.q}</div>
        <textarea id="aatext-${i}" placeholder="Jawabanmu..."></textarea>
      </div>`;
    } else {
      return `<div class="question-card">
        <span class="q-num">Q${i+1}</span>
        <div class="q-text">${d.q}</div>
        <input type="range" min="1" max="10" value="5" id="aaslider-${i}" oninput="document.getElementById('aasliderval-${i}').textContent=this.value">
        <div class="slider-value" id="aasliderval-${i}">5</div>
      </div>`;
    }
  }).join('');
  c.querySelectorAll('.options').forEach(optWrap=>{
    const q = optWrap.dataset.q;
    optWrap.querySelectorAll('.option').forEach(opt=>{
      opt.onclick = ()=>{
        playClick();
        optWrap.querySelectorAll('.option').forEach(o=>o.classList.remove('selected'));
        opt.classList.add('selected');
        state.asesmenAwalState[q] = parseInt(opt.dataset.oi);
      };
    });
  });
}
renderAsesmenAwal();
document.getElementById('btnAsesmenAwal').onclick = ()=>{
  playClick();
  let score = 0;
  [0,1,3].forEach(qi=>{
    const ans = state.asesmenAwalState[qi];
    if(ans !== undefined) score += asesmenAwalData[qi].map[ans] || 0;
  });
  const sliderVal = parseInt(document.getElementById('aaslider-4').value);
  score += Math.round((sliderVal/10)*3);
  const pct = Math.round((score/9)*100);
  let msg = '';
  if(pct < 40) msg = "🔍 <b>Radar pemahamanmu masih rendah.</b> Jangan khawatir — justru inilah saat terbaik untuk belajar! Ikuti materi dengan saksama.";
  else if(pct < 75) msg = "📡 <b>Radar pemahamanmu cukup baik.</b> Kamu sudah punya bekal. Materi akan memperkuat dasarmu.";
  else msg = "⚡ <b>Radar pemahamanmu tinggi!</b> Hebat — kamu mungkin sudah kenal konsep ini. Tetap ikuti materi untuk memastikan tidak ada yang terlewat.";
  document.getElementById('asesmenAwalResult').innerHTML = `
    <div class="result-box">
      <div style="font-size:13px;color:var(--muted);letter-spacing:1px;text-transform:uppercase;">Radar Pemahaman Awal</div>
      <div class="result-score">${pct}%</div>
      <div class="result-msg">${msg}</div>
      <button class="btn btn-primary" onclick="goTo('materi')">📚 Lanjut ke Materi Pokok</button>
    </div>`;
  state.visited.add('asesmen-awal');
  updateProgress();
  playCorrect();
};

/* ============ MATERI TABS ============ */
document.querySelectorAll('.materi-tab').forEach(tab=>{
  tab.onclick = ()=>{
    playClick();
    document.querySelectorAll('.materi-tab').forEach(t=>t.classList.remove('active'));
    tab.classList.add('active');
    document.querySelectorAll('.materi-content').forEach(c=>c.classList.remove('active'));
    document.getElementById('tab-'+tab.dataset.tab).classList.add('active');
  };
});

/* ============ SIMULASI 1: SUSUN ALGORITMA ============ */
const correctSteps = ["Didihkan air","Masukkan teh ke dalam cangkir","Tuang air panas ke cangkir","Tunggu 3 menit","Sajikan"];
let currentSteps = [];
function shuffle(arr){
  const a = [...arr];
  for(let i=a.length-1;i>0;i--){ const j = Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; }
  return a;
}
function renderSteps(){
  const c = document.getElementById('stepsList');
  c.innerHTML = currentSteps.map((s,i)=>`
    <div class="step-item" draggable="true" data-idx="${i}">
      <span class="drag-handle">⋮⋮</span>
      <span>${s}</span>
    </div>`).join('');
  attachDrag();
}
function attachDrag(){
  const items = document.querySelectorAll('.step-item');
  let dragEl = null;
  items.forEach(item=>{
    item.addEventListener('dragstart', ()=>{ dragEl = item; item.classList.add('dragging'); });
    item.addEventListener('dragend', ()=>{
      item.classList.remove('dragging'); dragEl = null;
      currentSteps = [...document.querySelectorAll('.step-item')].map(el=>el.querySelector('span:last-child').textContent);
    });
    item.addEventListener('dragover', (e)=>{
      e.preventDefault();
      const after = getDragAfterElement(item.parentNode, e.clientY);
      if(!dragEl) return;
      if(after == null) item.parentNode.appendChild(dragEl);
      else item.parentNode.insertBefore(dragEl, after);
    });
  });
}
function getDragAfterElement(container, y){
  const draggable = [...container.querySelectorAll('.step-item:not(.dragging)')];
  return draggable.reduce((closest, child)=>{
    const box = child.getBoundingClientRect();
    const offset = y - box.top - box.height/2;
    if(offset < 0 && offset > closest.offset) return {offset, element: child};
    return closest;
  }, {offset: Number.NEGATIVE_INFINITY}).element;
}
function resetSteps(){
  playClick();
  currentSteps = shuffle(correctSteps);
  renderSteps();
  document.getElementById('stepsResult').innerHTML = '';
}
resetSteps();
function checkSteps(){
  playClick();
  currentSteps = [...document.querySelectorAll('.step-item')].map(el=>el.querySelector('span:last-child').textContent);
  let correct = 0;
  document.querySelectorAll('.step-item').forEach((el,i)=>{
    el.classList.remove('correct','wrong');
    if(currentSteps[i] === correctSteps[i]){ el.classList.add('correct'); correct++; }
    else el.classList.add('wrong');
  });
  const res = document.getElementById('stepsResult');
  if(correct === correctSteps.length){
    res.innerHTML = `<div class="result-box" style="padding:20px;"><div class="result-msg">🎉 <b class="highlight-g">Sempurna!</b> Urutan algoritma kamu benar semua!</div></div>`;
    playCorrect();
  } else {
    res.innerHTML = `<div class="result-box" style="padding:20px;border-color:var(--magenta);"><div class="result-msg">Kamu benar <b class="highlight">${correct}/${correctSteps.length}</b> langkah. Perhatikan yang salah (merah) dan susun ulang.</div></div>`;
    playWrong();
  }
}

/* ============ SIMULASI 2: POLA ============ */
function checkPattern(ans){
  playClick();
  const res = document.getElementById('patternResult');
  if(ans === 10){
    res.innerHTML = `<div class="result-box" style="padding:20px;"><div class="result-msg">🎉 <b class="highlight-g">Tepat!</b> Polanya +2, jadi 8 + 2 = 10.</div></div>`;
    playCorrect();
  } else {
    res.innerHTML = `<div class="result-box" style="padding:20px;border-color:var(--magenta);"><div class="result-msg">Belum tepat. Coba lihat: 2→4 (+2), 4→6 (+2), 6→8 (+2)... jadi 8 + ? = ?</div></div>`;
    playWrong();
  }
}

/* ============ SIMULASI 3: ROBOT ============ */
const GRID_SIZE = 5;
let robotPos = {r:0,c:0};
let robotTarget = {r:4,c:4};
let robotObstacles = [{r:1,c:1},{r:2,c:3},{r:3,c:1}];
function renderRobot(){
  const grid = document.getElementById('robotGrid');
  grid.innerHTML = '';
  for(let r=0;r<GRID_SIZE;r++){
    for(let c=0;c<GRID_SIZE;c++){
      const cell = document.createElement('div');
      cell.className = 'cell';
      if(r===robotPos.r && c===robotPos.c){ cell.classList.add('robot'); cell.textContent='🤖'; }
      else if(r===robotTarget.r && c===robotTarget.c){ cell.classList.add('target'); cell.textContent='🎯'; }
      else if(robotObstacles.some(o=>o.r===r && o.c===c)){ cell.classList.add('obstacle'); cell.textContent='🚧'; }
      grid.appendChild(cell);
    }
  }
}
function moveRobot(dir){
  playClick();
  let {r,c} = robotPos;
  if(dir==='up') r--; if(dir==='down') r++; if(dir==='left') c--; if(dir==='right') c++;
  if(r<0||r>=GRID_SIZE||c<0||c>=GRID_SIZE) return;
  if(robotObstacles.some(o=>o.r===r && o.c===c)) return;
  robotPos = {r,c};
  renderRobot();
  if(r===robotTarget.r && c===robotTarget.c){
    document.getElementById('robotResult').innerHTML = `<div class="result-box" style="padding:16px;"><div class="result-msg">🎉 <b class="highlight-g">Robot sampai tujuan!</b> Itulah algoritma navigasi.</div></div>`;
    playCorrect();
  }
}
function resetRobot(){
  playClick();
  robotPos = {r:0,c:0};
  document.getElementById('robotResult').innerHTML = '';
  renderRobot();
}
renderRobot();

/* ============ KUIS ============ */
const kuisData = [
  { q:"Membagi masalah besar menjadi bagian kecil disebut...", opts:["Abstraksi","Dekomposisi","Algoritma","Pola"], ans:1 },
  { q:"Deret 3, 6, 12, 24, ... angka berikutnya adalah...", opts:["30","36","48","60"], ans:2 },
  { q:"Urutan langkah logis untuk menyelesaikan masalah disebut...", opts:["Pola","Abstraksi","Dekomposisi","Algoritma"], ans:3 },
  { q:"Mengabaikan detail yang tidak perlu termasuk...", opts:["Abstraksi","Pola","Algoritma","Dekomposisi"], ans:0 },
  { q:"Contoh dekomposisi dalam kehidupan sehari-hari adalah...", opts:["Menyusun jadwal","Membagi tugas kelompok","Menebak warna","Menghafal"], ans:1 }
];
function renderKuis(){
  const c = document.getElementById('kuisContainer');
  c.innerHTML = kuisData.map((d,i)=>`
    <div class="question-card">
      <span class="q-num">SOAL ${i+1}</span>
      <div class="q-text">${d.q}</div>
      <div class="options" data-q="${i}">
        ${d.opts.map((o,oi)=>`<div class="option" data-oi="${oi}"><span class="option-letter">${String.fromCharCode(65+oi)}</span>${o}</div>`).join('')}
      </div>
    </div>`).join('');
  c.querySelectorAll('.options').forEach(optWrap=>{
    const q = optWrap.dataset.q;
    optWrap.querySelectorAll('.option').forEach(opt=>{
      opt.onclick = ()=>{
        if(state.kuisSubmitted) return;
        playClick();
        optWrap.querySelectorAll('.option').forEach(o=>o.classList.remove('selected'));
        opt.classList.add('selected');
        state.kuisAnswers[q] = parseInt(opt.dataset.oi);
      };
    });
  });
}
renderKuis();
document.getElementById('btnKuis').onclick = ()=>{
  if(state.kuisSubmitted) return;
  playClick();
  let correct = 0;
  kuisData.forEach((d,i)=>{
    const wrap = document.querySelector(`#kuisContainer .options[data-q="${i}"]`);
    const ans = state.kuisAnswers[i];
    wrap.querySelectorAll('.option').forEach(o=>{
      o.classList.remove('selected');
      const oi = parseInt(o.dataset.oi);
      if(oi === d.ans) o.classList.add('correct');
      else if(oi === ans) o.classList.add('wrong');
    });
    if(ans === d.ans) correct++;
  });
  const pct = Math.round((correct/kuisData.length)*100);
  document.getElementById('kuisResult').innerHTML = `
    <div class="result-box">
      <div style="font-size:13px;color:var(--muted);letter-spacing:1px;">SKOR KUIS</div>
      <div class="result-score">${pct}%</div>
      <div class="result-msg">Kamu benar <b class="highlight">${correct}/${kuisData.length}</b> soal.
        ${pct>=80?'🎉 Luar biasa! Siap ke asesmen akhir.':pct>=60?'👍 Bagus! Pelajari lagi yang salah.':'💪 Ayo baca ulang materi dan coba lagi.'}
      </div>
      <button class="btn btn-primary" onclick="goTo('asesmen-akhir')">🏆 Lanjut ke Asesmen Akhir</button>
    </div>`;
  state.kuisSubmitted = true;
  state.visited.add('kuis');
  updateProgress();
  if(pct>=60) playCorrect(); else playWrong();
};

/* ============ ASESMEN AKHIR ============ */
const akhirData = [
  { q:"1. Memecah masalah besar menjadi bagian kecil disebut...", opts:["Abstraksi","Dekomposisi","Algoritma","Pola"], ans:1 },
  { q:"2. Deret 5, 10, 20, 40, ... angka berikutnya...", opts:["50","60","80","100"], ans:2 },
  { q:"3. Menyaring informasi penting dan mengabaikan detail tidak perlu adalah...", opts:["Pola","Algoritma","Abstraksi","Dekomposisi"], ans:2 },
  { q:"4. Urutan langkah logis disebut...", opts:["Algoritma","Pola","Abstraksi","Dekomposisi"], ans:0 },
  { q:"5. Contoh pengenalan pola dalam kehidupan sehari-hari...", opts:["Membagi tugas","Melihat jadwal bus yang berulang tiap jam","Menghafal lagu","Menebak warna"], ans:1 }
];
function renderAkhir(){
  const c = document.getElementById('akhirContainer');
  c.innerHTML = akhirData.map((d,i)=>`
    <div class="question-card">
      <span class="q-num">Q${i+1}</span>
      <div class="q-text">${d.q}</div>
      <div class="options" data-q="${i}">
        ${d.opts.map((o,oi)=>`<div class="option" data-oi="${oi}"><span class="option-letter">${String.fromCharCode(65+oi)}</span>${o}</div>`).join('')}
      </div>
    </div>`).join('') + `
    <div class="question-card">
      <span class="q-num">STUDI KASUS</span>
      <div class="q-text">6. Kamu diminta membuat aplikasi pengingat minum air. Jelaskan bagaimana kamu menerapkan 4 pilar berpikir komputasional!</div>
      <textarea id="akhirEsai" placeholder="Tulis jawabanmu (minimal 3 kalimat)..."></textarea>
    </div>`;
  c.querySelectorAll('.options').forEach(optWrap=>{
    const q = optWrap.dataset.q;
    optWrap.querySelectorAll('.option').forEach(opt=>{
      opt.onclick = ()=>{
        if(state.akhirSubmitted) return;
        playClick();
        optWrap.querySelectorAll('.option').forEach(o=>o.classList.remove('selected'));
        opt.classList.add('selected');
        state.akhirAnswers[q] = parseInt(opt.dataset.oi);
      };
    });
  });
}
renderAkhir();
document.getElementById('btnAkhir').onclick = ()=>{
  if(state.akhirSubmitted) return;
  playClick();
  let correct = 0;
  akhirData.forEach((d,i)=>{
    const wrap = document.querySelector(`#akhirContainer .options[data-q="${i}"]`);
    const ans = state.akhirAnswers[i];
    wrap.querySelectorAll('.option').forEach(o=>{
      o.classList.remove('selected');
      const oi = parseInt(o.dataset.oi);
      if(oi === d.ans) o.classList.add('correct');
      else if(oi === ans) o.classList.add('wrong');
    });
    if(ans === d.ans) correct++;
  });
  const esai = document.getElementById('akhirEsai').value.trim();
  const esaiScore = esai.length > 80 ? 1 : 0.5;
  const pct = Math.round(((correct + esaiScore)/6)*100);
  let msg = '';
  if(pct >= 80) msg = "🏆 <b>Luar biasa!</b> Kamu sudah menguasai 4 pilar berpikir komputasional!";
  else if(pct >= 60) msg = "👍 <b>Bagus!</b> Pemahamanmu sudah baik. Pelajari bagian yang masih salah.";
  else msg = "💪 <b>Jangan menyerah!</b> Baca ulang materi, lalu coba lagi. Kamu pasti bisa!";
  document.getElementById('akhirResult').innerHTML = `
    <div class="result-box">
      <div style="font-size:13px;color:var(--muted);letter-spacing:1px;">HASIL ASESMEN AKHIR</div>
      <div class="result-score">${pct}%</div>
      <div class="result-msg">${msg}</div>
      <div style="display:flex;gap:8px;justify-content:center;flex-wrap:wrap;margin-top:8px;">
        <div class="card" style="padding:12px 16px;margin:0;">✅ Pilihan ganda: <b>${correct}/${akhirData.length}</b></div>
        <div class="card" style="padding:12px 16px;margin:0;">✍️ Esai: <b>${esaiScore===1?'Baik':'Cukup'}</b></div>
      </div>
      <button class="btn btn-magenta" style="margin-top:16px;" onclick="goTo('refleksi')">🪞 Lanjut ke Refleksi</button>
    </div>`;
  state.akhirSubmitted = true;
  state.visited.add('asesmen-akhir');
  updateProgress();
  if(pct>=60) playCorrect(); else playWrong();
};

/* ============ REFLEKSI ============ */
document.querySelectorAll('#emojiPicker .emoji-opt').forEach(e=>{
  e.onclick = ()=>{
    playClick();
    document.querySelectorAll('#emojiPicker .emoji-opt').forEach(o=>o.classList.remove('selected'));
    e.classList.add('selected');
    state.refleksiEmoji = e.dataset.emoji;
  };
});
document.getElementById('btnRefleksi').onclick = ()=>{
  playClick();
  const pilar = document.getElementById('refleksiPilar').value;
  const kesan = document.getElementById('refleksiKesan').value.trim();
  const motivasi = document.getElementById('refleksiMotivasi').value.trim();
  const janji = document.getElementById('refleksiJanji').value.trim();
  const emoji = state.refleksiEmoji || '—';
  document.getElementById('refleksiResult').innerHTML = `
    <div class="reflection-card">
      <h3 style="margin-bottom:16px;font-size:18px;">🪞 Kartu Refleksi Kamu</h3>
      <h4>Pilar Favorit</h4><p>${pilar.charAt(0).toUpperCase()+pilar.slice(1)}</p>
      <h4>Perasaan</h4><p style="font-size:32px;">${emoji}</p>
      <h4>Kesan</h4><p>${kesan || '—'}</p>
      <h4>Motivasi</h4><p>${motivasi || '—'}</p>
      <h4>Janji Perubahan</h4><p>${janji || '—'}</p>
      <p style="margin-top:16px;color:var(--cyan);font-weight:600;">🎉 Terima kasih sudah menyelesaikan seluruh misi! Kamu adalah Agen Berpikir Komputasional! 🚀</p>
    </div>`;
  state.visited.add('refleksi');
  updateProgress();
  playCorrect();
  showToast('Kartu refleksi tersimpan! 🎉');
};

/* ============ TOAST ============ */
let toastTimeout;
function showToast(msg){
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimeout);
  toastTimeout = setTimeout(()=>t.classList.remove('show'), 2500);
}

/* ============ INIT ============ */
updateProgress();
</script>
</body>
</html>
