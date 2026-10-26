# Tünetnapló, ami csak egérrel működik – hibariport

Repó: https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y
Készítette: Czellecz Andrea-Julia, INFO-LM III.ev

## 1. hiba: Nem lehet nagyítani mobilon (Pinch-to-zoom letiltva)
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Mobilon vagy a böngésző mobil emulátorában kétujjas mozdulattal (pinch-to-zoom) nem lehet nagyítani az oldalt a gyengénlátó felhasználóknak.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** A viewport meta tagben szerepel a `maximum-scale=1, user-scalable=no` beállítás. Megsértett kritérium: **WCAG 2.2 - 1.4.4 Resize Text (AA szint)**.
- **Javítás (commit link, előtte/utána kód):** https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/6f1118bd221d1f230e5c56b209e647aa600f0121
  - Előtte: `<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">`
  - Utána: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Chrome DevTools mobil nézetben és Lighthouse Accessibility audittal ellenőrizve: a zoom letiltására vonatkozó hiba megszűnt.
- **Tanulság / érintett alapelv:** Ne tiltsuk le a felhasználó böngészőjének alapvető kisegítő beállításait; a reszponzivitás nem mehet az akadálymentesség rovására.

## 2. hiba: A tünetek választói nem érhetők el billentyűzettel és nem akadálymentesek
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Billentyűzettel navigálva (Tab billentyű) a fókusz teljesen átugorja a tüneteket, csak egérrel kattinthatók. Képernyőolvasóval (NVDA) vizsgálva az elemek sima szövegként jelennek meg, nem közlik a szerepüket (checkbox) és a kijelölt állapotukat.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** Sima `<div>` elemek lettek használva kattintáskezelővel valódi vezérlők helyett. Megsértett kritériumok:
  - **WCAG 2.2 - 2.1.1 Keyboard (A szint)**
  - **WCAG 2.2 - 4.1.2 Name, Role, Value (A szint)**
- **Javítás (commit link, előtte/utána kód):** https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/d133384d5802ec59b0a55b537a19bb348ae7263e
  - Előtte: `<div class="chk" data-v="fejfajas">Fejfájás</div>` JavaScript toggle eseménnyel.
  - Utána: Natív `<input type="checkbox">` és `<label>` szerkezet használata billentyűzet-fókusszal (`:focus-visible`) és állapottal (`:checked`).
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Csak billentyűzettel tesztelve: Tab-bal rálépve megjelenik a fekete fókuszkeret, a Space billentyű leütésére be- és kikapcsol a zöld jelölés.
- **Tanulság / érintett alapelv:** Az ARIA első szabálya: ha létezik natív HTML elem (pl. input checkbox), mindig azt használjuk `<div>` és egyedi szkriptelés helyett.

## 3. hiba: Hiányzó űrlapcímkék és szabálytalan címsor-hierarchia
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Képernyőolvasóval (NVDA) az Erősség és Megjegyzés mezőkre lépve csak annyi hangzik el, hogy „szerkesztés, szám” vagy „több soros”, a mezők rendeltetése nem hallható. Címsornavigáció során a felolvasó a főcím (H1) után rögtön H3 szintet talál.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** Az űrlapmezők nem rendelkeztek hozzájuk rendelt `<label for="...">` elemmel, és a címsorszintek hierarchiája sérült (H1 -> H3 ugrás). Megsértett kritériumok:
  - **WCAG 2.2 - 1.3.1 Info and Relationships (A szint)**
  - **WCAG 2.2 - 3.3.2 Labels or Instructions (A szint)**
  - **WCAG 2.2 - 2.4.6 Headings and Labels (AA szint)**
