# Surse de date

Toate sursele sunt gratuite și nu necesită cont sau cheie API. Se citesc cu WebFetch.

## Acțiuni americane (NASDAQ, NYSE)

Tickerul se scrie cu **litere mici** în URL-urile stockanalysis.com. Pentru finviz, majuscule.

| URL | Ce conține |
|---|---|
| `stockanalysis.com/stocks/{t}/` | Preț, capitalizare, P/E, randamentul dividendului, rezumat |
| `stockanalysis.com/stocks/{t}/company/` | Descriere, sector, industrie, CEO, an înființare, angajați, segmente |
| `stockanalysis.com/stocks/{t}/financials/` | Venituri, marjă brută / operațională / netă, EPS — pe 5+ ani |
| `stockanalysis.com/stocks/{t}/financials/balance-sheet/` | Cash, datorie totală, capitaluri proprii, acțiuni în circulație pe an |
| `stockanalysis.com/stocks/{t}/financials/cash-flow-statement/` | Cash flow operațional, capex, FCF, răscumpărări, dividende, SBC |
| `stockanalysis.com/stocks/{t}/financials/ratios/` | Multipli istorici pe an (P/E, P/FCF, EV/EBITDA), datorie/capitaluri, lichiditate curentă |
| `stockanalysis.com/stocks/{t}/statistics/` | Acțiuni în circulație + variație anuală, short interest, % din float, deținere instituțională și insideri, evaluare, total cash/debt |
| `stockanalysis.com/stocks/{t}/dividend/` | Istoric dividend, rata de acoperire (payout ratio) |
| `finviz.com/quote.ashx?t={T}` | **Indici**, Inst Trans, Insider Trans, Short Ratio, ROIC, PEG, Debt/Eq, bursa, consens analiști |

Pagina `/financials/ratios/` nu conține marje — acelea sunt pe `/financials/`. Nu insista pe ratios dacă ai nevoie de marje.

**Pagini care NU există pe stockanalysis** (dau 404, nu le încerca): `/holders/`, `/institutional-ownership/`, `/ownership/`. De acolo iei doar procentele, de pe `/statistics/`.

## Instituționali și insideri — MarketBeat

Sursa care acoperă golul lăsat de stockanalysis. URL-ul cere bursa în majuscule (`NASDAQ` sau `NYSE`) și tickerul tot cu majuscule:

| URL | Ce conține |
|---|---|
| `marketbeat.com/stocks/{BURSA}/{T}/institutional-ownership/` | % deținere instituțională, **câți investitori au cumpărat vs. câți au vândut** în ultimele 12 luni, **capital intrat vs. ieșit** în dolari, listă de deținători |
| `marketbeat.com/stocks/{BURSA}/{T}/insider-trades/` | Tranzacții nominale: nume, funcție, cumpărare/vânzare, număr de acțiuni, valoare, dată; plus numărul de insideri cumpărători vs. vânzători pe 12 luni |

**Cifra care contează pe pagina de instituționali e raportul intrări/ieșiri, nu lista de deținători.** „4.398 de cumpărători vs. 3.530 de vânzători, 291 mld $ intrați vs. 106 mld $ ieșiți" e un semnal interpretabil. Folosește-l ca sursă principală pentru secțiunea de sentiment; `Inst Trans` de la finviz e doar un procent agregat, mult mai sărac.

**Capcană la lista de deținători:** tabelul de pe MarketBeat e sortat după *ponderea în portofoliul deținătorului*, nu după mărimea poziției. Rezultatul e că apar firme mici de administrare care au pus aproape tot în acțiunea respectivă, nu BlackRock și Vanguard — care sunt aproape sigur cei mai mari deținători reali ai oricărei companii americane mari. Nu prezenta lista aceea ca fiind „cei mai mari acționari". Dacă utilizatorul chiar vrea deținătorii după mărime, spune că sursele gratuite nu le expun de încredere și că datele originale sunt formularele 13F de pe SEC EDGAR.

**Pe pagina de insideri, ce contează e asimetria.** Vânzările sunt zgomot — directorii primesc o parte din salariu în acțiuni și vând programat. „0 cumpărători, 12 vânzători" e situația normală la o companie de tehnologie, nu un semnal de alarmă. Invers însă, un director care cumpără din bani proprii e un semnal rar și puternic, pentru că există un singur motiv să faci asta. Formulează secțiunea în consecință, altfel sperii degeaba pe cineva care nu știe cum sunt plătiți executivii.

