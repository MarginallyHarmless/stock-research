# stock-research upgrade — podul dintre preț și afacere („de ce o iubește piața")

**Dată:** 2026-08-01 · **Status:** design aprobat, plan de implementare urmează
**Branch:** `hype-context` (divergență locală față de `main`; PR-abil ulterior)

## Problema

Rapoartele pe companii speculative ies aproape integral roșii — IREN 6 roșii și 2 galbene, SOFI 7 din 8 pe atenție sau risc — în timp ce piața le tranzacționează cu entuziasm. Rapoartele nu sunt greșite: cei opt piloni măsoară **afacerea de azi** (FCF-ul de anul trecut, datoria de pe bilanț, diluția din ultimii trei ani, multiplii pe profitul curent). Pentru o companie evaluată pe opționalitate, pilonii ăștia sunt roșii *prin construcție* — FCF negativ e cum arată un buildout de capital, diluția e cum se finanțează o companie pre-profit, multiplul pe profitul de azi nu descrie o afacere care se scalează.

Rezultatul pentru cititorul-țintă: vede șase roșii și două galbene, se uită apoi la +150% de la minim și trage una din două concluzii, ambele greșite — „raportul e prost" sau „piața e proastă". Adevărul e a treia: **raportul descrie o afacere, prețul descrie o așteptare, iar între ele nu există niciun pod.**

## Scopul

Construim podul. **Niciun prag din cele opt nu se schimbă.** Adăugăm patru lucruri care lipsesc între „afacerea arată așa" și „prețul spune altceva".

## Invariante

**Neschimbate:** română, conceptul înainte de cifră, nicio cifră inventată (`N/A (nu apare pe sursă)`), fără limbaj de cumpărare/vânzare/preț-țintă, surse gratuite, fără chei de API, un singur fișier HTML autonom, opt piloni, pragurile din `references/scorecard.md`.

**Noi, introduse de acest upgrade:**

1. **Hype-ul se descrie, nu se notează niciodată.** Zero semafoare noi; nicio culoare existentă nu se mișcă din cauza sentimentului.
2. **Așteptarea implicită e aritmetică pe cifre deja extrase**, nu prognoză. Ambele ancore ale calculului vin din pagini citite; niciuna nu e aleasă de model.
3. **Cazul taurului se citează, nu se scrie.** Fiecare afirmație urcă la o declarație publică a companiei sau la o cifră dintr-o pagină citită, cu dată. Dacă nu e sursabil, raportul spune că teza pieței nu e articulată public — și asta e în sine o constatare.

## Decizii înregistrate

- Scopul acoperă structura, calibrarea și tonul, într-un singur pas coerent.
- Calibrarea se face prin **etichetă de fază lângă culoare**, nu prin praguri paralele pe stadiu și nu printr-un al nouălea pilon. Motivul: praguri pe stadiu ar cere o încadrare subiectivă care mută toate cele opt culori și rupe regula „aceleași culori indiferent de zi" din `scorecard.md`.
- Așteptarea implicită se calculează prin **aritmetica ancorei**, nu prin DCF invers. Motivul: DCF-ul invers cere rată de actualizare și valoare terminală, ipoteze care nu se pot lua din surse — e prognoză pe ușa din dos.
- Conținutul nou intră **în secțiunile existente**; fără secțiune nouă și fără renumerotare. §12 se redenumește ca să devină simetric.

## Structura raportului după schimbare

Cele 13 secțiuni și cele 8 rânduri de scorecard rămân identice ca număr și ordine. Singura schimbare de titlu: **§12 Riscuri → §12 Cele două teze — ce trebuie să fie adevărat și ce ar strica.**

## Schimbare 1 — eticheta de fază pe rândurile de scorecard

Culoarea nu se mișcă. Fiecare rând 🔴 sau 🟡 primește un calificativ de un cuvânt: **structural** sau **de fază**.

