> 🇬🇧 [English version of this document](README.md)

# Pexesooo

Samostatná **pexesová hra** běžící v prohlížeči (v angličtině *Concentration* / *Pairs*), doprovodný **generátor sad** a **tiskárna**. Vytvořte si vlastní sady karet ze dvou složek obrázků, získejte jediný přenositelný soubor JSON a hrajte na obrazovce — nebo karty vytiskněte, vystřihněte a hrajte na papíře. Žádný server, žádný build, žádná instalace.

> **Stav:** v1.9.0 · tři statické soubory HTML · běží offline z `file://`

---

## Co je uvnitř

| Soubor | Co to je |
|---|---|
| `PexesoooGame.html` | Hra. Načte soubor sady a hraje ji pro 1–6 hráčů. |
| `PexesoooGenerator.html` | Nástroj pro tvorbu sad. Ze dvou složek obrázků vytvoří jeden soubor `.json`. |
| `PexesoooPrinter.html` | Tiskárna. Ze sady vytvoří PDF připravené k tisku a vystřižení. |
| `DESIGN.md` | Sdílený vizuální jazyk (návrhové tokeny, typografie, komponenty). |
| `README.md` | Tento soubor (anglicky). |

Všechny tři aplikace jsou jednotlivé soubory HTML bez závislostí. Stačí je otevřít v prohlížeči.

---

## Vlastnosti

- **Vytvářejte vlastní sady** z vlastních obrázků — vlajky, zvířata, slovíčka, rodinné fotky, cokoli.
- **Shodné (1:1) dvojice:** nasměrujte oba výběry složek na **stejnou složku** a vytvoříte klasické pexeso, kde obě karty dvojice ukazují *stejný* obrázek.
- **Dvojice jen z obrázků:** podporovány jsou soubory PNG, JPG a JPEG.
- **Dvě výstupní rozlišení:** vytvořte sady **online** (200×200, menší soubory) nebo **pro tisk** (600×600, kvalitnější obrázky pro tisk). Volba platí pro obrázky karet i pro rub.
- **Samostatné sady:** obrázky jsou vloženy přímo do JSON, takže sada je jediný soubor, který můžete sdílet, poslat e-mailem nebo uložit do repozitáře.
- **Tisk na papír:** libovolnou sadu převeďte na PDF a vystřihněte fyzické karty — **jednostranně**, **přeložením**, nebo **oboustranně (duplex)**, ve zvolené velikosti karet.
- **Náhled dvojic** před hraním — zobrazte si všechny dvojice sady najednou.
- **1–6 hráčů** s vlastními jmény a přehledným ukazatelem tahu.
- **Spravedlivé pořadí na tahu:** každé *Hrát znovu* posune, kdo začíná; *Spustit hru* pořadí resetuje.
- **Volitelná časomíra na tah** s ubývajícím pruhem; po vypršení se tah automaticky předá dál.
- **Skóre během hry, nebo skryté** — zobrazujte skóre během hry, nebo (výchozí nastavení) ho odhalte až ve výsledcích.
- **Celkový čas** a **počet tahů jednotlivých hráčů** se zobrazí ve výsledcích, pro hru jednoho i více hráčů.
- **Vlastní rub karet** (volitelně) — jinak se jako rub použije název sady.
- **Čeština / angličtina** (ve výchozím nastavení čeština), kdykoli přepínatelné.
- **Soukromí už z principu:** vše běží lokálně ve vašem prohlížeči. Žádné nahrávání, žádné sledování, žádné síťové požadavky (kromě načtení webových fontů).

---

## Rychlý start

### 1. Vytvoření sady (`PexesoooGenerator.html`)

1. Připravte si **dvě složky** s obrázky. Dvojice vznikne, když soubor ve složce A a soubor ve složce B mají **stejný název** (bez ohledu na velikost písmen, přípona se ignoruje). Například `france.png` ve složce A se spáruje s `france.jpg` ve složce B.

   **Tip — shodné dvojice:** chcete-li klasické pexeso, kde jsou obě strany dvojice *stejný* obrázek, zvolte pro Složku A i Složku B **stejnou složku**. Každý soubor se pak spáruje sám se sebou (1:1).
2. Otevřete `PexesoooGenerator.html`.
3. Zadejte **název sady** a **popis** (až 256 znaků; popis je dvouřádkové pole).
4. Zvolte **Složku A** a **Složku B** (otevře se dialog pro výběr složky — vybíráte složku, nikoli jednotlivé soubory). Po výběru složky ukazuje výběr **název složky a počet nalezených obrázků** v závorce.
5. *(Volitelné pro online sady, povinné pro sady určené k tisku)* zvolte obrázek **rubu karet**.
6. Zvolte **rozlišení**:
   - **200×200 — jen online** — menší soubory, ideální pro hraní na obrazovce.
   - **600×600 — pro tisk** — větší, kvalitnější obrázky pro tisk; **rub karet je povinný**.

   Rozlišení platí pro obrázky karet i pro rub.
