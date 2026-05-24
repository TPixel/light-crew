# Light Crew — Projektplan

> Komplet app til lyshold i dansk filmproduktion.  
> Single-page webapp (HTML/JS) · iPhone-first · offline-first · mørkt tema.

---

## 1. DATABASE (Master lysudstyr-wiki)

Opslagsværk over lysudstyr til film — ikke bundet til ejerskab.

### Felter per enhed

| Felt | Type | Bemærkning |
|------|------|-----------|
| Navn/model | tekst | Obligatorisk |
| Producent/mærke | tekst | |
| Kategori | valgmenu | Se kategorier nedenfor |
| Antal (ejet / lejet) | tal × 2 | |
| Watt | tal | |
| DMX-kanaler / modes | liste af modes | Hvert mode: navn + antal kanaler + kanalmap |
| Vægt (kg) | tal | |
| Farvetemperatur / CRI / TLCI | tekst | fx "5600 K · CRI 95 · TLCI 92" |
| Beam angle (min–max) | tal × 2 | Fast beam: kun ét felt |
| Strømtilslutning (ind/ud) | tekst | |
| Datablad-link | URL | |
| QR-kode | auto-genereret | Link til item i appen |
| Status | ok / service / defekt | Farvekodet |
| Foto | URL/fil | |

### Kategorier (preset)

Brugerdefinerede kategorier med farvekoder. Standard-preset:

- Bevægelige hoveder
- LED wash / pars
- LED spot / profiler
- Fresnel
- HMI
- Fluorescent / Kino Flo
- Praktiske / tungsten
- Blinders / strobes
- Atmosfære (røg/haze)
- Dimmere
- DMX-styring (borde/nodes)
- Kabler & signal
- Strøm & distro
- Rigging & ophæng
- Stativer & truss
- Forbrug & diverse

### Kategori-specifikke felter

| Kategori | Ekstra felter |
|----------|--------------|
| Stativer & truss | Fuld højde, pakket højde, footprint, spigot, nivellering, bæreevne |
| Kabler & signal | Længde, stiktype ind/ud |
| Rigging & ophæng | Max belastning, clamp-type |

### Funktionalitet

- Søgning og filtrering på alle felter
- Pre-loaded med gængse lamper fra dansk film (730+ items)
- Kun admin vedligeholder (redigér/slet/tilføj)
- Virker offline (localStorage + evt. IndexedDB)
- Import/eksport JSON + CSV

---

## 2. PRODUKTION

### Grundinfo

- Produktionsnavn
- Produktionsselskab
- Type: spillefilm / serie / reklame / kort / musikvideo
- Start- og slutdato
- Status: aktiv / afsluttet / arkiveret

### Kontakter

- Producer
- Instruktør
- DOP (Director of Photography)
- Gaffer / Belyser
- Best Boy
- Øvrige lyshold (liste)

### Praktisk

- Primær adresse (studie/kontor)
- Antal optagedage
- Budget lysudstyr (beløb)
- Lejeleverandør (firma + kontakt)

### Regler

- Flere aktive produktioner samtidig
- Hver produktion har sine egne location-lyslister
- Kan arkiveres (skjult men ikke slettet)
- Produktion er parent til studie- og location-lyslister

---

## 3. STUDIO LYSLISTE

Pakkeliste man sammensætter per produktion til studie-dage.

### Funktionalitet

- Flere studier muligt (Studie A, B, C…)
- Trækker udstyr fra master-databasen
- DMX-adressering:
  - Vælg mode per lampe
  - Tildel DMX-adresse + univers
  - Adressetæller (max 512 per univers)
  - Automatisk kollisionsdetektion
- Fixture-nummer per enhed (unik per studie)
- Placering i studiet: pipe, grid, gulv, truss (fritekst/tags)
- Patch-plan: DMX-adresse → dimmer/univers mapping
- Strømfordeling: fase/fordeling per enhed
- Lampe-pakke: standalone eller med ophæng/stativ bundlet
- Tabel-visning + visuel plan-visning (canvas)
- Markér rigged vs. skal tilføjes
- Print som pakkeliste eller patch-plan
- Udstyr kan flyttes mellem studie og location

---

## 4. LOCATION LYSLISTE

Samme funktionalitet som studie, plus location-specifikt.

