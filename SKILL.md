---
name: stock-research
description: Analizează o companie listată la bursă și explică în română, pentru un investitor de retail începător, ce se întâmplă cu ea — ce face compania, moat-ul, free cash flow, cash vs datorii, buyback vs diluție, creștere și marje, evaluare, instituționali, short interest, indici, insideri și riscuri — cu un scorecard verde/galben/roșu, explicații educaționale ale fiecărui concept și un raport HTML final cu grafice. Use this skill whenever the user mentions a stock ticker (NVDA, AAPL, PLTR, ASML...) or a listed company name and wants to understand it — including phrasings like "ce părere ai de X", "analizează X", "merită X", "ce se întâmplă cu X", "e scumpă X", "de ce a scăzut X", "explică-mi X", "should I look at X", "what's going on with X" — even when they don't use the word "analiză" or ask for a formal report. Also use when comparing two listed companies, or when the user asks about a specific pillar (moat, free cash flow, datorii, buyback, short interest, dilution) of a named company.
---

# Analiză de acțiuni pentru investitori începători

Publicul acestui skill e cineva care investește cu banii proprii, dar nu are background financiar. Nu are nevoie doar de cifre — are nevoie să înțeleagă **de ce contează** fiecare cifră. Un raport plin de rapoarte P/E și EV/EBITDA fără explicații nu îl ajută să ia decizii mai bune data viitoare; îl învață doar să se uite la numere pe care nu le înțelege.

De aceea raportul e construit ca o lecție cu exemplul concret al companiei, nu ca un buletin de date.

## Trei principii care ghidează tot

**1. Conceptul înainte de cifră.** Fiecare secțiune explică întâi ce e lucrul măsurat și de ce contează, abia apoi arată cifrele companiei. Cine citește trebuie să poată aplica ideea la orice altă companie, nu doar la asta.

**2. Nicio cifră inventată.** Dacă o valoare nu a putut fi extrasă, scrie `N/A (nu apare pe sursă)`. O cifră aproximată dar prezentată ca fapt e mai rea decât o cifră lipsă, pentru că cineva ar putea investi pe baza ei. Fiecare număr important vine cu anul fiscal.

**3. Fără recomandări.** Nu scrie „cumpără", „vinde", „e o oportunitate" și nu inventa un preț-țintă. Descrii compania și semnalele; concluzia e a cititorului. Asta nu e o formalitate — e diferența dintre a educa pe cineva și a-l face dependent de opinia ta.

## Faza 1 — Identifică exact compania

