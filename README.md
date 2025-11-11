<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Dark Notebook — Typing Banner</title>
  <style>
    :root{
      --bg:#0b0f14;
      --panel:#0f1720;
      --gutter:#0b1220;
      --text:#e6eef6;
      --accent:#1f6feb; /* original color from image */
      --muted:#94a3b8;
    }
    html,body{height:100%;margin:0;font-family: "Fira Code", ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", "Noto Sans Mono", monospace; background:linear-gradient(180deg,var(--bg),#020409);}

    .notebook {
      width:min(980px,92%);
      margin:48px auto;
      border-radius:12px;
      box-shadow:0 14px 40px rgba(2,6,23,0.7), inset 0 1px 0 rgba(255,255,255,0.02);
      overflow:hidden;
      display:grid;
      grid-template-columns:70px 1fr;
      background:linear-gradient(180deg,var(--panel),#071019);
      border:1px solid rgba(127,140,160,0.06);
    }

    /* left gutter (like file list / line numbers) */
    .gutter{
      background:linear-gradient(180deg,var(--gutter),#071018);
      padding:14px 10px;
      color:var(--muted);
      display:flex;flex-direction:column;gap:8px;align-items:center;
    }
    .gutter .dot{width:10px;height:10px;border-radius:50%;background:#ff5f56;box-shadow:0 0 0 6px rgba(255,95,86,0.06)}
    .gutter .dot.yellow{background:#ffbd2e}
    .gutter .dot.green{background:#27c93f}

    /* editor area */
    .editor{padding:22px 28px;min-height:160px;position:relative;color:var(--text)}
    .meta{display:flex;align-items:center;gap:10px;margin-bottom:10px}
    .filename{font-size:13px;color:var(--muted)}
    .codebox{background:transparent;border-radius:8px;padding:18px;font-size:20px;line-height:1.4;white-space:pre-wrap;word-break:break-word}

    .line{display:block}

    /* cursor */
    .cursor{display:inline-block;width:10px;height:1.05em;background:var(--accent);vertical-align:bottom;margin-left:4px;border-radius:2px;animation:blink 1s steps(2,start) infinite}
    @keyframes blink{50%{opacity:0}}

    /* small helper to look like code comment */
    .comment{color:#607B96;font-size:13px;margin-bottom:8px}

    /* selection style */
    ::selection{background:rgba(31,111,235,0.25)}

    /* responsive */
    @media (max-width:640px){.notebook{grid-template-columns:56px 1fr;padding:0}.editor{padding:12px}}

    /* subtle glow on accent text */
    .accent{color:var(--accent);text-shadow:0 1px 0 rgba(31,111,235,0.12)}
  </style>
</head>
<body>
  <div class="notebook" role="region" aria-label="Dark coding notebook example">
    <aside class="gutter" aria-hidden>
      <div class="dot"></div>
      <div class="dot yellow"></div>
      <div class="dot green"></div>
    </aside>

    <main class="editor">
      <div class="meta">
        <div class="filename">index.html</div>
        <div style="flex:1"></div>
        <div class="filename">UTF-8</div>
      </div>

      <div class="comment">// 여기는 코드 치는 느낌의 검은 노트북 환경입니다</div>
      <pre class="codebox" id="codebox" aria-live="polite"></pre>
    </main>
  </div>

  <script>
    // 텍스트 — 이모지 포함이므로 CSS typing 대신 JS로 문자별 타이핑 처리
    const lines = [
      "👋 Hi there! I'm Chaeryoung Kim;",

    ];

    const box = document.getElementById('codebox');

    // 타입 효과 함수
    async function typeLines(lines, el, opts={delay:50}){
      for(const line of lines){
        const lineEl = document.createElement('span');
        lineEl.className='line';
        el.appendChild(lineEl);
        for(const ch of line){
          lineEl.textContent += ch;
          // ensure cursor visible while typing
          showCursor(el);
          await sleep(opts.delay + randomInt(-10,40));
        }
        // 줄 끝에 줄바꿈
        el.appendChild(document.createTextNode('\n'));
        await sleep(120);
      }
      hideCursor(el);
    }

    // cursor helpers: render a blinking block after the last line
    function showCursor(el){
      removeCursor(el);
      const c = document.createElement('span');
      c.className='cursor';
      c.setAttribute('aria-hidden','true');
      el.appendChild(c);
      // scroll into view if needed
      el.parentElement.scrollTop = el.parentElement.scrollHeight;
    }
    function hideCursor(el){ removeCursor(el); }
    function removeCursor(el){ const old = el.querySelector('.cursor'); if(old) old.remove(); }

    function sleep(ms){ return new Promise(r=>setTimeout(r,ms)); }
    function randomInt(a,b){ return Math.floor(Math.random()*(b-a+1))+a }

    // 시작
    (async ()=>{
      box.textContent='';
      await sleep(300);
      await typeLines(lines, box, {delay:50});
      // 강조: 첫 줄의 이름만 색을 바꿔보기
      const first = box.querySelector('.line');
      if(first){
        // 간단한 하이라이트: 감싸서 색 지정
        const txt = first.textContent;
        first.textContent='';
        const span1 = document.createElement('span');
        span1.textContent = '👋 Hi there! ';
        const span2 = document.createElement('span');
        span2.textContent = "I'm Chaeryoung Kim;";
        span2.className='accent';
        first.appendChild(span1);
        first.appendChild(span2);
      }
      showCursor(box);
    })();
  </script>
</body>
</html>


