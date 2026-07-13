# ECS · Kalkulator ponuda za prevoz

Kalkulator za brzo pravljenje ponuda za limo/transfer usluge **Executive Car Service**,
napravljen na osnovu zvaničnog cenovnika (`cenovnik_2025.xlsx`).

## Šta radi

- **Kalkulator** — izbor vozila, tipa usluge, sati/kilometraže i automatski obračun cene.
  - **Satni najam** (1–12 h) sa uključenom kilometražom po paketu.
  - **Dnevni najam** (do 10 h) sa proširenom kilometražom (250–700 km).
  - **Dodatni kilometri** van paketa — obračun po ceni/km za dato vozilo.
  - **Popust (%)** — opciono umanjenje ukupnog iznosa.
  - **Valuta** — prikaz u **EUR** ili **RSD** (obe cene iz cenovnika).
- **Ponuda** — pregled u formi dokumenta sa ECS brendingom, podacima o klijentu,
  relaciji i napomeni. Dugmad **Štampaj / PDF** (štampa samo ponudu) i **Kopiraj tekst**.
- **Cenovnik** — kompletna tabela svih vozila (satni i dnevni najam), u izabranoj valuti.

## Pokretanje

Nije potreban server — dovoljno je otvoriti `index.html` u pregledaču
(dvoklik na fajl). Za objavu na sajtu, postaviti `index.html` i `pricing.js`
u isti direktorijum.

## Fajlovi

| Fajl | Opis |
|------|------|
| `index.html` | Kompletna aplikacija (UI + logika). |
| `pricing.js` | Podaci o cenama, generisani iz Excel cenovnika. |

## Uređivanje cena (u aplikaciji)

U tabu **Cenovnik** kliknite **✎ Uredi cene**. Otvara se režim za uređivanje:

- **Izmena cena** — sve ćelije postaju polja za unos; menjate cene direktno.
  Uređujete cene u trenutno izabranoj valuti (EUR ili RSD) — prebacite valutu
  gore desno da uredite drugu. Prazna polja (—) možete popuniti da dodate paket.
- **Preimenovanje** — promenite naziv vozila u polju levo.
- **＋ Dodaj vozilo** — novo vozilo sa svim tarifama (popunjavate cene).
- **⎘ Dupliraj** / **✕ Obriši** — po vozilu.
- **⬇ Izvezi pricing.js** — preuzima ažuriran `pricing.js` koji možete zameniti
  u projektu (trajno čuvanje / objava na sajtu).
- **⬇ Izvezi JSON** / **⬆ Uvezi JSON** — prenos cenovnika između uređaja.
- **↺ Fabričke cene** — vraća originalne cene iz `pricing.js`.

Izmene se automatski čuvaju u pregledaču (localStorage), pa ostaju i posle
osvežavanja stranice. Da bi izmene bile trajne i vidljive svima, izvezite
`pricing.js` i zamenite fajl u projektu.

## Struktura podataka

Cene su u `pricing.js` (`window.ECS_PRICING`). Za svako vozilo:

- `hourly[]` — po satu: `{ h, km, eur, rsd }`
- `day[]` — dnevni paketi: `{ km, eur, rsd }`
- `perKmEur` / `perKmRsd` — cena dodatnog kilometra

Kada dobijete novi Excel cenovnik, dovoljno je regenerisati `pricing.js`
(vrednosti su preslikane 1:1 iz kolona `2024` sheet-a) ili uneti izmene
direktno u aplikaciji i izvezti `pricing.js`.

## Logo

Trenutni logo je **verna vektorska rekonstrukcija** ECS znaka (šofer, četiri zlatne
zvezdice, „ECS" natpis) — originalni fajl nije bio dostupan u okruženju. Za tačan
originalni logo, pošaljite fajl (PNG/SVG) i biće ugrađen 1:1 (kao `data:` URI,
bez spoljnih zavisnosti). Logo se pojavljuje na dva mesta u `index.html`
(zaglavlje aplikacije i zaglavlje ponude).

## Napomena o godini

Fajl je nazvan `cenovnik_2025.xlsx`, ali aktivni radni list u Excel-u nosi naslov
**„CENOVNIK 2024"** — kalkulator koristi taj (najnoviji) list. Ako je 2025. cenovnik
drugačiji, prosledite ažuriran Excel pa ćemo osvežiti `pricing.js`.
