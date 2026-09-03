# Shared HTML Template Spec for Semester1 Study Guides

Every one of the 12 study-guide HTML files MUST follow this exact structure so all files look
identical. Only the title, the active tab, the nav links, and the `<main>` content change.

## Rules
- Self-contained single HTML file. Inline `<style>` (use the CSS block below verbatim).
- Load MathJax v3 from CDN. Write all math in LaTeX using `\( ... \)` for inline and `$$ ... $$` (or `\[ ... \]`) for display.
- Every content item lives inside a `.card` so the search box can show/hide it.
- Group cards under collapsible `<details class="module" open>` sections, one per source deck/lecture/case-study/module, with a `<summary>` giving the module name AND its source file (e.g. "Module 5 — DFNN · source: DNN_M5_DFNN (1).pdf").
- Each card must carry a `data-src` attribute naming the source file + page/slide (e.g. `data-src="ML_CS 1 · p12"`) and show it as a small `.src` tag.

## Exact page skeleton
Replace the tokens:
- `{{TITLE}}` → e.g. `ML — Concepts`
- `{{SUBJECT}}` → `ML` | `Maths` | `DNN` | `Stats`
- `{{KIND}}` → `Concepts` | `Formulas` | `Definitions`
- The three tab `<a>`s: the current kind gets `class="tab active"`, the other two link to sibling files (`SUBJECT-concepts.html`, `SUBJECT-formulas.html`, `SUBJECT-definitions.html`).
- `../index.html` is the "All Guides" link (files live in `StudyGuides/<Subject>/`).

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>{{TITLE}}</title>
<script>
MathJax = { tex: { inlineMath: [['\\(','\\)']], displayMath: [['$$','$$'],['\\[','\\]']] },
            svg: { fontCache: 'global' } };
