# Market-Thesis Bridge Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make the report explain *why the market pays today's price* — phase labels on scorecard rows, implied-expectation arithmetic for loss-making companies, demand mechanics in §9, and a symmetric §12 — without moving a single threshold.

**Architecture:** Five text files edited in dependency order: scorecard (defines the rules) → glossary (defines the concepts) → SKILL.md (orchestrates) → HTML template (presents) → README (documents). No section renumbering, no new fetches, no engine changes. §12 changes title only.

**Tech Stack:** Markdown (Romanian prose), single-file HTML/CSS template, git.

**Spec:** `docs/superpowers/specs/2026-08-01-market-thesis-design.md`

## Global Constraints

- All prose in **Romanian**, matching each file's existing register; technical terms get the English in parens at first mention.
- **Nothing here moves a semaphore.** The eight thresholds in `references/scorecard.md` §§1–8 are untouched. The phase label sits *next to* a color, never instead of it.
- **No forecasting.** The implied-expectation result is never converted into a price, a target, or a range. The words „va crește"/„va scădea" about market, rates or price stay absent.
- Missing data → `N/A (nu apare pe sursă)`. Never estimate an anchor multiple from memory.
- No new fetches, no new libraries, no API keys, no engine/JS changes.
- Repo: `~/.claude/skills/stock-research`, branch `hype-context`. Every commit message ends with `Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>`.
- Line numbers below refer to the files' state at commit `f92206f`. Verify each anchor against the surrounding text (quoted in every step) before editing.
- **One deliberate deviation from the spec's "fără CSS nou":** Task 4 adds exactly one CSS line (`.sec ~ .sec{margin-top:26px}`) so §12 can carry two sub-headers using the existing `.sec` component. No new class, no new component. Flagged rather than smuggled.

---

### Task 1: scorecard.md — route B, demand mechanics, phase rule, two theses

**Files:**
- Modify: `references/scorecard.md`

**Interfaces produced (later tasks reuse these names verbatim):**
- Label words: **„de fază"** and **„structural"**.
- Route-B step names: **multiplul / ancora / venitul implicit / anii impliciți**.
- §9 demand items: **rotația float-ului / combustibilul de squeeze / narativul**.
- §12 block names: **Ce trebuie să fie adevărat / Ce ar strica / Arbitrul**.

- [ ] **Step 1: Update the TOC line 17.** Replace:

```markdown
10. [Secțiunile fără semafor](#10-secțiunile-fără-semafor) — management, structura de vot, riscuri
```

with:

```markdown
10. [Secțiunile fără semafor](#10-secțiunile-fără-semafor) — management, structura de vot, cele două teze
```

- [ ] **Step 2: Add route B to §6.** Anchor: the paragraph ending „…rezultatele au fost bune, dar mai slabe decât ce era deja în preț." (line 130), immediately followed by `### Comparația cu competitorii` (line 132). Insert between them:

```markdown
**Când compania e pe pierdere, ruta de mai sus nu funcționează** — un P/E negativ sau absurd nu se compară cu nimic. Atunci calculul se mută pe venituri, în patru pași mecanici:

1. **Multiplul** — EV/Vânzări, nu P/S. La o companie cu datorie netă mare, P/S ascunde exact partea care contează: cine cumpără compania preia și datoriile. Folosește P/S doar dacă EV/Vânzări lipsește de pe sursă, și spune că ai făcut-o.
2. **Ancora** — valoarea mai mică dintre media proprie pe 5 ani a multiplului (pagina `/financials/ratios/`) și mediana competitorilor din tabel. Amândouă sunt cifre citite, iar „mai mică" e o regulă tocmai ca să nu alegi tu.
3. **Venitul implicit** — la valoarea de întreprindere de azi, ce venit anual ar duce multiplul înapoi la ancoră.
4. **Anii impliciți** — la ritmul de creștere al ultimelor patru trimestre, în cât timp se ajunge acolo.

Rezultatul se scrie ca o constatare: „prețul de azi încorporează un venit de 3,4 mld $ — de șase ori cel de acum — adică vreo cinci ani de creștere la ritmul actual, fără nicio încetinire." Cifra asta e podul dintre un scorecard roșu și o acțiune care urcă: nu spune că prețul e greșit, spune ce trebuie să se întâmple ca să fie corect.

Două lucruri obligatorii lângă rezultat:

- **Dacă lipsește oricare dintre cele două ancore**, scrie `N/A (nu apare pe sursă)` și oprește-te. Un venit implicit calculat cu un multiplu luat din memorie e mai rău decât niciunul, pentru că arată riguros.
- **Fraza-pereche:** calculul presupune că multiplul revine la ancoră. Poate să nu revină, în nicio direcție — piața a plătit ani la rând multipli peste medie pentru companii care au livrat.

**Rezultatul nu se convertește niciodată într-un preț.** Nici țintă, nici interval, nici „ar valora". Aceeași regulă ca peste tot: descrii ce presupune prețul, nu unde ajunge.

```

