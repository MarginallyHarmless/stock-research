# Context/Exposure Upgrade Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add market/macro context to the stock-research skill — an unscored "weather" snapshot in §1 and a scored exposure pillar as new §11 — plus fix six review gaps (competitor fetches, ratios page, headlines source, two-company workflow, sentiment contradiction, save location).

**Architecture:** Six text files edited in dependency order: scorecard (defines the new pillar) → glossary (defines the concepts) → data-sources (defines the sources) → SKILL.md (orchestrates) → HTML template (presents) → README (documents). Old §11→§12, §12→§13; scorecard grows 7→8 pillars. No engine/JS changes.

**Tech Stack:** Markdown (Romanian prose), single-file HTML/CSS template, git.

**Spec:** `docs/superpowers/specs/2026-07-31-context-exposure-upgrade-design.md`

## Global Constraints

- All prose in **Romanian**, matching each file's existing register; technical terms get the English in parens at first mention.
- **No forecasting**: describing where rates/markets *are* is allowed; where they *will go* is banned like price targets. The words "va scădea/va crește" about markets/rates must appear nowhere.
- Every macro claim carries a **date**. News/searches are narrative, never a source of figures. Missing data → `N/A (nu apare pe sursă)`.
- **Weather never changes the exposure color** — only slow company properties do.
- No new libraries, no API keys, no template-engine changes, no recommendation language.
- Repo: `~/.claude/skills/stock-research`, branch `local/context-upgrade`. Every commit message ends with `Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>`.
- Line numbers below refer to the files' state at commit `c5a04aa`. Verify each anchor with the surrounding text (given in every step) before editing.

---

### Task 1: scorecard.md — new pillar §8, sentiment fix, renumbering

**Files:**
- Modify: `references/scorecard.md`

**Interfaces:**
- Produces: pillar name **"Expunere la lume"**; four sub-exposure names **Dobânzi / Ciclu economic / Geografie și politică / Reglementare**; the flag-based 🟢🟡🔴 rule. Tasks 4–6 reuse these names verbatim.

- [ ] **Step 1: Update the TOC** (lines 14–16). Replace:

```markdown
7. [Sentiment de piață](#7-sentiment-de-piață)
8. [Excepții pe tip de companie](#8-excepții-pe-tip-de-companie)
9. [Secțiunile fără semafor](#9-secțiunile-fără-semafor) — management, structura de vot, riscuri
```

with:

```markdown
7. [Sentiment de piață](#7-sentiment-de-piață)
8. [Expunerea la lume](#8-expunerea-la-lume)
9. [Excepții pe tip de companie](#9-excepții-pe-tip-de-companie)
10. [Secțiunile fără semafor](#10-secțiunile-fără-semafor) — management, structura de vot, riscuri
```

- [ ] **Step 2: Fix the §7 green criterion** (line 132). Replace:

```markdown
- 🟢 — deținere instituțională peste 60%, capital instituțional intrat semnificativ mai mare decât cel ieșit în ultimele 12 luni, short interest sub 3% din float, prezență în indici mari.
```

with:

```markdown
- 🟢 — capital instituțional intrat semnificativ mai mare decât cel ieșit în ultimele 12 luni, short interest sub 3% din float, prezență în indici mari. Nivelul deținerii instituționale e context, nu criteriu.
```

(The rationale two paragraphs down — ">60% e situația normală… nu spune nimic. Direcția banilor spune." — already exists at line 136; leave it.)

- [ ] **Step 3: Insert new §8 before the exceptions section.** Anchor: the line `## 8. Excepții pe tip de companie` (line 152). Insert immediately before it:

```markdown
## 8. Expunerea la lume

Pilonii 1–7 judecă corabia. Acesta judecă cât de tare o clatină marea: cât de sensibilă e compania la lucruri pe care nu le controlează — dobânzi, recesiuni, tarife, alegeri, reglementare.

Distincția care ține pilonul onest: **expunerea se notează, vremea nu.** Unde sunt dobânzile azi sau ce sector e la modă se schimbă săptămânal și n-ar trece testul „aceleași culori indiferent de zi". Cât de sensibilă e compania la aceste mișcări e o proprietate a afacerii, aproape la fel de stabilă ca moat-ul — asta primește semafor. Vremea de azi apare în text, datată (secțiunea 1 din raport și textul acestei secțiuni), dar nu mută niciodată culoarea.

Evaluează patru expuneri, pe rând, ca la moat:

| Expunere | Întrebarea care o testează | Steag dacă… |
|---|---|---|
| **Dobânzi** (sensibilitatea evaluării) | Cât din prețul de azi e profit din anii îndepărtați? | Forward P/E mult peste piață (orientativ ~35+) **sau** FCF negativ azi |
| **Ciclu economic** | Ce pățește cererea într-o recesiune — clienții amână, reduc, sau nu pot renunța? | Cerere discreționară care dispare în recesiuni |
| **Geografie și politică** | Cât din venituri depinde de o singură țară sau de un lanț de aprovizionare sensibil politic? | O țară/regiune peste ~40% din venituri, sau dependență de tarife / licențe de export |
| **Reglementare** | Există o acțiune concretă, cu nume, în curs — antitrust, o lege în dezbatere, o autorizație în pericol? | O amenințare activă, numită, cu impact plauzibil material |

**Semafor:**
- 🟢 — niciun steag: cerere care rezistă în recesiune, evaluare care nu depinde de profituri îndepărtate, venituri distribuite geografic, nicio amenințare regulatorie activă.
- 🟡 — un steag (sau două parțiale).
- 🔴 — două sau mai multe steaguri, sau o singură amenințare regulatorie concretă cu impact material.

**De ce contează steagul de dobânzi cel mai mult la companiile de creștere:** prețul unei acțiuni cu P/E 60 e, în cea mai mare parte, profitul anilor 5–15 de acum încolo, adus în prezent. Când dobânda fără risc urcă, viitorul îndepărtat valorează mai puțin în banii de azi — și multiplul se comprimă. Compania poate livra exact ce a promis și acțiunea tot să scadă serios. Mecanismul e în glosar la *comprimarea multiplilor*; secțiunea 11 din raport îl predă cu cifrele companiei.

**Expunere mare nu înseamnă companie proastă.** O companie poate avea 🔴 aici și verde la toate celelalte — o corabie excelentă pe o mare agitată. Lecția pentru cititor nu e „evit-o", ci „dimensionează poziția ca să suporți valurile": volatilitatea vine la pachet cu expunerea, iar vânzarea în panică pe fundul valului e felul în care expunerea devine pierdere reală.

**Nu dubla riscurile.** Ce e sistematic (dobânzi, ciclu, geopolitică) stă aici; ce e specific companiei (concentrarea pe trei clienți, un produs nelansat) stă la Riscuri. Pentru companiile ciclice, citește întâi [excepțiile](#9-excepții-pe-tip-de-companie) — la ele ciclul e regula jocului, nu un steag surprinzător.

---

```

- [ ] **Step 4: Renumber the two following headers.** `## 8. Excepții pe tip de companie` → `## 9. Excepții pe tip de companie`; `## 9. Secțiunile fără semafor` → `## 10. Secțiunile fără semafor`.

- [ ] **Step 5: Verify.**

Run: `grep -n '^## ' references/scorecard.md`
Expected: sections 1–10 in order, with `## 8. Expunerea la lume`, `## 9. Excepții`, `## 10. Secțiunile fără semafor`.
Run: `grep -n 'deținere instituțională peste 60%' references/scorecard.md`
Expected: no match.

- [ ] **Step 6: Commit.**

```bash
git add references/scorecard.md
git commit -m "scorecard: add Expunerea la lume pillar, fix sentiment green criterion

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 2: glossary.md — four concepts, one capcană, two analogies

**Files:**
- Modify: `references/glossary.md`

**Interfaces:**
- Consumes: concept name *comprimarea multiplilor* referenced by Task 1's scorecard text.
- Produces: glossary entries **Dobânda fără risc**, **Comprimarea multiplilor**, **Rotația de sector**, **Vânt din față, vânt din spate** — SKILL.md (Task 4) tells the writer to take formulations from here.

- [ ] **Step 1: Insert four concepts after the Beta entry.** Anchor: the line `Tradu întotdeauna beta în bani: „la 10.000 € investiți și beta 2,2, o corecție obișnuită de piață de 10% înseamnă −2.200 € în portofoliu."` (line 75). Insert after it (keeping a blank line before the existing `### Etalon (benchmark)`):