</script>
<script async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<style>
:root{--bg:#0f172a;--panel:#1e293b;--card:#243049;--ink:#e2e8f0;--muted:#94a3b8;--accent:#38bdf8;--accent2:#f472b6;--border:#334155;}
*{box-sizing:border-box}
body{margin:0;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;background:var(--bg);color:var(--ink);line-height:1.6}
header{position:sticky;top:0;z-index:10;background:var(--panel);border-bottom:1px solid var(--border);padding:.75rem 1rem;box-shadow:0 2px 8px rgba(0,0,0,.3)}
.bar{display:flex;flex-wrap:wrap;gap:.5rem;align-items:center;max-width:1100px;margin:0 auto}
.bar h1{font-size:1.05rem;margin:0;margin-right:auto;color:#fff}
.bar h1 span{color:var(--accent)}
.tab{font-size:.85rem;text-decoration:none;color:var(--muted);padding:.35rem .7rem;border-radius:999px;border:1px solid var(--border)}
.tab:hover{color:var(--ink)}
.tab.active{background:var(--accent);color:#04263a;border-color:var(--accent);font-weight:600}
.tab.home{border-style:dashed}
#q{flex:1 1 100%;margin-top:.5rem;padding:.55rem .8rem;border-radius:8px;border:1px solid var(--border);background:#0b1220;color:var(--ink);font-size:.95rem}
main{max-width:1100px;margin:1.5rem auto;padding:0 1rem 4rem}
.intro{color:var(--muted);font-size:.9rem;margin-bottom:1.25rem}
.module{background:transparent;border:1px solid var(--border);border-radius:10px;margin:0 0 1rem}
.module>summary{cursor:pointer;padding:.7rem 1rem;font-weight:600;color:#fff;list-style:none}
.module>summary::-webkit-details-marker{display:none}
.module>summary::before{content:"▸ ";color:var(--accent)}
.module[open]>summary::before{content:"▾ "}
.module>summary small{color:var(--muted);font-weight:400}
.cards{padding:.5rem 1rem 1rem}
.card{background:var(--card);border:1px solid var(--border);border-left:3px solid var(--accent);border-radius:8px;padding:.75rem .9rem;margin:.6rem 0}
.card h3{margin:.1rem 0 .35rem;font-size:1rem;color:var(--accent)}
.card.def{border-left-color:var(--accent2)}
.card.def h3{color:var(--accent2)}
.card p{margin:.3rem 0}
.card .src{display:inline-block;margin-top:.4rem;font-size:.72rem;color:var(--muted);background:#0b1220;padding:.1rem .45rem;border-radius:4px}
.card ul{margin:.3rem 0 .3rem 1.1rem;padding:0}
/* --- Question Bank only --- */
.card.q{border-left-color:#a78bfa}
.card.q h3{color:#a78bfa}
.badge{display:inline-block;font-size:.68rem;font-weight:700;text-transform:uppercase;letter-spacing:.03em;padding:.1rem .5rem;border-radius:999px;margin-bottom:.35rem}
.badge.easy{background:#064e3b;color:#6ee7b7}
.badge.medium{background:#78350f;color:#fcd34d}
.badge.hard{background:#7f1d1d;color:#fca5a5}
.badge.mixed{background:#3730a3;color:#c7d2fe}
details.ans{margin-top:.5rem;border-top:1px dashed var(--border);padding-top:.4rem}
details.ans>summary{cursor:pointer;color:var(--accent);font-size:.85rem;font-weight:600;list-style:none}
details.ans>summary::-webkit-details-marker{display:none}
details.ans>summary::before{content:"▸ show answer";}
details.ans[open]>summary::before{content:"▾ answer";}
details.ans[open]>summary{color:var(--muted)}
details.ans .sol{margin-top:.4rem;color:var(--ink)}
.hidden{display:none!important}
.count{color:var(--muted);font-size:.8rem;margin-left:.4rem}
mark{background:#fde68a;color:#111;border-radius:3px}
a.tab:focus-visible,#q:focus{outline:2px solid var(--accent)}
@media(max-width:640px){.bar h1{flex:1 1 100%}}
</style>
</head>
<body>
<header>
  <div class="bar">
    <h1><span>{{SUBJECT}}</span> · {{KIND}}<span class="count" id="count"></span></h1>
    <a class="tab home" href="../index.html">All Guides</a>
    <a class="tab ..." href="{{SUBJECT}}-concepts.html">Concepts</a>
    <a class="tab ..." href="{{SUBJECT}}-formulas.html">Formulas</a>
    <a class="tab ..." href="{{SUBJECT}}-definitions.html">Definitions</a>
    <a class="tab ..." href="{{SUBJECT}}-questions.html">Question Bank</a>
    <input id="q" type="search" placeholder="Filter… (type to search this page)">
  </div>
</header>
<main>
  <p class="intro">{{one-line description of scope + source files covered}}</p>
  <!-- modules with cards go here -->
</main>
<script>
const q=document.getElementById('q'),cards=[...document.querySelectorAll('.card')],mods=[...document.querySelectorAll('.module')],cnt=document.getElementById('count');
function upd(){const t=q.value.trim().toLowerCase();let n=0;
  cards.forEach(c=>{const hit=!t||c.textContent.toLowerCase().includes(t);c.classList.toggle('hidden',!hit);if(hit)n++;});
  mods.forEach(m=>{const any=[...m.querySelectorAll('.card')].some(c=>!c.classList.contains('hidden'));m.classList.toggle('hidden',!any);if(t&&any)m.open=true;});
  cnt.textContent=t?` — ${n} match`+(n===1?'':'es'):` — ${cards.length} items`;}
q.addEventListener('input',upd);upd();
</script>
</body>
</html>
```

## Content quality bar
- **Concepts**: `<h3>` = concept name; `<p>` = 1–3 lines explaining what to understand and why it matters. Include worked-numerical intuition where the deck emphasised it.
- **Formulas**: `<h3>` = formula name; a `$$ ... $$` display equation; then a "where:" line/list defining each symbol; then a short "use:" note (when/why). Include prerequisite formulas needed to solve problems even if only implied (e.g. derivative/matrix/probability rules).
- **Definitions**: use `class="card def"`. `<h3>` = term; `<p>` = precise one/two-sentence definition. Roughly grouped by module.
- **Question Bank**: each question is a `<div class="card q">` containing, in order: a difficulty `<span class="badge easy|medium|hard|mixed">Easy</span>`, the question in a `<p>` (LaTeX for math), and a collapsible worked solution:
  `<details class="ans"><summary></summary><div class="sol"> …full step-by-step solution with final answer… </div></details>`.
  Give each question a `data-src`/`.src` tag naming the primary concept/formula it tests (e.g. `tests: OLS slope · Medium`). Group questions in `<details class="module" open>` sections. The active tab is "Question Bank".
- Deduplicate across decks; if a concept recurs, keep the fullest version and cite the primary source.
- Be exhaustive — sweep every page/slide. Do not silently skip a deck.
