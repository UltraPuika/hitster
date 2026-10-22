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

Labojumi tiek saglabāti **tikai tavā pārlūkā** (localStorage). Lai tie kļūtu pieejami visiem:

1. Cilnē **Data** nospied **Export songs (JSON)**.
2. Aizstāj `data/songs.json` repozitorijā ar lejupielādēto failu un veic commit.
3. Pēc tam vari nospiest **Discard local changes**.

Gada statusi: `verified` (pārbaudīts), `spotify` (gads no Spotify izdošanas datuma, nav neatkarīgi pārbaudīts), `check` (jāpārbauda; šīs dziesmas spēlē netiek izmantotas).

## Kāršu druka

Cilne **Print**: izvēlies dziesmas, papīra apgriešanas veidu (garā vai īsā mala) un lejupielādē druku vai drukā uzreiz. Lapas ir A4 formātā ar apgriešanas zīmēm, izlaidi un izlīdzināšanas testu. Vienā pusē ir QR kods, otrā mākslinieks, nosaukums un liels gads. Pirms visas drukas izdrukā vienu lapu abpusēji un pārbaudi, vai abas puses sakrīt. Cilnē **Data** poga **Test all QR codes** pārbauda, vai visi QR kodi nolasās.

## Spēles noteikumi (īsi)

Uzvar spēlētājs, kurš pirmais sasniedz mērķa punktu skaitu un kārtas beigās ir vismaz par vienu punktu priekšā visiem pārējiem. Ja tā nav, mērķis pieaug par 1 un spēle turpinās. Uzvarētājs tiek noteikts tikai kārtas beigās.

## Dziesmas bez atvērta Spotify

Šajā versijā QR kods atver dziesmu Spotify. Atskaņošanu pašā lapā (Spotify Web Playback SDK) nav iekļauta: tai vajadzīgs Spotify Premium, Spotify izstrādātāja konts ar Client ID un autorizācija.
