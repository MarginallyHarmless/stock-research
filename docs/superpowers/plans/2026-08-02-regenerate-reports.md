# Regenerarea rapoartelor publicate

**Dată:** 2026-08-02 · **Pentru:** un agent care preia sarcina fără contextul discuției de față

## De ce

Skill-ul `stock-research` a primit două upgrade-uri succesive, iar rapoartele publicate în `reports/` au rămas în urmă — unele cu o generație, altele cu două. Un cititor care compară două rapoarte din folder vede scorecard-uri cu structuri diferite, ceea ce anulează exact proprietatea pentru care există fișierul `references/scorecard.md`: comparabilitatea.

**Upgrade-ul 1 (31 iulie 2026)** — context macro: secțiunea 1 a primit „vremea pieței", a apărut §11 „Expunerea la lume" ca al optulea pilon, iar §11/§12 vechi au devenit §12/§13. Scorecard: șapte piloni → opt.

**Upgrade-ul 2 (1–2 august 2026, branch `hype-context`)** — podul dintre preț și afacere:
- fiecare rând 🟡/🔴 din scorecard primește al doilea cuvânt, *de fază* sau *structural*;
- §8 capătă o a doua rută de evaluare, pe venituri, pentru companiile pe pierdere;
- §9 capătă mecanica cererii — rotația float-ului, combustibilul de squeeze, narativul datat;
- §12 „Riscuri" devine „Cele două teze", cu teza taurului lângă cea a ursului.

Niciun prag nu s-a schimbat la niciun upgrade. Culorile unei companii nu trebuie să se miște din cauza acestor modificări — dacă se mișcă la regenerare, ori datele s-au schimbat între timp (spune-o), ori e un bug (raportează-l, nu-l ascunde).

## Starea actuală, verificată

| Raport | Piloni | Are §11 Expunere | Rânduri 🟡/🔴 care vor primi etichetă | Generații în urmă |
|---|---|---|---|---|
| `NFLX-2026-07-31.html` | 7 | nu | de recalculat | **2** |
| `ONDS-2026-07-31.html` | 7 | nu | de recalculat | **2** |
| `AMZN-2026-08-01.html` | 8 | da | de recalculat | 1 |
| `IREN-2026-08-01.html` | 8 | da | 8 din 8 | 1 |
| `META-2026-08-01.html` | 8 | da | 4 din 8 | 1 |
| `ORCL-2026-08-01.html` | 8 | da | de recalculat | 1 |
| `PLTR-2026-07-31.html` | 8 | da | 4 din 8 | 1 |
| `SOFI-2026-08-01.html` | 8 | da | 7 din 8 | 1 |

## Starea, la 2 august 2026

**Rezolvate deja, nu le mai face:** repo-ul e sincronizat, `hype-context` e merge-uit pe `main` și publicat, iar `IREN` și `PLTR` sunt regenerate și live.

**Regula pentru numele fișierelor.** IREN și PLTR au fost suprascrise pe același nume, ca să nu se miște linkurile publicate — dar asta a mers doar pentru că datele noi cădeau exact pe data din nume (IREN la 1 august, PLTR la închiderea de 31 iulie). La cele șase rămase nu se va potrivi: regenerezi pe 2 august sau mai târziu un fișier care se numește `NFLX-2026-07-31.html`.

Deci: **fișier nou cu data reală a datelor, iar cel vechi șters în același commit.** Un nume care spune 31 iulie peste un raport plin de cifre de august e o minciună mică, dar exact genul care se acumulează — iar cele șase nu sunt linkurile pe care proprietarul a cerut să le păstrăm. Nu lăsa niciodată două versiuni ale aceleiași companii în folder: cititorul n-are cum să știe care e cea bună.

Dacă vrei să propui altceva, varianta corectă pe termen lung e ca numele să nu mai conțină deloc data (`reports/NFLX.html`), cu data doar în pagină. Linkurile se schimbă o dată și pe urmă niciodată. E o decizie a proprietarului, nu una de luat în trecere.

## Stadiul, la 3 august 2026

| Raport | Stare |
|---|---|
| ONDS | gata, commit `cc73fb5` |
| SOFI | gata, commit `115fe4e` |
| NFLX | gata, commit `4d9fc76` |
| META | gata, commit `b076fde` |
| AMZN | gata, commit `935b72b` |
| ORCL | **date strânse, raport nescris** — vezi `_ORCL-notes.md` din directorul de lucru |

Publicat pe GitHub Pages la 3 aug. 2026. `IREN` a primit separat o corecție de emoji (`e909c81`).
Rămâne **doar ORCL**.

