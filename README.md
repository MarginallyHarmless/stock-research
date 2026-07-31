# stock-research

> A Claude Code skill that analyses a listed company for a beginner retail investor and explains, in Romanian, what is actually happening with the stock. Output: a markdown briefing plus a self-contained HTML report with charts.

Skill pentru [Claude Code](https://claude.com/claude-code) care analizează o companie listată la bursă și explică, pe înțelesul cuiva fără background financiar, ce se întâmplă cu ea.

Nu presupune că știi ce e un moat, un free cash flow sau un short interest — le explică pe măsură ce le folosește, cu cifrele companiei ca exemplu.

## Ce produce

Un raport pe 12 secțiuni, în română, cu termenii tehnici păstrați în engleză între paranteze:

| # | Secțiune | Răspunde la |
|---|---|---|
| — | Scorecard | Cum stă pe cei opt piloni, cu semafoare |
| 1 | Unde se află acțiunea acum | Poziția în interval, randamentul **față de S&P 500**, volatilitatea, ce a mutat prețul |
| 2 | Ce face compania | Cine plătește, pentru ce, de unde vin banii |
| 3 | Moat | Cele cinci tipuri de avantaj competitiv, evaluate pe rând |
| 4 | Free cash flow | Generează cash real sau doar profit contabil |
| 5 | Cash vs. datorii | Cât de solidă e la o criză sau la dobânzi mari |
| 6 | Buyback, dividend, diluție | Partea ta din companie crește sau scade |
| 7 | Creștere și marje | Afacerea se întărește sau se erodează |
| 8 | Evaluare | Companie bună — dar la ce preț, și ce presupune prețul ăsta |
| 9 | Sentiment de piață | Instituționali, short interest, indici, analiști |
| 10 | Management și insideri | Cine conduce, cumpără cu banii lor, structura de vot |
| 11 | Expunerea la lume | Cât o clatină dobânzile, ciclul, geopolitica — expunerea se notează, vremea nu |
| 12 | Riscuri | Ce ar invalida teza |
| 13 | Ce ai învățat | Concepte noi și întrebări pentru următoarea companie |

Plus un fișier HTML de sine stătător, cu grafice, temă deschisă/întunecată, care se deschide offline și se printează curat.

## Instalare

```bash
git clone https://github.com/MarginallyHarmless/stock-research ~/.claude/skills/stock-research
```

Nu are dependențe și nu cere nicio cheie API. Datele vin din surse publice gratuite (stockanalysis.com, finviz.com, marketbeat.com), citite cu WebFetch.

## Folosire

Se declanșează singur când menționezi un ticker sau o companie listată:

```
analizează-mi PLTR
ce se întâmplă cu Intel?
e scumpă ASML acum?
cum stă Palantir cu datoriile?
```

Ultima formă livrează doar secțiunea cerută, nu tot raportul.

## Ce face diferit

**Compară cu indicele.** Randamentul acțiunii apare mereu lângă cel al S&P 500, pe aceleași orizonturi. E comparația care lipsește din aproape orice analiză pentru începători și singura care spune dacă alegerea a meritat efortul — o acțiune care a făcut +12% într-un an în care indicele a făcut +18% a fost o decizie proastă, chiar dacă e pe verde în portofoliu.

**Se uită la numărul de acțiuni, nu la suma cheltuită pe buyback.** Multe companii răscumpără doar cât să compenseze acțiunile date angajaților: cheltuie miliarde, iar felia ta rămâne la fel.

**Traduce volatilitatea în bani.** Nu „beta 2,2", ci cât ai pierde la o corecție obișnuită de piață — pentru că majoritatea pierderilor reale ale investitorilor de retail vin din vânzări în panică, nu din alegerea companiei greșite.

**Pune compania în context.** Dobânzile, ciclul economic, tarifele și reglementarea intră în raport cu o regulă strictă: expunerea companiei la ele se notează — e o proprietate stabilă a afacerii — iar vremea de azi doar se descrie, datată. Prognozele de piață sunt interzise la fel ca prețurile-țintă.

**Are praguri scrise, nu impresii.** Fiecare semafor are criterii explicite în [`references/scorecard.md`](references/scorecard.md), cu excepții pentru bănci, REIT-uri, biotech, utilități și companii ciclice — unde aplicarea mecanică a grilei dă răspunsuri greșite.

**Nu dă recomandări.** Fără „cumpără" sau „vinde", fără preț-țintă propriu. Descrie compania și semnalele; concluzia rămâne a cititorului.

## Structură

```
SKILL.md                      workflow în 5 faze + structura raportului
references/scorecard.md       pragurile verde/galben/roșu și raționamentul
references/glossary.md        conceptele explicate în română + capcanele clasice
references/data-sources.md    URL-uri, burse non-US, ETF-uri, ce faci la 404
assets/report-template.html   template-ul HTML, cu motor de grafice inclus
```

## Limite

Datele sunt publice și au întârziere: până la un trimestru pentru raportări, două săptămâni pentru short interest, 45 de zile pentru dețineri instituționale. Analiza e despre calitatea afacerii, nu despre momentul potrivit de cumpărare, și nu prezice prețul.

**Nu este consultanță de investiții** și nu ține cont de situația financiară, orizontul de timp sau toleranța la risc a nimănui.

## Licență

MIT
