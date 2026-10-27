# English Trainer – aplikace pro výuku angličtiny

Interaktivní webová aplikace pro Čechy, kteří se učí anglicky. Uživatel zadá svou
pokročilost (A1–C1 ručně, nebo zařazovacím testem) a procvičuje slovíčka a gramatiku.

## Pravidla projektu
- **Jediný soubor aplikace je `index.html`** (požadavek GitHub Pages). Nepřejmenovávat.
- Čistý HTML + CSS + JS, bez build kroku a bez externích závislostí. Vše musí fungovat
  po otevření souboru v prohlížeči i na GitHub Pages.
- Rozhraní a vysvětlivky jsou **česky**, učený obsah **anglicky**.
- Kód dál rozšiřovat do `index.html`; pokud soubor příliš naroste, lze vytáhnout data do
  `data.js` / styly do `style.css` (relativní cesty, žádný bundler) a upravit tento soubor.

## Architektura (uvnitř `index.html`)
1. **DATA** – `LEVELS`, `VOCAB[level]` (`[en, cs, příklad]`), `GRAMMAR[level]`
   (`{q, options, a, tip}`). Prvních 2 otázky v `GRAMMAR[level]` slouží i pro zařazovací test.
2. **STATE** – objekt `S` uložený v `localStorage` pod klíčem `english-trainer-v1`:
   `level`, `xp`, `streak`, `lastDay`, `box` (Leitnerova krabička slovíček 0–5), `stats`.
   Při změně struktury stavu zvyš verzi klíče nebo zajisti zpětnou kompatibilitu.
3. **HELPERS** – `el()` (tvorba DOM bez innerHTML), `mount()`, `shuffle()`, `speak()` (Web Speech API).
4. **SCREENS** – `home`, `chooseLevel`, `flashcards`, `quizVocab`, `typing`, `quizGrammar`,
   `placementTest`; společný engine `session({title, items, onAnswer, after})`.

## Jak přidat obsah / funkci
- Nová slovíčka: přidej řádek do `VOCAB[úroveň]`. Nová gramatická otázka: objekt do `GRAMMAR[úroveň]`
  (`a` musí přesně odpovídat jedné z `options`; přidej `tip` česky).
- Nový režim: funkce, která postaví `items` ({prompt, sub, render(done)}) a zavolá `session(...)`,
  plus dlaždice v poli `modes` ve funkci `home()`.
- Nová úroveň: přidat do `LEVELS`, `VOCAB`, `GRAMMAR` (zařazovací test i přehled pokroku se přizpůsobí).

## Nápady na další rozvoj
- Poslech (diktát přes `speechSynthesis`), čtení krátkých textů, nepravidelná slovesa.
- Více obsahu (cíl 40+ slov a 15+ gramatických otázek na úroveň), tematické sady.
- Opravdové rozestupy opakování (datum příštího opakování místo jen krabičky).
- Export/import pokroku, tmavý/světlý přepínač, PWA (offline).

## Testování
Otevři `index.html` v prohlížeči; zkontroluj konzoli bez chyb, projdi všechny režimy,
zařazovací test a mobilní šířku. Po změně dat ověř, že každá `GRAMMAR.a` je v `options`.