7. Klikněte na **Připravit**. Zkontrolujte nalezené dvojice — neúplné nebo nečitelné dvojice jsou označeny a vyřazeny; odškrtněte ty, které nechcete.
8. Klikněte na **Generovat JSON**. Získáte soubor s názvem `Pexesooo__<Nazev_Sady>__200px.json` nebo `Pexesooo__<Nazev_Sady>__600px.json` (přípona zaznamenává rozlišení).

**Přijímané soubory:** `PNG`, `JPG`, `JPEG`.
**Zpracování:** obrázky karet i rub jsou oříznuty na střed na zvolené rozlišení — **200×200** (online) nebo **600×600** (pro tisk). Kvalita JPEG je vyšší u sad určených k tisku.

### 2. Hraní (`PexesoooGame.html`)

1. Otevřete `PexesoooGame.html`.
2. V sekci **Nastavení** načtěte sadu jedním ze dvou způsobů:
   - **Soubor** — zvolte přímo jeden soubor sady `.json`. Náhled ukáže název, popis, jeden řádek s **počtem dvojic, datem vytvoření a rozlišením**, ukázkové dvojice a rub karet.
   - **Složka** — zvolte složku; hra ji prohledá (jen nejvyšší úroveň, bez podsložek) a vypíše všechny nalezené platné soubory `.json`. Kliknutím na řádek sadu vyberete.
3. *(Volitelně)* klikněte na **Náhled** a zobrazte si všechny dvojice sady — obě karty každé dvojice vedle sebe, s názvem souboru dvojice pod nimi — ještě před začátkem hry.
4. Zvolte počet hráčů, zadejte jména a nastavte časomíru / možnosti skóre.
5. Klikněte na **Spustit hru**.

**Pravidla tahu**

- Otočte až dvě karty.
- **Shoda** → dvojici si necháte a **hrajete znovu**.
- **Neshoda** → karty zůstanou lícem nahoru, aby je všichni viděli; **klepnutím na své jméno** ukončíte tah (karty se otočí zpět a tah přejde dál). Je-li zapnutá časomíra a vyprší, tah skončí automaticky.
- Získané karty nechávají **pevné prázdné místo** — pozice se během hry nepřeskupují, takže na paměti stále záleží.
- Hra končí, když je hrací plocha prázdná. Sekce **Výsledky** ukáže skóre každého hráče, počet jeho tahů, získané dvojice a celkový čas.

Ve výsledcích můžete zvolit **Hrát znovu** (okamžitě začne nová hra se stejným nastavením a posunutým začínajícím hráčem) nebo **Jiná hra** (návrat do nastavení).

### 3. Tisk sady (`PexesoooPrinter.html`)

1. Otevřete `PexesoooPrinter.html`.
2. Načtěte sadu úplně stejně jako ve hře — přes **Soubor** (jeden `.json`) nebo přes **Složku** (tiskárna vypíše všechny platné sady nalezené na nejvyšší úrovni). U každé sady se zobrazí úplný náhled; kliknutím ji vyberete.
3. *(Volitelně)* otevřete sekci **Náhled** a zobrazte si všechny dvojice sady — obě karty každé dvojice vedle sebe, s názvem souboru dvojice pod nimi — stejné zobrazení jako Náhled ve hře.
4. V sekci **Možnosti tisku** zvolte:
   - **Co tisknout** — jeden ze čtyř níže uvedených scénářů.
   - **Velikost karty** — **40 / 45 / 50 / 60 mm** (čtverec; výchozí **50 mm**).
   - **Velikost stránky** — **A4** nebo **US Letter**.
   - *(pouze duplex)* **Posun rubu** — volitelný posun **X / Y** v milimetrech pro korekci zarovnání líce a rubu vaší tiskárny (viz poznámka k duplexu níže).
5. Klikněte na **Generovat PDF**. Soubor se stáhne lokálně, s názvem `Pexesooo__<NazevSady>__<Rozliseni>px__<Velikost>mm__<VelikostStranky>__<Scenar>[__x<X>_y<Y>mm].pdf` (nenulový posun rubu přidá segment `__x<X>_y<Y>mm`).

**Scénáře tisku**

- **Jen obrázky karet** — vytiskne se každý líc karty ve zvolené velikosti, s rohovými značkami pro střih. Ruby karet se ignorují.
- **Líce + ruby karet** — každá karta se vytiskne jako **proužek k přeložení**: líc plus sdílený rub sady (otočený tak, aby byl po přeložení správně čitelný). Vytiskněte, vystřihněte každý proužek, přeložte podél čárkované čáry a slepte poloviny zády k sobě.
- **Duplex — delší hrana** — líce a ruby na střídajících se stranách pro **oboustranný tisk s vazbou po delší hraně**; ruby jsou rozmístěny tak, aby po otočení seděly za líci.
- **Duplex — kratší hrana** — totéž, uspořádané pro **vazbu po kratší hraně**.

