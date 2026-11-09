<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="description" content="Medical literature hub: PICO search builder, topic shelf, evidence ladder and reporting guides.">
<title>MedLit Atlas</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,500;12..96,700&family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap">
<style>
/* Layout: query builder first, then a filterable topic grid, then a method shelf (evidence ladder, reporting guides, source directory). */
:root {
  --bg: #F2F7F9;
  --surface: #FFFFFF;
  --surface-2: #E6F0F3;
  --fg: #0D2931;
  --muted: #4F6D77;
  --line: #CFE0E6;
  --accent: #0A7C8A;
  --accent-ink: #FFFFFF;
  --blue: #1F58B8;
  --good: #1C7A4B;
  --font-display: "Bricolage Grotesque", "Segoe UI", system-ui, sans-serif;
  --font-body: "IBM Plex Sans", "Segoe UI", system-ui, sans-serif;
  --font-mono: "IBM Plex Mono", ui-monospace, Menlo, Consolas, monospace;
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #09191F; --surface: #102830; --surface-2: #17343D; --fg: #E2F0F3; --muted: #93AEB7;
    --line: #234651; --accent: #4CC4D1; --accent-ink: #06232A; --blue: #7FAAF2; --good: #5CCB93;
    color-scheme: dark;
  }
}
:root[data-theme="dark"] {
  --bg: #09191F; --surface: #102830; --surface-2: #17343D; --fg: #E2F0F3; --muted: #93AEB7;
  --line: #234651; --accent: #4CC4D1; --accent-ink: #06232A; --blue: #7FAAF2; --good: #5CCB93;
  color-scheme: dark;
}
* { box-sizing: border-box; }
body {
  background: var(--bg); color: var(--fg);
  font: 400 16px/1.55 var(--font-body);
  padding-block: 0 48px;
  padding-left: max(16px, env(safe-area-inset-left, 0px));
  padding-right: max(16px, env(safe-area-inset-right, 0px));
}
.wrap { max-width: 1080px; margin: 0 auto; display: flex; flex-direction: column; gap: 56px; }
a { color: var(--blue); }
a:focus-visible, button:focus-visible, input:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }
h1, h2, h3 { font-family: var(--font-display); text-wrap: balance; margin: 0; line-height: 1.12; letter-spacing: -0.01em; }
.eyebrow { font: 500 12px/1 var(--font-mono); letter-spacing: .12em; text-transform: uppercase; color: var(--accent); }

header.top { display: flex; align-items: center; justify-content: space-between; gap: 12px; padding-block: 18px; border-bottom: 1px solid var(--line); flex-wrap: wrap; }
.brand { font: 700 20px/1 var(--font-display); display: flex; align-items: center; gap: 10px; }
.brand svg { width: 28px; height: 28px; }
nav.links { display: flex; gap: 18px; font-size: 14px; flex-wrap: wrap; }
nav.links a { color: var(--muted); text-decoration: none; }
nav.links a:hover { color: var(--fg); }

.hero { display: grid; grid-template-columns: minmax(0, 1fr); gap: 28px; padding-top: 12px; }
.hero h1 { font-size: clamp(2rem, 6vw, 3.4rem); max-width: 18ch; }
.hero p.lede { color: var(--muted); max-width: 58ch; margin: 14px 0 0; font-size: 1.05rem; }

