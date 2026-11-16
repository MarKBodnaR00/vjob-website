# VJob weboldal és business portál – átadó jegyzet Claude Code-nak

Ezt a fájlt a Claude Code minden indításkor elolvassa. Utoljára frissítve: 2026-11-16 (a `csomagok.html` számlálója, a `fizetes-eredmeny.html` hibája).

## A projekt

- **Mi ez:** a VJob weboldala (getvjob.app) és a cégeknek szóló **business portál** (`business/`). A VJob egy videós
  állásmegfeleltető Android-app; tulajdonos és egyetlen fejlesztő: Márk (Infinity Gold Kft.).
- **Technológia:** statikus HTML/CSS/JS, build-lépés nélkül. Kiszolgálás: GitHub Pages, a `main` ágról (`CNAME`: getvjob.app).
- **Ágak:** a `main` **az élő oldal** – minden, ami oda kerül, azonnal kint van. Az új ÁSZF, a csomagok oldal és a portál a
  `claude/aszf-frissites` ágon van, élesben még nincs. Új munkát ebből az ágból nyitott új ágon végezz (pl. `claude/ujratervezes`).
  A `main`-re csak Márk kifejezett kérésére megy bármi.
- **Az app repója:** `MarKBodnaR00/VJob-app` (Márk gépén: `C:\Users\Acer\AndroidStudioProjects\VjOb`). A teljes kép:
  az app repójában a `docs/VJOB_ATTEKINTES.md`, a napi helyzet: az app `CLAUDE.md`-je. **Nagyobb feladat előtt ezeket olvasd el.**
  Ha a session nem látja az app mappáját, kérd meg Márkot, hogy adja hozzá (`/add-dir`), ne találgass.

## Oldalak

| Fájl | Mi ez |
|---|---|
| `index.html` | bemutató oldal (HU/EN/DE, három külön `lang-content` blokk, `setLang`) |
| `csomagok.html` | csomagok és árak (`const T` szótár, `?lang=`) |
| `business/index.html` | céges portál: Áttekintés, Csomagvásárlás, Hirdetéseim, Fizetések, Meghívás, Cégadatok (`const T`, `TABS`) |
| `terms.html`, `privacy-policy.html` | ÁSZF és adatvédelmi tájékoztató – **jogi szöveg**, csak Márk kérésére változhat |
| `delete-account.html`, `verify.html`, `reset-password.html` | fióktörlés, e-mail-megerősítés, jelszó-visszaállítás (az app e-mailjei ide mutatnak) |
| `fizetes-eredmeny.html` | a régi Barion-fizetés eredményoldala. **Ismert hiba:** a 2026-11-13-án törölt `paymentStatusPublic` függvényt hívja (élesben is); hogy mi legyen vele, Márk dönti el |
| `vasarlas.html` | régi cím, átirányít a portálra (a régi e-mailek miatt kell) |

## A portál és a szerver kapcsolata (a logika az app repójában van)

- A portál Firebase Auth-tal lép be (ugyanaz a `vjob-549b5` projekt, mint az appé), és **csak** ezeket a hívható
  szerverfüggvényeket használja (`europe-west1`): `listazCegHirdeteseit`, `arElonezet`, `createAtutalasosFizetes`, `alapitoAktival`,
  `programAdatok`, `nyilvanosSzamlalok`, `letoltCegDokumentumot`, `kuldJelszoVisszaallitasEmail`.
  A `csomagok.html` is hívja a `nyilvanosSzamlalok`-t (az alapító helyek élő számlálója).
- Ezek forrása az app repójában van (`functions/`). Mielőtt egy hívás bemenetéhez vagy válaszához nyúlsz, olvasd el ott a függvényt
  és a tesztjét. A szerverfüggvényt **ebből a repóból ne módosítsd**: ha szerverváltozás kell, írd le Márknak, és az az app repójában készül.
- Az árakat, kedvezményeket, kuponokat **mindig a szerver számolja** (`arElonezet`). A portál soha ne számoljon saját árat.
- A Firebase webes `apiKey` a kódban nyilvános, ez így normális; a védelem a szabályokban és a szerverben van.
- A fizetés, az alapító-aktiválás és a meghívás logikája üzleti szabály: a megjelenést átalakíthatod, a működést csak Márk kérésére.