**Poznámky**

- Rub karet je smyslem scénářů „líce + ruby“ a duplexu, takže sadu **bez rubu** lze vytisknout jen scénářem **Jen obrázky karet** — zbývající tři jsou zakázané, dokud sada nemá rub.
- Obrázky menší než **600 × 600 px** se vytisknou v pořádku, ale mohou být měkké; aplikace zobrazí upozornění. Pro nejlepší kvalitu tisku vytvořte v generátoru sady **pro tisk (600×600)**.
- U duplexu **nejdřív vytiskněte jednu zkušební stranu** a zkontrolujte, že ruby sedí za líci. Ověřte, že nastavení duplexu vaší tiskárny odpovídá scénáři (vazba po **delší** vs **kratší** hraně). Pokud jsou ruby soustavně posunuté, zadejte odchylku do pole **Posun rubu** (**X** = doprava, **Y** = nahoru, v mm) a vytiskněte znovu.

PDF se sestaví celé ve vašem prohlížeči — bez knihovny, bez nahrávání — s obrázky karet vloženými přímo.

---

## Parametry spuštění (URL)

Všechny tři aplikace čtou volitelné parametry z dotazovací části URL stránky, takže je můžete přednastavit z odkazu, záložky nebo zástupce na ploše. Fungují i z `file://` — mějte však na paměti, že je nelze přidat dvojklikem na soubor; napište nebo vložte celou URL, nebo si uložte záložku či zástupce, které je obsahují.

**Názvy i hodnoty parametrů nerozlišují velikost písmen** (zde jsou psány malými písmeny). Neznámé klíče a nerozpoznané hodnoty se ignorují a ponechají běžné výchozí nastavení. Jazykové přepínací tlačítko i ovládací prvky v aplikaci fungují poté normálně — parametry nastavují pouze *počáteční* stav.

| Parametr | Aplikace | Hodnoty | Účinek |
|---|---|---|---|
| `lang` | všechny tři | `en`, `cs` | Počáteční jazyk rozhraní. Neplatný nebo chybějící → čeština (výchozí). |
| `player` | hra | libovolný text, **lze opakovat** | Předvyplní jména hráčů. Klíč zopakujte jednou za každého hráče; počet výskytů určí počet hráčů (nejvýše 6). Prázdná hodnota ponechá dané pole prázdné (Spustit hru zůstane zakázané, dokud ho nevyplníte). |
| `timer` | hra | `on`, `off` | Zapne nebo vypne časomíru na tah. `on` použije výchozí počet sekund. |
| `score` | hra | `on`, `off` | Zobrazuje skóre během hry (on), nebo ho odhalí až ve výsledcích (off). |

Hodnoty je v případě potřeby nutné zakódovat pro URL — mezery jako `%20` nebo `+` a písmena s diakritikou jako jejich procentně zakódované UTF-8 (například `Bo%C5%BEena` → *Božena*).

**Příklady**

- `PexesoooGame.html?lang=en` — otevře hru v angličtině.
- `PexesoooGame.html?player=Adam&player=Bozena&player=Cecil` — tři hráči se jmény Adam, Bozena a Cecil.
- `PexesoooGame.html?lang=en&player=Adam&player=Bozena&timer=on&score=on` — angličtina, dva hráči, časomíra zapnutá, skóre zobrazené během hry.
- `PexesoooGenerator.html?lang=en` a `PexesoooPrinter.html?lang=en` — otevře generátor nebo tiskárnu v angličtině.

---

## Formát souboru sady

Sada je jediný soubor JSON. Obrázky jsou uloženy inline jako base64 data URI, takže soubor je plně přenositelný. Všechny tři aplikace čtou a zapisují tentýž formát.

```json
{
  "_source": "https://github.com/KarlM0/Pexesooo/",
  "format": "pexesooo",
  "version": 1,
  "generator": "PexesoooGenerator/1.9.0",
  "createdAt": "2026-06-06T12:00:00Z",
  "name": "World Flags",
  "description": "Match each flag to its country name",
  "thumb": { "size": 200, "fit": "cover", "type": "image/jpeg" },
  "pairCount": 24,
  "back": { "kind": "image", "src": "data:image/jpeg;base64,…" },
  "pairs": [
    {
      "id": "france",
      "a": { "kind": "image", "src": "data:image/jpeg;base64,…" },
      "b": { "kind": "image", "src": "data:image/jpeg;base64,…" }
    }
  ]
}
```

Poznámky:

- Každá dvojice vytváří **dvě karty**; shoda jsou dvě karty se stejným `id` dvojice.
- Obě strany karty jsou `{ "kind": "image", "src": "data:image/…" }`.
- `thumb.size` zaznamenává rozlišení, ve kterém byly obrázky vytvořeny — **200** (online) nebo **600** (pro tisk).
- `back` je ve formátu **volitelný**. Chybí-li (nebo je nečitelný), hra vykreslí jako rub karet **název sady**. (Pozn.: generátor rub *vyžaduje* při tvorbě sady pro tisk a tiskárna rub *vyžaduje* pro scénáře „líce + ruby“ a duplex.)
- `_source` udává URL projektu a je jen informativní; aplikace ho ignorují.
- `format` musí být `"pexesooo"`. Sady vytvořené staršími verzemi generátoru (které používaly `"pexeso"`) nejsou kompatibilní.

---

## Vzhled

Všechny tři aplikace sledují sdílený tmavý vizuální jazyk definovaný v `DESIGN.md` (vlastní vlastnosti CSS, tři písma — Fraunces / Manrope / JetBrains Mono — a malá knihovna komponent). Několik prvků jsou záměrná, zdokumentovaná rozšíření tohoto systému:

- **Otočení karty** — zdrženlivé 0,3s plynulé `rotateY`, uvedená výjimka ze základního pravidla pohybu „jediná vstupní animace“.
- **Dvojice výběru souboru / složky, seznam sad, přepínače rozlišení / voleb, pruh hráčů na tahu, mřížka náhledu dvojic, pole posunu rubu, odkaz na repozitář a indikátor načítání** — odvozené ze základních tokenů tam, kde knihovna komponent nemá existující vzor.
- **Tištěný výstup PDF** (rozvržení karet, rohové značky pro střih, čáry pro přeložení) je papír, nikoli rozhraní aplikace, takže je záměrně mimo systém tokenů.

Tyto prvky jsou v kódu označeny jako kandidáti na zařazení do `DESIGN.md`.

---

## Zabezpečení

Všechny tři aplikace jsou navrženy tak, aby běžely výhradně v prohlížeči bez zapojení serveru. Několik opatření omezuje dopad škodlivého souboru sady:

- **Content Security Policy** — značka `<meta>` s CSP blokuje veškeré odchozí síťové připojení stránky, omezuje obrázky na URI `data:` a `blob:` a fonty na Google Fonts.
- **Bezpečné vykreslování obrázků** — obrázky karet se vždy nastavují přes vlastnost DOM (`img.src = …`), nikdy se nevkládají do `innerHTML`, což brání útokům vkládáním atributů z podvržených souborů sad.
- **Omezení vstupu** — soubory sad s více než 500 dvojicemi nebo s jednotlivými zdroji obrázků většími než 3 MB jsou při validaci odmítnuty.

Načítejte pouze soubory sad z důvěryhodných zdrojů.

---

## Podpora prohlížečů a omezení

- Testováno jako standardní HTML/CSS/JS; funguje v aktuálním Chromiu, Firefoxu a Safari.
- Výběr složky v aplikacích používá vstup `webkitdirectory`; podpora je široká, ale přesné chování dialogu pro výběr složky se liší podle prohlížeče/OS.
- Obrázky **HEIC** (běžné na iPhonech) prohlížeče **nepodporují** — před tvorbou sady je převeďte na JPG/PNG.
- Orientace obrázku: generátor při překódování aplikuje orientaci z EXIF, ale ověřte to na otočené fotce z telefonu ve svém prohlížeči.
- **Tisk:** přesné chování duplexu závisí na vaší tiskárně/ovladači; pomocí **zkušební strany** a **Posunu rubu** dolaďte zarovnání líce a rubu a sladťte vazbu duplexu (delší vs kratší hrana) se zvoleným scénářem.
- **Žádné ukládání** — není zde uložení/pokračování; nastavení, průběh hry i hodnota posunu rubu v tiskárně existují jen v aktuální záložce.
- **Offline:** aplikace fungují z `file://`; bez internetu se webové fonty nahradí systémovými (rozvržení a chování to neovlivní).
- Výběr složky ve hře a v tiskárně prohledává jen **nejvyšší úroveň** zvolené složky; soubory JSON v podsložkách se ignorují.

---

## Soukromí

Vše se děje ve vašem prohlížeči. Vaše obrázky nikdy neopustí vaše zařízení; aplikace nevolají žádné API a nic neukládají vzdáleně. Také tiskárna sestavuje své PDF lokálně — k tisku se nic nenahrává.

---

## Verzování

- Verze aplikace je zobrazena v záhlaví každé aplikace (`PexesoooGame v1.9.0`, `PexesoooGenerator v1.9.0`, `PexesoooPrinter v1.9.0`).
- Soubor sady zaznamenává verzi generátoru v poli `generator` a verzi schématu v poli `version`.
