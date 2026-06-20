> 🇬🇧 [English version of this document](README.md)

# Pexesooo

Samostatná, prohlížečová **hra pexeso** (v angličtině známá jako *Concentration* nebo *Pairs*) a doprovodný **generátor sad**. Vytvořte vlastní sadu karet ze dvou složek obrázků (nebo krátkých textů), získejte jeden přenosný soubor JSON a hrajte — bez serveru, bez sestavení, bez instalace.

> **Stav:** v1.2.0 · dva statické soubory HTML · funguje offline z `file://`

---

## Co je součástí

| Soubor | Popis |
|---|---|
| `Pexesooo.html` | Hra. Načte soubor sady a spustí hru pro 1–6 hráčů. |
| `PexesoooGenerator.html` | Tvůrce sad. Převede dvě složky souborů na jeden soubor `.json`. |
| `DESIGN.md` | Sdílený vizuální jazyk (design tokeny, typografie, komponenty). |
| `README.md` | Anglická verze tohoto dokumentu. |

Oba soubory jsou samostatné soubory HTML bez závislostí. Stačí je otevřít v prohlížeči.

---

## Funkce

- **Vytvářejte vlastní sady** ze svých obrázků — vlajky, zvířata, slovní zásobu, rodinné fotky, cokoliv.
- **Smíšené typy karet:** párování obrázek↔obrázek, obrázek↔text nebo text↔text. Textové karty pocházejí ze souborů `.txt` (maximálně 64 znaků).
- **Samostatné sady:** obrázky jsou vloženy přímo do souboru JSON, takže sada tvoří jeden soubor, který lze sdílet, poslat e-mailem nebo uložit do repozitáře.
- **1–6 hráčů** s vlastními jmény a přehledným indikátorem tahu.
- **Spravedlivé pořadí:** každé *Hrát znovu* posune startujícího hráče o jednu pozici dál; *Spustit hru* pořadí resetuje.
- **Volitelná časomíra na tah** s odpočítávací lištou; po vypršení se tah předá automaticky.
- **Živé nebo skryté skóre** — zobrazujte skóre průběžně, nebo (výchozí) odhalte jej až ve výsledcích.
- **Celkový čas** a **počet tahů každého hráče** jsou zobrazeny ve výsledcích, v sólo i vícehráčském módu.
- **Vlastní rub karet** (volitelně) — nebo se jako rub použije název sady.
- **Čeština / angličtina** — přepínatelné kdykoliv.
- **Soukromé ze své podstaty:** vše probíhá lokálně v prohlížeči. Žádné nahrávání, žádné sledování, žádná síťová komunikace (kromě načítání webových fontů).

---

## Rychlý start

### 1. Vytvořte sadu (`PexesoooGenerator.html`)

1. Připravte **dvě složky** souborů. Pár vznikne, když soubor ve složce A a soubor ve složce B sdílejí **stejný název** (bez ohledu na velikost písmen a příponu). Například `france.png` ve složce A tvoří pár s `france.txt` ve složce B.
2. Otevřete `PexesoooGenerator.html`.
3. Zadejte **název sady** a **popis**.
4. Vyberte **Složku A** a **Složku B** (otevře se dialog pro výběr složky — vybíráte celou složku, ne jednotlivé soubory).
5. *(Volitelně)* vyberte **obrázek rubu karet**.
6. Klikněte na **Odeslat**. Zkontrolujte nalezené páry — neúplné nebo nečitelné páry jsou označeny a vyřazeny; odškrtněte ty, které nechcete zahrnout.
7. Klikněte na **Vytvořit JSON**. Získáte soubor pojmenovaný `Pexesooo__<Název_sady>.json`.

**Podporované soubory:** `PNG`, `JPG`, `JPEG` a `TXT`.
**Zpracování:** obrázky karet jsou ořezány na střed na 200×200; volitelný rub na 400×400; text je oříznut a omezen na 64 znaků.

### 2. Hrajte (`Pexesooo.html`)

1. Otevřete `Pexesooo.html`.
2. V sekci **Nastavení** vyberte soubor sady (soubor `.json` z generátoru). Náhled zobrazí název, popis, ukázkové páry a rub karet.
3. Zvolte počet hráčů, zadejte jména a nastavte časomíru a zobrazení skóre.
4. Klikněte na **Spustit hru**.

**Pravidla tahu**