- [ ] **Step 3: Add the demand-mechanics subsection to §7.** Anchor: the last bullet of §7, ending „…media nu înseamnă aproape nimic, și exact asta merită spus." (line 158), followed by `---` (line 160). Insert between them:

```markdown
### Mecanica cererii — de ce urcă o acțiune indiferent de bilanț

Pilonii de până aici explică afacerea. Nu explică de ce o companie cu opt semafoare galbene și roșii poate urca 20% într-o zi. Trei mărimi, toate din paginile deja citite, arată cine cumpără și de ce.

**Rotația float-ului** — `Avg Volume × 250 ÷ Shs Float`, ambele de pe finviz. Spune de câte ori pe an își schimbă proprietarul tot free float-ul. O companie mare, deținută de investitori pe termen lung, se rotește de câteva ori pe an; un titlu tranzacționat pe narativ, de zeci de ori. E cel mai ieftin indicator de speculație din raport, pentru că măsoară direct raportul dintre cei care tranzacționează și cei care dețin.

**Combustibilul de squeeze.** Short interest-ul mare și entuziasmul sunt aceeași monedă cu două fețe. Într-un titlu cu 30% din float vândut în lipsă, un anunț de contract nu mișcă prețul cu cât valorează contractul — îl mișcă cu cât costă ieșirea celor prinși pe partea greșită. Spune-o când cifrele o arată: aceeași știre, într-o companie fără shorturi, ar fi produs o fracțiune din mișcare. Nu contrazice avertismentul de mai sus — squeeze-ul rămâne imprevizibil și nu e o teză; e doar explicația mecanică a unei mișcări care altfel pare irațională.

**Narativul, numit și datat.** Ce poveste tranzacționează piața acum: buildout de centre de date pentru AI, fintech care devine bancă, software de apărare. Numește-o și pune-i data, din titlurile finviz și din căutarea din valul doi. Nu o aproba și nu o transforma în cifră — un narativ nu e o prognoză de venituri.

**Fraza care leagă tot pilonul:** hype-ul e o forță reală asupra prețului și zero forță asupra afacerii. Schimbă cât de bine poți vinde mâine, niciodată cât câștigă compania. De aceea sentimentul nu mută nicio culoare din celelalte șapte rânduri.

```

- [ ] **Step 4: Add the phase rule to the end of §9.** Anchor: the last table row of §9, ending „…Spune direct că e o situație specială, nu o companie „ieftină". |" (line 205), followed by `---` (line 207). Insert between them:

```markdown
### De fază sau structural — al doilea cuvânt de lângă culoare

Tabelul de mai sus rezolvă cazurile în care modelul de business schimbă *ce* măsori. Rămâne o a doua problemă, mai frecventă: o companie normală pentru stadiul ei arată identic în tabel cu una care pur și simplu pierde bani. Un operator care construiește centre de date și un biotech fără venituri primesc amândouă 🔴 pe free cash flow, dar lecția e opusă.

De aceea **fiecare rând 🔴 sau 🟡 primește un al doilea cuvânt**: *de fază* sau *structural*. Culoarea nu se schimbă — se schimbă doar explicația de lângă ea.

„De fază" se acordă numai dacă **toate trei** condițiile sunt îndeplinite:

1. **Cauza e investiția, nu operarea.** FCF-ul e negativ din capex, nu din pierdere operațională: cash flow-ul operațional e pozitiv sau zero, **sau** capex-ul depășește FCF-ul negativ. Alternativ, compania intră într-una din categoriile din tabelul de mai sus care prevede explicit situația.
2. **Finanțarea se vede în cifre.** Cash-ul și echivalentele plus facilitățile disponibile acoperă arderea pe cel puțin patru trimestre — *pista de cash* (cash runway), calculată din bilanț și cash flow, nu estimată din ochi.
3. **Există un prag numit, cu dată.** O țintă publicată de companie — ARR, backlog contractat, guidance — care arată dacă faza funcționează sau nu.

Pică oricare condiție → **structural**. Fără variante intermediare: rostul etichetei e ca doi oameni diferiți să pună același cuvânt.

**Eticheta nu compensează culoarea și nu se adună într-un scor.** Un raport cu opt „de fază" e tot un raport cu opt semafoare proaste. „De fază" înseamnă „normal pentru stadiul ăsta, uite cum verifici dacă merge", nu „nu-ți face griji".

```