.builder { background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 20px; display: flex; flex-direction: column; gap: 18px; min-width: 0; }
.builder-head { display: flex; justify-content: space-between; gap: 8px; align-items: baseline; flex-wrap: wrap; }
.builder-head h2 { font-size: 1.25rem; }
.builder-head span { font: 400 12px var(--font-mono); color: var(--muted); }
.pico { display: grid; grid-template-columns: repeat(auto-fit, minmax(210px, 1fr)); gap: 12px; }
.field { display: flex; flex-direction: column; gap: 6px; min-width: 0; }
.field label { font: 600 13px var(--font-body); display: flex; gap: 8px; align-items: center; }
.field label b { font: 500 12px var(--font-mono); background: var(--accent); color: var(--accent-ink); border-radius: 4px; padding: 2px 6px; }
.field input, .search input { width: 100%; font: 400 15px var(--font-body); color: var(--fg); background: var(--bg); border: 1px solid var(--line); border-radius: 8px; padding: 10px 12px; min-width: 0; }
.opts { display: flex; flex-wrap: wrap; gap: 8px; }
.opts label { display: flex; gap: 8px; align-items: center; font-size: 14px; border: 1px solid var(--line); border-radius: 999px; padding: 6px 12px; cursor: pointer; background: var(--bg); }
.opts input { accent-color: var(--accent); width: 16px; height: 16px; }
.query { font: 400 13px/1.6 var(--font-mono); background: var(--surface-2); border-radius: 8px; padding: 12px 14px; overflow-wrap: anywhere; min-width: 0; }
.actions { display: flex; flex-wrap: wrap; gap: 10px; }
.btn { font: 600 14px var(--font-body); border-radius: 8px; padding: 10px 16px; border: 1px solid var(--accent); background: var(--accent); color: var(--accent-ink); text-decoration: none; cursor: pointer; display: inline-block; }
.btn.alt { background: transparent; color: var(--fg); border-color: var(--line); }
.btn.alt:hover { border-color: var(--accent); }
.note { font-size: 13px; color: var(--muted); margin: 0; }

section.block { display: flex; flex-direction: column; gap: 20px; min-width: 0; }
.sec-head { display: flex; flex-direction: column; gap: 8px; max-width: 62ch; }
.sec-head h2 { font-size: clamp(1.5rem, 4vw, 2.1rem); }
.sec-head p { margin: 0; color: var(--muted); }

.tools { display: flex; flex-direction: column; gap: 12px; }
.search { max-width: 420px; }
.chips { display: flex; flex-wrap: wrap; gap: 8px; }
.chip { font: 500 13px var(--font-body); border: 1px solid var(--line); background: var(--surface); color: var(--fg); border-radius: 999px; padding: 6px 14px; cursor: pointer; }
.chip[aria-pressed="true"] { background: var(--accent); color: var(--accent-ink); border-color: var(--accent); }
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 14px; }
.card { background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 18px; display: flex; flex-direction: column; gap: 10px; min-width: 0; }
.card .spec { font: 500 11px var(--font-mono); letter-spacing: .1em; text-transform: uppercase; color: var(--accent); }
.card h3 { font-size: 1.1rem; }
.card p { margin: 0; color: var(--muted); font-size: 14.5px; flex: 1; }
.card .terms { font: 400 12px/1.5 var(--font-mono); color: var(--muted); overflow-wrap: anywhere; }
.card .go { display: flex; flex-wrap: wrap; gap: 14px; font-size: 14px; font-weight: 600; }
.empty { color: var(--muted); padding: 24px 0; }

.ladder { display: flex; flex-direction: column; gap: 8px; }
.rung { display: grid; grid-template-columns: minmax(0, var(--w)) minmax(0, 1fr); gap: 14px; align-items: stretch; }
.rung .bar { background: var(--accent); color: var(--accent-ink); border-radius: 6px; padding: 10px 12px; font: 600 14px/1.3 var(--font-display); min-width: 0; opacity: var(--o); }
.rung .why { font-size: 14px; color: var(--muted); align-self: center; min-width: 0; }
@media (max-width: 560px) { .rung { grid-template-columns: minmax(0, 1fr); gap: 4px; } .rung .bar { width: var(--w); min-width: 62%; } }

.tablewrap { overflow-x: auto; border: 1px solid var(--line); border-radius: 12px; background: var(--surface); }
table { border-collapse: collapse; width: 100%; min-width: 520px; font-size: 14.5px; }
th, td { text-align: left; padding: 12px 16px; border-bottom: 1px solid var(--line); vertical-align: top; }
tr:last-child td { border-bottom: 0; }
th { font: 500 11px var(--font-mono); letter-spacing: .1em; text-transform: uppercase; color: var(--muted); background: var(--surface-2); }
td:first-child { font: 600 14px var(--font-mono); color: var(--accent); white-space: nowrap; }

