
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font-mono)}
.wrap{background:#0d1117;border-radius:12px;padding:28px 32px;color:#c9d1d9;font-size:14px;line-height:1.7}
.green{color:#39d353}
.muted{color:#8b949e;font-size:12px}
h1{color:#e6edf3;font-size:22px;font-weight:500;margin-bottom:4px;font-family:var(--font-sans)}
.tag{display:inline-block;background:#161b22;border:1px solid #30363d;border-radius:6px;padding:2px 10px;font-size:12px;color:#8b949e;margin-right:6px;margin-bottom:6px}
.stats-row{display:flex;gap:16px;margin:20px 0;flex-wrap:wrap}
.stat-card{background:#161b22;border:1px solid #30363d;border-radius:10px;padding:16px 20px;flex:1;min-width:140px;text-align:center}
.stat-num{font-size:28px;font-weight:500;color:#39d353;display:block}
.stat-label{font-size:11px;color:#8b949e;margin-top:2px}
.stat-sub{font-size:10px;color:#484f58}
.ring-wrap{position:relative;display:inline-block;margin:8px 0}
svg.ring{display:block}
@keyframes spin{to{stroke-dashoffset:0}}
.streak-ring{stroke-dasharray:220;stroke-dashoffset:220;animation:spin 1.5s ease forwards}
.section-title{font-size:15px;font-weight:500;color:#e6edf3;border-bottom:1px solid #21262d;padding-bottom:6px;margin:20px 0 10px;font-family:var(--font-sans)}
.grid{display:grid;grid-template-columns:repeat(5,1fr);gap:6px;margin-bottom:16px}
.contrib-cell{height:12px;border-radius:2px;background:#161b22}
@keyframes fadeIn{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:none}}
.animate-in{animation:fadeIn .5s ease forwards}
.code-block{background:#161b22;border:1px solid #30363d;border-radius:8px;padding:14px 16px;font-family:var(--font-mono);font-size:11px;color:#c9d1d9;white-space:pre;overflow-x:auto;margin-top:16px;line-height:1.8}
.copy-btn{display:inline-block;margin-top:8px;background:#21262d;border:1px solid #30363d;color:#c9d1d9;border-radius:6px;padding:4px 12px;font-size:12px;cursor:pointer;font-family:var(--font-sans)}
.copy-btn:hover{background:#30363d}
.username-input{background:#21262d;border:1px solid #30363d;border-radius:6px;padding:5px 10px;color:#e6edf3;font-family:var(--font-mono);font-size:13px;width:200px;outline:none}
.username-input:focus{border-color:#39d353}
</style>

<div class="wrap">
  <div style="margin-bottom:16px">
    <label style="font-size:12px;color:#8b949e;display:block;margin-bottom:6px">your github username</label>
    <input class="username-input" id="un" placeholder="e.g. Prox-C" oninput="update()">
  </div>

  <h1>Hi, I'm <span id="disp-name">Your Name</span> <span style="font-size:18px">&#128075;</span></h1>
  <p class="green" style="font-size:15px;font-family:var(--font-mono);margin:6px 0 14px">i solve problems</p>

  <div class="stats-row animate-in">
    <div class="stat-card">
      <span class="stat-num" id="contrib-num">1,090</span>
      <div class="stat-label" style="color:#39d353">Total Contributions</div>
      <div class="stat-sub">Jan 22, 2023 – Present</div>
    </div>
    <div class="stat-card" style="display:flex;flex-direction:column;align-items:center">
      <div class="ring-wrap">
        <svg class="ring" width="72" height="72" viewBox="0 0 72 72">
          <circle cx="36" cy="36" r="30" fill="none" stroke="#21262d" stroke-width="5"/>
          <circle id="streak-ring" class="streak-ring" cx="36" cy="36" r="30" fill="none" stroke="#39d353" stroke-width="5" stroke-linecap="round" transform="rotate(-90 36 36)"/>
          <text x="36" y="40" text-anchor="middle" fill="#e6edf3" font-size="16" font-weight="500" font-family="monospace" id="streak-val">0</text>
        </svg>
      </div>
      <div class="stat-label" style="color:#39d353">Current Streak</div>
      <div class="stat-sub" id="streak-date">Apr 19</div>
    </div>
    <div class="stat-card">
      <span class="stat-num">32</span>
      <div class="stat-label" style="color:#39d353">Longest Streak</div>
      <div class="stat-sub">Nov 18 – Dec 19, 2024</div>
    </div>
  </div>

  <div style="margin:8px 0 20px">
    <div class="muted" style="margin-bottom:8px">contribution activity preview</div>
    <div class="grid" id="contrib-grid"></div>
  </div>

  <div class="section-title">about me</div>
  <p style="font-size:13px;color:#c9d1d9;margin-bottom:8px">Hello world! I'm <strong style="color:#e6edf3" id="about-name">Your Name</strong>, a frontend software engineer and creative visual designer based in the Philippines.</p>

  <div style="margin-top:20px">
    <div class="muted" style="margin-bottom:10px">paste this into your README.md</div>
    <div class="code-block" id="code-output">enter your username above</div>
    <button class="copy-btn" onclick="copyCode()">copy README code</button>
    <span id="copied" style="font-size:12px;color:#39d353;margin-left:8px;opacity:0;transition:opacity .3s"></span>
  </div>
</div>

<script>
const grid = document.getElementById('contrib-grid');
const colors = ['#161b22','#0e4429','#006d32','#26a641','#39d353'];
for(let i=0;i<52*7;i++){
  const c = document.createElement('div');
  c.className='contrib-cell';
  const r=Math.random();
  c.style.background = r<0.55?colors[0]:r<0.7?colors[1]:r<0.82?colors[2]:r<0.93?colors[3]:colors[4];
  grid.appendChild(c);
}

let sv=0;
const target=0;
const ring=document.getElementById('streak-ring');
const sval=document.getElementById('streak-val');
const circ=2*Math.PI*30;
ring.style.strokeDasharray=circ;
ring.style.strokeDashoffset=circ;
ring.style.animation='none';

function update(){
  const un=document.getElementById('un').value.trim()||'YOUR_USERNAME';
  document.getElementById('disp-name').textContent=un!=='YOUR_USERNAME'?un:'Your Name';
  document.getElementById('about-name').textContent=un!=='YOUR_USERNAME'?un:'Your Name';

  const code=`# Hi, I'm ${un} 👋\n<h3 align="left" style="color:#39d353">i solve problems</h3>\n\n---\n\n[![GitHub Streak](https://streak-stats.demolab.com?user=${un}&theme=github-dark-blue&hide_border=true&date_format=M%20j%5B%2C%20Y%5D&ring=39d353&fire=39d353&currStreakLabel=39d353)](https://git.io/streak-stats)\n\n[![trophy](https://github-profile-trophy.vercel.app/?username=${un}&theme=onedark&no-bg=true&row=1)](https://github.com/${un})\n\n## ℹ️ About me\n\nHello world! I'm **${un}**, a frontend software engineer and creative visual designer. Driven by passion for computers and tech, I turn imaginative ideas into practical solutions.\n\n<details>\n  <summary>▶ Status quo</summary>\n  Grinding everyday.\n</details>\n\n## 💚 Stuff I do\n\nExploring front-end technologies and learning UI/UX design principles.\n\n<details>\n  <summary>▶ Work in progress</summary>\n  Full-stack projects in progress.\n</details>\n\n## </> My tech stack\n\n[![My Skills](https://skillicons.dev/icons?i=js,ts,react,nextjs,tailwind,css,figma,git)](https://skillicons.dev)\n\n<details>\n  <summary>▶ My stack</summary>\n\n  **Languages:** JavaScript, TypeScript, HTML, CSS  \n  **Frameworks:** React, Next.js, Tailwind CSS  \n  **Tools:** Figma, Git, VS Code\n</details>`;

  document.getElementById('code-output').textContent=code;
}

function copyCode(){
  const txt=document.getElementById('code-output').textContent;
  if(txt==='enter your username above')return;
  navigator.clipboard.writeText(txt).then(()=>{
    const el=document.getElementById('copied');
    el.textContent='copied!';
    el.style.opacity='1';
    setTimeout(()=>el.style.opacity='0',2000);
  });
}

setTimeout(()=>{
  let n=0;
  const max=5;
  const iv=setInterval(()=>{
    n++;sval.textContent=n;
    const pct=n/max;
    ring.style.strokeDasharray=circ;
    ring.style.strokeDashoffset=circ*(1-pct);
    if(n>=max)clearInterval(iv);
  },200);
},600);

update();
</script>