## Munkamódszer (Márk kérései – ugyanazok, mint az appnál)

- Magyarul, egyszerűen, lépésenként. Márk nem profi programozó, a technikai szavakat magyarázd.
- **Látható változás előtt előbb terv vagy kép (képernyőkép, látványterv), és csak Márk jóváhagyása után építsd meg.**
- Minden nem apró feladat előtt nézd át az érintett kódot, és írd le a tervet: mit változtatsz, melyik fájlban, miért, mi a kockázata.
  Csak a jóváhagyás után kezdj bele.
- Csak azt változtasd meg, amit Márk kért. Ha közben mást is javítanál, előbb kérdezz. Ne találgass: nézd meg a kódban, vagy kérdezz.
- **`git push`, a `main` ág, és bármi, ami élő adatot vagy a szervert érinti: csak Márk engedélyével.** Élő lépés előtt írd le a pontos
  parancsot, és várd meg az engedélyt.
- Az aláíró kulcs (`C:\business\vjob-keystore.jks`) és bármilyen jelszó soha nem kerülhet a repóba, a chatbe vagy parancssorba.
- A végén röviden: mit változtattál, mihez nem nyúltál, mit ellenőriztél (képernyőképekkel), és mit kell Márknak megnéznie.

## Dizájn-szabályok

- **Nyelvek:** minden szöveg magyarul, angolul és németül is (HU/EN/DE). Új szöveg mindhárom nyelven kerüljön be, a meglévő mintát
  követve (`index.html`: a három nyelvi blokk; a portálon és a csomagoknál a `T` szótár).
- **Téma:** a portálnak világos és sötét módja van (`data-theme`, `vjob_theme`); mindkettőben jól kell kinéznie. Az alapértelmezés a sötét.
- **Jelenlegi arculat** (`index.html` `:root`): háttér `#07060E`, lila `#AA44FF` / `#7C3AED`, rózsaszín `#DD00BB`, zöld `#00C878`,
  betű: Inter. Új arculat csak jóváhagyott látványterv alapján.
- **Mobil először:** a látogatók nagy része telefonról jön. Minden oldal 390 px szélesen is legyen hibátlan (nincs vízszintes görgetés,
  nincs kilógó szöveg), és 1440 px-en is.
- **3D és animáció:** lehet látványos, de legyen visszafogott és gyors: egy fő 3D elem (pl. a nyitórészben), ne az egész oldal.
  Gyenge telefonon és `prefers-reduced-motion` beállításnál legyen sima képes tartalék. Külső könyvtár csak rögzített verzióval,
  megbízható CDN-ről (pl. cdnjs, jsdelivr), és csak ha tényleg kell.
- **Hozzáférhetőség:** elég kontraszt (szöveg és háttér legalább 4,5:1), billentyűzettel is használható gombok, `alt` a képeken.
- **Play-szabály:** a weboldalon lehet csomagot vásárolni. Az Android-appba viszont ne kerüljön webes vásárlásra terelő link vagy szöveg –
  ez nyitott Márk-döntés (lásd az app `CLAUDE.md`-jét).
- A `noindex` jelölésű oldalak (`business/`, `vasarlas.html`) ne kerüljenek a keresőkbe.

## Ellenőrzés

- **Helyben:** a repó gyökeréből `npx http-server -p 8080 -c-1`, majd `http://localhost:8080` (a portál: `/business/`).
- **Képernyőképek:** ha van Playwright-eszköz (MCP), minden látható változás után készíts képet 390 px és 1440 px szélesen,
  magyarul / angolul / németül, a portálon világos és sötét módban is, és nézd át őket. A `dizajn-ellenor` agent ebben segít.
- **Élő adat:** a portál az éles Firebase-hez csatlakozik. Fizetést, aktiválást, meghívást tesztelni csak Márk tudtával lehet
  (tesztfiókkal). Ne hozz létre valódi díjbekérőt kérés nélkül.
- **Jogi oldalak:** az ÁSZF-hez és az adatvédelmi tájékoztatóhoz csak Márk kifejezett kérésére nyúlj (ügyvédi átnézés vár rájuk).