.sources { display: grid; grid-template-columns: repeat(auto-fill, minmax(230px, 1fr)); gap: 12px; }
.src { display: flex; flex-direction: column; gap: 4px; padding: 14px 16px; border: 1px solid var(--line); border-radius: 10px; background: var(--surface); text-decoration: none; color: var(--fg); min-width: 0; }
.src:hover { border-color: var(--accent); }
.src strong { font-family: var(--font-display); font-size: 1rem; }
.src span { font-size: 13.5px; color: var(--muted); }

footer { border-top: 1px solid var(--line); padding-top: 20px; font-size: 13.5px; color: var(--muted); max-width: 70ch; }
@media (prefers-reduced-motion: reduce) { * { transition: none !important; } }
</style>

</head>
<body>
<div class="wrap">
  <header class="top">
    <div class="brand">
      <svg viewBox="0 0 32 32" fill="none" aria-hidden="true"><circle cx="16" cy="16" r="14" stroke="currentColor" stroke-width="2" style="color:var(--accent)"/><path d="M16 7v18M7 16h18" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" style="color:var(--accent)"/><circle cx="16" cy="16" r="3" fill="var(--blue)"/></svg>
      MedLit Atlas
    </div>
    <nav class="links" aria-label="Sections">
      <a href="#topics">Topics</a><a href="#evidence">Evidence</a><a href="#reporting">Reporting guides</a><a href="#sources">Sources</a>
    </nav>
  </header>

  <div class="hero">
    <div>
      <div class="eyebrow">Medical literature hub</div>
      <h1 style="margin-top:12px">Turn a clinical question into a literature search.</h1>
      <p class="lede">Frame your question with PICO, open it in PubMed, Europe PMC or Cochrane, then check the study design and reporting standard before you trust a result.</p>
    </div>

    <div class="builder" id="builder">
      <div class="builder-head"><h2>PICO search builder</h2><span>Example question prefilled</span></div>
      <div class="pico">
        <div class="field"><label for="p"><b>P</b>Population</label><input id="p" type="text" value="adults with type 2 diabetes" autocomplete="off"></div>
        <div class="field"><label for="i"><b>I</b>Intervention</label><input id="i" type="text" value="SGLT2 inhibitor" autocomplete="off"></div>
        <div class="field"><label for="c"><b>C</b>Comparison</label><input id="c" type="text" value="placebo" autocomplete="off"></div>
        <div class="field"><label for="o"><b>O</b>Outcome</label><input id="o" type="text" value="cardiovascular mortality" autocomplete="off"></div>
      </div>
      <div class="opts" role="group" aria-label="Filters">
        <label><input type="checkbox" id="f-sr" checked> Systematic reviews</label>
        <label><input type="checkbox" id="f-rct"> Randomized trials</label>
        <label><input type="checkbox" id="f-5y"> Last 5 years</label>
        <label><input type="checkbox" id="f-free"> Free full text</label>
      </div>
      <div class="query" id="query" aria-live="polite"></div>
      <div class="actions">
        <a class="btn" id="go-pubmed" target="_blank" rel="noopener">Search PubMed</a>
        <a class="btn alt" id="go-pmc" target="_blank" rel="noopener">Europe PMC</a>
        <a class="btn alt" id="go-cochrane" target="_blank" rel="noopener">Cochrane Library</a>
        <button class="btn alt" id="copy" type="button">Copy query</button>
      </div>
      <p class="note">Leave a box empty to drop it from the query. Add synonyms with OR, for example: heart failure OR cardiac failure. Filters apply to PubMed.</p>
    </div>
  </div>

  <section class="block" id="topics">
    <div class="sec-head">
      <div class="eyebrow">Topic shelf</div>
      <h2>Start from a clinical topic</h2>
      <p>Each card opens a ready-made search for systematic reviews on that topic. Narrow it further with the builder above.</p>
    </div>
    <div class="tools">
      <div class="search"><input id="q" type="search" placeholder="Filter topics, e.g. sepsis" aria-label="Filter topics"></div>
      <div class="chips" id="chips" role="group" aria-label="Specialty"></div>
    </div>
    <div class="grid" id="grid"></div>
  </section>

  <section class="block" id="evidence">
    <div class="sec-head">
      <div class="eyebrow">Evidence ladder</div>
      <h2>Which study design answers which question</h2>
      <p>Higher designs are usually less prone to bias for treatment questions. Design fit matters more than rank: a prognosis or harm question often needs a cohort study.</p>
    </div>
    <div class="ladder">
      <div class="rung" style="--w:46%;--o:1"><div class="bar">Systematic reviews and meta-analyses</div><div class="why">Pool and appraise all eligible studies on one question.</div></div>
      <div class="rung" style="--w:58%;--o:.92"><div class="bar">Randomized controlled trials</div><div class="why">Randomization balances known and unknown confounders.</div></div>
      <div class="rung" style="--w:70%;--o:.84"><div class="bar">Cohort studies</div><div class="why">Follow exposed and unexposed groups; suited to prognosis and harm.</div></div>
      <div class="rung" style="--w:82%;--o:.76"><div class="bar">Case-control studies</div><div class="why">Efficient for rare outcomes; prone to recall and selection bias.</div></div>
      <div class="rung" style="--w:92%;--o:.68"><div class="bar">Case series and case reports</div><div class="why">Signal generation for rare events; no comparison group.</div></div>
      <div class="rung" style="--w:100%;--o:.6"><div class="bar">Expert opinion and background reviews</div><div class="why">Useful for orientation; check what the opinion rests on.</div></div>
    </div>
  </section>

  <section class="block" id="reporting">
    <div class="sec-head">
      <div class="eyebrow">Reporting guides</div>
      <h2>Check a paper against its reporting standard</h2>
      <p>A good paper follows the checklist for its design. Open the EQUATOR Network library to find the full checklist and flow diagram.</p>
    </div>
    <div class="tablewrap">
      <table>
        <thead><tr><th>Guideline</th><th>Applies to</th><th>Look for</th></tr></thead>
        <tbody>
          <tr><td>CONSORT</td><td>Randomized trials</td><td>Participant flow diagram, allocation concealment, prespecified primary outcome</td></tr>
          <tr><td>PRISMA</td><td>Systematic reviews</td><td>Search strategy for each database, screening flow, risk-of-bias assessment</td></tr>
          <tr><td>STROBE</td><td>Cohort, case-control, cross-sectional</td><td>Confounder handling, missing data, how participants were selected</td></tr>
          <tr><td>STARD</td><td>Diagnostic accuracy studies</td><td>Reference standard, blinding of test readers, sensitivity and specificity with intervals</td></tr>
          <tr><td>CARE</td><td>Case reports</td><td>Timeline, patient perspective, consent</td></tr>
          <tr><td>TRIPOD</td><td>Prediction models</td><td>Validation cohort, calibration, handling of missing predictors</td></tr>
        </tbody>
      </table>
    </div>
    <div class="actions"><a class="btn alt" href="https://www.equator-network.org/reporting-guidelines/" target="_blank" rel="noopener">Open the EQUATOR library</a></div>
  </section>

  <section class="block" id="sources">
    <div class="sec-head">
      <div class="eyebrow">Source directory</div>
      <h2>Where to look</h2>
      <p>Databases and registries that index peer-reviewed work, trials and guidance.</p>
    </div>
    <div class="sources">
      <a class="src" href="https://pubmed.ncbi.nlm.nih.gov/" target="_blank" rel="noopener"><strong>PubMed</strong><span>Biomedical citations and abstracts from the US National Library of Medicine.</span></a>
      <a class="src" href="https://pmc.ncbi.nlm.nih.gov/" target="_blank" rel="noopener"><strong>PubMed Central</strong><span>Free full-text archive of biomedical journal articles.</span></a>
      <a class="src" href="https://www.cochranelibrary.com/" target="_blank" rel="noopener"><strong>Cochrane Library</strong><span>Systematic reviews of healthcare interventions.</span></a>
      <a class="src" href="https://europepmc.org/" target="_blank" rel="noopener"><strong>Europe PMC</strong><span>Articles, preprints and patents with text-mined links.</span></a>
      <a class="src" href="https://clinicaltrials.gov/" target="_blank" rel="noopener"><strong>ClinicalTrials.gov</strong><span>Registry of trials, useful for spotting unpublished results.</span></a>
      <a class="src" href="https://iris.who.int/" target="_blank" rel="noopener"><strong>WHO IRIS</strong><span>World Health Organization guidelines and reports.</span></a>
      <a class="src" href="https://doaj.org/" target="_blank" rel="noopener"><strong>DOAJ</strong><span>Directory of open-access journals with quality criteria.</span></a>
      <a class="src" href="https://medlineplus.gov/" target="_blank" rel="noopener"><strong>MedlinePlus</strong><span>Plain-language health topics for patient-facing background.</span></a>
    </div>
  </section>

  <footer>
    MedLit Atlas builds search links and teaches study appraisal. It does not store or summarize individual papers, and it is not clinical advice. Read the full article and current guidelines before applying a finding to patient care.
  </footer>