```markdown
### Dobânda fără risc (risk-free rate)
Randamentul pe care îl primești fără să riști practic nimic — de regulă dobânda obligațiunilor americane pe 10 ani. E gravitația tuturor evaluărilor: orice investiție riscantă trebuie să promită mai mult decât ea, altfel nimeni n-ar avea de ce să-și asume riscul. Când dobânda fără risc urcă, toate activele riscante valorează mai puțin în banii de azi — iar cel mai tare scad cele al căror profit e departe în viitor, adică exact acțiunile de creștere.

### Comprimarea multiplilor (multiple compression)
Fenomenul prin care o acțiune scade deși compania livrează: profitul crește, dar P/E-ul coboară de la 60 la 35, și prețul cade cu el. Multiplul e cât plătește piața pentru un leu de profit — și depinde de dobânzi și de dispoziția pieței, nu doar de companie. E a doua jumătate a răspunsului la nedumerirea clasică „rezultatele au fost bune, de ce a scăzut?": uneori așteptările erau și mai mari (vezi *beat/miss*), alteori s-a scumpit banul, nu s-a stricat afacerea.

### Rotația de sector (sector rotation)
Banii instituționali se mută periodic dintr-un sector în altul — dinspre creștere spre valoare când urcă dobânzile, dinspre ciclice spre defensive când vine frica de recesiune. De aceea o acțiune poate scădea 15% într-o lună în care compania n-a anunțat nimic: nu s-a schimbat compania, s-a schimbat direcția banilor. Verifică întâi cum s-a mișcat sectorul întreg înainte să cauți o explicație în companie.

### Vânt din față, vânt din spate (headwind / tailwind)
Metafora standard pentru contextul care ajută sau frânează o companie, independent de cât de bine e condusă. Un val de investiții în AI e vânt din spate pentru toți producătorii de cipuri; tarife noi pe importuri sunt vânt din față pentru cine produce în afară. Întrebarea de pus: cât din creșterea companiei e înotul ei și cât e valul — pentru că valurile se retrag, iar atunci se vede cine înoată.

```

- [ ] **Step 2: Append the capcană at the end of §2.** Anchor: the last capcană paragraph, ending `Când vezi creștere spectaculoasă, întreabă cât din ea e structurală și cât e un vârf de cerere care se va normaliza.` (line 125). Insert after it:

```markdown

### Confuzia dintre valul sectorului și calitatea companiei
Într-un sector la modă, aproape totul crește — și companiile bune, și cele mediocre. Randamentul acțiunii nu-ți spune dacă ai ales bine până nu-l compari cu sectorul ei: +40% când sectorul a făcut +60% înseamnă că ai ales unul dintre înotătorii slabi ai valului. Comparația cu S&P 500 nu prinde asta — compar-o și cu sectorul (un ETF de sector e etalonul comod). Iar când valul se retrage, diferența dintre înotători devine singurul lucru care contează.
```

- [ ] **Step 3: Append two analogies to §3.** Anchor: the last list item `- **Marja brută în scădere:** „e ca un magazin care trebuie să dea reduceri tot mai mari ca să vândă la fel de mult — încă vinde, dar puterea de negociere s-a dus."` (line 137). Insert after it:

```markdown
- **Dobânzile și evaluările:** „dobânda fără risc e gravitația prețurilor — când urcă, toate evaluările sunt trase în jos, și cad cel mai tare cele cocoțate cel mai sus."
- **Expunerea la context:** „o companie excelentă cu expunere mare e o corabie solidă pe o mare agitată — ajunge la destinație, dar nu ține pasagerii care sar peste bord la primul val."
```

- [ ] **Step 4: Verify.**

Run: `grep -c '^### ' references/glossary.md`
Expected: `34` (was 29: 22 concepts + 7 capcane; +4 concepts +1 capcană).
Run: `grep -n 'Comprimarea multiplilor\|Rotația de sector\|Dobânda fără risc\|vânt din spate' references/glossary.md`
Expected: all four found.
Run: `git diff --numstat references/glossary.md`
Expected: only insertions, 0 deletions.

- [ ] **Step 5: Commit.**

```bash
git add references/glossary.md
git commit -m "glossary: context concepts (rates, multiple compression, sector rotation, headwinds) + sector-wave trap

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 3: data-sources.md — sector/rates sources + competitor second wave

**Files:**
- Modify: `references/data-sources.md`

**Interfaces:**
- Produces: source entries `finviz.com/groups.ashx` and `fred.stlouisfed.org/series/DGS10`; the second-wave competitor pattern. SKILL.md (Task 4) points here.

- [ ] **Step 1: Insert two new subsections after the MarketBeat section.** Anchor: the line `**Surse verificate ca inaccesibile** — nu pierde timp cu ele: \`finance.yahoo.com/quote/{T}/holders/\` (503), \`wsj.com/market-data/quotes/\` (blocat), \`fintel.io\` (403), \`nasdaq.com/market-activity/\` (timeout).` (line 40). Insert after it:

```markdown

## Competitorii — valul doi

Tabelul comparativ din secțiunea de evaluare cere 2–3 competitori direcți, iar cifrele lor trebuie să vină tot din pagini citite, nu din memorie. După primul val afli industria (pagina `/company/`); alege competitori reali din aceeași nișă — nu doar companii mari din același sector — și adu-le `stockanalysis.com/stocks/{peer}/statistics/` într-un al doilea val paralel. Alegerea competitorilor e judecata ta; cifrele lor nu.

Dacă un competitor relevant e listat în afara SUA, folosește varianta `stockanalysis.com/quote/{bursa}/{TICKER}/statistics/` și spune în raport ce n-ai găsit.

## Context de piață și sector

| URL | Ce conține |
|---|---|
| `finviz.com/groups.ashx` | Performanța sectoarelor (zi, lună, YTD…) — sectorul companiei față de piață |
| `fred.stlouisfed.org/series/DGS10` | Dobânda obligațiunilor americane pe 10 ani — rezervă, dacă WebSearch nu dă o cifră datată |

Sectorul companiei îl iei din câmpul „Sector" de pe pagina ei finviz (fetch-ul 6), ca să alegi rândul corect din tabelul de grupuri. Dacă pagina de grupuri nu se lasă citită, ia performanța sectorului prin WebSearch și spune în raport de unde vine cifra.

Pentru dobânzi, sursa principală e WebSearch („10-year treasury yield"): vrei nivelul de azi și direcția pe ultimele ~12 luni, ambele cu dată. Regulile de la știri se aplică și aici: nivelul e un fapt, direcția trecută e un fapt, orice frază despre unde *vor merge* dobânzile nu intră în raport.
```

- [ ] **Step 2: Verify.**

Run: `grep -n 'groups.ashx\|DGS10\|valul doi\|Valul doi\|val doi' references/data-sources.md`
Expected: groups.ashx and DGS10 present; the "Competitorii — valul doi" heading present.

- [ ] **Step 3: Commit.**

```bash
git add references/data-sources.md
git commit -m "data-sources: sector/rates context sources + competitor second wave

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 4: SKILL.md — orchestration changes

**Files:**
- Modify: `SKILL.md`

**Interfaces:**
- Consumes: pillar/sub-exposure names from Task 1, glossary entry names from Task 2, source entries from Task 3 — all referenced by the exact names those tasks defined.
- Produces: report structure with §11 *Expunerea la lume* and §12/§13 renumbering; the "Valul doi" fetch step; the two-company variant. Tasks 5–6 mirror this structure.

- [ ] **Step 1: Extend the frontmatter description** (line 3). The description currently ends with `...of a named company.` — replace that ending with:

```text
...of a named company. Also use when the user asks how interest rates, tariffs, elections, or the overall market context affect a named listed company — "cum o afectează dobânzile pe X", "ce vânt are în față X", "contextul pieței pentru X".
```

- [ ] **Step 2: Add fetch rows 10–11.** Anchor: the table row (line 42):

```markdown
| 9 | `finviz.com/quote.ashx?t=SPY` | Performanța S&P 500 pe aceleași orizonturi — **etalonul** față de care compari acțiunea |
```

Append after it:

```markdown
| 10 | `stockanalysis.com/stocks/{T}/financials/ratios/` | Multipli istorici pe an (P/E, P/FCF, EV/EBITDA) — media proprie pe 5 ani, pentru secțiunea de evaluare |
| 11 | `finviz.com/groups.ashx` | Performanța sectoarelor — sectorul companiei vs. piață, pentru secțiunile 1 și 11 |
```

- [ ] **Step 3: Add the "Valul doi" subsection.** Anchor: the paragraph starting `Fetch-ul 9 pare o risipă, dar e cea mai importantă cifră din raport...` (line 46). Insert after that paragraph:

```markdown

### Valul doi — competitori și context

După ce primul val ți-a dat industria, trimite în paralel:

- **2–3 competitori direcți** — `stockanalysis.com/stocks/{peer}/statistics/` pentru fiecare. Alege competitori reali din aceeași nișă; alegerea e judecata ta, dar cifrele lor vin doar din paginile citite — tabelul comparativ din secțiunea 8 nu se umple din memorie.
- **WebSearch, dobânzi** — nivelul dobânzii americane pe 10 ani azi și direcția ei pe ultimele ~12 luni, cu dată.
- **WebSearch, politică și reglementare** — `{compania} antitrust / tarife / export controls / reglementare`, limitat la ultimele luni. În raport intră doar ce e concret și numit.

Pentru toate trei: știrile și căutările sunt narativ, nu sursă de cifre; fiecare afirmație macro primește data ei; iar despre piață și dobânzi descrii unde *sunt*, niciodată unde *vor merge* — prognoza e interzisă la fel ca prețul-țintă.
```

- [ ] **Step 4: Update the report skeleton** (lines 66–102). Three edits inside the ```markdown block:

(a) In the scorecard table, after the row `| Sentiment de piață | | |` add:

```markdown
| Expunere la lume | | |
```

(b) Replace the last three section headings:

```markdown
## 10. Management, insideri și structura de vot
## 11. Riscuri — ce ar strica teza
## 12. Ce ai învățat aici + întrebări pentru data viitoare
```

with:

```markdown
## 10. Management, insideri și structura de vot
## 11. Expunerea la lume — vânt din față sau din spate?
## 12. Riscuri — ce ar strica teza
## 13. Ce ai învățat aici + întrebări pentru data viitoare
```

- [ ] **Step 5: Fix §1 element 4 and add element 5.** Anchor: item 4 of the section-1 list (line 109). Replace within it:

```text
Folosește titlurile de pe pagina principală stockanalysis și, dacă e nevoie de context, WebSearch
```

with:

```text
Folosește titlurile din tabelul de știri de pe pagina finviz (fetch-ul 6) și, dacă e nevoie de context, WebSearch
```

Then append a new list item after item 4:

```markdown
5. **Vremea pieței** — trei cifre datate care separă valul de înotător: unde e S&P 500 față de maximul pe 52 de săptămâni (din fetch-ul 9), dobânda americană pe 10 ani și direcția ei pe ultimul an (din valul doi), și sectorul companiei YTD față de S&P 500 (din fetch-ul 11). Dacă acțiunea scade odată cu tot sectorul, spune-o direct — „asta nu e o știre despre companie, e vremea". Adaugă 1–2 propoziții datate despre curentele politice sau de reglementare care ating compania acum, dacă există. Lecția întreagă despre expunere e în secțiunea 11 — aici doar constați vremea, nu o prognozezi.
```

- [ ] **Step 6: Add the §11 guidance paragraph.** Anchor: the paragraph `Secțiunea asta **nu primește semafor**. Performanța trecută a prețului nu spune nimic despre calitatea afacerii, iar un verde acolo ar sugera exact confuzia pe care skill-ul încearcă s-o prevină.` (line 111). Insert after it:

```markdown

**Secțiunea 11 pune compania în contextul pe care nu-l controlează.** Patru expuneri, judecate pe rând ca la moat: sensibilitatea evaluării la dobânzi (câtă parte din preț e profit din anii îndepărtați — aici predai *comprimarea multiplilor*, cu forward P/E-ul companiei ca exemplu), ciclicitatea cererii, geografia veniturilor (tarife, licențe de export, concentrare pe o țară) și suprafața de reglementare (doar acțiuni concrete, numite). Steagurile și pragurile sunt în `references/scorecard.md`; formulările conceptelor, în glosar. Semaforul notează expunerea — o proprietate stabilă a afacerii — nu vremea de azi; expunere mare nu înseamnă companie proastă, ci volatilitate pe care cititorul trebuie să știe dinainte că o poate duce. Ce e specific companiei rămâne la Riscuri (secțiunea 12); aici stă doar ce e sistematic.
```

- [ ] **Step 7: Fix the pre-existing off-by-one** (line 137). Replace:

```text
Secțiunea 11 nu e un rezumat — e partea care rămâne.
```

with:

```text
Secțiunea 13 nu e un rezumat — e partea care rămâne.
```

- [ ] **Step 8: Add the two-company variant.** Anchor: the line `## Faza 5 — Generează raportul HTML` (line 139). Insert before it:

```markdown
## Varianta: două companii comparate

Când utilizatorul cere o comparație („X sau Y?", „compară X cu Y"), fă **un singur raport**, nu două:

- Rulează Faza 2 pentru ambele companii într-un singur val paralel (sursele comune — SPY, sectoare, dobânzi — o singură dată).
- Scorecard-ul primește două coloane de semafoare, una pe companie.
- Fiecare secțiune discută ambele companii, una lângă alta, pe aceleași cifre. Graficele `lines` pot purta ambele serii (motorul suportă trei); randamentul față de indice primește câte un bloc `compare` pe companie.
- Finalul nu e un verdict, ci **„diferențele care contează"**: 3–4 contraste concrete (cine se diluează, cui îi urcă marja, cine depinde de o țară sau de un client). „X e mai bună" nu apare — regula fără recomandări rămâne.

Fișierul: `{T1}-vs-{T2}-{AAAA-LL-ZZ}.html`.

```

- [ ] **Step 9: Specify the save location in Faza 5** (line 141). Replace:

```text
Salvează-l ca `{TICKER}-{AAAA-LL-ZZ}.html` și trimite-l utilizatorului cu SendUserFile
```

with:

```text
Salvează-l ca `{TICKER}-{AAAA-LL-ZZ}.html` în directorul de lucru curent — nu în directorul skill-ului — și trimite-l utilizatorului cu SendUserFile
```

- [ ] **Step 10: Verify.**

Run: `grep -n '^## \|^### ' SKILL.md`
Expected: Faza 2 now followed by "### Valul doi — competitori și context"; "## Varianta: două companii comparate" before Faza 5.
Run: `grep -n 'pagina principală stockanalysis' SKILL.md`
Expected: no match.
Run: `grep -n 'Secțiunea 1[13]' SKILL.md`
Expected: "Secțiunea 11 pune compania în contextul" and "Secțiunea 13 nu e un rezumat" — no other "Secțiunea 11/13" claims that contradict the new numbering.
Run: `grep -c '| Expunere la lume' SKILL.md`
Expected: 1.

- [ ] **Step 11: Commit.**

```bash
git add SKILL.md
git commit -m "SKILL: weather in section 1, exposure section 11, second-wave fetches, two-company variant, save location

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 5: report-template.html — §11 markup, weather tiles, renumbering, CSS cleanup

**Files:**
- Modify: `assets/report-template.html`

**Interfaces:**
- Consumes: section title "Expunerea la lume" and sub-exposure row labels (Dobânzi / Ciclu economic / Geografie și politică / Reglementare) from Task 1; section order from Task 4.
- Produces: nothing downstream; README (Task 6) is independent.

- [ ] **Step 1: Remove dead grouped-bars CSS** (lines 125–126). Delete:

```css
.grp{position:absolute;inset:0;display:flex;gap:2px}
.grp .gc{flex:1;position:relative}
```

- [ ] **Step 2: Scorecard header 7→8** (line 257). Replace `<h2>Cei șapte piloni</h2>` with `<h2>Cei opt piloni</h2>`.

- [ ] **Step 3: Extend the scorecard comment** (line 274). Replace:

```html
    <!-- restul: Buyback / diluție · Creștere și marje · Evaluare · Sentiment -->
```

with:

```html
    <!-- restul: Buyback / diluție · Creștere și marje · Evaluare · Sentiment · Expunere la lume -->