- **Javítás (commit link, előtte/utána kód):** https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/99977d7a792c9e3620f736477eb9fc966abbe31a 
  - Előtte: `<h3>Erősség (0–10)</h3><input id="eros" ...>`
  - Utána: `<h2><label for="eros">Erősség (0–10)</label></h2><input id="eros" ...>` (és ugyanez a megjegyzés mezőnél, valamint a Tünetek címénél H3 -> H2 javítás).
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** A címkékre kattintva a fókusz automatikusan a kapcsolódó beviteli mezőre ugrik; böngésző ellenőrzővel vizsgálva a mezők megkapták az Accessible Name tulajdonságot.
- **Tanulság / érintett alapelv:** Minden beviteli mezőnek programozott kapcsolattal rendelkező címkével kell bírnia; a fejlécstruktúrát logikai tartalmi fa felépítésére kell használni ugrások nélkül.


## 4. hiba: Felcserélt fókuszsorrend pozitív tabindex miatt
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Csak billentyűzetes navigációval (Tab billentyű) a fókusz nem a természetes olvasási sorrendben halad: az oldal betöltése után a fókusz átugorja a tüneteket, először a Megjegyzés mezőre ugrik, onnan vissza az Erősségre, majd csak ezután a tünetekre és az oldal tetejére.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** Pozitív értékű `tabindex` attribútumok (`tabindex="1"`, `tabindex="2"`) jelenléte, amelyek felülírják a normál DOM-sorrendet. Megsértett kritérium: **WCAG 2.2 - 2.4.3 Focus Order (Fókusz sorrend - A szint)**.
- **Javítás (commit link, előtte/utána kód):** 
    https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/246da6665e9396c19bc4e76c76b69d4b86067d73
  - Előtte: `<input id="eros" type="number" min="0" max="10" tabindex="2">` és `<textarea id="megj" tabindex="1"></textarea>`
  - Utána: A pozitív `tabindex` attribútumok teljes eltávolítása: `<input id="eros" type="number" min="0" max="10">` és `<textarea id="megj"></textarea>`.
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Csak billentyűzettel tesztelve (Tab billentyű): a fókusz pontosan felülről lefelé halad a DOM-struktúra szerint (Tünetek választói -> Erősség mező -> Megjegyzés mező).
- **Tanulság / érintett alapelv:** Pozitív `tabindex` használata szigorúan kerülendő (antiminta); a logikus és kiszámítható tab-sorrendet mindig a HTML forráskód megfelelő szerkezeti sorrendjének kell biztosítania.

## 5. hiba: A Mentés vezérlő billentyűzettel nem elérhető és nem aktiválható
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Csak billentyűzetes navigációval (Tab) a fókusz átugorja a Mentés elemet, az Enter és Space billentyűk lenyomására az űrlap nem küldhető be. Képernyőolvasóval nem azonosítható gombként.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** Sima `<div>` elemet használtak gombként, ami nem kap billentyűzet-fókuszt és nem rendelkezik button szerepkörrel. Megsértett kritériumok:
  - **WCAG 2.2 - 2.1.1 Keyboard (A szint)**
  - **WCAG 2.2 - 4.1.2 Name, Role, Value (A szint)**
- **Javítás (commit link, előtte/utána kód):**
    https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/0ad4a25c80a0e0fe1f5bc1ae606d84a40e43eba1
  - Előtte: `<div class="btn" id="ment">Mentés</div>`
  - Utána: `<button type="button" class="btn" id="ment">Mentés</button>` hozzáadott `:focus-visible` stílussal.
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Csak billentyűzetes teszteléssel: a Tab billentyűvel a fókusz rákerül a Mentés gombra, az Enter és Space billentyűk lenyomására lefut a validáció és mentési logika.
- **Tanulság / érintett alapelv:** Az ARIA első szabálya: valódi kattintható műveletekhez mindig natív `<button>` elemet kell használni `<div>` helyett, így a billentyűzetkezelés és az akadálymentes szerepkör automatikusan biztosított.

## 6. hiba: Kizárólag színen alapuló hibajelzés és szöveges hibaüzenet hiánya
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Hibás vagy üres erősségérték megadásakor a rendszer kizárólag a keret színét változtatja pirosra (`.hibas`). Színtévesztő vagy képernyőolvasót használó felhasználó számára semmilyen szöveges vagy auditív információ nem jelenik meg a hiba jellegéről.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** Az információ és hibaállapot kizárólag színkóddal van kifejezve, hiányzik a leíró hibaüzenet és az `aria-invalid` / `aria-describedby` kapcsolat. Megsértett kritériumok:
  - **WCAG 2.2 - 1.4.1 Use of Color (A szint)**
  - **WCAG 2.2 - 3.3.1 Error Identification (A szint)**
  - **WCAG 2.2 - 3.3.3 Error Suggestion (AA szint)**