**Regula de decizie, mecanică.** „De fază" se acordă doar dacă **toate trei** condițiile sunt adevărate:

1. **Cauza e investiția, nu operarea** — FCF-ul e negativ din capex, nu din pierdere operațională (cash flow operațional ≥ 0, **sau** capex > |FCF|); sau compania intră într-o categorie din `scorecard.md` §9 Excepții care prevede explicit situația.
2. **Finanțarea se vede în cifre** — cash și echivalente plus facilități disponibile acoperă arderea pe cel puțin patru trimestre (*cash runway*, calculat din bilanț și cash flow, nu estimat).
3. **Există un prag numit, cu dată** — o țintă publicată de companie (ARR, backlog contractat, guidance) care arată dacă faza funcționează.

Pică oricare condiție → **structural**. Fără nuanțe intermediare; scopul e ca doi oameni diferiți să pună aceeași etichetă.

**Două precizări adăugate după testul pe IREN**, fără de care regula nu era mecanică:

- **Ce cifră de cash intră în pistă** — numărătorul e cash-ul din ultimul bilanț depus (10-Q/10-K), nu dintr-un comunicat preliminar al companiei. Dacă cele două dau etichete diferite, câștigă bilanțul. Numitorul e arderea medie pe patru trimestre; când arderea accelerează prin plan, se spune că pista reală e mai scurtă, dar cifra nu se ajustează. Fără regula asta, aceleași fetch-uri dădeau șase „de fază" sau zero, după cum alegea cititorul.
- **Care rânduri pot primi eticheta** — doar cele patru pe care le cauzează mecanic un buildout: FCF, cash vs. datorii, buyback/diluție, creștere și marje. Moat, evaluare, sentiment și expunere sunt întotdeauna „structural". Evaluarea în special: prețul de azi e un fapt despre azi, iar „evaluare de fază" s-ar citi ca o scuză pentru preț.

**Unde apare.** Prefix în coloana „în două cuvinte" a scorecard-ului, cu `<b>`. Zero CSS nou, zero componentă nouă.

**Exemplu (IREN).** FCF rămâne 🔴, dar textul devine „**de fază** — negativ din capex de buildout, finanțat, testul e ARR-ul de 4 mld $ la raportarea din 27 aug". Dacă runway-ul nu ține patru trimestre, eticheta e **structural** și raportul spune de ce distincția contează.

**Ce nu face.** Eticheta nu compensează culoarea și nu se însumează într-un scor. Un raport cu opt „de fază" e tot un raport cu opt roșii.

## Schimbare 2 — aritmetica așteptării implicite (§8 Evaluare)

`scorecard.md` §6 are deja subsecțiunea *Ce presupune prețul actual*, dar funcționează exclusiv pe diferența P/E curent → forward — inutilizabilă exact la companiile pe pierdere. Se adaugă a doua rută, fără a o modifica pe prima.

**Ruta A — compania e pe profit.** Neschimbată: diferența dintre P/E curent și forward arată ce creștere de profit e deja în preț.

**Ruta B — compania e pe pierdere sau P/E-ul nu are sens.** Pe venituri:

1. **Multiplul folosit** = EV/Vânzări, pentru că include datoria — la o companie cu 1,75 mld $ datorie netă, P/S ascunde exact partea care contează. P/S doar dacă EV/Vânzări lipsește de pe sursă, și atunci se spune explicit.
2. **Ancora** = valoarea mai mică dintre media proprie pe 5 ani a multiplului (fetch 10, `/financials/ratios/`) și mediana peers (valul doi, `/statistics/`). Ambele sunt cifre extrase; alegerea e o regulă, nu o judecată. Sub cinci ani de istoric public, se folosesc anii care există și se spune câți. Când peers-ii sunt și ei aproape fără venituri, mediana lor e aritmetic uriașă și fără conținut — se spune asta, iar ancora rămâne pe istoricul propriu (testul pe IREN a dat o mediană peers de 40,8 față de 5,95 propriu).
3. **Venitul implicit** = la EV-ul de azi, ce venit anual duce multiplul la ancoră.
4. **Anii impliciți** = la ritmul de creștere al ultimelor patru trimestre (fetch 12), în câți ani se atinge venitul implicit.