**Surse verificate ca inaccesibile** — nu pierde timp cu ele: `finance.yahoo.com/quote/{T}/holders/` (503), `wsj.com/market-data/quotes/` (blocat), `fintel.io` (403), `nasdaq.com/market-activity/` (timeout).

## Burse non-americane

Structura URL-ului se schimbă: `stockanalysis.com/quote/{bursa}/{TICKER}/`, cu tickerul în majuscule.

| Bursă | Cod | Exemplu |
|---|---|---|
| Londra | `lon` | `stockanalysis.com/quote/lon/SHEL/` |
| Toronto | `tsx` | `stockanalysis.com/quote/tsx/SHOP/` |
| Frankfurt / Xetra | `etr` | `stockanalysis.com/quote/etr/SAP/` |
| Amsterdam | `ams` | `stockanalysis.com/quote/ams/ASML/` |
| Paris | `epa` | `stockanalysis.com/quote/epa/MC/` |
| Tokyo | `tyo` | `stockanalysis.com/quote/tyo/7203/` |

Subpaginile sunt aceleași (`/financials/`, `/statistics/` etc.). Finviz acoperă doar bursele americane — pentru companii europene, indicii și short interest fie lipsesc, fie trebuie căutate separat. Spune în raport ce n-ai putut obține, în loc să lași secțiunea să pară completă.

Multe companii mari non-americane au și un ADR listat în SUA (ASML, SAP, TM, SHEL). Dacă utilizatorul se referă la varianta americană, folosește tickerul US — datele sunt mai bogate. Menționează care variantă ai analizat, pentru că prețurile diferă.

## ETF-uri și fonduri

`stockanalysis.com/etf/{t}/` — dacă tickerul e un ETF, nu o companie, cea mai mare parte a grilei nu se aplică (nu are moat, FCF sau buyback). Spune-i asta utilizatorului și oferă în schimb: ce urmărește fondul, comisionul anual (expense ratio), top dețineri, concentrare, randament istoric.

## Surse de rezervă

Când sursele principale nu răspund sau datele par greșite:

| Sursă | Bună pentru |
|---|---|
| `macrotrends.net/stocks/charts/{T}/{nume-companie}/financial-statements` | Istoric lung (10+ ani) — util pentru companii ciclice |
| `sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={T}&type=10-K` | Sursa primară: rapoartele originale. Cel mai de încredere pentru datorii, riscuri (secțiunea *Risk Factors*) și SBC |
| `wsj.com/market-data/quotes/{T}/financials` | Verificare încrucișată |
| `roic.ai/quote/{T}` | ROIC și indicatori de calitate a capitalului |

Pentru context recent (raportări, știri, motivele unei mișcări de preț), folosește WebSearch — dar tratează rezultatele ca pe context calitativ, nu ca sursă de cifre. Cifrele vin din situațiile financiare.

## Reguli de interpretare a datelor

**Anul fiscal.** Multe companii nu au anul fiscal aliniat cu cel calendaristic. La NVIDIA, „FY2026" se încheie în ianuarie 2026, deci acoperă în cea mai mare parte anul 2025. Apple își încheie anul în septembrie. Verifică pe `/company/` și menționează-l în raport, altfel cifrele par decalate.

**Prospectiv vs. retrospectiv.** `PE Ratio` e calculat pe ultimele 12 luni raportate (trailing). `Forward PE` e pe estimările analiștilor pentru următoarele 12 luni — adică pe presupuneri, nu pe fapte. Când diferă mult, spune de ce: piața se așteaptă la o creștere sau o scădere mare a profitului.

**Întârzieri.** Short interest se publică de două ori pe lună, cu câteva zile întârziere. Dețineri instituționale: până la 45 de zile după încheierea trimestrului. Situațiile financiare: câteva săptămâni după încheierea trimestrului. Nimic din raport nu e informație în timp real.

**Când două surse diferă cu peste 5%** la aceeași valoare, arată ambele cifre cu sursa fiecăreia. Diferențele apar de obicei din definiții diferite (de exemplu, dacă leasingurile intră sau nu în „datorie totală") — nu presupune că una e greșită.