Confirmă tickerul și bursa înainte de a scoate date. `ARM` pe NASDAQ e altceva decât `ARM.L` pe Londra, iar dacă utilizatorul a dat un nume de companie („Palantir"), mapează-l la ticker și spune explicit pe ce ai lucrat.

Dacă tickerul e ambiguu sau nu există, întreabă înainte de a continua. E mai ieftin decât un raport întreg despre altă companie.

## Faza 2 — Colectează datele

Trimite **toate** fetch-urile de mai jos într-un singur mesaj, ca să ruleze în paralel. Secvențial durează de câteva ori mai mult fără niciun câștig.

Pentru un ticker american `{T}` (scris cu litere mici în URL):

| # | URL | Ce scoți |
|---|-----|----------|
| 1 | `stockanalysis.com/stocks/{T}/company/` | Descriere, sector, industrie, CEO, an înființare, nr. angajați, segmente de venit |
| 2 | `stockanalysis.com/stocks/{T}/financials/` | Venituri pe 5 ani, creștere %, marjă brută / operațională / netă |
| 3 | `stockanalysis.com/stocks/{T}/financials/cash-flow-statement/` | Cash flow operațional, capex, **free cash flow**, **răscumpărări de acțiuni**, dividende |
| 4 | `stockanalysis.com/stocks/{T}/financials/balance-sheet/` | Cash și investiții, datorie totală, capitaluri proprii, nr. acțiuni pe an |
| 5 | `stockanalysis.com/stocks/{T}/statistics/` | Acțiuni în circulație + variația anuală, short interest, % din float, deținere instituțională și insideri, P/E, forward P/E, P/FCF, EV/EBITDA, total cash, total debt |
| 6 | `finviz.com/quote.ashx?t={T}` | **Indici** (S&P 500, NDX, DJIA), Inst Trans, Insider Trans, Short Ratio, ROIC, PEG, Debt/Eq, bursa — plus **Beta, Volatility, 52W Range, Perf YTD / Year / 3Y / 5Y, data următoarei raportări, Target Price, Recom** |
| 7 | `marketbeat.com/stocks/{BURSA}/{T}/institutional-ownership/` | Câți investitori instituționali au **cumpărat vs. vândut** în 12 luni și **cât capital a intrat vs. a ieșit** în dolari |
| 8 | `marketbeat.com/stocks/{BURSA}/{T}/insider-trades/` | Tranzacțiile insiderilor, nominal: cine, ce funcție, cumpărare sau vânzare, câte acțiuni, ce dată |
| 9 | `finviz.com/quote.ashx?t=SPY` | Performanța S&P 500 pe aceleași orizonturi — **etalonul** față de care compari acțiunea |

`{BURSA}` e `NASDAQ` sau `NYSE`, cu majuscule, iar tickerul tot cu majuscule. Dacă nu știi bursa, pune varianta cea mai probabilă în primul val de fetch-uri și reia doar aceste două URL-uri cu cealaltă dacă dau 404 — bursa reală o afli oricum din finviz în același val.

Fetch-ul 9 pare o risipă, dar e cea mai importantă cifră din raport pentru cineva care învață: fără ea nu poate ști dacă acțiunea a mers *bine* sau doar a mers odată cu piața. Vezi explicația din secțiunea 1 a raportului.

Bursele non-americane, ETF-uri, surse de rezervă, capcane de interpretare și ce faci când o pagină dă 404: citește `references/data-sources.md`.

Dacă utilizatorul a cerut explicit doar un pilon (de exemplu „cum stă Palantir cu datoriile?"), scoate doar paginile relevante și livrează secțiunea aceea, nu tot raportul.

## Faza 3 — Verifică înainte de a scrie

Datele de pe web sunt uneori inconsistente. Câteva verificări care prind majoritatea problemelor:

- **FCF ar trebui să fie ≈ cash flow operațional − capex.** Dacă nu se potrivește, sursa folosește o altă definiție; spune ce definiție ai folosit.
- **Buyback mare, dar numărul de acțiuni nu scade** → compania răscumpără doar cât să compenseze acțiunile date angajaților. Asta schimbă complet interpretarea secțiunii de buyback. Vezi capcana explicată în `references/glossary.md`.
- **Datorie netă = datorie totală − cash și echivalente.** Calculeaz-o tu; puține surse o afișează direct.
- **Anul fiscal ≠ anul calendaristic** pentru multe companii. La NVIDIA, „FY2026" se încheie în ianuarie 2026, deci acoperă aproape tot 2025. Menționează asta când e cazul, altfel cifrele par decalate cu un an.
- **Dacă două surse diferă cu peste 5%** la aceeași valoare, arată ambele și spune care e mai recentă.

## Faza 4 — Scrie raportul

Structura de mai jos. Adaptează lungimea la companie — o firmă cu bilanț simplu nu are nevoie de trei paragrafe despre datorii — dar păstrează ordinea și titlurile, pentru ca rapoartele pe companii diferite să fie comparabile.

```markdown
# {Nume companie} ({TICKER}) — analiză pentru investitor începător
*Date la {data}. Surse: stockanalysis.com, finviz.com, marketbeat.com. Nu e consultanță de investiții.*

## Fișa rapidă
| | |
|---|---|
| Preț / Capitalizare | |
| Sector / Industrie | |
| Indici | |
| Anul fiscal se încheie | |
| Următoarea raportare | |

### Scorecard
| Pilon | Semafor | În două cuvinte |
|---|---|---|
| Moat (avantaj competitiv) | 🟢/🟡/🔴 | |
| Free cash flow | | |
| Cash vs. datorii | | |
| Buyback vs. diluție | | |
| Creștere și marje | | |
| Evaluare (preț) | | |
| Sentiment de piață | | |

## 1. Unde se află acțiunea acum și de ce
## 2. Ce face compania, pe înțelesul oricui
## 3. Moat — are ceva ce o face greu de înlocuit?
## 4. Free cash flow — banii care rămân efectiv
## 5. Cash vs. datorii — cât de solidă e la o furtună
## 6. Buyback, dividend, diluție — partea ta crește sau scade?
## 7. Creștere și marje — afacerea se întărește sau se erodează?
## 8. Evaluare — la ce preț, și ce presupune prețul ăsta
## 9. Sentiment de piață — instituționali, short interest, analiști
## 10. Management, insideri și structura de vot
## 11. Riscuri — ce ar strica teza
## 12. Ce ai învățat aici + întrebări pentru data viitoare
```

**Secțiunea 1 e cea care răspunde direct la „ce se întâmplă cu ea".** Pune-o prima, pentru că e întrebarea din capul omului. Conține patru lucruri:

1. **Unde e prețul** în intervalul de 52 de săptămâni — aproape de maxim, la mijloc, sau prăbușit.
2. **Randamentul vs. S&P 500** pe YTD, 1 an, 3 ani și 5 ani, în tabel, unul lângă altul. Asta e comparația care lipsește din 99% din analizele pe care le citește un începător, și e singura care contează cu adevărat: dacă acțiunea a făcut +12% într-un an în care indicele a făcut +18%, investitorul a pierdut bani față de alternativa care nu cerea nicio muncă. Spune-o direct când se întâmplă.
3. **Cât de violent se mișcă** — beta și volatilitatea, traduse în ceva concret: „beta 2,2 înseamnă că, istoric, când piața scade 10%, acțiunea asta scade în jur de 22%. Dacă ai 10.000 € aici, o corecție normală de piață înseamnă −2.200 €. Dacă cifra asta te-ar face să vinzi în panică, poziția e prea mare."
4. **Ce a mutat prețul recent și ce urmează** — ultima raportare (a depășit sau a ratat așteptările, ce a spus conducerea despre trimestrul următor) și data următoarei raportări. Folosește titlurile de pe pagina principală stockanalysis și, dacă e nevoie de context, WebSearch — dar tratează știrile ca narativ, nu ca sursă de cifre.

Secțiunea asta **nu primește semafor**. Performanța trecută a prețului nu spune nimic despre calitatea afacerii, iar un verde acolo ar sugera exact confuzia pe care skill-ul încearcă s-o prevină.

Ce trebuie să conțină fiecare secțiune, ce praguri decid culoarea semaforului și explicațiile educaționale gata scrise:

- **`references/scorecard.md`** — pragurile verde/galben/roșu pentru fiecare pilon, cu raționamentul din spate. Citește-l înainte de a atribui semafoare, ca să nu evaluezi „după feeling"; scopul e ca aceeași companie să primească aceleași culori indiferent de zi.
- **`references/glossary.md`** — definițiile în română ale conceptelor (moat, FCF, diluție, short interest, P/FCF etc.), plus capcanele clasice în care cad începătorii. Ia de aici formulările, nu le reinventa de fiecare dată.
- **`references/data-sources.md`** — hărți de URL-uri, burse non-US, ETF-uri, ce faci la 404.

## Cum scrii, ca omul să și învețe

**Fiecare secțiune are aceeași formă:** un bloc scurt care explică conceptul, apoi cifrele companiei, apoi ce înseamnă ele împreună.

> **📚 Ce e free cash flow-ul?**
> Banii care rămân efectiv în cont după ce compania și-a plătit toate cheltuielile *și* investițiile în echipamente sau fabrici. Profitul e o opinie contabilă — depinde de cum amortizezi, cum recunoști veniturile, ce provizioane faci. Cash flow-ul e un fapt bancar. O companie poate raporta profit ani la rând și totuși să rămână fără bani.
>
> **📊 La {TICKER}:** FCF a crescut de la X la Y în ultimii 5 ani. 🟢
>
> **Ce înseamnă:** {interpretarea}

Câteva reguli de ton care fac diferența:

- **Analogii concrete, nu metafore vagi.** „Datorie netă/EBITDA de 4x înseamnă că, dacă ar folosi tot profitul operațional doar pentru datorii, i-ar lua patru ani să scape de ele" ajută. „Îndatorare ridicată" nu ajută.
- **Termenii tehnici în engleză, în paranteză, prima dată când apar** — pentru că pe orice site de bursă îi va vedea în engleză și trebuie să-i recunoască.
- **Spune și când un semnal e slab.** Brandul puternic e un moat mai fragil decât costurile de schimbare. Short interest mare e mai des un avertisment decât o oportunitate. Un începător care nu află asta de la tine o va afla de pe piață, mai scump.
- **Nu netezi lucrurile.** Dacă bilanțul arată prost, spune direct. Un raport care sună mereu echilibrat și pozitiv nu ajută pe nimeni.

Secțiunea 11 nu e un rezumat — e partea care rămâne. Enumeră 2-3 concepte pe care le-a întâlnit aici pentru prima dată și 3-4 întrebări concrete pe care să și le pună la următoarea companie (de exemplu: „numărul de acțiuni a scăzut în ultimii 3 ani?", „marja brută urcă sau coboară?").

## Faza 5 — Generează raportul HTML

După raportul din chat, produ și un fișier HTML de sine stătător. Pornește de la `assets/report-template.html`: copiază-l, înlocuiește conținutul dintre `{{...}}` și șterge secțiunile care nu se aplică. Salvează-l ca `{TICKER}-{AAAA-LL-ZZ}.html` și trimite-l utilizatorului cu SendUserFile (`display: "render"`).

Template-ul conține deja CSS-ul, motorul de grafice și componentele. Nu rescrie stilurile și nu adăuga biblioteci — fișierul trebuie să rămână unul singur, care se deschide offline, cu temă deschisă și întunecată.

**Graficele se descriu cu date, nu cu SVG scris de mână.** Fiecare `<figure class="viz" data-viz="…">` conține un `<script type="application/json">` cu numerele; motorul din pagină desenează. Tu scrii doar valorile.

| `data-viz` | Pentru ce | Formă |
|---|---|---|
| `bars` | O serie pe ani (FCF, venituri, dividende) | `{labels, series:[{name,values}], suffix}` |
| `lines` | Una sau mai multe serii unde contează tendința (marje, număr de acțiuni) | la fel; până la 3 serii |
| `compare` | Acțiunea față de un etalon, pe mai multe orizonturi | `{a, b, rows:[{label,a,b}], suffix, deltaUnit}` |

Trei reguli care țin graficele oneste:

- **`bars` pornește întotdeauna de la zero** — motorul îl impune. Când variația e de câteva procente (numărul de acțiuni e cazul tipic), barele de la zero ar arăta plate, iar cele cu axa tăiată ar transforma o scădere de 3% într-una vizuală de 70%. Folosește `lines`, care are voie să nu pornească de la zero și își afișează explicit minimul și maximul axei.
- **Randamentul față de indice merge pe `compare`, nu pe bare grupate.** Randamentul pe 5 ani depășește de zeci de ori pe cel YTD; pe o scală comună, orizonturile scurte devin invizibile. `compare` scalează fiecare rând separat și calculează singur diferența în puncte procentuale.
- **Niciun semafor nu se transmite doar prin culoare.** Fiecare stare are icon plus etichetă text (`Solid` / `Atenție` / `Risc` / `Absent`), pentru că aproximativ un bărbat din doisprezece nu distinge roșu de verde. Paleta e deja aleasă ca să treacă pragurile de contrast în ambele teme — nu o schimba.

Fără emoji în HTML: în chat sunt utile, într-un document pe care omul îl păstrează arată neîngrijit. Template-ul are icoane SVG pentru toate stările.

## Limitele skill-ului

Spune-le deschis dacă utilizatorul întreabă, și include disclaimerul scurt la finalul raportului:

- Datele sunt din surse publice gratuite, cu întârziere de până la un trimestru pentru raportări și până la două săptămâni pentru short interest.
- Analiza e despre calitatea afacerii, nu despre momentul potrivit de cumpărare. Nu prezice prețul.
- Nu e consultanță de investiții și nu ține cont de situația financiară, orizontul de timp sau toleranța la risc a nimănui.