</div>

<script>
(function () {
  var $ = function (id) { return document.getElementById(id); };

  /* ---------- PICO builder ---------- */
  function group(v) {
    v = v.trim();
    if (!v) return "";
    return /\s(OR|AND)\s|\(/.test(v) ? "(" + v + ")" : v;
  }
  function build() {
    var parts = ["p", "i", "c", "o"].map(function (k) { return group($(k).value); }).filter(Boolean);
    var q = parts.join(" AND ");
    var plain = q;
    var f = [];
    if ($("f-sr").checked) f.push("systematic[sb]");
    if ($("f-rct").checked) f.push("randomized controlled trial[pt]");
    if ($("f-free").checked) f.push("free full text[sb]");
    if ($("f-5y").checked) {
      var d = new Date(); var y = d.getFullYear() - 5;
      var mm = String(d.getMonth() + 1).padStart(2, "0"), dd = String(d.getDate()).padStart(2, "0");
      f.push('("' + y + "/" + mm + "/" + dd + '"[dp] : "3000"[dp])');
    }
    var full = q;
    if (q && f.length) full = "(" + q + ") AND " + f.join(" AND ");
    else if (!q && f.length) full = f.join(" AND ");
    $("query").textContent = full || "Type a population, intervention, comparison or outcome to build a query.";
    var e = encodeURIComponent(full);
    $("go-pubmed").href = "https://pubmed.ncbi.nlm.nih.gov/?term=" + e;
    $("go-pmc").href = "https://europepmc.org/search?query=" + encodeURIComponent(plain);
    $("go-cochrane").href = "https://www.cochranelibrary.com/search?q=" + encodeURIComponent(plain);
    return full;
  }
  ["p", "i", "c", "o", "f-sr", "f-rct", "f-5y", "f-free"].forEach(function (id) {
    $(id).addEventListener("input", build);
    $(id).addEventListener("change", build);
  });
  $("copy").addEventListener("click", function () {
    var btn = this, text = build();
    function done(msg) { btn.textContent = msg; setTimeout(function () { btn.textContent = "Copy query"; }, 1600); }
    try {
      navigator.clipboard.writeText(text).then(function () { done("Copied"); }, function () { sel(); });
    } catch (e) { sel(); }
    function sel() {
      var r = document.createRange(); r.selectNodeContents($("query"));
      var s = window.getSelection(); s.removeAllRanges(); s.addRange(r); done("Selected");
    }
  });
  build();

  /* ---------- Topics ---------- */
  var topics = [
    { s: "Cardiology", t: "Heart failure with preserved ejection fraction", d: "Diagnosis and drug therapy for symptomatic heart failure when systolic function is preserved.", k: "heart failure with preserved ejection fraction treatment" },
    { s: "Cardiology", t: "Hypertension management", d: "Blood pressure targets, drug classes and lifestyle measures in adults.", k: "hypertension blood pressure lowering adults" },
    { s: "Endocrinology", t: "Type 2 diabetes pharmacotherapy", d: "Glucose-lowering drugs and their effects on cardiovascular and kidney outcomes.", k: "type 2 diabetes glucose-lowering drugs outcomes" },
    { s: "Endocrinology", t: "Thyroid nodules and cancer", d: "Ultrasound risk stratification, fine-needle aspiration and surveillance.", k: "thyroid nodule ultrasound risk stratification" },
    { s: "Infectious disease", t: "Sepsis in adults", d: "Early recognition, antimicrobial timing and fluid resuscitation strategies.", k: "sepsis adults antibiotics fluid resuscitation" },
    { s: "Infectious disease", t: "Antimicrobial resistance", d: "Stewardship programmes and surveillance of resistant organisms.", k: "antimicrobial stewardship resistance" },
    { s: "Oncology", t: "Complementary medicine in cancer care", d: "Use, safety and evidence for complementary and alternative approaches among people with cancer.", k: "complementary alternative medicine cancer patients" },
    { s: "Oncology", t: "Cancer screening programmes", d: "Benefits, harms and overdiagnosis in population screening.", k: "cancer screening overdiagnosis benefits harms" },
    { s: "Neurology", t: "Acute ischaemic stroke", d: "Reperfusion therapy, time windows and secondary prevention.", k: "acute ischemic stroke thrombolysis thrombectomy" },
    { s: "Psychiatry", t: "Depression treatment", d: "Psychotherapy and antidepressants, alone and combined, in adults.", k: "major depressive disorder psychotherapy antidepressant" },
    { s: "Pathology", t: "Digital pathology and AI", d: "Computer-assisted slide analysis and its diagnostic accuracy.", k: "digital pathology artificial intelligence diagnostic accuracy" },
    { s: "Paediatrics", t: "Childhood vaccination", d: "Effectiveness, safety and schedule timing of routine immunisation.", k: "childhood vaccination effectiveness safety" },
    { s: "Public health", t: "Tobacco cessation", d: "Behavioural support and pharmacotherapy to help people quit smoking.", k: "smoking cessation interventions" },
    { s: "Public health", t: "Air pollution and health", d: "Particulate exposure and respiratory, cardiovascular and pregnancy outcomes.", k: "air pollution PM2.5 health outcomes" },
    { s: "Surgery", t: "Enhanced recovery after surgery", d: "Perioperative care pathways and their effect on length of stay and complications.", k: "enhanced recovery after surgery" }
  ];
  var specs = ["All"].concat(topics.map(function (x) { return x.s; }).filter(function (v, i, a) { return a.indexOf(v) === i; }));
  var active = "All";
  function esc(s) { return s.replace(/[&<>"]/g, function (c) { return { "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" }[c]; }); }
  function renderChips() {
    $("chips").innerHTML = specs.map(function (s) {
      return '<button type="button" class="chip" aria-pressed="' + (s === active) + '" data-s="' + esc(s) + '">' + esc(s) + "</button>";
    }).join("");
  }
  function renderGrid() {
    var q = $("q").value.trim().toLowerCase();
    var list = topics.filter(function (x) {
      return (active === "All" || x.s === active) && (!q || (x.t + " " + x.d + " " + x.k + " " + x.s).toLowerCase().indexOf(q) > -1);
    });
    if (!list.length) { $("grid").innerHTML = '<div class="empty">No topics match. Clear the filter or build your own query above.</div>'; return; }
    $("grid").innerHTML = list.map(function (x) {
      var pm = "https://pubmed.ncbi.nlm.nih.gov/?term=" + encodeURIComponent("(" + x.k + ") AND systematic[sb]");
      var cc = "https://www.cochranelibrary.com/search?q=" + encodeURIComponent(x.k);
      return '<article class="card"><div class="spec">' + esc(x.s) + "</div><h3>" + esc(x.t) + "</h3><p>" + esc(x.d) +
        '</p><div class="terms">' + esc(x.k) + '</div><div class="go"><a href="' + pm + '" target="_blank" rel="noopener">PubMed reviews</a><a href="' + cc + '" target="_blank" rel="noopener">Cochrane</a></div></article>';
    }).join("");
  }
  $("chips").addEventListener("click", function (e) {
    var b = e.target.closest("button[data-s]"); if (!b) return;
    active = b.getAttribute("data-s"); renderChips(); renderGrid();
  });
  $("q").addEventListener("input", renderGrid);
  renderChips(); renderGrid();
})();
</script>
</body>
</html>