### Funktionalitet

- Samme DMX, fixture-nr, lampe-pakker som studie
- Ubegrænset antal locations per produktion
- **Location Info-boks** (ADSKILT fra selve lyslisten):
  - Adresse
  - Kontaktperson + tlf
  - Strøm (amp / fase / generator)
  - Adgang (elevator, trappe, distance)
  - Parkering
  - Foto(s)
  - Kort / GPS-koordinat
- Trækker fra database + pakke-bibliotek
- Kopier studie-liste som udgangspunkt
- Automatisk pakkeliste + returliste (genereret)
- Flere set-ups per location (fx dag/nat, scene 1/scene 2)
- Link til optagedage (kalender)

---

## 5. PAKKE-BIBLIOTEK

Selvstændigt modul, uafhængigt af produktioner.

### Funktionalitet

- Gemte lister af udstyr fra databasen
- Pakker kan indeholde andre pakker (nesting)
- Kombinér frit i lyslister (studie + location)
- Eksempler: Grundpakke, LED-pakke, Rig-pakke, Studie-basis, HMI 6K sæt
- Hver pakke:
  - Navn
  - Beskrivelse
  - Liste af items (fra database) + antal
  - Beregnet total-vægt og total-watt
  - Dato oprettet/ændret
- Import/eksport som JSON
- Drag-and-drop tilføjelse til lyslister

---

## 6. PRINT

### Label-print

- Individuelle størrelser (håndteres separat / Brother P-touch)
- Fixture-nr + DMX-adresse + QR-kode per label

### Pakke / retur / defekt-lister

- Afkrydsningslister til pakning og retur
- Grupperet efter kategori med farvekoder
- Defekt-liste med noter og fotos

### Patch-plan

- Sorteret efter univers / fixture / placering
- Plads til håndskrevne noter
- Tydelig tabel med DMX → dimmer mapping

### Strøm-oversigt

- Tabel + visuel fordeling
- Advarsel ved overbelastning (beregnet amp > tilgængelig)
- Fase-balance visualisering

### Generelt for print

- Kategorier har gennemgående farver i app og print
- Max størrelse og tydelighed (optimeret til A4)
- PDF-generering
- Delbar via mail / AirDrop / link

---

## 7. DELING MED CREW

### Adgang

- Kun lysholdet (ikke hele produktionen)
- Lysmester er admin, giver rettigheder
- Login / kode påkrævet
- Link + QR-kode deling (invitationslink)

### Delt indhold

- Alt deles: lyslister, pakkelister, patch, location-info, defekt, noter
- Crew kan kommentere (noter/chat per item)
- Push-notifikation ved ændringer
- Real-time sync mellem enheder
- Changelog (hvem ændrede hvad, hvornår)

### Roller

| Rolle | Rettigheder |
|-------|------------|
| Admin (Belyser) | Fuld adgang, inviterer, sletter |
| Best Boy | Redigér lyslister, pakker, patch |
| Elektriker | Se + kommentér, markér pakket/returneret |
| Gæst | Kun se (read-only) |

---

## Teknisk arkitektur

| Lag | Teknologi |
|-----|-----------|
| Frontend | Single HTML/JS/CSS — ingen framework |
| Storage | localStorage + IndexedDB (offline-first) |
| Sync (fase 2) | Firebase Realtime Database eller Supabase |
| Auth (fase 2) | Invite-link med kode / Firebase Auth |
| Hosting | GitHub Pages (tpixel.github.io/light-crew) |
| Font | Tahoma Bold |
| Tema | Mørkt (match Time App æstetik) |
| Target | iPhone-first, max 430px primær |

---

## Milepæle

| Fase | Indhold | Status |
|------|---------|--------|
| 0 | Master-database med 730+ items, søg/filter | ✅ Done |
| 1 | App-ramme med alle 6 blokke, navigation, Tahoma Bold | 🔨 Nu |
| 2 | Produktion CRUD + studie/location lyslister med DMX | Næste |
| 3 | Pakke-bibliotek med nesting | Næste |
| 4 | Print-modul (PDF, patch, pakkelister) | Planlagt |
| 5 | Deling + sync + auth | Planlagt |
| 6 | Labels + QR-generering | Planlagt |

---

*Sidst opdateret: 2026-05-24*
