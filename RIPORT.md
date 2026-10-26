# Tünetnapló, ami csak egérrel működik – hibariport

Repó: https://github.com/AndreaJulia24/umfst-bh11-tunetnaplo-a11y
Készítette: Czellecz Andrea-Julia, INFO-LM III.ev

## 1. hiba: Nem lehet nagyítani mobilon (Pinch-to-zoom letiltva)
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** Mobilon vagy a böngésző mobil emulátorában kétujjas mozdulattal (pinch-to-zoom) nem lehet nagyítani az oldalt a gyengénlátó felhasználóknak.
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** A viewport meta tagben szerepel a `maximum-scale=1, user-scalable=no` beállítás. Megsértett kritérium: **WCAG 2.2 - 1.4.4 Resize Text (AA szint)**.
- **Javítás (commit link, előtte/utána kód):**
  - Előtte: `<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">`
  - Utána: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** Chrome DevTools mobil nézetben és Lighthouse Accessibility audittal ellenőrizve: a zoom letiltására vonatkozó hiba megszűnt.
- **Tanulság / érintett alapelv:** Ne tiltsuk le a felhasználó böngészőjének alapvető kisegítő beállításait; a reszponzivitás nem mehet az akadálymentesség rovására.

## 2. hiba
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** 
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** 
- **Javítás (commit link, előtte/utána kód):** 
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** 
- **Tanulság / érintett alapelv (Az ARIA első szabálya: natív elem előbb; Ne csak színnel közölj információt):** 

## 3. hiba
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** 
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** 
- **Javítás (commit link, előtte/utána kód):** 
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** 
- **Tanulság / érintett alapelv (Az ARIA első szabálya: natív elem előbb; Ne csak színnel közölj információt):** 

## 4. hiba
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** 
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** 
- **Javítás (commit link, előtte/utána kód):** 
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** 
- **Tanulság / érintett alapelv (Az ARIA első szabálya: natív elem előbb; Ne csak színnel közölj információt):** 

## 5. hiba
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** 
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** 
- **Javítás (commit link, előtte/utána kód):** 
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** 
- **Tanulság / érintett alapelv (Az ARIA első szabálya: natív elem előbb; Ne csak színnel közölj információt):** 

## 6. hiba
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** 
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** 
- **Javítás (commit link, előtte/utána kód):** 
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** 
- **Tanulság / érintett alapelv (Az ARIA első szabálya: natív elem előbb; Ne csak színnel közölj információt):** 

## 7. hiba
- **Hiba leírása és reprodukálása (milyen segédtechnológiával, milyen lépésekkel):** 
- **Gyökérok + megsértett WCAG 2.2 sikerkritérium (szám és név):** 
- **Javítás (commit link, előtte/utána kód):** 
- **Hogyan tesztelted (axe DevTools, Lighthouse, csak billentyűzet, NVDA/VoiceOver, kontrasztmérő):** 
- **Tanulság / érintett alapelv (Az ARIA első szabálya: natív elem előbb; Ne csak színnel közölj információt):** 

## Összegzés
- Mit tanultam: 
- Melyik alapelv sérült a leggyakrabban: 