**Formularea în raport**, ca o constatare: *„prețul de azi încorporează un venit de X mld $ — de N ori cel de acum — adică vreo M ani de creștere la ritmul actual, fără nicio încetinire."*

**Două garduri obligatorii:**
- Dacă lipsește oricare dintre cele două ancore → `N/A (nu apare pe sursă)`. Nu se estimează, nu se derivă, nu se completează din memorie.
- Fraza-pereche apare întotdeauna: calculul presupune că multiplul revine la ancoră, ceea ce poate să nu se întâmple, în nicio direcție.

**Interdicție explicită.** Rezultatul nu se convertește niciodată într-un preț, un preț-țintă sau un interval de preț.

## Schimbare 3 — mecanica cererii (§9 Sentiment)

De ce urcă o acțiune indiferent de bilanț. Toate trei elementele ies din fetch-urile existente; niciun fetch nou.

- **Rotația float-ului** — `Avg Volume × 250 ÷ Shs Float` (ambele de pe finviz, fetch 6): de câte ori pe an își schimbă proprietarul tot free float-ul. Măsoară direct raportul dintre speculație și deținere. Se dă cu o ancoră de interpretare: o companie mare, deținută de investitori pe termen lung, se rotește de câteva ori pe an; un titlu tranzacționat pe narativ, de zeci de ori.
- **Combustibilul de squeeze** — short % din float plus days to cover (fetch 5 și 6), formulate ca aceeași monedă cu hype-ul: într-un titlu cu 30% din float pe short, un anunț de contract produce o mișcare mult mai mare decât o justifică știrea. Cifrele există deja în raport; lipsește legătura cauzală. Nu contrazice avertismentul existent din `scorecard.md` §7 — squeeze-ul rămâne un eveniment rar și imprevizibil, nu o strategie.
- **Narativul, numit și datat** — ce poveste tranzacționează piața (buildout de AI, fintech care devine bancă, software de apărare), din titlurile finviz (fetch 6) și WebSearch-ul din valul doi. Se numește și se datează; nu se aprobă și nu se convertește în cifră.

**Fraza-lecție, obligatorie în secțiune:** hype-ul e o forță reală asupra prețului și zero forță asupra afacerii — schimbă când poți vinde bine, niciodată cât câștigă compania.

**Semaforul pilonului 7 nu se schimbă.** Mecanica se descrie în proză; pragurile de sentiment rămân cele din `scorecard.md` §7.

## Schimbare 4 — §12 devine simetric

Titlu nou: **„Cele două teze — ce trebuie să fie adevărat și ce ar strica"**. Trei blocuri, pe aceleași cifre:

1. **Teza taurului** — 3–4 lucruri care trebuie să devină adevărate, fiecare cu o cifră și o dată deja publicate (guidance, backlog contractat, țintă ARR, contracte anunțate). Nu opinii, nu extrapolări.
2. **Teza ursului** — riscurile actuale, neatinse ca conținut: concentrarea veniturilor, disrupția, execuția. Ciclul, geografia și reglementarea rămân la §11, ca acum.
3. **Arbitrul** — 2–3 puncte de verificare observabile în următoarele trimestre, cu data lor. Exemplu (IREN): apare ARR-ul de 4 mld $ în raportarea din 27 august, și trece venitul trimestrial de vârful de 240,3 mln $ din septembrie 2025?

**Regula care ține blocul onest:** teza taurului se raportează, nu se compune. Dacă nicio afirmație nu poate fi sursată, secțiunea scrie că teza pieței nu e articulată public — o constatare validă, nu un eșec.

Asta rezolvă și reclamația de ton, fără să atingă un prag: roșiile stau acum lângă o teză a taurului corect prezentată și lângă lista lucrurilor care ar demonstra-o.