```

- [ ] **Step 4: Add the weather tiles to §1.** Anchor: the closing `</div>` of the beta/volatility stats block (line 317), immediately before the `<p>{{Ce a mutat prețul recent:...` paragraph. Insert between them:

```html
  <!-- vremea pieței — toate valorile cu dată; constată, nu prognoza -->
  <div class="stats">
    <div class="stat"><div class="l">S&P 500 față de max 52s</div><div class="v">{{−3,2%}}</div><div class="s">{{la 31 iul 2026}}</div></div>
    <div class="stat"><div class="l">Dobânda 10 ani (SUA)</div><div class="v">{{4,3%}}</div><div class="s">{{de la ~3,9% acum un an}}</div></div>
    <div class="stat"><div class="l">Sector YTD vs. S&P 500</div><div class="v">{{+11 pp}}</div><div class="s">{{tehnologie +20,2% · indice +9,2%}}</div></div>
  </div>
```

- [ ] **Step 5: Insert the new §11 section.** Anchor: the closing `</section>` of section 10 (Management, line 486), before the `<!-- ══ 11. RISCURI ...` banner. Insert between them:

```html

<!-- ══ 11. EXPUNEREA LA LUME ═══════════════════════════════════════════════
     Semaforul notează EXPUNEREA — proprietate stabilă a companiei — nu vremea.
     Vremea de azi (dobânzi, sector, piață) apare în secțiunea 1, datată.
     Steagurile și pragurile: references/scorecard.md §8.
─────────────────────────────────────────────────────────────────────────── -->
<section>
  <div class="sec"><span class="n">11</span><h2>Expunerea la lume</h2></div>
  <div class="concept">
    <div class="h">COMPRIMAREA MULTIPLILOR</div>
    <p>{{La P/E 60, cea mai mare parte a prețului e profitul anilor 5–15 de acum
    încolo, adus în prezent. Când dobânda fără risc urcă, viitorul îndepărtat
    valorează mai puțin în banii de azi — multiplul se comprimă. Compania poate
    livra exact ce a promis și acțiunea tot să scadă. Nu s-a stricat afacerea;
    s-a scumpit banul.}}</p>
  </div>
  <div class="score">
    <div class="row"><span class="k">Dobânzi</span>
      <span class="st" data-s="crit"><svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M4 4l8 8M12 4l-8 8"/></svg>Risc</span>
      <span class="t">{{forward P/E 48 — evaluarea depinde de anii îndepărtați}}</span></div>
    <div class="row"><span class="k">Ciclu economic</span>
      <span class="st" data-s="good"><svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M3 8.5l3.2 3.2L13 5"/></svg>Solid</span>
      <span class="t">{{cerere greu de amânat și în recesiune}}</span></div>
    <div class="row"><span class="k">Geografie și politică</span>
      <span class="st" data-s="warn"><svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M8 2.6l6 10.8H2z"/><path d="M8 6.6v3.2M8 11.6v.1"/></svg>Atenție</span>
      <span class="t">{{~25% din venituri dintr-o singură țară, sub licențe de export}}</span></div>
    <div class="row"><span class="k">Reglementare</span>
      <span class="st" data-s="none"><svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2.2"><path d="M3.5 8h9"/></svg>Absent</span>
      <span class="t">{{nicio acțiune concretă în curs}}</span></div>
  </div>
  <p>{{Sinteza: expunere mare nu înseamnă companie proastă — înseamnă volatilitate
  pe care trebuie să o poți duce dinainte, nu să o descoperi la prima furtună.
  Corabie excelentă, mare agitată.}}</p>
</section>
```

- [ ] **Step 6: Renumber old sections 11→12, 12→13.** Four replacements:
  - `<!-- ══ 11. RISCURI ═════════════════════════════════════════════════════════ -->` → `<!-- ══ 12. RISCURI ═════════════════════════════════════════════════════════ -->`
  - `<div class="sec"><span class="n">11</span><h2>Riscuri</h2></div>` → `<div class="sec"><span class="n">12</span><h2>Riscuri</h2></div>`
  - `<!-- ══ 12. CE AI ÎNVĂȚAT ═══════════════════════════════════════════════════ -->` → `<!-- ══ 13. CE AI ÎNVĂȚAT ═══════════════════════════════════════════════════ -->`
  - `<div class="sec"><span class="n">12</span><h2>Întrebări pentru următoarea companie</h2></div>` → `<div class="sec"><span class="n">13</span><h2>Întrebări pentru următoarea companie</h2></div>`

- [ ] **Step 7: Verify.**

Run: `grep -o 'class="n">[A-Z0-9]*<' assets/report-template.html`
Expected sequence: SCORECARD, 01…10, 11 (Expunerea), 12, 13 — each number exactly once.
Run: `grep -n 'grp\|șapte piloni' assets/report-template.html`
Expected: no matches.
Run: `grep -c 'data-viz' assets/report-template.html`
Expected: `5`, same as before the task (4 figure attributes + 1 JS selector) — engine untouched.

- [ ] **Step 8: Commit.**

```bash
git add assets/report-template.html
git commit -m "template: section 11 exposure markup, weather tiles, renumbering, drop dead .grp CSS

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 6: README.md — document the new pillar

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: section names/numbers from Task 4.

- [ ] **Step 1: Update the pillar count** (line 15). Replace:

```markdown
| — | Scorecard | Cum stă pe cei șapte piloni, cu semafoare |
```

with:

```markdown
| — | Scorecard | Cum stă pe cei opt piloni, cu semafoare |
```

- [ ] **Step 2: Update the sections table** (lines 26–27). Replace:

```markdown
| 11 | Riscuri | Ce ar invalida teza |
| 12 | Ce ai învățat | Concepte noi și întrebări pentru următoarea companie |
```

with:

```markdown
| 11 | Expunerea la lume | Cât o clatină dobânzile, ciclul, geopolitica — expunerea se notează, vremea nu |
| 12 | Riscuri | Ce ar invalida teza |
| 13 | Ce ai învățat | Concepte noi și întrebări pentru următoarea companie |
```

- [ ] **Step 3: Add a "Ce face diferit" bullet.** Anchor: the paragraph starting `**Traduce volatilitatea în bani.**` (line 58). Insert after that paragraph:

```markdown

**Pune compania în context.** Dobânzile, ciclul economic, tarifele și reglementarea intră în raport cu o regulă strictă: expunerea companiei la ele se notează — e o proprietate stabilă a afacerii — iar vremea de azi doar se descrie, datată. Prognozele de piață sunt interzise la fel ca prețurile-țintă.
```

- [ ] **Step 4: Verify.**

Run: `grep -n 'șapte\|opt piloni\|Expunerea la lume' README.md`
Expected: "opt piloni" present, "șapte" absent, table row for Expunerea la lume present.

- [ ] **Step 5: Commit.**

```bash
git add README.md
git commit -m "README: document context pillar and renumbered sections

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 7: Cross-file consistency verification

**Files:** none modified (verification only; fix-and-amend if a check fails).

- [ ] **Step 1: Numbering and naming sweep.**

Run, from the repo root:

```bash
grep -rn 'șapte piloni' SKILL.md README.md assets/report-template.html; \
grep -c 'Expunere la lume\|Expunerea la lume' SKILL.md references/scorecard.md assets/report-template.html README.md; \
grep -n '^## 1[0-3]\.' SKILL.md references/scorecard.md
```

Expected: zero "șapte piloni"; every file mentions the new pillar at least once; SKILL.md skeleton runs to `## 13.`, scorecard.md to `## 10.`.

- [ ] **Step 2: Anti-forecast sweep.**

Run: `grep -rn 'va scădea\|va crește\|vor scădea\|vor crește' SKILL.md references/ | grep -v 'nu\|interzis\|niciodată'`
Expected: no matches outside sentences that ban the practice. Judge each hit in context; the new text must never *assert* a future market/rate move.

- [ ] **Step 3: Anchor sanity for scorecard TOC.**

Run: `grep -n '](#' references/scorecard.md`
Expected: TOC anchors match the renumbered headings (`#8-expunerea-la-lume`, `#9-excepții-pe-tip-de-companie`, `#10-secțiunile-fără-semafor`), and the §8 body link points to `#9-excepții-pe-tip-de-companie`.

- [ ] **Step 4: Confirm clean tree and log.**

Run: `git status --short && git log --oneline main..HEAD`
Expected: clean tree; six new task commits on `local/context-upgrade` (after the spec and plan commits).

---

### Task 8: Live end-to-end smoke test (acceptance criterion 4)

**Files:** produces `PLTR-<date>.html` in the **user's cwd** (`/Users/bogdandraghici/Desktop/vibes/stock skill`), not in the repo.

This task runs in the main session (needs WebFetch/WebSearch and the Skill tool). Invoke the updated skill end-to-end on PLTR and check:

- [ ] **Step 1:** Invoke `stock-research` for PLTR ("analizează-mi PLTR"), following the updated SKILL.md as written — including fetch rows 10–11, the second wave (real competitor pages fetched), and the WebSearches.
- [ ] **Step 2:** Verify the chat report has: §1 with dated weather (S&P 500 position, 10Y level + 12-month direction, sector YTD vs index), a scored §11 with four judged sub-exposures, §12 Riscuri without duplicating systematic exposures, competitor table where every number traces to a fetched page.
- [ ] **Step 3:** Verify the HTML file: saved in cwd; weather tiles present with dates; §11 rendered with icon+label states; section numbers run 01–13; opens with charts working (no console errors that matter — the engine was untouched).
- [ ] **Step 4:** Verify the negative constraints: no "va scădea/va crește" about markets/rates, no buy/sell language, `N/A (nu apare pe sursă)` for anything unfetchable.
- [ ] **Step 5:** If any check fails, fix the responsible file, amend the relevant commit or add a fix commit, and re-verify that check.