**Avertisment pentru ORCL, din datele deja strânse.** Nu e un raport de rutină ca META sau AMZN:
acțiunea e cu 62% sub maximul de 52 de săptămâni, marja brută scade **patru ani la rând** (79,08% →
65,82%), datoria netă e la **4,16× EBITDA** — peste pragul de roșu de 3× — iar ROIC-ul e 8,60%, sub
pragul de 15% al dovezii de moat. Cel puțin două rânduri arată roșu pe praguri, iar dacă versiunea
publicată le are altfel, **e o mutare reală de culoare care trebuie explicată în raport, nu trecută
cu vederea**. Anul fiscal se încheie în mai, deci ultimele cifre sunt din raportarea de 10 iun. 2026
și au aproape două luni — de spus explicit.

**Regula de nume, decisă de proprietar pe 3 aug. 2026: linkurile publicate nu se mișcă.** Fiecare
raport regenerat se suprascrie pe numele lui deja publicat, oricare ar fi data din el. Secțiunea
„Regula pentru numele fișierelor" de mai jos spunea contrariul — fișier nou cu data reală, cel vechi
șters — și a fost aplicată la SOFI și META înainte de decizie; ambele au fost redenumite înapoi la
`-2026-08-01.html`. Restul secțiunii rămâne pentru context, dar **nu mai e regula în vigoare**.