- **Javítás (commit link, előtte/utána kód):**
    https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/5d9891306a3ce98efda02194616c4bc87fbd532c
  - Előtte: Csak `eros.classList.add('hibas');` lefutása vizuális piros kerettel.
  - Utána: Szöveges hibaüzenet megjelenítése (`role="alert"`), `aria-invalid="true"` jelzés és `aria-describedby="eros-hiba"` összekapcsolás a beviteli mezővel, amely konkrét javítási útmutatást ad (0 és 10 közötti érték).
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Üres és érvénytelen mezőértékkel beküldve: a képernyőn szövegesen megjelenik a figyelmeztetés („Kérjük, adjon meg egy 0 és 10 közötti számot!”), a képernyőolvasó a `role="alert"` miatt azonnal felolvassa, a fókusz pedig automatikusan visszaugrik a javítandó mezőbe.
- **Tanulság / érintett alapelv:** Ne csak színnel közölj információt; az űrlaphibákat mindig szövegesen, programozottan társítva és a helyes kitöltésre vonatkozó javaslattal együtt kell közölni.

## 7. hiba: Elégtelen színkontraszt és állapotüzenet bejelentésének hiánya
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Sikeres mentés után a megjelenő „Mentve!” szöveg világoszöld színe (#2ecc71) a fehér háttéren alig olvasható a gyengénlátóknak. Képernyőolvasóval (NVDA) tesztelve a dinamikusan megjelenő szöveget a felolvasó nem mondja be, így a látássérült felhasználó nem kap visszajelzést a mentés sikerességéről.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** A szöveg és a háttér közötti kontrasztarány 1.87:1 volt (a minimális 4.5:1 helyett), továbbá az üzenetet megjelenítő bekezdés nem rendelkezett ARIA élő régióval (`aria-live="polite"` / `role="status"`). Megsértett kritériumok:
  - **WCAG 2.2 - 1.4.3 Contrast (Minimum) (AA szint)**
  - **WCAG 2.2 - 4.1.3 Status Messages (AA szint)**
- **Javítás (commit link, előtte/utána kód):**
    https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y/commit/7a69939c9f1ff3c57e685b202059499c596cd818
  - Előtte: `<p id="ok"></p>` és CSS: `#ok { color: #2ecc71; font-weight: bold; }`
  - Utána: `<p id="ok" role="status" aria-live="polite"></p>` és CSS: `#ok { color: #145a32; font-weight: bold; }` (4.5:1 kontrasztarányt meghaladó sötétzöld szín).
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Chrome DevTools színkontraszt-mérővel ellenőrizve: a szín megfelelt a WCAG AA szintnek. NVDA képernyőolvasóval ellenőrizve a Mentés gomb megnyomásakor a felolvasó a fókusz elmozdítása nélkül azonnal bemondja a „Mentve!” üzenetet.

## Összegzés
## Mit tanultam: 
A webes akadálymentesség (a11y) nem csupán vizuális kiegészítés, hanem alapvető szoftverminőségi elvárás. A megfelelő szemantikus HTML elemek (gombok, címkék, jelölőnégyzetek, logikus címsorok) beépítetten biztosítják a billentyűzet- és képernyőolvasó-támogatást, minimalizálva a bonyolult egyedi szkriptek szükségességét.
## Melyik alapelv sérült a leggyakrabban: 
** A **Robusztus (Robust)** és a **Kezelhető (Operable)** alapelvek, különösen az ARIA első szabályának figyelmen kívül hagyása (natív HTML elemek helyett `<div>`-ek használata) és a billentyűzet-navigáció sérülése a pozitív `tabindex` miatt.
