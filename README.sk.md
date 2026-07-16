# FAVpage

[English](README.md) · **Slovensky**

![FAVpage screenshot](screenshot.jpg)

Osobná štartovacia stránka / homelab dashboard v duchu Homarr či Heimdall — ibaže celá appka je **jediný HTML súbor bez závislostí**. Žiadny server, žiadny build, žiadny framework. Otvoríš v prehliadači a funguje.

## Funkcie

- **Voľné rozloženie** — dlaždice aj skupiny potiahneš kamkoľvek na 20 px mriežke. Prvky sa nikdy neprekrývajú: pustenie na obsadené miesto odsunie susedov nadol („sprav miesto") a keď sa skupina zmenší, prvky pod ňou sa prisunú späť nahor.
- **Skupiny** — kontajnery s vlastným názvom a farbou, meniteľnou veľkosťou. Dlaždica môže žiť v skupine alebo voľne na ploche.
- **Viac plôch (tabov)** — zvislý rad bodiek na ľavom okraji prepína medzi plochami (napr. „Doma“, „Práca“); názvy sa ukážu pri hoveri a posledné je „+“, ktoré otvorí správcu plôch s kurzorom pripraveným v pridávacom riadku. Všetko ostáva v jednom `links.js` (existujúce dáta sa automaticky presunú na prvú plochu), posledná aktívna plocha sa pamätá per prehliadač a z klávesnice prepína `Alt+1…9` alebo `Ctrl+↑`/`Ctrl+↓` (cyklovanie). Keď vyhľadávanie nenájde nič na aktívnej ploche, `Enter` skočí na plochu, kde výsledok je. Správa plôch v menu pod kolieskom: premenovanie, farba bodky, poradie ťahaním (drag & drop), skryť/zobraziť a zmazanie (obsah zmazanej plochy sa presunie na prvú); nová plocha sa pridáva prázdnym riadkom pod zoznamom.
- **Otvorenie celej skupiny** — pri hoveri nad skupinou sa v jej hlavičke objaví tlačidlo, ktoré otvorí všetky linky skupiny do nových tabov (rešpektuje aktívny filter vyhľadávania). Pri viac ako 8 linkách sa najprv spýta; ak popup blocker prehliadača niektoré taby zablokuje, hint poradí, ako ich povoliť.
- **Lasso výber** — v režime úprav ťahaním po prázdnej ploche označíš viac dlaždíc naraz a celý výber presunieš jedným ťahom (na plochu aj do skupiny) so zachovaním rozostupov.
- **Poznámky** — pastelové lístky (4 farby), ktoré žijú na ploche ako dlaždice. Klikneš do poznámky a rovno píšeš — žiadny dialóg; prvý riadok sa zobrazuje tučne ako nadpis a zmeny sa ukladajú samy. Veľkosť zmeníš kedykoľvek ťahaním za pravý dolný roh (úchyt sa ukáže pri hoveri), vytvoríš ju z menu pod kolieskom (kurzor skočí rovno do nej), presúvanie/farba/mazanie v režime úprav; vyhľadávanie hľadá aj v texte poznámok.
- **Pridanie linku ťahom** — potiahni URL z adresného riadka, záložku alebo odkaz zo stránky priamo na plochu a dlaždica vznikne na mieste kurzora (názov sa vezme z textu odkazu alebo domény, ikona sa doplní automaticky). Pustením do skupiny sa link zaradí do skupiny; na voľnej ploche vznikne samostatná dlaždica.
- **Inteligentné ikony** — kaskáda: vlastná hodnota (názov z [dashboard-icons](https://github.com/homarr-labs/dashboard-icons), URL obrázka alebo `data:image/...` URI) → favicon stránky → farebný písmenový avatar. Jedným klikom sa všetky ikony stiahnu a vložia ako base64 pre plne offline použitie. Stiahnutý obsah sa validuje (MIME typ + magic bytes), takže HTML stránka vrátená namiesto faviconu sa nikdy nevloží.
- **Sledovanie dostupnosti** — zaškrtni pri linke „Sledovať dostupnosť" a v pravom dolnom rohu dlaždice sa objaví bodka stavu: zelená = online, červená = offline, sivá = nezistené. Kontroluje sa pri načítaní stránky a potom každých 5 minút (max 4 requesty naraz, timeout 6 s, offline až po 2 zlyhaniach za sebou kvôli falošným poplachom od adblocku/proxy). Ide o HTTP kontrolu cez `no-cors` fetch — skutočný ICMP ping z prehliadača nejde a odpoveď je „opaque", takže zelená znamená „server odpovedal", nie „stránka je bez chyby".
- **Upratovaná hlavička** — trvalo len dve tlačidlá: lupa a ozubené koliesko. Všetko ostatné (uložiť, pripojiť, upraviť, pridať link/skupinu, vložiť ikony, akcent, téma, jazyk) sa rozroluje ako stĺpec ikon pod kolieskom. Koliesko nesie bodku pripojenia a pri neuložených zmenách svieti akcentovou farbou — stav vidno aj so zavretým menu.
- **Dva jazyky** — rozhranie hovorí anglicky aj slovensky; prepínač je v menu pod kolieskom a voľba sa pamätá v prehliadači. Predvolená je angličtina.
- **Svetlá a tmavá téma** — prepínač slnko/mesiac v menu pod kolieskom; prvá návšteva sa riadi systémovou preferenciou, potom sa voľba pamätá v prehliadači. Akcentová farba má picker v režime úprav a žije v dátach (`meta.accent` v `links.js`), takže sa synchronizuje medzi počítačmi — aj animované pozadie sa prefarbí okamžite.
- **Hover efekt** — rotujúci gradientový okraj vo farbe dominantnej farby ikony (extrahuje sa za behu, cachuje sa).
- **Animované pozadie** — jemná sieť bodov (alfa 30 %) kreslená na canvase pod obsahom; pri skrytom tabe sa sama pozastaví.
- **Okamžité vyhľadávanie** — stlač `/` alebo klikni na lupu a pole sa rozroluje; výsledky sa filtrujú počas písania, `↓`/`↑` medzi nimi prepínajú a `Enter` otvorí označený v novom tabe. Keď nič nenájde, `Enter` pošle výraz do webového vyhľadávača (predvolene Google; iný nastavíš cez `meta.searchUrl` v `links.js`, napr. `"https://duckduckgo.com/?q="`). `Esc` pole vyčistí a zbalí.
- **Žiadne účty, žiadny cloud** — dáta sú jeden čitateľný JavaScript súbor vedľa HTML.

## Ako začať

1. Vezmi `index.html` a `links.js` a daj ich do jedného priečinka.
2. Otvor `index.html` v **Chrome, Edge alebo Brave** (ukladanie používa File System Access API; ostatné prehliadače stránku zobrazia, ale neuložia).
3. Otvor **menu pod kolieskom** (vpravo hore) a klikni na **sponku** — vyber svoj `links.js` a odteraz sa každá zmena ukladá automaticky. Stav pripojenia ukazuje malá bodka na kolieska aj na sponke.
4. V menu pod kolieskom klikni na **ceruzku** — alebo podrž dlaždicu či skupinu **3 sekundy** — pre režim úprav:
   - skupiny ťahaj za úchyt `⠿`, veľkosť meň pravým dolným rohom,
   - dlaždice ťahaj kamkoľvek — v rámci skupiny, do inej skupiny alebo na voľnú plochu,
   - tlačidlami `+` pridávaš linky a skupiny.

## Dáta, synchronizácia medzi počítačmi

Všetky dáta žijú v `links.js` (`window.FAV = {...}`) — skupiny, linky, pozície, ikony — s časovou značkou obsahu (`meta.updatedAt`), ktorá sa posúva pri každej zmene.

- Každá zmena sa zároveň cachuje v `localStorage`, takže neuložená práca prežije refresh aj pád prehliadača.
- **Pri štarte** vyhráva novšia z dvojice súbor vs. cache; prevzatá neuložená práca sa označí, aby si ju nezabudol uložiť.
- **Na pozadí** (pri fokuse okna a raz za minútu) sa porovnáva časová značka pripojeného súboru — keď iný počítač zapísal novšiu verziu, načíta sa automaticky. Lokálne neuložené zmeny sa nikdy potichu neprepíšu; konflikt len zobrazí upozornenie.

Priečinok daj do ľubovoľného synchronizovaného umiestnenia (Nextcloud, Dropbox, Syncthing, sieťový disk…) a každý počítač vidí rovnakú plochu. Linky môže pridávať ktorýkoľvek stroj; časové značky to vyriešia.

Ako poistka sa pri každom uložení sanitizujú ikony: každá `data:` ikona, ktorá nie je skutočný obrázok, sa zahodí a linka spadne späť na automatickú kaskádu ikon.

## Klávesové skratky

| Klávesa | Akcia |
|---|---|
| `/` | otvor a zafokusuj vyhľadávanie |
| `↓` / `↑` | prepínaj medzi výsledkami vyhľadávania |
| `Alt+1…9` | prepni na plochu 1…9 |
| `Ctrl+↑` / `Ctrl+↓` | cykluj medzi plochami |
| `Enter` | otvor označený výsledok — alebo hľadaj na webe, keď nič nesedí; potvrď dialóg |
| `Esc` | vyčisti vyhľadávanie / zruš výber / zavri dialóg |

## Technické poznámky

- Jediný súbor vanilla JavaScriptu (štýl ES5) a CSS, bez závislostí.
- Kolízne rozloženie je jednoduchý osovo zarovnaný „pack down" algoritmus — pustený prvok ostáva, prekrývajúci sa susedia sa posunú nadol len o toľko, koľko treba; vždy skončí, lebo y iba rastie.
- Kontrola dostupnosti používa `fetch(url, { mode: "no-cors" })` s timeoutom cez `AbortController`; linky sa prihlasujú jednotlivo (`ping: true` v `links.js`), takže stránka nestrieľa requesty za linky, ktoré nesleduješ.
- Lokálny náhľad pri vývoji: ľubovoľný statický server, napr. `python -m http.server`.

## Podpora prehliadačov

Plná funkčnosť (vrátane ukladania) vyžaduje Chromium prehliadač s File System Access API — Chrome, Edge, Brave, Opera. Firefox a Safari stránku zobrazia, ale do `links.js` nezapíšu.
