# Aluetyökalujen aktivointi – ohje

Tämä ohje kuvaa tiedoston `aluetyokalut_aktivointi.csv`, jolla Mikko Kesä Oy ohjaa, mitkä työttömyysturvatyökalun aluekohtaiset versiot saavat hakea uutta dataa.

## Sijainti

Tiedosto on samassa repossa kuin työkalun muut datatiedostot, juurihakemistossa:

```
https://raw.githubusercontent.com/mikkokesa/tyottomyysturvatyokalu/refs/heads/main/aluetyokalut_aktivointi.csv
```

Nimi ja sijainti on kirjoitettu aluetyökalujen koodiin. **Älä nimeä tiedostoa uudelleen äläkä siirrä sitä**, sillä silloin kaikki aluetyökalut lakkaavat päivittämästä dataa.

## Muoto

- Merkistö UTF-8, erottimena puolipiste `;`.
- Ensimmäinen rivi on otsikkorivi, jossa on täsmälleen nämä sarakkeet:

```
lisenssi_id;alue;asiakas;aktiivinen;lisatieto
TLP-2026-01;Tunturi-Lappi ja Pello;Tunturi-Lapin ja Pellon työllisyysalue;1;Ensimmäinen aluetyökalu, toimitettu 10/2026
PIL-2026-01;Pohjois- ja Itä-Lappi;Pohjois- ja Itä-Lapin työllisyysalue;1;Toimitettu 10/2026
```

| Sarake | Pakollinen | Merkitys |
|---|---|---|
| `lisenssi_id` | kyllä | Aluetyökalun yksilöivä tunnus. Sama tunnus on kirjoitettu kyseisen työkalun koodiin. Vertailu on kirjainkoon mukainen ja tarkka. |
| `alue` | ei (ylläpidon tieto) | Työllisyysalue, jolle työkalu on rajattu. Työkalu ei lue tätä saraketta. |
| `asiakas` | ei (ylläpidon tieto) | Kenelle työkalu on luovutettu. |
| `aktiivinen` | kyllä | `1` = saa päivittää dataa. Mikä tahansa muu arvo (esim. `0` tai tyhjä) = ei saa. |
| `lisatieto` | ei | Vapaa muistiinpano, esim. sopimuksen numero tai toimituspäivä. |

Sarakkeiden järjestyksellä ei ole väliä, koska työkalu etsii sarakkeet otsikon perusteella. Älä kuitenkaan käytä puolipistettä tekstikentissä.

## Lisenssitunnuksen muoto

`<ALUEKOODI>-<VUOSI>-<JUOKSEVA NRO>`, esim. `TLP-2026-01`.

- Aluekoodi on lyhyt tunniste alueelle.

| Tunnus | Alue |
|---|---|
| TLP-2026-01 | Tunturi-Lappi ja Pello |
| PIL-2026-01 | Pohjois- ja Itä-Lappi |
- Jos samalle alueelle annetaan työkalu useammalle asiakkaalle, kukin saa oman juoksevan numeron (`TLP-2026-02`, ...). Silloin ne voi sulkea toisistaan riippumatta.
- Jos työkalu toimitetaan uudelleen esim. uuden sopimuskauden alkaessa, sille voi antaa uuden tunnuksen ja vanhan rivin voi poistaa.

## Käyttö

**Uusi aluetyökalu:** lisää tiedostoon rivi, jossa on työkalun lisenssitunnus ja `aktiivinen` = `1`. Rivi pitää lisätä ennen kuin asiakas painaa ensimmäisen kerran datan päivitystä.

**Käytön lopettaminen:** poista työkalun rivi tai muuta `aktiivinen` arvoon `0`. Arvon muuttaminen jättää historian näkyviin tiedostoon.

**Uudelleenaktivointi:** palauta rivi tai muuta arvo takaisin `1`:ksi.

## Mitä työkalussa tapahtuu

- Työkalu tarkistaa tiedoston aina ensin, ennen kuin se hakee verkosta uutta dataa, riippumatta siitä, kumpaa datapäivitysnappia käyttäjä painaa. Tarkistus ei näy käyttäjälle.
- **Aktiivinen:** data päivittyy normaalisti.
- **Ei aktiivinen, rivi puuttuu tai tiedostoa ei saada luettua:** uutta dataa ei haeta, ja käyttäjä näkee viestin *"Datan päivitys ei ole käytettävissä tälle työkalulle (lisenssi …). Ota yhteyttä Mikko Kesä Oy:hyn."*
- Selaimeen aiemmin ladattu data jää käyttöön. Työkalu toimii sillä, mutta ei pysty enää päivittämään sitä.
- Ainoa poikkeus on kuntatyyppiluokitus (`Kuntatyyppiverkostot_Manner-Suomi.csv`). Työkalu saa hakea sen aina, jotta Ranking toimii myös välimuistidatalla.
- Työkalussa ei ole päättymispäivää. Käyttö loppuu vain muokkaamalla tätä tiedostoa.

## Viiveet

- GitHub välimuistittaa raw-tiedostoja muutaman minuutin. Muutos näkyy työkaluille yleensä viiden minuutin kuluessa.
- Kun tarkistus on kerran onnistunut, se on voimassa niin kauan kuin työkalu on auki selaimessa. Poisto vaikuttaa viimeistään, kun käyttäjä avaa työkalun seuraavan kerran.

## Huomioita

- Tiedosto on julkisessa repossa, joten kuka tahansa voi lukea sen. Jos asiakkaan nimi on luottamuksellinen, jätä `asiakas`-sarake tyhjäksi tai käytä sopimuksen numeroa ja pidä asiakasrekisteri muualla.
- Vain repon omistaja voi muokata tiedostoa. Käyttäjä ei pysty aktivoimaan työkalua itse.
- Aktivointi on käyttöä ohjaava kytkin, ei murtosuojaus. Lisäksi jaettava versio on hämärretty, jokaiseen kopioon on merkitty lisenssitunnus (alapalkki, sivun metatieto ja Excel-viennit), ja käyttösopimuksessa kielletään muokkaus ja jakelu.