## Fișier cu fișier

### 1. SKILL.md
- **Faza 2** — neschimbată. Zero fetch-uri noi.
- **Faza 3 (verificări)** — două verificări noi: cash runway calculat, nu estimat, când se acordă o etichetă „de fază"; ambele ancore ale așteptării implicite provin din pagini citite.
- **Faza 4** — spec pentru eticheta de fază în scorecard; §8 primește ruta B; §9 primește cele trei elemente de mecanică plus fraza-lecție; §12 primește noul titlu și cele trei blocuri.
- **Verificarea finală** — trei bife noi: nicio etichetă „de fază" fără cele trei condiții îndeplinite; așteptarea implicită nu s-a convertit în preț; fiecare afirmație din teza taurului are sursă și dată.

### 2. references/scorecard.md
- **§6 Evaluare** — subsecțiunea *Ce presupune prețul actual* se extinde cu ruta B (ancoră, venit implicit, ani impliciți) și cele două garduri.
- **§7 Sentiment** — subsecțiune nouă *Mecanica cererii* cu rotația float-ului, combustibilul de squeeze și narativul. Pragurile semaforului rămân neatinse.
- **§9 Excepții** — regula de decizie „de fază vs. structural", cu cele trei condiții, plus nota că eticheta nu compensează culoarea.
- **§10 Secțiunile fără semafor** — subsecțiunea *Riscuri* devine *Cele două teze*, cu regula de citare.

### 3. references/glossary.md
Patru concepte noi, în registrul existent, cu termenul englez în paranteză: **așteptare implicită** (implied expectations), **rotația float-ului** (float turnover), **pistă de cash** (cash runway), **narativ de piață** (market narrative).

O capcană nouă: **„Roșul de fază citit ca roșu structural — și invers"** — un buildout finanțat și un business care pierde bani arată identic în tabel; diferența e runway-ul și pragul numit. Ambele erori costă: prima face să ratezi o companie normală pentru stadiul ei, a doua face să ții una care nu are cum să ajungă la mal.

### 4. assets/report-template.html
- Titlul §12 și comentariile aferente.
- Blocurile pentru cele două teze folosesc **numai componente existente** (`.concept`, `.stats`, liste). Eticheta de fază e `<b>` în coloana existentă.
- Fără CSS nou, fără componente noi, fără modificări la motorul de grafice, fără biblioteci.

### 5. README.md
Un bullet nou în „Ce face diferit": explică ce presupune prețul de azi și de ce e roșu fiecare roșu, fără să înmoaie niciun prag.

## În afara scopului, deliberat

DCF invers. Pilon nou sau renumerotare. Date de opțiuni sau social media (fără sursă gratuită de încredere). Orice semafor pe hype. Orice modificare a celor opt praguri. Fetch-uri noi. Redesign de template.

## Criterii de acceptare

1. Cele șase fișiere actualizate; opt piloni peste tot; nicio renumerotare de secțiune (grep după „13" și „Riscuri" nu găsește rămășițe inconsistente).
2. `references/scorecard.md` §9 conține regula de fază cu exact cele trei condiții, formulate mecanic.
3. Zero fetch-uri noi în Faza 2 față de versiunea curentă.
4. Rerularea pe **IREN** produce exact aceleași opt culori ca raportul anterior — **șase roșii și două galbene** — plus etichete de fază pe fiecare, o cifră de așteptare implicită pe ruta B, rotația float-ului în §9 și o teză a taurului cu surse datate în §12.
5. Rerularea pe **PLTR** (companie pe profit) folosește ruta A, neschimbată, și produce etichete de fază coerente pe cele trei roșii.
6. Cuvintele „va crește"/„va scădea" despre piață, dobânzi sau preț apar nicăieri; niciun preț-țintă; așteptarea implicită nu apare convertită în preț.
7. Commis pe `hype-context`; `main` neatins.
