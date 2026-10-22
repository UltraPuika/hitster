# Laika Līnija

HITSTER tipa latviešu mūzikas spēle: katrai kārtij ir QR kods, kas atver dziesmu Spotify, un spēlētāji liek dziesmas hronoloģiskā secībā. Kopā 555 latviešu dziesmas.

Tīmekļa lietotne bez pakalpojumiem un bez būvēšanas: viens `index.html` un dziesmu saraksts `data/songs.json`.

## Faili

| Fails | Kas tas ir |
|---|---|
| `index.html` | Visa spēle: spēle (Play), kārtis (Cards), druka (Print), dati (Data) |
| `data/songs.json` | Dziesmu saraksts, kuru lietotne ielādē |
| `data/songs.csv` | Tas pats saraksts CSV formātā (Excel / Google Sheets) |
| `.nojekyll` | Neļauj GitHub Pages apstrādāt failus ar Jekyll |

## Publicēšana ar GitHub Pages

1. Izveido jaunu GitHub repozitoriju un augšupielādē visus šos failus saknē (arī `.nojekyll`).
2. Repozitorijā: **Settings → Pages → Build and deployment**. Avots: **Deploy from a branch**, zars `main`, mape `/ (root)`.
3. Pēc minūtes spēle būs adresē `https://<lietotājs>.github.io/<repozitorijs>/`.

Lapu nevar atvērt ar dubultklikšķi (`file://`), jo pārlūks neļauj nolasīt `data/songs.json`. Lokāli palaid serveri:

```
python3 -m http.server 8000
```

un atver http://localhost:8000. Ja saraksts neielādējas, lietotne piedāvā pogu **Open songs.json**, ar kuru failu var izvēlēties pašrocīgi.

QR bibliotēkas (`qrcode-generator`, `jsQR`) tiek ielādētas no CDN, tāpēc vajadzīgs internets.

## Dziesmu labošana un jaunu pievienošana

Labojumus var veikt cilnē **Cards** (poga Edit pie dziesmas) vai uzreiz pēc dziesmas atklāšanas spēlē. Var arī pievienot jaunas dziesmas un dzēst esošās. Cilnē **Data** var importēt CSV vai JSON.

Labojumi vispirms tiek saglabāti **tikai tavā pārlūkā** (localStorage), jo GitHub Pages ir statiska vietne un pati nevar saglabāt failus. Lai tie kļūtu pieejami visiem, ir divi veidi:

**A. Publicēt tieši no lapas (ieteicams)**

1. Izveido [fine-grained personal access token](https://github.com/settings/personal-access-tokens/new): *Repository access* → tikai šis repozitorijs, *Permissions* → **Contents: Read and write**.
2. Cilnē **Data**, panelī **Publish to GitHub**, ievadi īpašnieku, repozitoriju, zaru un tokenu (GitHub Pages adresē tie tiek aizpildīti automātiski).
3. Nospied **Publish changes**. Lapa izveido commit ar atjauninātu `data/songs.json`; GitHub Pages to parāda visiem pēc ~1 minūtes. Citu cilvēku izmaiņas, kas veiktas pa to laiku, netiek pārrakstītas.

Tokens glabājas tikai tavā pārlūkā. Nelieto to koplietotos datoros; vajadzības gadījumā nospied **Forget token** vai atsauc tokenu GitHub iestatījumos.

**B. Manuāli**

1. Cilnē **Data** nospied **Export songs (JSON)**.
2. Aizstāj `data/songs.json` repozitorijā ar lejupielādēto failu un veic commit.
3. Pēc tam vari nospiest **Discard local changes**.

Gada statusi: `verified` (pārbaudīts), `spotify` (gads no Spotify izdošanas datuma, nav neatkarīgi pārbaudīts), `check` (jāpārbauda; šīs dziesmas spēlē netiek izmantotas).

## Kāršu druka

Cilne **Print**: izvēlies dziesmas, papīra apgriešanas veidu (garā vai īsā mala) un lejupielādē druku vai drukā uzreiz. Lapas ir A4 formātā ar apgriešanas zīmēm, izlaidi un izlīdzināšanas testu. Vienā pusē ir QR kods, otrā mākslinieks, nosaukums un liels gads. Pirms visas drukas izdrukā vienu lapu abpusēji un pārbaudi, vai abas puses sakrīt. Cilnē **Data** poga **Test all QR codes** pārbauda, vai visi QR kodi nolasās.

## Spēles noteikumi (īsi)

Uzvar spēlētājs, kurš pirmais sasniedz mērķa punktu skaitu un kārtas beigās ir vismaz par vienu punktu priekšā visiem pārējiem. Ja tā nav, mērķis pieaug par 1 un spēle turpinās. Uzvarētājs tiek noteikts tikai kārtas beigās.

## Atskaņošana bez nosaukuma atklāšanas

Poga ▶ spēlē atskaņo dziesmu tieši lapā, izmantojot paslēptu Spotify iegulto atskaņotāju, tāpēc nosaukums, izpildītājs un vāks netiek parādīti līdz kārts atklāšanai.

- Ja šajā pārlūkā esi pieslēdzies Spotify (un pārlūks atļauj trešo pušu sīkdatnes), skan visa dziesma; citādi tikai 30 sekunžu fragments.
- Pilnai dziesmai vienmēr var izmantot saiti **Open in Spotify instead** (tā gan parāda nosaukumu) vai noskenēt QR kodu.

### Pilnas dziesmas ar Spotify Premium

Cilnē **Data**, panelī **Full songs (Spotify Premium)**:

1. [Spotify izstrādātāja panelī](https://developer.spotify.com/dashboard) izveido lietotni, atzīmē *Web API* un *Web Playback SDK*.
2. Pievieno Redirect URI, ko rāda panelis (piem. `https://<lietotājs>.github.io/<repozitorijs>/`). Lokāli jāizmanto `http://127.0.0.1:8000/`, nevis `localhost`.
3. Sadaļā *User Management* pievieno Spotify kontus, kas spēlēs.
4. Ielīmē Client ID un nospied **Connect Spotify**.

Pēc tam dziesmas lapā skan pilnā garumā, nosaukums netiek rādīts. Vajadzīgs Spotify Premium. Ņem vērā: Spotify lietotne citās ierīcēs (piem. telefonā) var parādīt, kas pašlaik skan.

## Gada labošana spēles laikā

Pēc kārts atklāšanas zem rezultāta ir lauks **Wrong year on the card?**. Ievadi pareizo gadu un nospied **Fix & re-check**: gads tiek saglabāts (kā vietēja izmaiņa, ko pēc tam var publicēt), un atbilde tiek pārbaudīta no jauna. Ja ar pareizo gadu minējums bija pareizs, kārts paliek laika līnijā. Arī **Edit song** dialogā veiktie labojumi spēles laikā atbildi pārbauda no jauna.