- [ ] **Step 5: Replace the Riscuri subsection with the two theses.** Anchor: `### Riscuri` (line 225) through the final line of the file, „…Aici stă doar ce e specific acestei companii." (line 233). Replace that whole block with:

```markdown
### Cele două teze

Secțiunea 12 pune față în față ce trebuie să fie adevărat ca prețul de azi să aibă sens și ce ar strica povestea. Simetria nu e o politețe: un raport care listează doar riscuri lasă cititorul fără explicația pentru ce vede pe ecran, iar unul care listează doar promisiuni e o broșură.

**Teza taurului se citează, nu se compune.** Trei-patru lucruri care trebuie să devină adevărate, fiecare cu o cifră și o dată deja publicate: guidance, backlog contractat, o țintă de ARR, un contract anunțat. Nu opinii, nu extrapolări, nu „piața crede că". Dacă nimic nu poate fi sursat, scrie că teza pieței nu e articulată public — e o constatare validă și adesea cea mai utilă din raport.

**Teza ursului** rămâne riscurile specifice acestei afaceri, în ordinea asta:

1. **Concentrarea veniturilor** — cât din vânzări vine de la cei mai mari câțiva clienți sau de la un singur produs? O companie unde trei clienți fac 40% din venituri e o companie fragilă, indiferent cât de bune arată cifrele. Datele apar în segmentele de pe pagina `/company/` și, complet, în secțiunea *Risk Factors* din 10-K.
2. **Riscul de disrupție** — există o tehnologie sau un model de business care poate face produsul irelevant în 5 ani?
3. **Riscul de execuție** — compania depinde de o singură persoană, de o integrare dificilă după o achiziție, sau de un produs care încă nu s-a lansat?

Nu enumera riscuri generice („concurența", „reglementarea") — se aplică la orice companie și nu ajută pe nimeni.

**Arbitrul** închide secțiunea: două-trei puncte de verificare observabile în trimestrele următoare, cu data lor — apare ținta în raportarea din {data}, trece venitul trimestrial de vârful anterior, se convertește backlogul în venit. Un investitor care își notează dinainte ce l-ar face să se răzgândească ia decizii mult mai bune decât unul care improvizează când prețul cade.

Ciclul, geografia, politica și reglementarea nu se enumeră aici — se judecă la [Expunerea la lume](#8-expunerea-la-lume). Aici stă doar ce e specific acestei companii.
```

- [ ] **Step 6: Fix the stale cross-reference in §8.** Anchor line 188 contains „ce e specific companiei (concentrarea pe trei clienți, un produs nelansat) stă la Riscuri." Replace that sentence fragment so it reads:

```markdown
**Nu dubla riscurile.** Ce e sistematic (dobânzi, ciclu, geopolitică) stă aici; ce e specific companiei (concentrarea pe trei clienți, un produs nelansat) stă la cele două teze (secțiunea 12). Pentru companiile ciclice, citește întâi [excepțiile](#9-excepții-pe-tip-de-companie) — la ele ciclul e regula jocului, nu un steag surprinzător.
```

- [ ] **Step 7: Verify.** Run:

```bash
cd ~/.claude/skills/stock-research && grep -n "de fază\|structural\|Ancora\|Rotația float\|Cele două teze" references/scorecard.md
```

Expected: matches in §6 (Ancora), §7 (Rotația float), §9 (de fază / structural), §10 (Cele două teze). Then run:

```bash
cd ~/.claude/skills/stock-research && grep -n "### Riscuri" references/scorecard.md
```

Expected: no output (subsection renamed).

- [ ] **Step 8: Commit.**

