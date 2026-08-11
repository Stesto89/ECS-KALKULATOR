# ECS · Kalkulator ponuda za prevoz

Kalkulator za brzo pravljenje ponuda za limo/transfer usluge **Executive Car Service**,
napravljen na osnovu zvaničnog cenovnika (`cenovnik_2025.xlsx`).

## Šta radi

- **Kalkulator (više stavki u jednoj ponudi)** — svaka stavka je jedno vozilo
  sa svojim tipom usluge i trajanjem; klikom na **＋ Dodaj u ponudu** dodaje se
  u listu. Jedna ponuda može imati proizvoljno mnogo vozila i različitih trajanja.
  - **Satni najam** (1–12 h) sa uključenom kilometražom po paketu.
  - **Dnevni najam** (do 10 h) sa proširenom kilometražom (250–700 km).
  - **Dodatni kilometri** van paketa — obračun po ceni/km za dato vozilo.
  - **Broj vozila** — količina istih vozila u istom terminu (množi stavku).
  - **Valuta** — prikaz u **EUR** ili **RSD** (obe cene iz cenovnika).
- **Ponuda** — dokument sa ECS brendingom: lista svih stavki (svaka se može
  ukloniti dugmetom ✕), **međuzbir**, opcioni **popust (%)** na ukupno, i
  **ukupan iznos** u obe valute. Podaci o klijentu, relaciji i napomeni.
  Dugmad **Štampaj / PDF** (štampa samo ponudu) i **Kopiraj tekst**.
- **Cenovnik** — kompletna tabela svih vozila (satni i dnevni najam), u izabranoj valuti.

## Prijava (pristup)

Aplikacija ima ekran za prijavu — pristupa se samo sa ovlašćenim nalogom
(email + lozinka). Nakon prijave stanje se pamti u pregledaču (dugme
**Odjava** je gore desno). Lozinka se ne čuva kao čist tekst, već samo njen
SHA-256 heš.

> ⚠️ **Bezbednosna napomena:** ovo je statična HTML aplikacija bez servera, pa
> je prijava „meka" zaštita — sprečava slučajan pristup, ali je nije nemoguće
> zaobići (ko ima fajl može da vidi kod). Za pravu zaštitu potreban je server
> sa autentikacijom ili hosting sa lozinkom (npr. HTTP Basic Auth / zaštićena
> zona na sajtu). Nalog/lozinku menjate u `index.html`: `AUTH_EMAIL` i
> `AUTH_HASH` (SHA-256 heš nove lozinke).

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

Koristi se **zlatni ECS logo** (`Executive Car Service gold logo.png`), ugrađen u
`index.html` kao `data:` URI (bez spoljnih zavisnosti — aplikacija ostaje jedan
samostalan fajl). Optimizovana verzija je `ecs-logo-gold.webp`. Pošto je logo
zlatan/svetao, stoji na **tamnim** pločicama — u zaglavlju aplikacije i kao
memorandum na ponudi.

Za zamenu logotipa: iskodirajte novu sliku u base64 i zamenite vrednost
konstante `ECS_LOGO` u `index.html` (po potrebi prilagodite pozadinu pločica
`.logo-plaque` / `.offer-logo`).

## Napomena o godini

Fajl je nazvan `cenovnik_2025.xlsx`, ali aktivni radni list u Excel-u nosi naslov
**„CENOVNIK 2024"** — kalkulator koristi taj (najnoviji) list. Ako je 2025. cenovnik
drugačiji, prosledite ažuriran Excel pa ćemo osvežiti `pricing.js`.