- Otočte až dvě karty.
- **Shoda** → získáte pár a **hrajete znovu**.
- **Neshoda** → karty zůstanou otočeny lícem nahoru, aby je všichni viděli; **klepnutím na své jméno** ukončíte tah (karty se otočí zpět a hra přejde na dalšího hráče). Pokud je zapnutá časomíra a vyprší čas, tah se ukončí automaticky.
- Získané karty zanechají **pevnou prázdnou mezeru** — pozice se v průběhu hry nemění, takže paměť stále hraje roli.
- Hra končí, když je hrací plocha vyčištěna. V sekci **Výsledky** vidíte skóre každého hráče, počet tahů, získané páry a celkový čas.

Ve výsledcích můžete zvolit **Hrát znovu** (okamžitě restartuje se stejným nastavením a posune startujícího hráče) nebo **Jiná hra** (vrátí se do nastavení).

---

## Formát souboru sady

Sada je jeden soubor JSON. Obrázky jsou uloženy přímo jako base64 data URI, takže soubor je plně přenosný.

```json
{
  "format": "pexeso",
  "version": 1,
  "generator": "PexesoooGenerator/1.1.0",
  "createdAt": "2026-06-06T12:00:00Z",
  "name": "Vlajky světa",
  "description": "Přiřaďte každou vlajku k názvu její země",
  "thumb": { "size": 200, "fit": "cover", "type": "image/jpeg" },
  "pairCount": 24,
  "back": { "kind": "image", "src": "data:image/jpeg;base64,…" },
  "pairs": [
    {
      "id": "france",
      "a": { "kind": "image", "src": "data:image/jpeg;base64,…" },
      "b": { "kind": "text",  "text": "Francie" }
    }
  ]
}
```

Poznámky:

- Každý pár vytvoří **dvě karty**; shoda jsou dvě karty se stejným `id` páru.
- Strana karty je buď `{ "kind": "image", "src": "data:image/…" }` nebo `{ "kind": "text", "text": "…" }`.
- `back` je **volitelný**. Pokud chybí (nebo je nečitelný), hra zobrazí **název sady** jako rub karet. Starší sady bez pole `back` se načtou bez problémů.

---

## Design

Oba soubory HTML sdílejí společný vizuální jazyk definovaný v `DESIGN.md` (CSS custom properties, tři písma — Fraunces / Manrope / JetBrains Mono — a malá knihovna komponent). Některé prvky jsou záměrná, zdokumentovaná rozšíření tohoto systému:

- **Překlápění karet** — střídmý přechod `rotateY` trvající 0,3 s, výslovná výjimka ze základního pravidla jedné vstupní animace.
- **Výběr souboru/složky, mřížka kontroly, pruh tahů hráčů a náhled rubu** — odvozeno z základních tokenů tam, kde knihovna komponent nemá existující vzor.

Tyto prvky jsou v zdrojovém kódu označeny jako kandidáti na přidání do `DESIGN.md`.

---

## Podpora prohlížečů a omezení

- Testováno jako standardní HTML/CSS/JS; funguje v aktuálních verzích Chromium, Firefox a Safari.
- Výběr složky v generátoru využívá vstup `webkitdirectory`; podpora je široká, ale přesné chování dialogu se liší podle prohlížeče a operačního systému.
- **HEIC** obrázky (běžné na iPhonech) prohlížeče **nepodporují** — nejprve převeďte na JPG/PNG.
- Orientace obrázků: generátor aplikuje orientaci EXIF při překódování, ale ověřte funkčnost s otočenou fotografií z telefonu ve vašem prohlížeči.
- **Bez ukládání** — hra nemá funkci uložení a obnovení; nastavení a průběh hry existují pouze v aktuální záložce.
- **Offline:** oba soubory fungují z `file://`; bez připojení k internetu se webové fonty vrátí na systémová písma (rozvržení ani chování nejsou ovlivněny).
- Načtení sady vyžaduje ruční výběr souboru `.json` — statická stránka nemůže automaticky zobrazit seznam sad ze složky.

---

## Soukromí

Vše probíhá ve vašem prohlížeči. Vaše obrázky a texty nikdy neopustí vaše zařízení; soubory neprovádějí žádná API volání a nic neukládají vzdáleně.

---

## Verzování

- Verze aplikace je zobrazena v záhlaví každého souboru (`Pexesooo v1.2.0`, `PexesoooGenerator v1.2.0`).
- Soubor sady zaznamenává verzi generátoru v poli `generator` a verzi schématu v poli `version`.

---

## Licence

_Zvolte licenci pro svůj repozitář (např. [MIT](https://choosealicense.com/licenses/mit/)) a přidejte soubor `LICENSE`. Tuto sekci příslušně upravte._
