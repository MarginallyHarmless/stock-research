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

## Înainte de orice — trei lucruri de rezolvat

**1. Sincronizează repo-ul.** `main` local e cu două commituri în urmă față de `origin/main` (rapoartele AMZN și ORCL există pe origin, nu local). Rulează `git pull` înainte de a atinge ceva, altfel vei crede că lipsesc.

**2. Merge-uiește `hype-context`.** Upgrade-ul 2 stă pe branch-ul `hype-context`, nemerge-uit. Rapoartele regenerate trebuie să iasă din skill-ul nou, deci branch-ul trebuie să ajungă pe `main` întâi. Dacă merge-ul nu e încă decis de proprietar, **oprește-te și întreabă** — nu regenera din skill-ul vechi.

**3. Întreabă ce se întâmplă cu fișierele vechi.** Regenerarea produce fișiere cu data de azi (`NFLX-2026-08-02.html`), nu suprascrie `NFLX-2026-07-31.html`. Folderul ar rămâne cu două versiuni ale aceleiași companii, iar cititorul n-are cum să știe care e cea bună.

Recomandarea mea: **șterge versiunea veche în același commit cu cea nouă.** Rapoartele nu sunt un arhivă istorică — sunt exemple de ce produce skill-ul, iar un exemplu depășit dezinformează. Dar e decizia proprietarului; întreab-o explicit înainte să ștergi ceva.

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