```bash
cd ~/.claude/skills/stock-research && git add references/scorecard.md && git commit -m "scorecard: add route B, demand mechanics, phase rule, two theses

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>"
```

---

### Task 2: glossary.md — four concepts and one capcană

**Files:**
- Modify: `references/glossary.md`

**Interfaces consumed:** the label words and step names from Task 1.

- [ ] **Step 1: Add the four concepts.** Anchor: the `### Stock-based compensation (SBC)` entry ending „…e echivalentul a 5–20% din venituri." (line 114), followed by `---` (line 116). Insert between them:

```markdown
### Așteptare implicită (implied expectations)
Ce trebuie să se întâmple ca prețul de azi să fie corect. Un multiplu nu e o etichetă de „scump" — e o afirmație despre viitor pe care piața a făcut-o deja. Dacă acțiunea se tranzacționează la de șase ori venitul pe care l-ar justifica multiplul ei normal, prețul spune că veniturile se fac de șase ori. Poate se întâmplă. Dar acum știi pe ce pariezi, în loc să te întrebi dacă e „scumpă".

### Rotația float-ului (float turnover)
De câte ori pe an își schimbă proprietarul tot free float-ul: volumul mediu zilnic înmulțit cu ~250 de zile de tranzacționare, împărțit la float. O companie deținută de investitori pe termen lung se rotește de câteva ori pe an; una tranzacționată pe narativ, de zeci de ori. Cifra separă deținerea de speculație mai bine decât orice altceva gratuit.

### Pistă de cash (cash runway)
Câte trimestre mai poate funcționa o companie care arde bani, cu banii pe care îi are: cash și echivalente plus facilități disponibile, împărțite la arderea trimestrială. E întrebarea care decide dacă un free cash flow negativ e o etapă sau un sfârșit. Sub patru trimestre, diluția sau împrumutul devin aproape sigure — și amândouă te costă.

### Narativ de piață (market narrative)
Povestea care explică de ce cumpără lumea acum: „buildout de AI", „fintech care devine bancă", „renașterea apărării europene". Narativele mișcă prețuri real și rapid, pentru că sunt mai ușor de cumpărat decât un bilanț. Nu sunt însă o prognoză de venituri, iar cel mai scump moment al unei acțiuni e de obicei acela în care narativul e cel mai limpede.

```

- [ ] **Step 2: Add the capcană.** Anchor: the `### Confuzia dintre valul sectorului și calitatea companiei` entry ending „…diferența dintre înotători devine singurul lucru care contează." (line 149), followed by `---` (line 151). Insert between them:

```markdown
### Roșul de fază citit ca roșu structural — și invers
Un buildout finanțat și o afacere care pur și simplu pierde bani arată identic în tabel: free cash flow negativ, diluție, evaluare peste medie. Diferența nu e culoarea, e ce o cauzează — investiția sau operarea — plus câte trimestre de bani mai există și ce prag numit arată dacă planul merge.

Ambele confuzii costă. Citit ca structural, un roșu de fază te face să ratezi o companie normală pentru stadiul ei. Citit ca de fază, un roșu structural te face să ții una care nu are cum să ajungă la mal — „mai are nevoie doar de un an" e ce își spune cineva doi ani la rând. Testul e mecanic: pista de cash și pragul cu dată. Dacă lipsesc, e structural.

```

- [ ] **Step 3: Verify.** Run:

```bash
cd ~/.claude/skills/stock-research && grep -c "^### " references/glossary.md
```

Expected: `42` (37 before + 5 new entries).

- [ ] **Step 4: Commit.**

```bash
cd ~/.claude/skills/stock-research && git add references/glossary.md && git commit -m "glossary: implied expectations, float turnover, cash runway, market narrative

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>"
```

---

### Task 3: SKILL.md — orchestration for the four changes

**Files:**
- Modify: `SKILL.md`

**Interfaces consumed:** all names from Task 1.

- [ ] **Step 1: Add two verification bullets to Faza 3.** Anchor: the last bullet of Faza 3, „**Dacă două surse diferă cu peste 5%** la aceeași valoare, arată ambele și spune care e mai recentă." (line 82). Insert immediately after it:

```markdown
- **Pista de cash se calculează, nu se estimează.** O etichetă „de fază" fără cash runway calculat din bilanț și cash flow e o judecată deghizată în regulă. Dacă cifrele nu ajung pentru calcul, eticheta e „structural".
- **Ambele ancore ale așteptării implicite vin din pagini citite** — media proprie pe 5 ani din `/financials/ratios/` și mediana competitorilor din valul doi. Un multiplu „tipic pentru sector" luat din memorie e o cifră inventată cu extra pași.
```

- [ ] **Step 2: Rename §12 in the report structure block.** Anchor line 132. Replace:

```markdown
## 12. Riscuri — ce ar strica teza
```

with:

```markdown
## 12. Cele două teze — ce trebuie să fie adevărat și ce ar strica
```

- [ ] **Step 3: Fix the stale cross-reference in the §11 paragraph.** Anchor line 146 ends with „Ce e specific companiei rămâne la Riscuri (secțiunea 12); aici stă doar ce e sistematic." Replace that sentence with:

```markdown
Ce e specific companiei rămâne la cele două teze (secțiunea 12); aici stă doar ce e sistematic.
```

- [ ] **Step 4: Add the four new spec paragraphs.** Anchor: the end of the §11 paragraph edited in Step 3 (line 146), followed by the blank line and „Ce trebuie să conțină fiecare secțiune, ce praguri decid culoarea semaforului…" (line 148). Insert between them:

```markdown
**Fiecare semafor galben sau roșu primește un al doilea cuvânt: *de fază* sau *structural*.** Un buildout finanțat și o afacere care pierde bani arată identic în tabel, dar lecția e opusă. Regula are trei condiții, toate obligatorii: cauza e investiția și nu operarea, finanțarea se vede în cifre (pista de cash de cel puțin patru trimestre, calculată), și există o țintă publicată cu dată care arată dacă faza funcționează. Pică una → structural. Condițiile complete sunt în `references/scorecard.md`. Eticheta nu înmoaie culoarea și nu se adună într-un scor: opt „de fază" e tot un raport cu opt semafoare proaste.

**Secțiunea 8 trebuie să spună ce presupune prețul, nu doar cât e.** La o companie pe profit, diferența dintre P/E-ul curent și cel prospectiv arată creșterea deja inclusă în preț. La una pe pierdere — exact cazul în care întrebarea „de ce o iubește piața?" e cea mai ascuțită — ruta e pe venituri: EV/Vânzări de azi față de o ancoră (cea mai mică dintre media proprie pe 5 ani și mediana competitorilor), venitul care ar duce multiplul înapoi la ancoră, și câți ani de creștere la ritmul actual înseamnă asta. Ambele ancore vin din pagini citite; dacă una lipsește, rezultatul e `N/A (nu apare pe sursă)`, nu o estimare. Iar rezultatul nu se convertește niciodată într-un preț sau într-un interval de preț.

**Secțiunea 9 explică și mecanica cererii, nu doar nivelul ei.** Trei mărimi din fetch-urile existente arată cine cumpără și de ce: rotația float-ului (`Avg Volume × 250 ÷ Shs Float`, de pe finviz) ca măsură a speculației față de deținere; short interest-ul mare ca amplificator al oricărei știri bune, nu doar ca avertisment; și narativul pieței, numit și datat, luat din titluri și din căutarea din valul doi — niciodată aprobat, niciodată transformat în cifră. Fraza care leagă pilonul: hype-ul e o forță reală asupra prețului și zero forță asupra afacerii.

**Secțiunea 12 e simetrică.** Teza taurului — trei-patru lucruri care trebuie să devină adevărate, fiecare cu o cifră și o dată deja publicate de companie — stă lângă teza ursului, riscurile specifice. Teza taurului se citează, nu se compune: dacă nimic nu poate fi sursat, scrie că teza pieței nu e articulată public. La final, arbitrul: două-trei verificări observabile în trimestrele următoare, cu datele lor.

```

- [ ] **Step 5: Add three items to the final checklist.** Anchor: the checklist item „- [ ] **Cifrele competitorilor** există în paginile citite în valul doi, nu din memorie." (line 216). Insert immediately after it:

```markdown
- [ ] Fiecare **etichetă „de fază"** are toate trei condițiile îndeplinite și verificabile în text; altfel scrie „structural".
- [ ] **Așteptarea implicită nu apare convertită în preț** — nici țintă, nici interval, nici „ar valora".
- [ ] Fiecare afirmație din **teza taurului** are sursă și dată; niciuna nu e o extrapolare proprie.
```