Compromisul e asumat: numele `SOFI-2026-08-01.html` și `META-2026-08-01.html` conțin o dată care nu
e data datelor. Ce ține raportul onest e că **fiecare pagină își scrie propria dată în subsol**
(„date de piață la închiderea din 31 iulie 2026") — de verificat că rămâne așa la fiecare
regenerare, pentru că acum e singurul loc unde cititorul o mai găsește. Varianta curată pe termen
lung rămâne cea din secțiunea de mai jos: nume fără dată deloc (`reports/SOFI.html`), cu data doar
în pagină. Linkurile s-ar schimba o dată și pe urmă niciodată.

**Data datelor pentru tot lotul e închiderea de vineri 31 iul. 2026.** Pe 3 august piața americană
era încă deschisă când s-a lucrat, deci o închidere de 3 august nici nu exista. La AMZN și ORCL,
multiplii dependenți de preț se rescalează
la închiderea de 31 iulie (factorul e `închidere_31iul / preț_curent`); EPS, vânzările și cash
flow-ul pe acțiune nu se mișcă intraday, deci rescalarea e aritmetică exactă, nu estimare.
Pagina `stockanalysis.com/stocks/{t}/financials/ratios/` calculează oricum multiplii pe închiderea
precedentă — verifică întâi acolo înainte să rescalezi ceva de mână.

**Unelte lăsate în directorul de lucru**, ca să nu se rescrie de fiecare dată:
`_shell_head.html` / `_shell_tail.html` (carcasa raportului, fără corp), `_rp.py` (citește o pagină
R din EDGAR ca text), `_snap.py` (snapshot finviz + istoric de preț), `_fin.py` (tabelele
financiare de pe stockanalysis), `_verify.py` (rulează lista de verificare pe fișierul salvat și
validează JSON-ul fiecărui grafic).

## Ordinea de lucru

Ordonată după cât de mult contează, nu alfabetic. Nu regenera în paralel — fiecare raport cere ~12 fetch-uri plus un val doi, iar rulările paralele se calcă pe surse și consumă bugetul de sesiune.

**Un avertisment din experiență directă:** trei rulări succesive au murit de limită de sesiune *după* ce adunaseră toate datele, chiar în momentul scrierii HTML-ului. Scrie fișierul devreme și complet, apoi verifică-l — un raport nescris e pierdere totală, unul scris și neverificat se poate verifica oricând.

### Deja făcute — refolosește, nu regenera

Două rapoarte au fost regenerate în timpul verificării upgrade-ului 2 și sunt conforme cu regulile finale:

- `IREN` → `C:\Users\bogda\AppData\Local\Temp\claude\D--vibes-stonks-research-skill\cbdadecc-a91e-4a47-9935-6f0d06ddf0d8\scratchpad\IREN-verify2.html`
- `PLTR` → aceeași cale, `PLTR-verify.html`

Ambele au fost verificate rând cu rând: culorile identice cu versiunile anterioare, etichetele corecte, ruta B aplicată la IREN și absentă la PLTR. **Verifică singur înainte să te bazezi pe asta** — deschide fiecare fișier, confirmă că scorecard-ul are opt rânduri, că fiecare rând 🟡/🔴 începe cu *de fază* sau *structural*, și că §12 se numește „Cele două teze". Dacă da: redenumește cu data de azi, copiază în `reports/`, gata.

Atenție: `PLTR-verify.html` a fost generat **înainte** de strângerea regulii de fază. A rămas conform pentru că toate rândurile lui colorate sunt din categoria „mereu structural" plus buyback-ul, care e corect etichetat structural (diluția vine din plata în acțiuni, politică permanentă, nu fază finanțată). Reconfirmă acest raționament citind `references/scorecard.md` §9 înainte de a-l accepta.

### 1. ONDS (Ondas Holdings) — prioritate maximă

Două generații în urmă **și** e o companie pe pierdere, deci e singurul raport rămas care exercită ruta B pe venituri. Cel mai mare câștig din toată lista.

Ce trebuie să apară și nu are cum să existe azi: al optulea pilon (§11 Expunere), vremea pieței în §1, etichetele de fază, blocul „CE VENIT ÎNCORPOREAZĂ PREȚUL" în §8, rotația float-ului în §9, §12 simetric.

Verificare specifică: la o companie mică și speculativă, **ancora rutei B poate să nu existe** — dacă `/financials/ratios/` nu dă istoric de EV/Vânzări sau peers-ii n-au venituri, regula cere `N/A (nu apare pe sursă)`, nu o estimare. Un `N/A` corect e un rezultat bun, nu un eșec.

### 2. SOFI

Șapte rânduri din opt primesc etichetă — cea mai densă aplicare a regulii noi dintre companiile rămase. E și o bancă, deci intră sub excepțiile din `references/scorecard.md` §9: FCF-ul nu se aplică, se judecă pe ROE și capital.

Punctul delicat: SOFI e pe profit (P/E ~34 la ultima rulare), deci **ruta A**, nu B. Blocul de venituri nu trebuie să apară. Iar diluția ei — +16% acțiuni într-un an — cere o judecată reală: e finanțarea creșterii creditării, sau politică permanentă? Regula spune că rândul e „de fază" doar dacă investiția e cauza dominantă *a acelui rând*. Argumentează în text, nu doar eticheta.

### 3. NFLX

Două generații în urmă, dar companie mare și profitabilă — puține rânduri colorate, ruta A. E în mare parte muncă de aducere la structura curentă.

### 4–6. META, AMZN, ORCL

Una câte una, în ordinea asta. Toate profitabile, toate ruta A, toate au deja opt piloni — le lipsește doar stratul din upgrade-ul 2. META are 4 rânduri colorate, AMZN și ORCL trebuie recalculate.

## Cum rulezi fiecare raport

Citește și urmează, în ordinea asta: `SKILL.md`, `references/scorecard.md`, `references/glossary.md`, `assets/report-template.html`. `references/data-sources.md` doar dacă o pagină dă 404.

Skill-ul salvează în directorul de lucru curent, nu în directorul lui. Generează acolo, verifică, apoi copiază în `reports/` și comite. Un raport per commit, cu mesajul în forma existentă în istoric: `Add {TICKER} report ({data})` sau `Regenerate {TICKER} report ({data})`.

## Verificarea fiecărui raport, înainte de commit

Nu comite un raport care pică vreunul din punctele astea. Rulează greps pe fișierul salvat, nu din memorie.

- [ ] `grep -c "{{"` dă **0**. Un placeholder rămas înseamnă secțiune neterminată.
- [ ] Scorecard-ul are **opt rânduri** și titlul „Cei opt piloni".
- [ ] Fiecare rând 🟡/🔴 începe cu `<b>de fază</b>` sau `<b>structural</b>`. Rândurile verzi nu primesc al doilea cuvânt.
- [ ] Moat, evaluare, sentiment și expunere sunt **întotdeauna** „structural" — dacă vreunul e „de fază", ai aplicat greșit regula.
- [ ] Pentru fiecare „de fază": cele trei condiții sunt vizibile în text — cauza e capexul nu pierderea operațională, o pistă de cash **calculată** cu cifră și sursa ei depusă la SEC, și o țintă numită cu dată.
- [ ] §8: ruta B apare **numai** la companiile pe pierdere. La cele profitabile, blocul „CE VENIT ÎNCORPOREAZĂ PREȚUL" se șterge.
- [ ] §9 conține rotația float-ului cu formula (`Avg Volume × 250 ÷ Shs Float`) și narativul pieței **cu dată**.
- [ ] §12 se numește „Cele două teze" și are toate trei blocurile: ce trebuie să fie adevărat, ce ar strica, arbitrul. Fiecare linie din teza taurului are sursă și dată.
- [ ] `grep -n "va crește\|va scădea\|ar valora"` — zero apariții care să fie predicții. Interdicțiile proprii ale skill-ului și avertismentul despre țintele analiștilor sunt legitime; inspectează fiecare potrivire, nu presupune.
- [ ] Preț × acțiuni ≈ capitalizare (±10%) încă ține în fișier.
- [ ] Fără emoji în HTML.
- [ ] Culorile față de raportul anterior: dacă vreuna s-a mutat, spune de ce — date noi sau bug. Nu trece peste.

## Ce să nu faci

Nu edita rapoartele vechi manual ca să pară noi — se regenerează din surse, altfel cifrele rămân de acum o săptămână sub o structură nouă, ceea ce e mai rău decât un raport vechi cinstit.

Nu regenera din skill-ul de pe `main` dacă `hype-context` n-a fost merge-uit.

Nu șterge nimic din `reports/` fără să întrebi.