- [ ] **Step 6: Verify.** Run:

```bash
cd ~/.claude/skills/stock-research && grep -n "Riscuri" SKILL.md
```

Expected: no output. Then run:

```bash
cd ~/.claude/skills/stock-research && grep -c "de fază" SKILL.md
```

Expected: `3` or more.

- [ ] **Step 7: Commit.**

```bash
cd ~/.claude/skills/stock-research && git add SKILL.md && git commit -m "SKILL: phase labels, implied-expectation route B, demand mechanics, symmetric s12

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>"
```

---

### Task 4: report-template.html — present the four changes

**Files:**
- Modify: `assets/report-template.html`

**Interfaces consumed:** all names from Task 1.

- [ ] **Step 1: Add the one CSS line for sub-headers.** Anchor: line 73, `.sec h2{font-size:17px}`. Insert immediately after it:

```css
.sec ~ .sec{margin-top:26px}
```

- [ ] **Step 2: Update the TOC entry for §12.** Anchor line 279. Replace:

```html
  <a href="#s12"><span class="tn">12</span>Riscuri</a>
```

with:

```html
  <a href="#s12"><span class="tn">12</span>Teze</a>
```

- [ ] **Step 3: Document the phase label in the scorecard comment.** Anchor: line 286, `     Nu pune semafor pe „Unde se află acțiunea" — nu e un indicator de calitate.` Insert immediately after it:

```html
     Fiecare stare warn/crit deschide .t cu al doilea cuvânt, în <b>: de fază sau
     structural. Regula (trei condiții) e în references/scorecard.md §9.
```

- [ ] **Step 4: Show the label in the two non-green example rows.** Anchor lines 299 and 304. Replace line 299:

```html
      <span class="t">{{…}}</span>
```

(the one inside the `Free cash flow` row, directly under the `data-s="warn"` span) with:

```html
      <span class="t"><b>de fază</b> — {{…}}</span>
```

Then replace line 304 (the one inside the `Cash vs. datorii` row, directly under the `data-s="crit"` span) with:

```html
      <span class="t"><b>structural</b> — {{…}}</span>
```

- [ ] **Step 5: Add the route-B block to §8.** Anchor: the closing `</div>` of the existing `CE PRESUPUNE PREȚUL ACTUAL` concept block (line 522), followed by `</section>` (line 523). Insert between them:

```html
  <!-- Ruta pe venituri: numai când compania e pe pierdere sau P/E-ul n-are sens.
       Ancora = cea mai mică dintre media proprie pe 5 ani și mediana peers.
       Șterge blocul la companiile pe profit. -->
  <div class="concept">
    <div class="h">CE VENIT ÎNCORPOREAZĂ PREȚUL</div>
    <p>{{La EV/Vânzări 19,7 față de o ancoră de 8,3 (media proprie FY2025), prețul de
    azi încorporează un venit anual de 1,4 mld $ — de 2,4 ori cel de acum. La ritmul
    ultimelor patru trimestre, sunt vreo patru ani de creștere fără nicio încetinire.}}</p>
    <p>{{Calculul presupune că multiplul revine la ancoră. Poate să nu revină, în nicio
    direcție — piața a plătit ani la rând multipli peste medie pentru companii care au
    livrat. Nu e un preț-țintă și nu se traduce într-unul.}}</p>
  </div>
```

- [ ] **Step 6: Add the float-turnover tile to §9.** Anchor line 531, the `Short interest` stat tile. Insert immediately after it:

```html
    <div class="stat"><div class="l">Rotația float-ului</div><div class="v">{{31×}}</div><div class="s">pe an · {{se tranzacționează, nu se deține}}</div></div>
```

- [ ] **Step 7: Add the demand-mechanics concept to §9.** Anchor: the closing `</div>` of the `SHORT INTEREST — CITEȘTE-L CA RISC` concept block (line 539), followed by `</section>` (line 540). Insert between them:

```html
  <div class="concept">
    <div class="h">DE UNDE VINE CEREREA</div>
    <p>{{Rotația float-ului de 31× pe an înseamnă că tot free float-ul își schimbă
    proprietarul de 31 de ori într-un an — se tranzacționează, nu se deține. Cu 30% din
    float vândut în lipsă, un anunț de contract nu mișcă prețul cu cât valorează
    contractul, ci cu cât costă ieșirea celor prinși pe partea greșită: +19,6% într-o
    zi, pe 20 iulie 2026. Narativul tranzacționat acum: buildout de centre de date
    pentru AI.}}</p>
    <p>Hype-ul e o forță reală asupra prețului și zero forță asupra afacerii. Schimbă
    cât de bine poți vinde mâine, niciodată cât câștigă compania.</p>
  </div>
```

- [ ] **Step 8: Replace §12 entirely.** Anchor: lines 588–602, from `<!-- ══ 12. RISCURI ═…` through the `</section>` that precedes `<!-- ══ 13. CE AI ÎNVĂȚAT ═…`. Replace that whole block with:

```html
<!-- ══ 12. CELE DOUĂ TEZE ══════════════════════════════════════════════════
     Teza taurului se citează, nu se compune: fiecare rând are cifră și dată
     publicate de companie. Dacă nimic nu e sursabil, scrie asta explicit.
─────────────────────────────────────────────────────────────────────────── -->
<section id="s12">
  <div class="sec"><span class="n">12</span><h2>Cele două teze</h2></div>
  <div class="sec"><span class="n">TAUR</span><h2>Ce trebuie să fie adevărat</h2></div>
  <ul class="list">
    <li><b>{{Ținta de ARR de peste 4 mld $ la finalul lui 2026}}</b> — {{anunțată de companie pe 20 iulie 2026}}</li>
    <li><b>{{Conversia backlogului de 2,8 mld $ în venit raportat}}</b> — {{contracte semnate, nu estimări}}</li>
  </ul>
  <div class="sec"><span class="n">URS</span><h2>Ce ar strica</h2></div>
  <!-- clasa "crit" pe riscurile majore -->
  <ul class="list">
    <li class="crit"><b>{{Concentrarea veniturilor}}</b> — {{specific, nu generic}}</li>
    <li><b>{{Un produs nelansat / o integrare după achiziție}}</b> — {{…}}</li>
  </ul>
  <div class="concept">
    <div class="h">ARBITRUL</div>
    <p>{{Două-trei verificări observabile, cu data lor: apare ținta în raportarea din
    27 august, trece venitul trimestrial de vârful de 240,3 mln $ din septembrie 2025.
    Un investitor care își notează dinainte ce l-ar face să se răzgândească ia decizii
    mult mai bune decât unul care improvizează când prețul cade.}}</p>
  </div>
</section>
```

- [ ] **Step 9: Verify.** Run:

```bash
cd ~/.claude/skills/stock-research && grep -c "class=\"sec\"" assets/report-template.html
```

Expected: `16` (14 before + 2 new sub-headers in §12). Then run:

```bash
cd ~/.claude/skills/stock-research && grep -n "RISCURI\|>Riscuri<" assets/report-template.html
```

Expected: no output.

- [ ] **Step 10: Open the template in a browser and confirm §12 renders with two spaced sub-headers, and that no other section's spacing changed.** The only structural risk in this task is the `.sec ~ .sec` rule; every other section has exactly one `.sec`, so nothing else can match.

- [ ] **Step 11: Commit.**

```bash
cd ~/.claude/skills/stock-research && git add assets/report-template.html && git commit -m "template: phase labels, route-B block, demand mechanics, symmetric s12

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>"
```

---

### Task 5: README.md — document what changed

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Update the §12 row of the "Ce produce" table.** Anchor line 27. Replace:

```markdown
| 12 | Riscuri | Ce ar invalida teza |
```

with:

```markdown
| 12 | Cele două teze | Ce trebuie să fie adevărat ca prețul să aibă sens, și ce ar strica |
```

- [ ] **Step 2: Update the §8 and §9 rows to mention the new content.** Anchor lines 23–24. Replace:

```markdown
| 8 | Evaluare | Companie bună — dar la ce preț, și ce presupune prețul ăsta |
| 9 | Sentiment de piață | Instituționali, short interest, indici, analiști |
```

with:

```markdown
| 8 | Evaluare | Companie bună — dar la ce preț, ce creștere e deja în preț, ce venit încorporează |
| 9 | Sentiment de piață | Instituționali, short interest, indici, analiști, de unde vine cererea |
```

- [ ] **Step 3: Add a bullet to "Ce face diferit".** Anchor: the `**Cifrele critice vin din rapoartele SEC.**` bullet ending „Sursa primară, nu un agregator." (line 59). Insert immediately after it, separated by a blank line:

```markdown
**Explică de ce e roșu fiecare roșu.** O companie care construiește și una care pierde bani arată identic în tabel: free cash flow negativ, diluție, evaluare peste medie. Fiecare semafor galben sau roșu primește al doilea cuvânt — *de fază* sau *structural* — după o regulă cu trei condiții verificabile, nu după impresie. Iar acolo unde piața plătește un preț pe care cifrele de azi nu-l justifică, raportul spune ce venit încorporează prețul și în câți ani de creștere se traduce. Fără preț-țintă și fără recomandare: pragurile rămân la fel de dure, doar explicația e completă.
```

- [ ] **Step 4: Verify.** Run:

```bash
cd ~/.claude/skills/stock-research && grep -n "Riscuri" README.md
```

Expected: no output.

- [ ] **Step 5: Commit.**

```bash
cd ~/.claude/skills/stock-research && git add README.md && git commit -m "README: document phase labels and implied expectations

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>"
```

---

### Task 6: Live verification on IREN and PLTR

This is the real test. Text edits can look right and still produce a bad report.

**Files:**
- Create: `IREN-<today>.html`, `PLTR-<today>.html` in a scratch directory (not the repo)

- [ ] **Step 1: Run the skill on IREN** — the route-B case (loss-making, buildout, 7 reds). Use a fresh session so the skill loads from disk.

- [ ] **Step 2: Check the IREN output against these five criteria.** Each is a pass/fail, not a judgment call:

  1. Still **seven red pillars**. If any color moved, the change leaked into the thresholds — revert and find where.
  2. Every 🟡/🔴 row opens with **de fază** or **structural**, and for each „de fază" the three conditions are visible in the report text (cause, cash runway with a number, named target with a date).
  3. §8 carries the **route-B block** with all four numbers: multiple, anchor, implied revenue, implied years. Anchor traceable to `/financials/ratios/` or the peer table.
  4. §9 carries the **float-turnover figure** and names the current narrative with a date.
  5. §12 has both theses; every bull-thesis line has a source and a date.

- [ ] **Step 3: Grep the IREN file for banned language.** Run:

```bash
grep -n "va crește\|va scădea\|preț-țintă\|ar valora\|recomand" IREN-*.html
```

Expected: no output, except any occurrence inside the fixed disclaimer text about analyst targets — inspect each hit rather than assuming.

- [ ] **Step 4: Run the skill on PLTR** — the route-A case (profitable, three reds).

- [ ] **Step 5: Check the PLTR output.** Route A (P/E → forward P/E) must be used, the route-B block must be **absent**, and the three red rows must carry coherent phase labels. PLTR's buyback red is *structural* (dilution is ongoing policy, not a financed phase) — if the report labels it „de fază", the rule was applied loosely and Task 1 Step 4 needs tightening.

- [ ] **Step 6: Record the outcome.** If both pass, note it in the commit. If either fails, fix the source file, re-run, and only then commit.

- [ ] **Step 7: Commit the verification note.**

```bash
cd ~/.claude/skills/stock-research && git commit --allow-empty -m "verify: IREN (route B) and PLTR (route A) reruns pass all criteria

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>"
```

---

## Self-review notes

**Spec coverage:** phase labels → Task 1 Step 4, Task 3 Step 4, Task 4 Steps 3–4. Route B → Task 1 Step 2, Task 3 Step 4, Task 4 Step 5. Demand mechanics → Task 1 Step 3, Task 3 Step 4, Task 4 Steps 6–7. Symmetric §12 → Task 1 Step 5, Task 3 Steps 2–4, Task 4 Step 8. Glossary → Task 2. README → Task 5. Acceptance criteria 1–7 → Task 6 plus the per-task greps.

**Known deviations, both deliberate:** one CSS line (`.sec ~ .sec`), Task 4 Step 1, documented in Global Constraints; and the reworded §8/§9 rows of the README table in Task 5 Step 2, which the spec did not ask for but which would otherwise describe the sections inaccurately after this change.

**Stale cross-references** to „Riscuri" exist in three files and are each handled: `scorecard.md` §8 (Task 1 Step 6), `SKILL.md` §11 paragraph (Task 3 Step 3), `README.md` table (Task 5 Step 1). Task 3 Step 6 and Task 5 Step 4 grep for stragglers.
